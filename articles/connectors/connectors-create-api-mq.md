---
title: Подключение к серверу IBM MQ
description: Отправляйте и получайте сообщения с помощью Azure или локального сервера IBM MQ и Azure Logic Apps.
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, estfan, logicappspm
ms.topic: article
ms.date: 05/14/2020
tags: connectors
ms.openlocfilehash: e9e554fdc092e49f5a87049de0e3dc3163105f58
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85609509"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Подключение к серверу IBM MQ из Azure Logic Apps

Соединитель IBM MQ отправляет и получает сообщения, хранящиеся локально на сервере IBM MQ или в Azure. Этот соединитель включает в себя клиент Microsoft MQ для взаимодействия с удаленным сервером IBM MQ по сети TCP/IP. Эта статья является базовым руководством для использования соединителя MQ. Вы можете начать с просмотра одного сообщения в очереди и затем попробовать другие действия.

Соединитель IBM MQ включает следующие действия, но не предоставляет никаких триггеров.

- Просмотр отдельного сообщения без удаления его с сервера IBM MQ.
- Просмотр пакета сообщений без удаления сообщений с сервера IBM MQ.
- Получение и удаление отдельного сообщения с сервера IBM MQ.
- Получение и удаление пакета сообщений с сервера IBM MQ.
- Отправка отдельного сообщения на сервер IBM MQ.

Ниже приведены официально поддерживаемые версии IBM WebSphere MQ.

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0
  * MQ 9.1

## <a name="prerequisites"></a>Предварительные требования

* Если используется локальный сервер MQ, [установите локальный шлюз данных](../logic-apps/logic-apps-gateway-install.md) на сервере в пределах сети. Чтобы соединитель MQ работал, на сервере, где установлен локальный шлюз данных, также должна быть установлена платформа .NET Framework 4.6.

  После завершения установки шлюза необходимо также создать в Azure ресурс для локального шлюза данных. Дополнительные сведения см. в разделе [Настройка подключения к шлюзу данных](../logic-apps/logic-apps-gateway-connection.md).

  Если сервер MQ является общедоступным или доступным в Azure, вам не нужно использовать шлюз данных.

* Приложение логики, в которое необходимо добавить действие MQ. Это приложение логики должно использовать то же расположение, что и подключение к локальному шлюзу данных, и должно уже иметь триггер, запускающий ваш рабочий процесс.

  Соединитель MQ не имеет триггеров, поэтому сначала необходимо добавить триггер в ваше приложение логики. Например, можно использовать триггер повторения. Если вы пока не знакомы с приложениями логики, ознакомьтесь с этим [кратким руководством по созданию первого приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-connection"></a>

## <a name="create-mq-connection"></a>Создание подключения MQ

Если при добавлении действия MQ у вас еще нет подключения MQ, вам будет предложено создать такое подключение, например:

![Предоставление сведений о подключении](media/connectors-create-api-mq/connection-properties.png)

1. Если вы подключаетесь к локальному серверу MQ, выберите **Подключаться с помощью локального шлюза данных**.

1. Укажите сведения для подключения к серверу MQ.

   * В поле **Сервер** укажите имя сервера MQ или введите IP-адрес, после него поставьте двоеточие, а затем укажите номер порта.

   * Чтобы использовать SSL, выберите **Включить SSL?** .

     Сейчас соединитель MQ поддерживает только проверку подлинности сервера, но не проверку подлинности клиента. Дополнительные сведения см. в разделе [Проблемы при подключении и проверке подлинности](#connection-problems).

1. В разделе **Шлюз** выполните следующие шаги.

   1. В списке **Подписка** выберите подписку Azure, связанную с вашим ресурсом шлюза Azure.

   1. В списке **Шлюз для подключения** выберите ресурс шлюза Azure, который хотите использовать.

1. Когда все будет готово, выберите **Создать**.

<a name="connection-problems"></a>

### <a name="connection-and-authentication-problems"></a>Проблемы при подключении и проверке подлинности

Когда приложение логики пытается подключиться к локальному серверу MQ, может появиться следующее сообщение об ошибке.

`"MQ: Could not Connect the Queue Manager '<queue-manager-name>': The Server was expecting an SSL connection."`

* Если вы используете соединитель MQ непосредственно в Azure, сервер MQ должен использовать сертификат, выданный доверенным [центром сертификации](https://www.ssl.com/faqs/what-is-a-certificate-authority/).

* Если вы используете локальный шлюз данных, постарайтесь по возможности использовать сертификат, выданный доверенным [центром сертификации](https://www.ssl.com/faqs/what-is-a-certificate-authority/). Однако если это невозможно, можно использовать самозаверяющий сертификат, который не выдается доверенным [центром сертификации](https://www.ssl.com/faqs/what-is-a-certificate-authority/) и считается менее безопасным.

  Чтобы установить самозаверяющий сертификат сервера, можно воспользоваться средством **диспетчера сертификации Windows** (certmgr.msc). В этом сценарии на локальном компьютере, где запущена служба локального шлюза данных, нужно установить сертификат в хранилище сертификатов **Локальный компьютер** на уровне **Доверенные корневые центры сертификации**.

  1. На компьютере, где запущена служба локального шлюза данных, откройте меню "Пуск", найдите и выберите **Управление сертификатами пользователей**.

  1. Когда средство диспетчера сертификации Windows откроется, перейдите в папку **Сертификаты — локальный компьютер** >  **Доверенные корневые центры сертификации** и установите сертификат.

     > [!IMPORTANT]
     > Убедитесь, что сертификат устанавливается в хранилище на локальном компьютере **Сертификаты — локальный компьютер** > **Доверенные корневые центры сертификации**.

* Для сервера MQ необходимо определить спецификацию шифра, которая будет использоваться для соединений TLS/SSL. Однако SslStream в .NET не позволяет указывать порядок спецификаций шифров. Чтобы обойти это ограничение, можно изменить конфигурацию сервера MQ в соответствии с первой спецификацией шифра в наборе, который соединитель отправляет в согласовании TLS/SSL.

  При попытке подключения сервер MQ регистрирует сообщение о событии, указывающее на сбой соединения, так как другая сторона использовала неверную спецификацию шифра. Сообщение о событии содержит спецификацию шифра, которая является первой в списке. Измените спецификацию шифра в конфигурации канала, чтобы она соответствовала спецификации шифра в сообщении о событии.

## <a name="browse-single-message"></a>Просмотр одного сообщения

1. В приложении логики в разделе триггера или другого действия выберите **Новый шаг**.

1. В поле поиска введите `mq` и выберите действие **Просмотреть сообщение**.

   ![Выбор действия "Просмотреть сообщение"](media/connectors-create-api-mq/browse-message.png)

1. Если вы еще не создали подключение MQ, вам будет предложено [создать его](#create-connection).

1. После создания подключения настройте свойства действия **Просмотреть сообщение**.

   | Свойство | Описание |
   |----------|-------------|
   | **Очередь** | Если очередь отличается от указанной в соединении, укажите эту очередь. |
   | **MessageId**, **CorrelationId**, **GroupId** и другие свойства | Поиск сообщения на основе различных свойств сообщений MQ |
   | **IncludeInfo** | Чтобы включить в выходные данные дополнительные сведения о сообщении, выберите **true**. Чтобы не включать в выходные данные дополнительные сведения о сообщении, выберите **false**. |
   | **Timeout** | Введите значение, чтобы определить, как долго ожидать получения сообщения в пустой очереди. Если это значение не задано, то извлекается первое сообщение в очереди, и, следовательно, время на ожидание сообщения не тратится. |
   |||

   Пример:

   ![Свойства действия "Просмотреть сообщение"](media/connectors-create-api-mq/browse-message-properties.png)

1. По завершении нажмите кнопку **Сохранить** на панели инструментов конструктора. Чтобы протестировать приложение, выберите **Запустить**.

   После завершения выполнения конструктор отображает шаги рабочего процесса и их состояние, чтобы можно было просмотреть выходные данные.

1. Чтобы просмотреть сведения о каждом шаге, щелкните его заголовок. Чтобы просмотреть дополнительные сведения о выходных данных шага, выберите **Показать необработанные выходные данные**.

   ![Просмотр сообщения: выходные данные](media/connectors-create-api-mq/browse-message-output.png)

   Ниже приведен пример необработанных выходных данных.

   ![Просмотр сообщения: необработанные выходные данные](media/connectors-create-api-mq/browse-message-raw-output.png)

1. Если для параметра **IncludeInfo** задано значение **true**, отображаются дополнительные выходные данные.

   ![Просмотр сообщения: IncludeInfo](media/connectors-create-api-mq/browse-message-include-info.png)

## <a name="browse-multiple-messages"></a>Просмотр нескольких сообщений

Действие **Просмотреть сообщения** имеет параметр **BatchSize**, указывающий, сколько сообщений должно быть возвращено из очереди. Если значение для параметра **BatchSize** не задано, возвращаются все сообщения. Возвращаемые выходные данные представляют собой массив сообщений.

1. Выполните описанные выше действия, но добавьте вместо этого действие **Просмотреть сообщения**.

1. Если вы еще не создали подключение MQ, вам будет предложено [создать его](#create-connection). В противном случае по умолчанию используется первое ранее настроенное соединение. Для создания соединения выберите **Изменить подключение** или выберите другое соединение.

1. Укажите сведения для действия.

1. Сохраните и запустите приложение логики.

   Ниже приведен пример выходных данных для действия **Просмотреть сообщения**, отображаемых после завершения выполнения приложения логики.

   ![Пример выходных данных для действия "Просмотреть сообщения"](media/connectors-create-api-mq/browse-messages-output.png)

## <a name="receive-single-message"></a>Получение одного сообщения

Действие **Получить сообщение** имеет такие входные и выходные данные, что и действие **Browse message** (Просмотреть сообщение). При использовании действия **Получить сообщение** это сообщение удаляется из очереди.

## <a name="receive-multiple-messages"></a>Получение нескольких сообщений

Действие **Получить сообщения** имеет такие входные и выходные данные, что и действие **Browse messages** (Просмотреть сообщения). При использовании действия **Получить сообщения** эти сообщения удаляются из очереди.

> [!NOTE]
> При выполнении действия просмотра или получения в очереди, в которой нет сообщений, действие завершается сбоем и выводит следующие данные.
>
> ![Ошибка MQ при отсутствии сообщений](media/connectors-create-api-mq/mq-no-message-error.png)

## <a name="send-message"></a>Отправить сообщение

1. Выполните описанные выше действия, но добавьте вместо этого действие **Отправить сообщение**.

1. Если вы еще не создали подключение MQ, вам будет предложено [создать его](#create-connection). В противном случае по умолчанию используется первое ранее настроенное соединение. Для создания соединения выберите **Изменить подключение** или выберите другое соединение.

1. Укажите сведения для действия. Для **MessageType** выберите допустимый тип сообщений: **датаграмма**, **ответ** или **запрос**.

   ![Свойства действия "Отправить сообщение"](media/connectors-create-api-mq/send-message-properties.png)

1. Сохраните и запустите приложение логики.

   Ниже приведен пример выходных данных для действия **Отправить сообщения**, отображаемых после завершения выполнения приложения логики.

   ![Пример выходных данных для действия "Отправить сообщения"](media/connectors-create-api-mq/send-message-output.png)

## <a name="connector-reference"></a>Справочник по соединителям

Дополнительные технические сведения о действиях и ограничениях, которые приведены в описании Swagger соединителя, можно найти на [странице справки](/connectors/mq/) соединителя.

## <a name="next-steps"></a>Дальнейшие действия

* См. дополнительные сведения о других [соединителях Logic Apps](../connectors/apis-list.md).
