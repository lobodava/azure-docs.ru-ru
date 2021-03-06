---
title: Использование обозревателя Azure Cosmos DB для управления данными
description: Обозреватель Azure Cosmos DB — это автономный веб-интерфейс, который позволяет просматривать и управлять данными, которые хранятся в базе данных Azure Cosmos DB.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/23/2020
ms.author: dech
ms.openlocfilehash: 691a2e56230d312416aed3d68bffd361f1d63558
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2020
ms.locfileid: "94637121"
---
# <a name="work-with-data-using-azure-cosmos-explorer"></a>Работа с данными с помощью обозревателя Azure Cosmos 
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Обозреватель Azure Cosmos DB — это автономный веб-интерфейс, который позволяет просматривать и управлять данными, которые хранятся в базе данных Azure Cosmos DB. Обозреватель Azure Cosmos DB эквивалентен существующей вкладке **Обозреватель данных** , которая доступна на портале Azure при создании учетной записи Azure Cosmos DB. Основные преимущества обозревателя Azure Cosmos DB над существующим обозревателем данных перечислены ниже.

* У вас есть возможность полноэкранного просмотра данных, запуска запросов, хранимых процедур, триггеров и просмотра их результатов.  

* Можно предоставить временный или постоянный доступ на чтение или чтение и запись для своей учетной записи базы данных и ее коллекциям для других пользователей, которые не имеют доступа к порталу Azure или подписке.  

* Результатами запроса можно поделиться с другими пользователями, у которых нет доступа к порталу Azure или подписке.  

## <a name="access-azure-cosmos-db-explorer"></a>Доступ к обозревателю Azure Cosmos DB

1. Войдите на [портал Azure](https://portal.azure.com/). 

2. Из **Всех ресурсов** найдите и перейдите к своей учетной записи Azure Cosmos DB, выберите "Ключи" и скопируйте **Первичную строку подключения**.  

3. Перейдите к https://cosmos.azure.com/, вставьте строку подключения и выберите **Подключить**. Используя строку подключения, можно получить доступ к обозревателю Azure Cosmos DB без каких-либо ограничений по времени.  

   Если нужно предоставить другим пользователям временный доступ к учетной записи Azure Cosmos DB, вы можете сделать это, используя URL-адреса для чтения или чтения и записи. 

4. Откройте колонку **Обозреватель данных** и выберите **Открыть во весь экран**. Во всплывающем диалоговом окне можно просмотреть два URL-адреса доступа — **Чтение и запись** и **Чтение**. Эти URL-адреса позволяют временно использовать вашу учетную запись Azure Cosmos DB совместно с другими пользователями. Доступ к учетной записи истекает через 24 часа, после чего вы можете повторно подключиться, используя новый URL-адрес доступа или строку подключения. 

   **Чтение и запись**. При совместном использовании URL-адреса для чтения и записи с другими пользователями они могут просматривать и изменять базы данных, коллекции, запросы и другие ресурсы, связанные с этой конкретной учетной записью.

   **Чтение**. При совместном использовании URL-адреса с другими пользователями только для чтения, они могут просматривать базы данных, коллекции, запросы и другие ресурсы, связанные с этой конкретной учетной записью. Например, если вы хотите поделиться результатами запроса со своими коллегами, у которых нет доступа к порталу Azure или вашей учетной записи Azure Cosmos DB, вы можете предоставить им этот URL-адрес.

   Выберите тип доступа, с которым вы хотите открыть учетную запись, и нажмите **Открыть**. После того, как вы откроете обозреватель, возможности будут такими же, как и на вкладке "Обозреватель данных" на портале Azure.

   :::image type="content" source="./media/data-explorer/open-data-explorer-with-access-url.png" alt-text="Открыть обозреватель Azure Cosmos DB":::

## <a name="known-issues"></a>Известные проблемы

В настоящее время функция **Открыть во весь экран** , которая позволяет использовать временный доступ для чтения и записи, еще не поддерживается для учетных записей Azure Cosmos DB Gremlin и API таблиц. Вы по-прежнему можете просматривать свои учетные записи Gremlin и API таблицы, передавая строку подключения обозревателю Azure Cosmos DB. 

В настоящее время просмотр документов, содержащих UUID, не поддерживается в обозреватель данных. Это не влияет на загрузку коллекций, просмотрев только отдельные документы или запросы, содержащие эти документы. Чтобы просматривать эти документы и управлять ими, пользователи должны продолжать использовать средство, которое использовалось для создания этих документов.

Клиенты, получающие ошибки HTTP-401, могут быть вызваны недостаточными разрешениями Azure RBAC для учетной записи Azure клиента, особенно если у учетной записи есть настраиваемая роль. Любые пользовательские роли должны иметь `Microsoft.DocumentDB/databaseAccounts/listKeys/*` действие для использования обозреватель данных при входе с использованием учетных данных Azure Active Directory.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как начать работу с обозревателем Azure Cosmos DB для управления данными, вы можете:

* Начать определение [запросов](./sql-query-getting-started.md) с помощью синтаксиса языка SQL и выполнить [программирование на стороне сервера](stored-procedures-triggers-udfs.md) с помощью хранимых процедур, UDF, триггеров.
