---
title: Учебник. Создание назначения политики с помощью портала Azure
description: В этом учебнике рассказывается о том, как с помощью портала Azure создать назначение в службе "Политика Azure", позволяющее определить ресурсы, которые не соответствуют требованиям.
ms.topic: tutorial
ms.date: 10/07/2020
ms.openlocfilehash: 9a07e490525ce532f8f843b30b3b83715e65ce3c
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91826591"
---
# <a name="tutorial-create-a-policy-assignment-to-identify-non-compliant-resources"></a>Руководство по Создание назначения политики для идентификации ресурсов, не соответствующих требованиям

Чтобы понять, соответствуют ли ресурсы требованиям в Azure, прежде всего нужно определить их состояние. Политика Azure поддерживает аудит состояния сервера с поддержкой Arc с помощью политик гостевой конфигурации. В настоящее время политики гостевой конфигурации не применяют какие-либо конфигурации, они только выполняют аудит параметров в компьютере. В этом руководстве описывается процесс создания и назначения политики, определяющей, на каких серверах с поддержкой Arc не установлен агент Log Analytics.

По завершении этого процесса вы сможете успешно обнаруживать компьютеры, на которых не установлен агент Log Analytics для Windows или Linux. так как _не соответствуют_ назначению политики.

## <a name="prerequisites"></a>Предварительные требования

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

## <a name="create-a-policy-assignment"></a>Создание назначения политики

В этом руководстве описано, как создать назначение политики и назначить определение политики _\[Предварительная версия]. На виртуальных машинах Azure Arc с Linux должен быть установлен агент Log Analytics_.

1. Запустите службу Политика Azure на портале Azure, щелкнув **Все службы**, а затем выполнив поиск и выбрав **Политика**.

   :::image type="content" source="./media/tutorial-assign-policy-portal/search-policy.png" alt-text="Поиск политики в разделе Все службы" border="false":::

1. Выберите **Назначения** на странице службы Политики Azure слева. Назначение — это политика, которая назначена в рамках определенной области.

   :::image type="content" source="./media/tutorial-assign-policy-portal/select-assignment.png" alt-text="Поиск политики в разделе Все службы" border="false":::

1. Выберите **Назначить политику** в верхней части страницы **Policy — Assignments** (Политика — назначения).

   :::image type="content" source="./media/tutorial-assign-policy-portal/select-assign-policy.png" alt-text="Поиск политики в разделе Все службы" border="false":::

1. На странице **Назначить политику** выберите **Область**, нажав кнопку с многоточием и выбрав либо группу управления, либо подписку. Либо выберите группу ресурсов. Она определяет, к каким ресурсам или группе ресурсов принудительно применяется назначение политики. Теперь щелкните **Выбрать** в нижней части страницы **Область**.

   В этом примере используется подписка **Parnell Aerospace**. Ваша подписка будет отличаться.

1. Ресурсы можно исключить на основе параметра **Область**. **Исключения** начинаются на один уровень ниже уровня параметра **Область**. **Исключения** являются необязательными, поэтому пока оставьте это поле пустым.

1. Выберите многоточие рядом с пунктом **Определение политики**, чтобы открыть список доступных определений. Служба "Политика Azure" поставляется со встроенными определениями политик, которые можно использовать. Доступны многие политики, такие как:

   - Принудительное применение тега и его значения
   - применять тег и его значение;
   - Наследовать тег из группы ресурсов, если он отсутствует

   См. [полный список всех доступных встроенных политик Azure](../../../governance/policy/samples/index.md).

1. В списке определений политик найдите определение _\[Предварительная версия]. На виртуальных машинах Azure Arc с Windows должен быть установлен агент Log Analytics_, если на компьютере с ОС Windows активирован агент для серверов с поддержкой Arc. Для компьютера под управлением Linux найдите соответствующее определение политики _\[Предварительная версия]. На виртуальных машинах Azure Arc с Linux должен быть установлен агент Log Analytics_. Щелкните эту политику, а затем выберите действие **Выбрать**.

   :::image type="content" source="./media/tutorial-assign-policy-portal/select-available-definition.png" alt-text="Поиск политики в разделе Все службы" border="false":::

1. **Имя назначения** автоматически заполняется выбранным именем политики, но его можно изменить. Для данного примера оставьте вариант _\[Предварительная версия]. На виртуальных машинах Azure Arc с Windows должен быть установлен агент Log Analytics_ или _\[Предварительная версия]. На виртуальных машинах Azure Arc с Linux должен быть установлен агент Log Analytics_ в зависимости от того, что было выбрано ранее. При желании вы можете добавить необязательное **описание**. Описание содержит сведения о назначении этой политики.
   Поле **Назначено** будет заполнено автоматически, в зависимости от текущего пользователя. Это поле не является обязательным, поэтому можно ввести произвольные значения.

1. Оставьте флажок **Создать управляемое удостоверение** неустановленным. Его _необходимо_ устанавливать, когда политика или инициатива содержит политику с действием [deployIfNotExists](../../../governance/policy/concepts/effects.md#deployifnotexists). Так как политика, используемая в этом кратком руководстве, не такая, оставьте соответствующее поле пустым. Дополнительные сведения см. в статье [Что такое управляемые удостоверения для ресурсов Azure?](../../../active-directory/managed-identities-azure-resources/overview.md) и разделе [How remediation security works](../../../governance/policy/how-to/remediate-resources.md#how-remediation-security-works) (Как работает безопасность исправлений).

1. Щелкните **Назначить**.

Теперь все готово к обнаружению ресурсов, которые не соответствуют требованиям, что позволит оценить состояние соответствия в среде.

## <a name="identify-non-compliant-resources"></a>Выявление несоответствующих ресурсов

Выберите **Соответствие** в левой части страницы Затем найдите созданное вами назначение политики **\[Предварительная версия]. На виртуальных машинах Azure Arc с Windows должен быть установлен агент Log Analytics** или **\[Предварительная версия]. На виртуальных машинах Azure Arc с Linux должен быть установлен агент Log Analytics**.

:::image type="content" source="./media/tutorial-assign-policy-portal/policy-compliance.png" alt-text="Поиск политики в разделе Все службы" border="false":::

Существующие ресурсы, которые не соответствуют новому назначению, отображаются в разделе **Несоответствующие ресурсы**.

Если условие применяется к существующим ресурсам и оказывается верным, такие ресурсы помечаются как несовместимые с настроенной политикой. В следующей таблице показано, как действуют разные политики в сочетании с оценкой условий для определения итогового состояния соответствия. Хотя логика оценки не отображается на портале Azure, там доступны результаты, полученные при оценке состояния соответствия. Они могут быть такими: "Соответствует" либо "Не соответствует".

| **Состояние ресурса** | **Действие** | **Оценка политики** | **Состояние соответствия** |
| --- | --- | --- | --- |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Не соответствует |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Соответствует |
| Создать | Audit, AuditIfNotExist\* | True | Не соответствует |
| Создать | Audit, AuditIfNotExist\* | False | Соответствует |

\*Для эффектов Append, DeployIfNotExist и AuditIfNotExist требуется, чтобы оператор IF имел значение TRUE.
Эффекты также требуют, чтобы условие существования FALSE было несоответствующим. Когда установлено значение TRUE, условие IF запускает оценку условия существования для связанных ресурсов.

## <a name="clean-up-resources"></a>Очистка ресурсов

Чтобы удалить созданное назначение, выполните следующие действия:

1. Выберите **Соответствие** (или **Назначения**) в левой части страницы Политики Azure и перейдите к созданному вами назначению политики **\[Предварительная версия]. На виртуальных машинах Azure Arc с Windows должен быть установлен агент Log Analytics** или **\[Предварительная версия]. На виртуальных машинах Azure Arc с Linux должен быть установлен агент Log Analytics**.

1. Щелкните правой кнопкой мыши назначение политики и выберите **Удалить назначение**.

   :::image type="content" source="./media/tutorial-assign-policy-portal/delete-assignment.png" alt-text="Поиск политики в разделе Все службы" border="false":::

## <a name="next-steps"></a>Дальнейшие действия

С помощью этого учебника вы назначили определение политики для области и изучили отчет о соответствии. Определение политики проверяет, все ли ресурсы в области соответствуют требованиям, и определяет, какие из них не соответствуют. Теперь все готово для мониторинга компьютера с серверами с поддержкой Azure Arc с помощью Azure Monitor для виртуальных машин.

Чтобы узнать, как отслеживать и просматривать производительность, запустите процесс и его зависимости с компьютера и перейдите к этому учебнику:

> [!div class="nextstepaction"]
> [Включение Azure Monitor для виртуальных машин](tutorial-enable-vm-insights.md)