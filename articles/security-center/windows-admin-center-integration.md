---
title: Как защитить серверы центра администрирования Windows с помощью центра безопасности Azure
description: В этой статье объясняется, как интегрировать центр безопасности Azure с центром администрирования Windows.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: memildin
ms.openlocfilehash: 36f519ce41ccfbfb48ca696ed2a61c6131a75998
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906327"
---
# <a name="protect-windows-admin-center-resources-with-security-center"></a>Защита ресурсов центра администрирования Windows с помощью центра безопасности

Центр администрирования Windows — это средство управления для серверов Windows. Это единое место для системных администраторов, которое может получить доступ к большинству наиболее часто используемых средств администрирования. В центре администрирования Windows вы можете напрямую подключить локальные серверы к центру безопасности Azure. Вы можете просмотреть сводку рекомендаций по безопасности и оповещений непосредственно в центре администрирования Windows.

> [!NOTE]
> Чтобы включить интеграцию центра администрирования Windows, в подписке Azure и Log Analytics связанной рабочей области необходимо включить защитник Azure.
> Защитник Azure предоставляется бесплатно в течение первых 30 дней, если вы ранее не использовали его в подписке и рабочей области. Дополнительные сведения см. [на странице сведений о ценах](security-center-pricing.md).
>

После успешного подключения сервера из центра администрирования Windows в центр безопасности Azure вы можете:

* Просмотр оповещений системы безопасности и рекомендаций в расширении центра безопасности в центре администрирования Windows
* Просмотрите уровень безопасности и получите дополнительные подробные сведения об управляемых серверах центра администрирования Windows в центре безопасности в портал Azure (или через API).

Комбинируя эти два средства, центр безопасности превращается в единую область стекла для просмотра всей информации о безопасности, независимо от ресурса: Защита центра администрирования Windows, управляемых локальными серверами, виртуальными машинами и дополнительными рабочими нагрузками PaaS.

## <a name="onboard-windows-admin-center-managed-servers-into-security-center"></a>Встроенные управляемые серверы центра администрирования Windows в центр безопасности

1. В центре администрирования Windows выберите один из серверов, а затем в области **средства** выберите расширение центра безопасности Azure:

    ![Расширение центра безопасности Azure в центре администрирования Windows](./media/windows-admin-center-integration/onboarding-from-wac.png)

    > [!NOTE]
    > Если сервер уже подключен к центру безопасности, окно настройки не появится.

1. Щелкните **войти в Azure и настройте**.
    ![Подключение расширения центра администрирования Windows к центру безопасности Azure](./media/windows-admin-center-integration/onboarding-from-wac-welcome.png)

1. Следуйте инструкциям, чтобы подключить сервер к центру безопасности. После того как вы ввели необходимые сведения и подтвердили, центр безопасности вносит необходимые изменения в конфигурацию, чтобы обеспечить соблюдение следующих условий.
    * Шлюз Azure зарегистрирован.
    * Сервер содержит рабочую область для отчета и связанную подписку.
    * Решение Log Analytics центра безопасности включено в рабочей области. Это решение предоставляет функции защитника Azure для *всех* серверов и виртуальных машин, отправляющих отчеты в эту рабочую область.
    * В подписке включен Защитник Azure для серверов.
    * Агент Log Analytics установлен на сервере и настроен для передачи отчетов в выбранную рабочую область. Если сервер уже передает данные в другую рабочую область, он также будет настроен для передачи в только что выбранную рабочую область.

    > [!NOTE]
    > После адаптации для отображения рекомендаций может потребоваться некоторое время. Фактически, в зависимости от активности сервера вы не *можете получить оповещения* . Чтобы создать тестовые оповещения для проверки правильности работы предупреждений, следуйте инструкциям в [процедуре проверки предупреждений](security-center-alert-validation.md).


## <a name="view-security-recommendations-and-alerts-in-windows-admin-center"></a>Просмотр рекомендаций по безопасности и оповещений в центре администрирования Windows

После подключения вы можете просматривать оповещения и рекомендации непосредственно в области центра безопасности Azure центра администрирования Windows. Щелкните рекомендацию или оповещение, чтобы просмотреть их в портал Azure. Здесь вы получите дополнительные сведения и узнаете, как устранить проблемы.

[![Рекомендации и оповещения центра безопасности, как показано в центре администрирования Windows](media/windows-admin-center-integration/asc-recommendations-and-alerts-in-wac.png)](media/windows-admin-center-integration/asc-recommendations-and-alerts-in-wac.png#lightbox)

## <a name="view-security-recommendations-and-alerts-for-windows-admin-center-managed-servers-in-security-center"></a>Просмотр рекомендаций по безопасности и оповещений для управляемых серверов центра администрирования Windows в центре безопасности
Из центра безопасности Azure:

* Чтобы просмотреть рекомендации по безопасности для всех серверов центра администрирования Windows, откройте [инвентаризацию ресурсов](asset-inventory.md) и примените фильтр к типу компьютера, который необходимо исследовать. Перейдите на вкладку **виртуальные машины и компьютеры** .

* Чтобы просмотреть оповещения системы безопасности для всех серверов центра администрирования Windows, откройте **оповещения системы безопасности**. Щелкните **Фильтр** **и убедитесь,** что выбрано значение не Azure.

    :::image type="content" source="./media/windows-admin-center-integration/filtering-alerts-by-environment.png" alt-text="Фильтрация оповещений системы безопасности для управляемых серверов центра администрирования Windows" lightbox="./media/windows-admin-center-integration/filtering-alerts-by-environment.png":::