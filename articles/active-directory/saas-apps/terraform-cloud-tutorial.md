---
title: Руководство по интеграции единого входа Azure Active Directory с Terraform Cloud | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Terraform Cloud.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/29/2020
ms.author: jeedes
ms.openlocfilehash: 0c93cecae194a61a51cb63159938b04f5a7b12b6
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133342"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-terraform-cloud"></a>Руководство по интеграции единого входа Azure Active Directory с Terraform Cloud

В этом учебнике описано, как интегрировать Terraform Cloud с Azure Active Directory (Azure AD). Интеграция Terraform Cloud с Azure AD обеспечивает следующие возможности.

* Контроль доступа к Terraform Cloud с помощью Azure AD.
* Включение автоматического входа пользователей в Terraform Cloud с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Terraform Cloud с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Terraform Cloud поддерживает единый вход, инициированный **поставщиком служб и поставщиком удостоверений**.
* Terraform Cloud поддерживает **JIT**-подготовку пользователей.


## <a name="adding-terraform-cloud-from-the-gallery"></a>Добавление Terraform Cloud из коллекции

Чтобы настроить интеграцию Terraform Cloud с Azure AD, необходимо добавить Terraform Cloud из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Terraform Cloud**.
1. Выберите **Terraform Cloud** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-terraform-cloud"></a>Настройка и проверка единого входа Azure AD для Terraform Cloud

Настройте и проверьте единый вход Azure AD в Terraform Cloud с помощью тестового пользователя **B. Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Terraform Cloud.

Чтобы настроить и проверить единый вход Azure AD в Terraform Cloud, выполните действия из следующих стандартных блоков.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Terraform Cloud](#configure-terraform-cloud-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Terraform Cloud](#create-terraform-cloud-test-user)** требуется для того, чтобы в Terraform Cloud существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Terraform Cloud** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://app.terraform.io/sso/saml/samlconf-<ID>/metadata`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://app.terraform.io/session`.

    > [!NOTE]
    > Значение идентификатора приведено для примера и не является реальным. Вместо него нужно указать фактический идентификатор. Чтобы получить эти значения, обратитесь в [группу поддержки клиентов Terraform Cloud](mailto:tf-cloud@hashicorp.support). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)
### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю B. Simon доступ к Terraform Cloud, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Terraform Cloud**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-terraform-cloud-sso"></a>Настройка единого входа Terraform Cloud

1. В другом окне веб-браузера войдите на веб-сайт Terraform Cloud с правами администратора.

2. Последовательно выберите **Settings > SSO > Edit Settings** (Параметры > Единый вход > Изменить параметры).

    ![Параметры Terraform Cloud](./media/terraform-cloud-tutorial/sso-settings.png)

3. На странице **Edit SSO** (Изменить единый вход) выполните следующие действия.

    ![Изменение единого входа Terraform Cloud](./media/terraform-cloud-tutorial/edit-sso.png)

    а. В текстовое поле **Sign-On URL** (URL-адрес для входа) вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    б. В текстовое поле **Entity ID or Identifier** (Идентификатор сущности или идентификатор) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    c. Откройте в блокноте скачанный с портала Azure файл **сертификата** и вставьте его содержимое в текстовое поле **Public Certificate** (Общедоступный сертификат).

    d. Нажмите кнопку **Save settings** (Сохранить параметры).

### <a name="create-terraform-cloud-test-user"></a>Создание тестового пользователя Terraform Cloud

Во время работы с этим разделом мы создадим в Terraform Cloud пользователя Britta Simon. В Terraform Cloud поддерживается JIT-подготовка пользователей, включенная по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Terraform Cloud, он создается после проверки подлинности.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Terraform Cloud, где можно инициировать поток входа.  

* Перейдите по URL-адресу для входа в Terraform Cloud и инициируйте поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* Выберите **Тестировать приложение** на портале Azure, и вы автоматически войдете в Terraform Cloud, для которого настроен единый вход 

Вы можете также использовать Панель доступа корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку Terraform Cloud на Панели доступа, вы будете перенаправлены на страницу входа приложения, если выполнены настройки для использования в режиме поставщика услуг, или автоматически войдете в приложение Terraform Cloud, для которого настроен единый вход, если выполнены настройки для использования в режиме поставщика удостоверений. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)


## <a name="next-steps"></a>Дальнейшие действия

После настройки Terraform Cloud вы сможете применить функцию управления сеансом, которая защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в режиме реального времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


