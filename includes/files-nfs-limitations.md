---
title: включить файл
description: включить файл
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 10177dd949ac531027e13cf633b11c16674fd4ab
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2020
ms.locfileid: "94386515"
---
В предварительной версии NFS имеются следующие ограничения.

- NFS 4,1 в настоящее время поддерживает только обязательные функции из [спецификации протокола](https://tools.ietf.org/html/rfc5661). Дополнительные функции, такие как делегирование и обратный вызов всех типов, блокировка обновлений и понижение уровня, а также проверка подлинности и шифрование Kerberos не поддерживаются.
- Если большинство запросов ориентированы на метаданные, то задержка будет хуже по сравнению с операциями чтения, записи и обновления.
- Чтобы создать общую папку NFS, необходимо создать новую учетную запись хранения.
- Поддерживаются только API-интерфейсы RESTFUL для плоскости управления. Интерфейсы API недоступности для плоскости данных недоступны. Это означает, что такие средства, как Обозреватель службы хранилища, не будут работать с общими папками NFS, и вы сможете просматривать данные общего доступа NFS в портал Azure.
- Доступно только для уровня "Премиум".
- Сейчас доступно только с локально избыточным хранилищем (LRS).

### <a name="azure-storage-features-not-yet-supported"></a>Функции службы хранилища Azure пока не поддерживаются

Кроме того, следующие функции службы файлов Azure недоступны в общих папках NFS:

- Проверка подлинности на основе удостоверений
- Поддержка резервного копирования Azure
- Моментальные снимки
- Обратимое удаление
- Полное шифрование — поддержка по транзитному протоколу (Дополнительные сведения см. в статье [Безопасность NFS](../articles/storage/files/storage-files-compare-protocols.md#security)).
- Синхронизация файлов Azure (доступно только для клиентов Windows, которые не поддерживаются NFS 4,1)
