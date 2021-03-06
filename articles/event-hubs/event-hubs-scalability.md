---
title: Масштабируемость — концентраторы событий Azure | Документация Майкрософт
description: Эта статья содержит сведения о том, как масштабировать концентраторы событий Azure с помощью секций и единиц пропускной способности.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 4dacb24ace2332f590db54959cbf1f06694b982b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86521961"
---
# <a name="scaling-with-event-hubs"></a>Масштабирование с помощью концентраторов событий

Существует два фактора, которые влияют на масштабирование с помощью концентраторов событий.
*   Единицы пропускной способности
*   Секции

## <a name="throughput-units"></a>Единицы пропускной способности

Пропускная способность концентраторов событий управляется *единицами пропускной способности*. Единицы пропускной способности — это предварительно приобретенные единицы емкости. Одна пропускная способность позволяет:

* Входящий трафик: до 1 МБ в секунду либо 1000 событий в секунду (в зависимости от того, что произойдет раньше).
* Исходящий трафик: до 2 МБ в секунду или 4096 событий в секунду.

Превышение входящей емкости, выраженной в приобретенных единицах пропускной способности, подлежит регулированию с вызовом исключения [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception). Хотя исходящий трафик не связан с возвратом исключения регулирования, он все равно ограничен емкостью, выраженной в приобретенных пропускных единицах. Если возвращаются исключения скорости публикации или ожидается повышенный объем исходящих данных, проверьте количество единиц пропускной способности, приобретенных для пространства имен. Вы можете управлять единицами пропускной способности из колонки **Масштабирование** пространств имен на [портале Azure](https://portal.azure.com). Это также можно реализовать программным способом с помощью [интерфейсов API Центров событий](./event-hubs-samples.md).

Единицы пропускной способности тарифицируются почасово и приобретаются заранее. После приобретения единицы пропускной способности тарифицируются минимум за один час. Для пространства имен Центров событий можно приобрести до 20 единиц пропускной способности (этот объем можно распределить между всеми концентраторами событий в пространстве имен).

Функция **автоматического расширения** Центров событий автоматически масштабирует число единиц пропускной способности в соответствии с потребностями. Увеличение единиц пропускной способности предотвращает сценарии регулирования, в которых:

- скорости входящего трафика данных превышают установленные единицы пропускной способности;
- скорости запросов исходящего трафика данных превышают установленные единицы пропускной способности.

Служба "Центры событий" увеличивает пропускную способность, когда загрузка превышает минимальное пороговое значение. При это ни запросы не возвращают ошибки ServerBusy. 

Дополнительные сведения о функции автоматического расширения см. в разделе [Автоматическое масштабирование единиц пропускной способности](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Секции
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]

### <a name="partition-key"></a>Ключ секции

Вы можете использовать [ключ секции](event-hubs-programming-guide.md#partition-key), чтобы сопоставлять входные данные событий с определенными секциями для организации данных. Ключ секции — это указываемое отправителем значение, передаваемое в концентратор событий. Оно обрабатывается с помощью статической функции хэширования, в результате чего создается назначение секции. Если при публикации события не указать ключ секции, назначение создается с помощью циклического перебора.

Издателю событий известен только ключ секции, но не сама секция, в которой публикуются события. Благодаря разделению ключа и секции отправителю не нужно располагать избыточными сведениями о последующей обработке и хранении событий. Уникальное удостоверение устройства или пользователя является хорошим ключом секции, но другие атрибуты, например географическое положение, можно также использовать для группировки связанных событий в одну секцию.


## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о Центрах событий см. в следующих источниках:

- [Автоматическое масштабирование единиц пропускной способности](event-hubs-auto-inflate.md)
- [Обзор службы Центров событий](./event-hubs-about.md)
