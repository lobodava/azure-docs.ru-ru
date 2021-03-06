---
title: Azure Red Hat OpenShift с OpenShift 4. Настройка проверки подлинности Azure Active Directory с помощью командной строки
description: Узнайте, как настроить проверку подлинности Azure Active Directory для кластера Azure Red Hat OpenShift с OpenShift 4 с помощью командной строки.
ms.service: container-service
ms.topic: article
ms.date: 03/12/2020
author: sabbour
ms.author: asabbour
keywords: aro, openshift, az aro, red hat, cli
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 03ecd0e11df5fa20f134b6fd87baf788078a2203
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92748036"
---
# <a name="configure-azure-active-directory-authentication-for-an-azure-red-hat-openshift-4-cluster-cli"></a>Настройка проверки подлинности Azure Active Directory для кластера Azure Red Hat OpenShift 4 (CLI)

Если вы решили установить и использовать CLI локально, для работы с этой статьей требуется Azure CLI версии 2.6.0 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).

Получите URL-адреса, относящиеся к конкретному кластеру, которые будут использоваться для настройки приложения Azure Active Directory.

Создайте URL-адрес обратного вызова OAuth кластера и сохраните его в переменной **оаускаллбаккурл** . Не забудьте заменить **АТО-RG** именем группы ресурсов, а **АТО-Cluster —** именем кластера.

> [!NOTE]
> `AAD`Раздел в URL-адресе обратного вызова OAuth должен соответствовать имени поставщика удостоверений OAuth, которое будет настроено позже.

```azurecli-interactive
domain=$(az aro show -g aro-rg -n aro-cluster --query clusterProfile.domain -o tsv)
location=$(az aro show -g aro-rg -n aro-cluster --query location -o tsv)
apiServer=$(az aro show -g aro-rg -n aro-cluster --query apiserverProfile.url -o tsv)
webConsole=$(az aro show -g aro-rg -n aro-cluster --query consoleProfile.url -o tsv)
oauthCallbackURL=https://oauth-openshift.apps.$domain.$location.aroapp.io/oauth2callback/AAD
```

## <a name="create-an-azure-active-directory-application-for-authentication"></a>Создание приложения Azure Active Directory для проверки подлинности

Создайте приложение Azure Active Directory и получите созданный идентификатор приложения. Замените **\<ClientSecret>** защищенным паролем.

```azurecli-interactive
az ad app create \
  --query appId -o tsv \
  --display-name aro-auth \
  --reply-urls $oauthCallbackURL \
  --password '<ClientSecret>'
```

Вы должны получить нечто вроде этого. Запишите его, так как это **идентификатор AppID** , который потребуется в последующих шагах.

```output
6a4cb4b2-f102-4125-b5f5-9ad6689f7224
```

Получите идентификатор клиента подписки, владеющей приложением.

```azure
az account show --query tenantId -o tsv
```

Вы должны получить нечто вроде этого. Запишите его, так как это идентификатор **TenantId** , который потребуется в последующих шагах.

```output
72f999sx-8sk1-8snc-js82-2d7cj902db47
```

## <a name="create-a-manifest-file-to-define-the-optional-claims-to-include-in-the-id-token"></a>Создание файла манифеста для определения необязательных утверждений для включения в маркер идентификации

Разработчики приложений могут использовать [необязательные утверждения](../active-directory/develop/active-directory-optional-claims.md) в своих приложениях Azure AD, чтобы указать, какие утверждения они хотят в токенах, отправляемых в приложение.

Необязательные утверждения можно использовать, чтобы:

- выбрать дополнительные утверждения для включения в токены для приложения;
- изменить поведение определенных утверждений в токенах, возвращаемых Azure AD;
- добавлять пользовательские утверждения для приложения и обращаться к ним.

Мы настроим OpenShift на использование `email` утверждения и вернемся к `upn` установке предпочтительного имени пользователя, добавив в `upn` маркер идентификации, возвращенный Azure Active Directory.

Создайте **manifest.js** для файла, чтобы настроить приложение Azure Active Directory.

```bash
cat > manifest.json<< EOF
[{
  "name": "upn",
  "source": null,
  "essential": false,
  "additionalProperties": []
},
{
"name": "email",
  "source": null,
  "essential": false,
  "additionalProperties": []
}]
EOF
```

## <a name="update-the-azure-active-directory-applications-optionalclaims-with-a-manifest"></a>Обновление optionalClaims приложения Azure Active Directory с помощью манифеста

Замените **\<AppID>** идентификатором, который вы получили ранее.

```azurecli-interactive
az ad app update \
  --set optionalClaims.idToken=@manifest.json \
  --id <AppId>
```

## <a name="update-the-azure-active-directory-application-scope-permissions"></a>Обновление разрешений области приложения Azure Active Directory

Чтобы иметь возможность считывать сведения о пользователе из Azure Active Directory, необходимо определить соответствующие области.

Замените **\<AppID>** идентификатором, который вы получили ранее.

Добавьте разрешение для области **графа Azure Active Directory Graph. пользователь. Read** , чтобы включить вход и чтение профиля пользователя.

```azurecli-interactive
az ad app permission add \
 --api 00000002-0000-0000-c000-000000000000 \
 --api-permissions 311a71cc-e848-46a1-bdf8-97ff7156d8e6=Scope \
 --id <AppId>
```

> [!NOTE]
> Если вы не прошли проверку подлинности в качестве глобального администратора для этого Azure Active Directory, можно проигнорировать сообщение, чтобы предоставить согласие, так как после входа в свою учетную запись вам будет предложено сделать это.

## <a name="assign-users-and-groups-to-the-cluster-optional"></a>Назначение пользователей и групп кластеру (необязательно)

Приложения, зарегистрированные в клиенте Azure Active Directory (Azure AD), по умолчанию являются доступными для всех пользователей клиента, которые успешно прошли проверку подлинности. Azure AD позволяет администраторам и разработчикам клиента ограничить доступ к приложению определенными пользователями или группами безопасности в клиенте.

Следуйте инструкциям в документации по Azure Active Directory, чтобы [назначить пользователей и группы для приложения](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md#app-registration).

## <a name="configure-openshift-openid-authentication"></a>Настройка проверки подлинности OpenShift OpenID Connect

Получите `kubeadmin` учетные данные. Выполните следующую команду, чтобы найти пароль для пользователя `kubeadmin`.

```azurecli-interactive
az aro list-credentials \
  --name aro-cluster \
  --resource-group aro-rg
```

В следующем примере вывода показано, что пароль будет находиться в `kubeadminPassword`.

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Войдите на сервер API кластера OpenShift с помощью следующей команды. `$apiServer`Переменная была задана [ранее](). Замените **\<kubeadmin password>** паролем, который вы получили.

```azurecli-interactive
oc login $apiServer -u kubeadmin -p <kubeadmin password>
```

Создайте секрет OpenShift для хранения секрета приложения Azure Active Directory, заменив **\<ClientSecret>** его секретом, полученным ранее.

```azurecli-interactive
oc create secret generic openid-client-secret-azuread \
  --namespace openshift-config \
  --from-literal=clientSecret=<ClientSecret>
```

Создайте файл **oidc. YAML** , чтобы настроить проверку подлинности OpenShift openid connect в Azure Active Directory. Замените **\<AppID>** и **\<TenantId>** значениями, полученными ранее.

```bash
cat > oidc.yaml<< EOF
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: AAD
    mappingMethod: claim
    type: OpenID
    openID:
      clientID: <AppId>
      clientSecret:
        name: openid-client-secret-azuread
      extraScopes:
      - email
      - profile
      extraAuthorizeParameters:
        include_granted_scopes: "true"
      claims:
        preferredUsername:
        - email
        - upn
        name:
        - name
        email:
        - email
      issuer: https://login.microsoftonline.com/<TenantId>
EOF
```

Примените конфигурацию к кластеру.

```azurecli-interactive
oc apply -f oidc.yaml
```

Вы получите ответ, аналогичный приведенному ниже.

```output
oauth.config.openshift.io/cluster configured
```

## <a name="verify-login-through-azure-active-directory"></a>Проверка имени входа с помощью Azure Active Directory

Если теперь выйти из веб-консоли OpenShift и повторить попытку входа, отобразится новый параметр для входа с помощью **AAD** . Возможно, потребуется подождать несколько минут.

![Экран входа с Azure Active Directory параметром](media/aro4-login-2.png)
