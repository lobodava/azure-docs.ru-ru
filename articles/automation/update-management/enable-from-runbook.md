---
title: Включение Управления обновлениями службы автоматизации в последовательности runbook
description: Эта статья содержит сведения об Управлении обновлениями в последовательности runbook.
services: automation
ms.topic: conceptual
ms.date: 09/30/2020
ms.custom: mvc
ms.openlocfilehash: ec102015355e3312f5dc15fa526fa543da75e0de
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92222665"
---
# <a name="enable-update-management-from-a-runbook"></a>Включение Управления обновлениями в последовательности runbook

Эта статья содержит сведения об использовании runbook для включения функции [Управление обновлениями](overview.md) виртуальных машин в вашей среде. Чтобы включить масштабирование виртуальных машин Azure, необходимо включить существующую виртуальную машину с Управление обновлениями.

> [!NOTE]
> При включении функции "Управление обновлениями" только определенные регионы поддерживают связывание рабочей области Log Analytics и учетной записи службы автоматизации. Список поддерживаемых пар сопоставлений см. в статье о [сопоставлении региона для учетной записи службы автоматизации и рабочей области Log Analytics](../how-to/region-mappings.md).

Этот метод использует два модуля Runbook:

* **Enable-MultipleSolution** — основной модуль Runbook, который запрашивает сведения о конфигурации, запрашивает УКАЗАНную виртуальную машину и выполняет другие проверки, а затем вызывает модуль Runbook **Enable-аутоматионсолутион** для настройки Управление обновлениями для каждой виртуальной машины в указанной группе ресурсов.
* **Enable-аутоматионсолутион** — включает управление обновлениями для одной или нескольких виртуальных машин, указанных в Целевой группе ресурсов. Он проверяет соблюдение предварительных требований, проверяет, установлено ли Log Analytics расширение виртуальной машины и устанавливает его, если оно не найдено, и добавляет виртуальные машины в конфигурацию области в указанной Log Analytics рабочей области, связанной с учетной записью службы автоматизации.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас ее нет, [активируйте преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) или [зарегистрируйте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Учетная запись службы автоматизации](../automation-security-overview.md) для управления компьютерами.
* [Рабочая область Log Analytics](../../azure-monitor/platform/design-logs-deployment.md)
* [Виртуальная машина](../../virtual-machines/windows/quick-create-portal.md).
* Два ресурса автоматизации, которые используются модулем Runbook **Enable-аутоматионсолутион** . Этот модуль Runbook, если он еще не существует в вашей учетной записи службы автоматизации, автоматически импортируется модулем Runbook **Enable-MultipleSolution** во время первого запуска.
    * *Ласолутионсубскриптионид*: идентификатор подписки, в которой находится рабочая область log Analytics.
    * *Ласолутионворкспацеид*: идентификатор рабочей области log Analytics, связанной с вашей учетной записью службы автоматизации.

    Эти переменные используются для настройки рабочей области подключенной виртуальной машины. Если они не указаны, сценарий сначала выполняет поиск любой виртуальной машины, подключенной к Управление обновлениями в своей подписке, за которой следует подписка, в которой находится учетная запись службы автоматизации, а затем все другие подписки, к которым имеет доступ учетная запись пользователя. Если они не настроены должным образом, это может привести к подключению компьютеров к некоторой случайной Log Analytics рабочей области.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на [портал Azure](https://portal.azure.com).

## <a name="enable-update-management"></a>Включение управления обновлениями

1. В портал Azure перейдите к **учетным записям службы автоматизации**. На странице **учетные записи службы автоматизации** выберите свою учетную запись из списка.

2. В учетной записи службы автоматизации в области **Управление обновлениями** выберите **Управление обновлениями**.

3. Выберите рабочую область Log Analytics и нажмите кнопку **Включить**. При включении Управление обновлениями отображается баннер.

    ![Включение управления обновлениями](media/enable-from-runbook/enable-update-management.png)

## <a name="install-and-update-modules"></a>установка и обновление модулей;

Чтобы успешно включить Управление обновлениями для виртуальных машин с помощью модуля Runbook, необходимо выполнить обновление до последних модулей Azure и импортировать модуль [AZ. OperationalInsights](/powershell/module/az.operationalinsights) .

1. В учетной записи службы автоматизации выберите элемент **Модули** в области **Общие ресурсы**.

2. Выберите **Обновить модули Azure**, чтобы обновить модули Azure до последней версии.

3. Щелкните вариант **Да**, чтобы обновить все существующие модули Azure до последней версии.

    ![Обновление модулей](media/enable-from-runbook/update-modules.png)

4. Вернитесь к параметру **Модули** в разделе **Общие ресурсы**.

5. Щелкните **Обзор коллекции**, чтобы открыть коллекцию модулей.

6. Найдите `Az.OperationalInsights` и импортируйте этот модуль в учетную запись службы автоматизации.

    ![Импорт модуля OperationalInsights](media/enable-from-runbook/import-operational-insights-module.png)

## <a name="select-azure-vm-to-manage"></a>Выбор виртуальной машины Azure для управления

После включения Управления обновлениями можно добавить виртуальную машину Azure для получения обновлений.

1. В учетной записи службы автоматизации выберите **Управление обновлениями** в разделе **Управление обновлениями**.

2. Щелкните элемент **Добавить виртуальные машины Azure**, чтобы добавить виртуальную машину.

3. Выберите виртуальную машину из списка и нажмите кнопку **включить** , чтобы настроить виртуальную машину для управления.

   ![Включение Управления обновлениями для виртуальной машины](media/enable-from-runbook/enable-update-management-vm.png)

    > [!NOTE]
    > Если вы попытаетесь включить другую функцию до завершения установки Управления обновлениями, появится следующее сообщение: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="import-a-runbook-to-enable-update-management"></a>Импорт модуля runbook для включения Управления обновлениями

1. В учетной записи службы автоматизации выберите **Модули Runbook** в области **Автоматизация процессов**.

2. Выберите **Обзор коллекции**.

3. Поиск **обновлений и отслеживание изменений**.

4. Выберите модуль Runbook и нажмите кнопку **Импорт** на странице **Просмотр исходного кода** .

5. Нажмите кнопку **ОК**, чтобы импортировать runbook в учетную запись службы автоматизации.

   ![Импорт модуля runbook для настройки](media/enable-from-runbook/import-from-gallery.png)

6. На странице **Runbook** выберите Runbook **Enable-MultipleSolution** и нажмите кнопку **изменить**. В текстовом редакторе выберите  **опубликовать**.

7. При появлении запроса на подтверждение нажмите кнопку **Да** , чтобы опубликовать модуль Runbook.

## <a name="start-the-runbook"></a>Запуск модуля runbook

Для запуска модуля runbook необходимо включить Управление обновлениями. Для настройки одной или нескольких виртуальных машин в Целевой группе ресурсов требуется существующая виртуальная машина и группа ресурсов с включенной функцией.

1. Откройте runbook с именем **Enable-MultipleSolution**.

   ![Модуль runbook для нескольких решений](media/enable-from-runbook/runbook-overview.png)

2. Нажмите кнопку "Пуск" и введите значения параметров в следующих полях:

   * **VMNAME** — имя существующей виртуальной машины, добавляемой в Управление обновлениями. Оставьте это поле пустым, чтобы добавить все виртуальные машины в группе ресурсов.
   * **VMRESOURCEGROUP** — имя группы ресурсов для включения виртуальных машин.
   * **SUBSCRIPTIONID** — идентификатор подписки новой включаемой виртуальной машины. Оставьте это поле пустым, чтобы использовать подписку рабочей области. При использовании другого идентификатора подписки добавьте учетную запись запуска для учетной записи службы автоматизации в качестве участника подписки.
   * **ALREADYONBOARDEDVM** — имя виртуальной машины, которая уже включена вручную для обновлений.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** — имя группы ресурсов, в которую входит виртуальная машина.
   * **SOLUTIONTYPE** — введите **Обновления**.

   ![Параметры модуля runbook Enable-MultipleSolution](media/enable-from-runbook/runbook-parameters.png)

3. Выберите **ОК**, чтобы выполнить задание runbook.

4. Отслеживайте ход выполнения задания Runbook и всех ошибок на странице **задания** .

## <a name="next-steps"></a>Дальнейшие действия

* Сведения об использовании Управление обновлениями для виртуальных машин см. в статье [Управление обновлениями и исправлениями для виртуальных машин](manage-updates-for-vm.md).

* Дополнительные сведения см. в статье [Устранение неполадок с Управлением обновлениями](../troubleshoot/update-management.md).
