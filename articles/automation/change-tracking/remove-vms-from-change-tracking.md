---
title: Исключение виртуальных машин из процедуры отслеживания изменений и инвентаризации в службе автоматизации Azure
description: Эта статья описывает, как исключить виртуальные машины из процедуры отслеживания изменений и инвентаризации.
services: automation
ms.subservice: change-inventory-management
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: 0b79fa22d3203504e63161aba03b32830d74d016
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93131281"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Исключение виртуальных машин из Отслеживания изменений и инвентаризации

Завершив отслеживание изменений на виртуальных машинах в вашей среде, вы можете запретить управление ими с помощью функции [Отслеживание изменений и инвентаризации](overview.md) . Чтобы больше не управлять ими, сохраненный запрос поиска будет изменен `MicrosoftDefaultComputerGroup` в рабочей области log Analytics, связанной с учетной записью службы автоматизации.

## <a name="sign-into-the-azure-portal"></a>Войдите на портал Azure.

Войдите на [портал Azure](https://portal.azure.com).

## <a name="to-remove-your-vms"></a>Удаление виртуальных машин

1. В портал Azure запустите **Cloud Shell** из верхней панели навигации портал Azure. Если вы не знакомы с Azure Cloud Shell, см. раздел [обзор Azure Cloud Shell](../../cloud-shell/overview.md).

2. Используйте следующую команду, чтобы указать UUID компьютера, который требуется удалить из управления.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. В портал Azure перейдите в раздел **log Analytics рабочие области** . Выберите рабочую область в списке.

4. В рабочей области Log Analytics выберите **журналы** , а затем выберите **Обозреватель запросов** в меню "основные действия".

5. В **обозревателе запросов** на правой панели разверните **сохраненные куериес\упдатес** и выберите сохраненный поисковый запрос, `MicrosoftDefaultComputerGroup` чтобы изменить его.

6. В редакторе запросов Проверьте запрос и найдите UUID для виртуальной машины. Удалите UUID виртуальной машины и повторите шаги для всех остальных виртуальных машин, которые необходимо удалить.

7. Сохраните сохраненный поиск по завершении редактирования, нажав кнопку **сохранить** на верхней панели.

>[!NOTE]
>Компьютеры по-прежнему отображаются после отмены регистрации, так как мы получаем отчет по всем компьютерам, оцененным за последние 24 часа. После удаления компьютера необходимо подождать 24 часа, прежде чем они перестанут отображаться в списке.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы снова включить эту функцию, см. статью [включение отслеживание изменений и инвентаризации из учетной записи службы автоматизации](enable-from-automation-account.md), [Включение отслеживание изменений и инвентаризацию путем просмотра портал Azure](enable-from-portal.md), [включения отслеживание изменений и инвентаризации из модуля runbook](enable-from-runbook.md), а также [включения отслеживание изменений и инвентаризации на виртуальной машине Azure](enable-from-vm.md).