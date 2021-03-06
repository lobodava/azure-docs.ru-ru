---
title: Краткое руководство. Отправка настраиваемых событий в Функции Azure с помощью Сетки событий
description: Краткое руководство. Используйте службу "Сетка событий Azure" и Azure CLI или портал, чтобы иметь возможность публиковать темы и подписываться на эти события. Служба "Функции Azure" используется для конечной точки.
ms.date: 07/07/2020
ms.topic: quickstart
ms.openlocfilehash: aea52bcaa94d6f288e86e44e1a0f294796d8e4a3
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91324440"
---
# <a name="quickstart-route-custom-events-to-an-azure-function-with-event-grid"></a>Краткое руководство. Маршрутизация настраиваемых событий в Функции Azure с помощью Сетки событий

"Сетка событий Azure" — это служба обработки событий для облака. Функции Azure — это один из поддерживаемых обработчиков событий. В рамках этой статьи с помощью портала Azure вы создадите пользовательский раздел, подпишетесь на раздел и активируете событие для просмотра результата. Затем вы отправите события в Функции Azure.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-azure-function"></a>Создание Функций Azure
Перед подпиской на пользовательский раздел необходимо создать функцию для обработки событий. 

1. Создайте приложение-функцию, используя инструкции из [соответствующего](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) раздела.
2. Создайте функцию с помощью **триггера Сетки событий**. Если вы используете этот триггер впервые, возможно, потребуется щелкнуть "Установить", чтобы установить расширение.
    1. На странице **Приложение-функция** в меню слева выберите **Функции**, в шаблонах выполните поиск **Сетки событий**, а затем выберите **Azure Event Grid trigger** (Триггер Сетки событий Azure). 

        :::image type="content" source="./media/custom-event-to-function/function-event-grid-trigger.png" alt-text="Выбор триггера Сетки событий":::
3. На странице **Новая функция** введите имя функции, а затем выберите **Создать функцию**.

    :::image type="content" source="./media/custom-event-to-function/new-function-page.png" alt-text="Выбор триггера Сетки событий":::
4. Используйте страницу **Код и тестирование**, чтобы просмотреть имеющийся код функции и обновить его. 

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]

## <a name="create-a-custom-topic"></a>Создание пользовательской темы

Раздел сетки событий содержит определяемую пользователем конечную точку, в которой можно размещать свои события. 

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите **Все службы** в меню навигации слева, найдите пункт **Сетка событий** и выберите **Разделы сетки событий**. 

    ![Выбор разделов Сетки событий](./media/custom-event-to-function/select-event-grid-topics.png)
3. На странице **Разделы сетки событий** на панели инструментов выберите **+ Добавить**. 

    ![Добавление кнопки "Раздел сетки событий"](./media/custom-event-to-function/add-event-grid-topic-button.png)

4. На странице **Создать раздел** выполните следующие действия:

    1. Укажите уникальное **имя** для пользовательского раздела. Имя раздела должно быть уникальным, так как оно представлено записью службы доменных имен (DNS). Не используйте имя, указанное на изображении. Вместо этого создайте собственное имя длиной от 3 до 50 символов, которое может содержать только буквы (a–z, A–Z), цифры (0–9) и дефисы (-).
    2. Выберите свою **подписку Azure**.
    3. Выберите группу ресурсов, использованную для предыдущих шагов.
    4. Выберите **расположение** для раздела Сетки событий.
    5. В поле **Схема событий** оставьте значение по умолчанию **Схема сетки событий**. 

       ![Страница "Создать раздел"](./media/custom-event-to-function/create-custom-topic.png)
    6. Нажмите кнопку **создания**. 

5. После создания пользовательского раздела появится уведомление об успехе. Выберите **Перейти к группе ресурсов**. 

   ![Уведомление об успехе](./media/custom-event-to-function/success-notification.png)

6. На странице **Группа ресурсов** выберите раздел сетки событий. 

   ![Выбор ресурса раздела сетки событий](./media/custom-event-to-function/select-event-grid-topic.png)

7. Отобразится страница **Раздел сетки событий** для соответствующей Сетки событий. Оставьте эту страницу открытой. Она понадобится позже при работе с кратким руководством. 

    ![Домашняя страница раздела Сетки событий](./media/custom-event-to-function/event-grid-topic-home-page.png)

## <a name="subscribe-to-custom-topic"></a>Подписка на события пользовательского раздела

Подпишитесь на соответствующий раздел, чтобы определить в Сетке событий Azure, какие события необходимо отслеживать и куда их отправлять.

1. Теперь на странице **Раздел сетки событий** выберите для пользовательского раздела **+ Подписка на события** на панели инструментов.

   ![Добавление подписки на события](./media/custom-event-to-function/new-event-subscription.png)

2. На странице **Создать подписку на событие** выполните следующие действия:
    1. Укажите **имя** подписки на события.
    3. Выберите **Функции Azure** в качестве **типа конечной точки**. 
    4. Выберите **Выбрать конечную точку**. 

       ![Указание значений подписки на события](./media/custom-event-to-function/provide-subscription-values.png)

    5. В качестве конечной точки функции выберите подписку Azure и группу ресурсов Azure, в которой находится ваше приложение-функция, а затем выберите приложение-функцию и функцию, созданную ранее. Выберите **Подтвердить выбор**.

       ![Указание URL-адреса конечной точки](./media/custom-event-to-function/provide-endpoint.png)
    6. Этот шаг не является обязательным, но рекомендуется для рабочих сценариев. На странице **Создать подписку на события** перейдите на вкладку **Дополнительные параметры** и задайте значения для параметров **Максимальное число событий на пакет** и **Предпочтительный размер пакета в килобайтах**. 
    
        Пакетная обработка может обеспечить высокую пропускную способность. Для параметра **Максимальное число событий на пакет** задайте максимальное количество событий, которые подписка будет включать в пакет. Параметр предпочтительного размера пакета задает нужное верхнее ограничение размера пакета в килобайтах, но допускает превышение, если одно событие больше этого порогового значения.
    
        :::image type="content" source="./media/custom-event-to-function/enable-batching.png" alt-text="Выбор триггера Сетки событий":::
    6. Вернитесь на страницу **Создать подписку на события** и выберите **Создать**.

## <a name="send-an-event-to-your-topic"></a>Отправка события в тему

Теперь необходимо активировать событие, чтобы увидеть, как Сетка событий Azure распределяет сообщение к вашей конечной точке. Отправьте тестовое событие в пользовательский раздел с помощью PowerShell или Azure CLI. Как правило, приложение или служба Azure отправит данные события.

В первом примере используется Azure CLI. Он возвращает URL-адрес, ключ пользовательского раздела и пример данных события. Используйте имя пользовательского раздела для `<topic name>`. Будет создан пример данных событий. Элемент `data` JSON отображает полезные данные события. Любое значение JSON с правильным форматом может быть в этом поле. Кроме того, можно использовать поле темы для дополнительной маршрутизации и фильтрации. CURL — это служебная программа, которая отправляет HTTP-запросы.


### <a name="azure-cli"></a>Azure CLI
1. На портале Azure выберите **Cloud Shell**. Выберите **Bash** в левом верхнем углу окна Cloud Shell. 

    ![Cloud Shell — Bash](./media/custom-event-quickstart-portal/cloud-shell-bash.png)
1. Выполните следующую команду, чтобы получить **конечную точку** для раздела: После копирования и вставки команды перед ее выполнением измените соответствующим образом **имя раздела** и **имя группы ресурсов**. 

    ```azurecli
    endpoint=$(az eventgrid topic show --name <topic name> -g <resource group name> --query "endpoint" --output tsv)
    ```
2. Выполните следующую команду, чтобы получить **ключ** для пользовательского раздела. После копирования и вставки команды перед ее выполнением измените соответствующим образом **имя раздела** и **имя группы ресурсов**. 

    ```azurecli
    key=$(az eventgrid topic key list --name <topic name> -g <resource group name> --query "key1" --output tsv)
    ```
3. Скопируйте следующую инструкцию с определением события и нажмите клавишу **ВВОД**. 

    ```json
    event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/motorcycles", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "make": "Ducati", "model": "Monster"},"dataVersion": "1.0"} ]'
    ```
4. Выполните следующую команду **Curl**, чтобы отправить событие:

    ```
    curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
    ```

### <a name="azure-powershell"></a>Azure PowerShell
Во втором примере для выполнения аналогичных действий используется PowerShell.

1. На портале Azure выберите **Cloud Shell** (или перейдите по адресу `https://shell.azure.com/`). Выберите **PowerShell** в левом верхнем углу окна Cloud Shell. Пример того, как выглядит окно **Cloud Shell**, приведен в разделе "Инфраструктура CLI Azure".
2. Задайте следующие переменные. После копирования и вставки команды перед ее выполнением измените соответствующим образом **имя раздела** и **имя группы ресурсов**.

    ```powershell
    $resourceGroupName = <resource group name>
    $topicName = <topic name>
    ```
3. Выполните следующую команду, чтобы получить **конечную точку** и **ключи** для раздела:

    ```powershell
    $endpoint = (Get-AzEventGridTopic -ResourceGroupName $resourceGroupName -Name $topicName).Endpoint
    $keys = Get-AzEventGridTopicKey -ResourceGroupName $resourceGroupName -Name $topicName
    ```
4. Подготовьте событие. Скопируйте и выполните инструкции в окне Cloud Shell. 

    ```powershell
    $eventID = Get-Random 99999

    #Date format should be SortableDateTimePattern (ISO 8601)
    $eventDate = Get-Date -Format s

    #Construct body using Hashtable
    $htbody = @{
        id= $eventID
        eventType="recordInserted"
        subject="myapp/vehicles/motorcycles"
        eventTime= $eventDate   
        data= @{
            make="Ducati"
            model="Monster"
        }
        dataVersion="1.0"
    }
    
    #Use ConvertTo-Json to convert event body from Hashtable to JSON Object
    #Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
    $body = "["+(ConvertTo-Json $htbody)+"]"
    ```
5. Используйте командлет **Invoke-WebRequest** для отправки события. 

    ```powershell
    Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
    ```

### <a name="verify-in-the-event-grid-viewer"></a>Проверка в средстве просмотра Сетки событий
Вы активировали событие, а служба "Сетка событий" отправила сообщение в конечную точку, настроенную вами при оформлении подписки. Перейдите к функции для триггеров Сетки событий и откройте журналы. В них должна содержаться копия полезных данных события. Если найти их не удается, сначала откройте окно журналов или нажмите кнопку повторного подключения, а затем попробуйте снова отправить тестовое событие.

![Успешное выполнение журнала функций триггера](./media/custom-event-to-function/successful-function.png)

## <a name="clean-up-resources"></a>Очистка ресурсов
Если вы планируете продолжить работу с этим событием, не удаляйте ресурсы, созданные при работе с этой статьей. В противном случае удалите ресурсы, созданные при работе с этой статьей.

1. Выберите **Группы ресурсов** в меню слева. Если этот параметр не отображается, выберите **Все службы** в меню слева и выберите **Группы ресурсов**. 
2. Выберите группу ресурсов, чтобы открыть страницу **Группа ресурсов**. 
3. На панели инструментов выберите **Удалить группу ресурсов**. 
4. Подтвердите удаление. Для этого введите имя группы ресурсов и выберите **Удалить**. 

    ![Группы ресурсов](./media/custom-event-to-function/delete-resource-groups.png)

    Другая группа ресурсов, показанная на рисунке, создана и используется в окне Cloud Shell. Удалите ее, если вы не планируете использовать окно Cloud Shell в дальнейшем. 

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы знаете, как создавать темы и подписки на события, ознакомьтесь с дополнительными сведениями о сетке событий, которые могут помочь вам:

- [An introduction to Azure Event Grid](overview.md) (Общие сведения о службе "Сетка событий Azure")
- [Перенаправление событий хранилища BLOB-объектов в пользовательскую конечную веб-точку (предварительная версия)](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Отслеживание изменений виртуальной машины с помощью Azure Logic Apps и службы "Сетка событий Azure")
- [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md)
