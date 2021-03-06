---
title: включить файл
description: включить файл
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ba715d510dc296ffa8f9c0ee58841f284416a118
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86027343"
---
## <a name="protect-your-access-keys"></a>Защита ключей доступа

Ключи доступа к учетной записи хранения аналогичны корневому паролю для вашей учетной записи хранения. Всегда будьте внимательны при защите ключей доступа. Используйте Azure Key Vault для безопасного управления ключами и их смены. Старайтесь не распространять ключи доступа для других пользователей, жестко программировать их или сохранять в любом месте в виде обычного текста, доступного другим пользователям. Если вы считаете, что они могли быть скомпрометированы, поворачивайте свои ключи.

> [!NOTE]
> Корпорация Майкрософт рекомендует использовать Azure Active Directory (Azure AD) для авторизации запросов к данным большого двоичного объекта и очереди, если это возможно, вместо общего ключа. Azure AD обеспечивает более высокую безопасность и простоту использования общего ключа. Дополнительные сведения о предоставлении доступа к данным с помощью Azure AD см. в статье [авторизация доступа к BLOB-объектам и очередям Azure с помощью Azure Active Directory](../articles/storage/common/storage-auth-aad.md).
