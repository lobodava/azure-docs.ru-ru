---
title: включить файл
description: включить файл
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ef9efe389894af7c792e980922ca422e9d05929b
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95562208"
---
В следующей таблице приведены стандартные ограничения таких учетных записей хранения Azure: общего назначения версии 1 и версии 2, хранилища BLOB-объектов и хранилища блочных BLOB-объектов. Под ограничением на *входящий трафик* подразумеваются все данные, отправляемые в учетную запись хранения. Под ограничением на *исходящий трафик* подразумеваются все данные, получаемые из учетной записи хранения.

> [!NOTE]
> Вы можете запросить более высокую емкость и ограничение на входящий трафик. Чтобы отправить такой запрос, обратитесь в [Службу поддержки Azure](https://azure.microsoft.com/support/faq/).

| Ресурс | Ограничение |
| --- | --- |
| Количество учетных записей хранения на регион и подписку, включая учетные записи хранения ценовых категорий "Стандартный" и "Премиум".| 250 |
| Максимальная емкость учетной записи хранения | 5 ПиБ <sup>1</sup>|
| Максимальное количество контейнеров BLOB-объектов, BLOB-объектов, общих папок, таблиц, очередей, сущностей или сообщений на учетную запись хранения | Без ограничений |
| Максимальная частота запросов<sup>1</sup> на учетную запись хранения | 20 000 запросов в секунду |
| Максимальная скорость входящего трафика<sup>1</sup> на учетную запись хранения (регионы США и Европы) | 10 Гбит/с |
| Максимальная скорость входящего трафика<sup>1</sup> на учетную запись хранения (регионы, за пределами США и Европы) | 5 Гбит/с для хранилищ RA-GRS и GRS, 10 Гбит/с для хранилищ LRS и ZRS<sup>2</sup> |
| Максимальная скорость исходящего трафика для учетной записи хранения общего назначения версии 2 и учетной записи хранения BLOB-объектов (все регионы) | 50 Гбит/с |
| Максимальная скорость исходящего трафика для учетной записи хранения общего назначения версии 1 (регионы США) | 20 Гбит/с для хранилищ RA-GRS и GRS, 30 Гбит/с для хранилищ LRS и ZRS<sup>2</sup> |
| Максимальная скорость исходящего трафика для учетной записи хранения общего назначения версии 1 (регионы за пределами США) | 10 Гбит/с для хранилищ RA-GRS и GRS, 15 Гбит/с для хранилищ LRS и ZRS<sup>2</sup> |
| Максимальное число правил виртуальной сети на учетную запись хранения | 200 |
| Максимальное число правил IP-адресов на учетную запись хранения | 200 |

<sup>1</sup> Учетные записи хранения Azure (цен. категории "Стандартный") поддерживают больший объем и более высокую скорость для входящего трафика, предоставляемые по запросу. Чтобы подать запрос на увеличение ограничений для учетной записи, обратитесь в [службу поддержки Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup> Если для учетной записи хранения с поддержкой геоизбыточного хранилища (RA-GRS) и хранилища, избыточного между зонами (RA-GZRS), включен доступ на чтение, входящие целевые объекты для вторичного расположения будут идентичны целевым объектам для первичного расположения. Дополнительные сведения см. в статье [Репликация службы хранилища Azure](../articles/storage/common/storage-redundancy.md).

> [!NOTE]
> Корпорация Майкрософт рекомендует для большинства сценариев использовать учетные записи хранения общего назначения версии 2. Учетную запись хранения общего назначения версии 1 или хранилища BLOB-объектов Azure можно легко обновить до учетной записи общего назначения версии 2 без простоев или копирования данных. Дополнительные сведения см. в статье [Обновление до учетной записи хранения общего назначения версии 2](../articles/storage/common/storage-account-upgrade.md).

Все учетные записи хранения работают в плоской топологии сети независимо от того, когда они были созданы. Дополнительные сведения о неструктурированной сетевой архитектуре службы хранилища Azure и масштабируемости см. в статье [SOSP Paper – Windows Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](/archive/blogs/hanuk/windows-azures-flat-network-storage-to-enable-higher-scalability-targets) (Хранилище Microsoft Azure: высокодоступная служба облачного хранения со строгой согласованностью).