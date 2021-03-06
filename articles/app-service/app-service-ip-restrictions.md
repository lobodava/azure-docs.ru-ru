---
title: Ограничения доступа для службы приложений Azure
description: Узнайте, как защитить приложение в службе приложений Azure, настроив ограничения доступа.
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: e1549dda367105db34272eab8a90c1760dd5bb5c
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576450"
---
# <a name="set-up-azure-app-service-access-restrictions"></a>Настройка ограничений доступа для службы приложений Azure

Настроив ограничения доступа, можно определить упорядоченный по приоритету список разрешений и запретов, который управляет сетевым доступом к приложению. Список может включать IP-адреса или подсети виртуальной сети Azure. Если имеется одна или несколько записей, в конце списка существует неявный *Запрет ALL* .

Возможность ограничения доступа работает со всеми рабочими нагрузками, размещенными в службе приложений Azure. Рабочие нагрузки могут включать веб-приложения, приложения API, приложения Linux, приложения-контейнеры Linux и функции.

Когда в приложение вносится запрос, адрес отдается по правилам IP-адресов в списке ограничений доступа. Если адрес отправителя находится в подсети, для которой настроены конечные точки службы Microsoft. Web, исходная подсеть сравнивается с правилами виртуальной сети в списке ограничений доступа. Если адрес не разрешен на основе правил в списке, служба отвечает с кодом состояния [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) .

Возможность ограничения доступа реализована в интерфейсных ролях службы приложений, которые являются вышестоящим потоком рабочих узлов, на которых выполняется код. Следовательно, ограничения доступа — это фактические списки управления доступом к сети (ACL).

Возможность ограничения доступа к веб-приложению из виртуальной сети Azure включается [конечными точками службы][serviceendpoints]. С конечными точками службы можно ограничить доступ к многоклиентской службе из выбранных подсетей. Это не позволяет ограничить трафик для приложений, размещенных в Среда службы приложений. Если вы находитесь в Среда службы приложений, вы можете управлять доступом к приложению, применяя правила IP-адресов.

> [!NOTE]
> Конечные точки службы должны быть включены как на стороне сети, так и для службы Azure, с которой они включены. Список служб Azure, поддерживающих конечные точки служб, см. в статье [конечные точки службы виртуальной сети](../virtual-network/virtual-network-service-endpoints-overview.md).
>

![Схема потока ограничений доступа.](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="add-or-edit-access-restriction-rules-in-the-portal"></a>Добавление или изменение правил ограничения доступа на портале

Чтобы добавить правило ограничения доступа в приложение, выполните следующие действия.

1. Войдите на портал Azure.

1. На левой панели выберите **сети**.

1. В области **Сетевые подключения** в разделе **ограничения доступа** выберите **Настройка ограничений доступа**.

   ![Снимок экрана: область параметров сети службы приложений в портал Azure.](media/app-service-ip-restrictions/access-restrictions.png)  

1. На странице **ограничения доступа** проверьте список правил ограничения доступа, определенных для приложения.

   ![Снимок экрана: страница "ограничения доступа" в портал Azure, показывающая список правил ограничения доступа, определенных для выбранного приложения.](media/app-service-ip-restrictions/access-restrictions-browse.png)

   В списке отображаются все текущие ограничения, примененные к приложению. Если у вас есть ограничение виртуальной сети для приложения, в таблице показано, включены ли конечные точки службы для Microsoft. Web. Если для приложения не определены ограничения, приложение будет доступно из любого места.  

### <a name="add-an-access-restriction-rule"></a>Добавление правила ограничения доступа

Чтобы добавить правило ограничения доступа в приложение, на панели **ограничения доступа** выберите **Добавить правило**. После добавления правила оно вступает в силу немедленно. 

Правила применяются в порядке приоритета, начиная с наименьшего числа в столбце **приоритет** . Неявный *Запрет ALL* действует после добавления даже одного правила.

На панели **Добавление ограничения IP-адресов** при создании правила выполните следующие действия.

1. В разделе **действие** выберите значение **Разрешить** или **запретить**.  

   ![Снимок экрана: панель "Добавление ограничения IP-адресов".](media/app-service-ip-restrictions/access-restrictions-ip-add.png)
   
1. При необходимости введите имя и описание правила.  
1. В раскрывающемся списке **тип** выберите тип правила.  
1. В поле **Priority (приоритет** ) введите значение приоритета.  
1. В раскрывающихся списках **Подписка** , **Виртуальная сеть** и **подсеть** выберите, к чему нужно ограничить доступ.  

### <a name="set-an-ip-address-based-rule"></a>Задание правила на основе IP-адреса

Выполните процедуру, описанную в предыдущем разделе, но с указанным ниже вариантом.
* Для шага 3 в раскрывающемся списке **тип** выберите **IPv4** или **IPv6**. 

Укажите IP-адрес в нотации Inter-Domain маршрутизации (CIDR) для адресов IPv4 и IPv6. Чтобы указать адрес, можно использовать нечто вроде *1.2.3.4/32* , где первые четыре октета представляют IP-адрес, а */32* — маску. Нотацией CIDR IPv4 для всех адресов является 0.0.0.0/0. Дополнительные сведения об нотации CIDR см. в статье [маршрутизация Inter-Domain](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

## <a name="use-service-endpoints"></a>Использование конечных точек служб

С помощью конечных точек службы можно ограничить доступ к выбранным подсетям виртуальной сети Azure. Чтобы ограничить доступ к определенной подсети, создайте правило ограничения с типом **виртуальной сети** . Затем можно выбрать подписку, виртуальную сеть и подсеть, к которым вы хотите разрешить или запретить доступ. 

Если конечные точки службы не включены в Microsoft. Web для выбранной подсети, они будут включены автоматически, если не установлен флажок **игнорировать отсутствующие конечные точки Microsoft. Web Service** . Сценарий, в котором может потребоваться включить конечные точки службы в приложении, но не подсеть, зависит главным образом от наличия разрешений на их включение в подсети. 

Если требуется, чтобы другие пользователи включили конечные точки службы в подсети, установите флажок **игнорировать отсутствующие конечные точки Microsoft. Web Service** . Приложение будет настроено для конечных точек службы, чтобы они были включены позже в подсети. 

![Снимок экрана: панель "Добавление ограничения IP-адресов" с выбранным типом виртуальной сети.](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Конечные точки службы нельзя использовать для ограничения доступа к приложениям, выполняемым в Среда службы приложений. Когда приложение находится в Среда службы приложений, вы можете управлять доступом к нему, применяя правила IP-доступа. 

С конечными точками службы можно настроить приложения с помощью шлюзов приложений или других устройств с брандмауэром веб-приложения (WAF). Кроме того, можно настроить многоуровневые приложения с защищенными серверными интерфейсами. Дополнительные сведения см. в разделе [Сетевые компоненты, служба приложений](networking-features.md) и [Интеграция шлюза приложений с конечными точками службы](networking/app-gateway-with-service-endpoints.md).

> [!NOTE]
> - Конечные точки службы сейчас не поддерживаются для веб-приложений, использующих виртуальный IP-адрес IP-SSL (SSL).
> - Существует ограничение в 512 строк с ограничениями IP-адресов или конечных точек службы. Если вам требуется более 512 строк ограничений, рекомендуем установить автономный продукт безопасности, например, переднюю дверцу Azure, шлюз приложений Azure или WAF.
>

## <a name="manage-access-restriction-rules"></a>Управление правилами ограничения доступа

Существующее правило ограничения доступа можно изменить или удалить.

### <a name="edit-a-rule"></a>Изменение правила

1. Чтобы начать редактирование существующего правила ограничения доступа, на странице **ограничения доступа** дважды щелкните правило, которое необходимо изменить.

1. На панели **изменить ограничение IP-адресов** внесите изменения, а затем выберите **Обновить правило**. Изменения вступают в силу немедленно, включая изменения в порядке приоритета.

   ![Снимок экрана панели "изменить ограничение IP-адресов" в портал Azure с отображением полей для существующего правила ограничения доступа.](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

   > [!NOTE]
   > При изменении правила нельзя переключаться между правилом IP-адреса и правилом виртуальной сети. 

   ![Снимок экрана: панель "изменение ограничений IP-адресов" в портал Azure, отображающая параметры для правила виртуальной сети.](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

### <a name="delete-a-rule"></a>Удаление правила

Чтобы удалить правило, на странице **ограничения доступа** нажмите кнопку с многоточием ( **...** ) рядом с правилом, которое необходимо удалить, а затем выберите **Удалить**.

![Снимок экрана со страницей "ограничения доступа", на которой отображается многоточие "Удалить" рядом с правилом ограничения доступа, которое необходимо удалить.](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="block-a-single-ip-address"></a>Блокировать один IP-адрес

При добавлении первого правила ограничения IP-адресов служба добавляет явное правило *Deny All* с приоритетом 2147483647. На практике явное правило *Deny All* является конечным правилом, которое должно быть выполнено, и блокирует доступ к любому IP-адресу, который явно не разрешен правилом *allow* .

В случае, когда необходимо явно заблокировать один IP-адрес или блок IP-адресов, но разрешить доступ всем остальным, добавьте явное правило *Разрешить все* .

![Снимок экрана: страница "ограничения доступа" в портал Azure, в которой отображается один заблокированный IP-адрес.](media/app-service-ip-restrictions/block-single-address.png)

## <a name="restrict-access-to-an-scm-site"></a>Ограничение доступа к сайту SCM 

Помимо возможности управления доступом к приложению, можно ограничить доступ к сайту SCM, используемому вашим приложением. Сайт SCM — это конечная точка веб-развертывания и консоль KUDU. Можно назначить ограничения доступа к сайту SCM отдельно от приложения или использовать одинаковый набор ограничений как для приложения, так и для сайта SCM. При выборе тех **же ограничений, что \<app name>** и флажок, все остается пустым. Если снять этот флажок, параметры сайта SCM будут применены повторно. 

![Снимок экрана со страницей "ограничения доступа" в портал Azure, показывающий, что для сайта SCM или приложения не заданы ограничения доступа.](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="manage-access-restriction-rules-programatically"></a>Программное управление правилами ограничения доступа

Вы можете добавить ограничения доступа программно, выполнив одно из следующих действий. 

* Используйте [Azure CLI](/cli/azure/webapp/config/access-restriction?view=azure-cli-latest&preserve-view=true). Пример:
   
  ```azurecli-interactive
  az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
  --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
  ```

* Используйте [Azure PowerShell](/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule?view=azps-3.1.0&preserve-view=true). Пример:


  ```azurepowershell-interactive
  Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
      -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
  ```

Значения можно также задать вручную, выполнив одно из следующих действий.

* Используйте операцию [REST API](/rest/api/azure/) постановки Azure в конфигурации приложения в Azure Resource Manager. Расположение этих сведений в Azure Resource Manager:

  management.azure.com/subscriptions/ **ИД_подписки** /resourceGroups/ **группы_ресурсов** /providers/Microsoft.Web/sites/ **имя_веб-приложения** /config/web?api-version=2018-02-01

* Используйте шаблон ARM. Например, можно использовать resources.azure.com и изменить блок ipSecurityRestrictions, чтобы добавить необходимый код JSON.

  Ниже показан синтаксис JSON для приведенного выше примера.

  ```json
  {
    "properties": {
      "ipSecurityRestrictions": [
        {
          "ipAddress": "122.133.144.0/24",
          "action": "Allow",
          "priority": 100,
          "name": "IP example rule"
        }
      ]
    }
  }
  ```

## <a name="set-up-azure-functions-access-restrictions"></a>Настройка ограничений доступа для функций Azure

Ограничения доступа также доступны для приложений функций с теми же функциями, что и планы службы приложений. При включении ограничений доступа Вы также отключаете портал Azure редактор кода для любых недопустимых IP-адресов.

## <a name="next-steps"></a>Дальнейшие действия
[Ограничения доступа для функций Azure](../azure-functions/functions-networking-options.md#inbound-access-restrictions)  
[Интеграция шлюза приложений с конечными точками службы](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
