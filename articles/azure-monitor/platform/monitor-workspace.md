---
title: Мониторинг работоспособности рабочей области Log Analytics в Azure Monitor
description: В этой статье описывается, как отслеживать работоспособность рабочей области Log Analytics с помощью данных в таблице операций.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/20/2020
ms.openlocfilehash: 07d9ae0d7cdf8e823bb59cb376d40cdf846bb2cb
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93092761"
---
# <a name="monitor-health-of-log-analytics-workspace-in-azure-monitor"></a>Мониторинг работоспособности рабочей области Log Analytics в Azure Monitor
Чтобы обеспечить производительность и доступность рабочей области Log Analytics в Azure Monitor, необходимо иметь возможность упреждающего обнаружения возникающих проблем. В этой статье описывается, как отслеживать работоспособность рабочей области Log Analytics с помощью данных в таблице [операций](https://docs.microsoft.com/azure/azure-monitor/reference/tables/operation) . Эта таблица включается в каждую Log Analytics рабочую область и содержит ошибки и предупреждения, происходящие в рабочей области. Следует регулярно проверять эти данные и создавать оповещения, чтобы заранее получать уведомления при наличии в рабочей области важных инцидентов.

## <a name="_logoperation-function"></a>Функция _LogOperation

Azure Monitor журналы отправляют сведения о проблемах в таблицу [операций](https://docs.microsoft.com/azure/azure-monitor/reference/tables/operation) в рабочей области, в которой возникла проблема. **_LogOperation** системная функция основана на таблице **операций** и предоставляет упрощенный набор сведений для анализа и оповещений.

## <a name="columns"></a>Столбцы

Функция **_LogOperation** возвращает столбцы в следующей таблице.

| Столбец | Описание |
|:---|:---|
| TimeGenerated | Время возникновения инцидента в формате UTC. |
| Категория  | Группа категорий операций. Может использоваться для фильтрации по типам операций и создания более точного системного аудита и оповещений. Список категорий см. в разделе ниже. |
| Операция  | Описание типа операции. Это может означать один из ограничений Log Analytics, тип операции или часть процесса. |
| Level | Степень серьезности проблемы:<br>-INFO: особое внимание не требуется.<br>-Warning: процесс не был завершен должным образом, и требуется вмешательство.<br>-Ошибка: сбой процесса, и требуется срочное вмешательство. 
| Подробный сведения | Подробное описание операции включает конкретное сообщение об ошибке, если оно существует. |
| _ResourceId | Идентификатор ресурса Azure, связанного с операцией.  |
| Компьютер | Имя компьютера, если операция связана с агентом Azure Monitor. |
| CorrelationId | Используется для группирования последовательных связанных операций. |


## <a name="categories"></a>Категории

В следующей таблице описаны категории из функции _LogOperation. 

| Категория | Описание |
|:---|:---|
| Прием           | Операции, которые являются частью процесса приема данных. Дополнительные сведения см. ниже. |
| Агент               | Указывает на ошибку при установке агента. |
| Сбор данных     | Операции, связанные с процессами сбора данных. |
| Нацеленность на решение  | Операция типа *конфигуратионскопе* была обработана. |
| Решение для оценки | Выполнен процесс оценки. |


### <a name="ingestion"></a>Прием
Операции приема — это проблемы, которые произошли во время приема данных, включая уведомления о достижении ограничений рабочей области Azure Log Analytics. Условия ошибок в этой категории могут предложить потери данных, поэтому они особенно важны для отслеживания. В таблице ниже приводятся подробные сведения об этих операциях. См. раздел [ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) для ограничений службы для рабочих областей log Analytics.


| Операция | Level | Подробный сведения | Связанная статья |
|:---|:---|:---|:---|
| Пользовательский журнал | Ошибка   | Достигнут предел для столбцов настраиваемых полей. | [Ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) |
| Пользовательский журнал | Ошибка   | Сбой приема пользовательских журналов. | |
| Метаданные. | Ошибка | Обнаружена ошибка конфигурации. | |
| Сбор данных | Ошибка   | Данные были удалены, так как запрос был создан раньше, чем число дней набора. | [Управление использованием и затратами с помощью журналов Azure Monitor](manage-cost-storage.md#alert-when-daily-cap-reached)
| Сбор данных | Info    | Обнаружена конфигурация компьютера сбора.| |
| Сбор данных | Info    | Сбор данных начат из-за нового дня. | [Управление использованием и затратами с помощью журналов Azure Monitor](/azure/azure-monitor/platform/manage-cost-storage#alert-when-daily-cap-reached) |
| Сбор данных | Предупреждение | Сбор данных остановлен, так как достигнут ежедневный предел.| [Управление использованием и затратами с помощью журналов Azure Monitor](/azure/azure-monitor/platform/manage-cost-storage#alert-when-daily-cap-reached) |
| Обработка данных | Ошибка   | Недопустимый формат JSON. | [Отправка данных журналов в Azure Monitor c помощью API сборщика данных HTTP (общедоступная предварительная версия)](data-collector-api.md#request-body) | 
| Обработка данных | Предупреждение | Значение сокращено до максимально допустимого размера. | [Ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) |
| Обработка данных | Предупреждение | Достигнуто значение поля, усеченное как ограничение размера. | [Ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) | 
| Скорость приема | Info | Достигнут предел скорости приема на 70%. | [Ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) |
| Скорость приема | Предупреждение | Достигнут предел скорости приема. | [Ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) |
| Скорость приема | Ошибка   | Достигнут предел скорости. | [Ограничения службы Azure Monitor](../service-limits.md#log-analytics-workspaces) |
| Память | Ошибка   | Не удается получить доступ к учетной записи хранения, так как используемые учетные данные недопустимы.  |



   

## <a name="alert-rules"></a>Правила оповещения
Используйте [оповещения о запросах журнала](../platform/alerts-log-query.md) в Azure Monitor, чтобы заранее получать уведомления при обнаружении проблемы в рабочей области log Analytics. Следует использовать стратегию, которая позволяет своевременно реагировать на проблемы и свести к минимуму затраты. Плата за каждое правило генерации оповещений наследуется по стоимости, в зависимости от частоты оценки.

Рекомендуемая стратегия состоит в том, чтобы начать с двух правил генерации оповещений на основе уровня проблемы. Используйте короткий интервал, например каждые 5 минут для ошибок, и более длинную частоту, например 24 часа для предупреждений. Поскольку ошибки свидетельствуют о возможной потере данных, необходимо быстро реагировать на них, чтобы избежать потери. Предупреждения обычно указывают на проблемы, которые не требуют немедленного вмешательства, поэтому их можно просматривать ежедневно.

Используйте процесс [создания, просмотра и управления оповещениями журнала с помощью Azure Monitor](../platform/alerts-log.md) , чтобы создать правила генерации оповещений журнала. В следующих разделах описаны подробные сведения о каждом правиле.


| Запрос | Пороговое значение | Период | Частота |
|:---|:---|:---|:---|
| `_LogOperation | where Level == "Error"`   | 0 | 5 | 5 |
| `_LogOperation | where Level == "Warning"` | 0 | 1440 | 1440 |

Эти правила генерации оповещений будут отвечать на все операции с ошибкой или предупреждением. По мере знакомства с операциями, создающими предупреждения, может возникнуть необходимость реагирования на определенные операции. Например, может потребоваться отправить уведомления различным людям для выполнения определенных операций. 

Чтобы создать правило генерации оповещений для определенной операции, используйте запрос, включающий столбцы **категории** и **операции** . 

В следующем примере создается предупреждающее оповещение, если ставка объема приема достигла 80% от предела.

- Цель: выберите рабочую область Log Analytics
- Критерии:
  - "Название сигнала" — "Поиск по пользовательским журналам";
  - Поисковый запрос: `_LogOperation | where Category == "Ingestion" | where Operation == "Ingestion rate" | where Level == "Warning"`
  - "На основе" — Количество результатов
  - "Условие" — Больше
  - "Пороговое значение" — 0
  - "Период" — 5 (минут);
  - "Частота" — 5 (минут);
- "Имя правила генерации оповещений" — "Достигнут ежедневный предел сбора данных";
- "Уровень серьезности" — "Предупреждение (серьезность 1)"


В следующем примере создается предупреждающее оповещение, если сбор данных достиг ежедневного предела. 

- Цель: выберите рабочую область Log Analytics
- Критерии:
  - "Название сигнала" — "Поиск по пользовательским журналам";
  - Поисковый запрос: `_LogOperation | where Category == "Ingestion" | where Operation == "Data Collection" | where Level == "Warning"`
  - "На основе" — Количество результатов
  - "Условие" — Больше
  - "Пороговое значение" — 0
  - "Период" — 5 (минут);
  - "Частота" — 5 (минут);
- "Имя правила генерации оповещений" — "Достигнут ежедневный предел сбора данных";
- "Уровень серьезности" — "Предупреждение (серьезность 1)"



## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об [оповещениях журнала](alerts-log.md).
- [Собирайте данные аудита запросов](../log-query/query-audit.md) для рабочей области.
