---
title: Определение типа приложения — масштабирование (Цитус) — база данных Azure для PostgreSQL
description: Оценка приложения для эффективного моделирования распределенных данных
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 92333857177d33307d6997bfcbdf79787d3ab127
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90895945"
---
# <a name="determining-application-type"></a>Определение типа приложения

Для выполнения эффективных запросов в группе серверов с горизонтальным масштабированием (Цитус) необходимо, чтобы таблицы были правильно распределены между серверами. Рекомендуемое распределение зависит от типа приложения и его шаблонов запросов.

Существует два вида приложений, которые хорошо подходят для масштабирования (Цитус). Первым шагом в моделировании данных является указание того, какое из них более близко напоминает ваше приложение.

## <a name="at-a-glance"></a>Краткий обзор

| Приложения с несколькими клиентами                                 | Real-Time приложения                                |
|-----------------------------------------------------------|-------------------------------------------------------|
| Иногда десятки или сотни таблиц в схеме          | Небольшое число таблиц                                |
| Запросы, относящиеся к одному клиенту (компании или магазину) за раз | Сравнительно простые аналитические запросы с агрегатами |
| Рабочие нагрузки OLTP для обслуживания веб-клиентов                    | Высокий объем принимаемых в основном неизменяемых данных           |
| Рабочие нагрузки OLAP, которые обслуживают аналитические запросы для каждого клиента   | Часто по центру вокруг большой таблицы событий            |

## <a name="examples-and-characteristics"></a>Примеры и характеристики

**Приложение с несколькими клиентами**

> Обычно это приложения SaaS, которые обслуживают другие компании, учетные записи или организации. Большинство приложений SaaS по сути являются реляционными. Они имеют естественное измерение для распределения данных между узлами: только сегментирование по \_ идентификатору клиента.
>
> Масштабирование (Цитус) позволяет масштабировать базу данных до миллионов клиентов без необходимости повторной архитектуры приложения. Вы можете обеспечить необходимую реляционную семантику, например объединения, ограничения внешнего ключа, транзакции, ACID и согласованность.
>
> -   **Примеры**: веб-сайты, на которых размещен магазин — передняя часть для других организаций, например решение для цифрового маркетинга или средство автоматизации продаж.
> -   **Характеристики**: запросы, относящиеся к одному клиенту, а не объединять данные между клиентами. Сюда входят рабочие нагрузки OLTP для обслуживания веб-клиентов, а также рабочие нагрузки OLAP, которые обслуживают аналитические запросы для каждого клиента. Наличие десятков или сотен таблиц в схеме базы данных также является индикатором для модели данных с несколькими клиентами.
>
> Масштабирование приложения с несколькими клиентами с помощью масштабирования (Цитус) также требует внесения минимальных изменений в код приложения. Мы поддерживаем популярные платформы, такие как Ruby на шинах и Django.

**Аналитика в режиме реального времени**

> Приложениям требуется массовый параллелизм, координирующий сотни ядер для быстрого получения результатов для числовых, статистических или инвентаризационных запросов.  При сегментировании и параллелизации запросов SQL на нескольких узлах масштабирование (Цитус) позволяет выполнять запросы в режиме реального времени в миллиардах записей за секунду.
>
> Таблицы в моделях данных аналитики в режиме реального времени обычно распределяются по столбцам \_ , например идентификатору пользователя, \_ идентификатору узла или \_ идентификатору устройства.
>
> -   **Примеры**. на панелях мониторинга анализа, ориентированных на клиентов, требуются время отклика в секунду.
> -   **Характеристики**: несколько таблиц, часто исключаясь о больших таблицах событий устройств, веб-узлов или пользователей и требуя большого объема данных, которые в основном являются неизменяемыми. Сравнительно простые (но не требующие вычислений) аналитические запросы, включающие несколько агрегатов и ГРУППовых бис.

Если ситуация похожа на приведенный выше, то следующим шагом является выбор способа сегментирования данных в группе серверов. Выбор столбцов распределения администратором базы данных \' должен соответствовать шаблонам доступа типовых запросов для обеспечения производительности.

## <a name="next-steps"></a>Дальнейшие шаги

* [Выберите столбец распределения](concepts-hyperscale-choose-distribution-column.md) для таблиц в приложении, чтобы эффективно распределять данные
