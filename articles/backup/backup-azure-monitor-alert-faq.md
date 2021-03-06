---
title: Вопросы и ответы о мониторинге
description: В этой статье содержатся ответы на часто задаваемые вопросы о предупреждении мониторинга Azure Backup и Azure Backupх отчетов.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: cf6929b9b926a6e6469f3fa789a19e60d5883d21
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89181499"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Оповещение мониторинга Azure Backup — часто задаваемые вопросы

В этой статье содержатся ответы на часто задаваемые вопросы о мониторинге Azure Backup и создании отчетов.

## <a name="configure-azure-backup-reports"></a>Настройка отчетов службы Azure Backup

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-log-analytics-la-workspace"></a>Разделы справки проверить, начали ли данные отчетов поступать в Log Analytics (LA) рабочую область?

Перейдите к настроенной рабочей области LA. Перейдите к пункту меню **журналы** и выполните запрос `CoreAzureBackup | take 1` . Если отображается возвращаемая запись, это означает, что данные начали поступать в рабочую область. Начальная отправка данных может занять до 24 часов.

### <a name="what-is-the-frequency-of-data-push-to-an-la-workspace"></a>Какова частота отправки данных в LA рабочую область?

Данные диагностики из хранилища передаются в рабочую область Log Analytics с некоторой задержкой. Каждое событие поступает в рабочую область Log Analytics через 20–30 минут после его отправки из хранилища Служб восстановления. Ниже приведены дополнительные сведения об этой задержке.

* Во всех решениях встроенные оповещения службы резервного копирования передаются сразу после их создания. Поэтому они обычно отображаются в рабочей области Log Analytics через 20–30 минут.
* Во всех решениях задания резервного копирования по запросу и задания восстановления передаются сразу после их завершения.
* Во всех решениях, кроме резервного копирования SQL, запланированные задания резервного копирования передаются сразу после их завершения.
* Для резервного копирования SQL, так как резервное копирование журналов может происходить каждые 15 минут, сведения обо всех завершенных запланированных заданиях резервного копирования, включая журналы, помещаются в пакеты и передаются каждые 6 часов.
* Во всех решениях другие сведения, такие как элемент резервного копирования, политика, точки восстановления, хранилище и т. п., передаются по меньшей мере один раз в день.
* Изменение конфигурации резервного копирования (например, смена или изменение политики) активирует принудительную передачу всех связанных сведений резервного копирования.

### <a name="how-long-can-i-retain-reporting-data"></a>Сколько времени можно хранить данные отчетов?

После создания "LA" рабочей области вы можете хранить данные не более 2 лет. По умолчанию Ла Рабочая область содержит данные в течение 31 дня.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-la-workspace"></a>Отображаются ли все мои данные в отчетах после настройки LA Workspace?

 Все данные, созданные после настройки параметров диагностики, помещаются в рабочую область LA и доступны в отчетах. Выполняющиеся задания не отражаются в отчетах. Сведения о них будут переданы после их завершения или сбоя.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Могу ли я ли просмотреть отчеты по нескольким хранилищам и подпискам?

Да, можно просматривать отчеты по хранилищам и подпискам, а также по регионам. Данные могут находиться в одной LA рабочей области или в группе LA рабочей области.

### <a name="can-i-view-reports-across-tenants"></a>Можно ли просматривать отчеты по клиентам?

Если вы являетесь пользователем [Azure лигхсаусе](https://azure.microsoft.com/services/azure-lighthouse/) с делегированным доступом к подпискам ваших клиентов или La рабочие области, вы можете использовать отчеты резервного копирования для просмотра данных по всем клиентам.

## <a name="recovery-services-vault"></a>Хранилище Служб восстановления

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Сколько времени займет отображение состояния задания агента Azure Backup на портале?

Портал Azure может занять до 15 минут, чтобы отразить состояние задания агента Azure Backup.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Сколько требуется времени на создание оповещения при сбое задания резервного копирования?

Оповещение создается в течение 20 минут после сбоя Azure Backup.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Возможна ли ситуация, когда сообщение электронной почты не будет отправлено при настроенных уведомлениях?

Да. В следующих ситуациях уведомления не отправляются:

* настроена почасовая отправка уведомлений, а предупреждение создается и устраняется в течение часа;
* задание отменено;
* второе задание резервного копирования не удалось выполнить из-за того, что еще выполняется предыдущее задание резервного копирования.

## <a name="next-steps"></a>Дальнейшие шаги

См. другие статьи с вопросами и ответами:

* [Распространенные вопросы](backup-azure-vm-backup-faq.md) о резервном копировании виртуальных машин Azure
* [Распространенные вопросы](backup-azure-file-folder-backup-faq.md) об агенте Azure Backup
