---
title: Настройка прослушивателя DNN для группы доступности
description: Узнайте, как настроить прослушиватель имени распределенной сети (DNN), чтобы заменить прослушиватель имени виртуальной сети (VNN) и направить трафик в группу доступности Always On в SQL Server на виртуальной машине Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/07/2020
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: abfcd6a13bc5e8ad262fe47111eb680ad00a34df
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2020
ms.locfileid: "92168979"
---
# <a name="configure-a-dnn-listener-for-an-availability-group"></a>Настройка прослушивателя DNN для группы доступности
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

С SQL Server на виртуальных машинах Azure имя распределенной сети (DNN) направляет трафик в соответствующий кластеризованный ресурс. Он предоставляет более простой способ подключения к Always On группе доступности (AG), чем прослушиватель имени виртуальной сети (VNN) без необходимости в Azure Load Balancer. 

В этой статье описывается, как настроить прослушиватель DNN, чтобы заменить прослушиватель VNN и направить трафик в группу доступности с SQL Server на виртуальных машинах Azure для обеспечения высокой доступности и аварийного восстановления (HADR). 

Функция прослушивателя DNN в настоящее время доступна только начиная с SQL Server 2019 CU8 на Windows Server 2016 и более поздних версиях. 

Для альтернативного варианта подключения рассмотрим [VNN прослушиватель и Azure Load Balancer](availability-group-vnn-azure-load-balancer-configure.md) . 

## <a name="overview"></a>Обзор

Прослушиватель распределенного сетевого имени (DNN) заменяет традиционный прослушиватель группы доступности имени виртуальной сети (VNN) при использовании с [группы доступности Always on на виртуальных машинах SQL Server](availability-group-overview.md). Это отрицательно потребует Azure Load Balancer для маршрутизации трафика, упрощения развертывания, обслуживания и повышения отработки отказа. 

Используйте прослушиватель DNN, чтобы заменить существующий прослушиватель VNN или использовать его в сочетании с существующим прослушивателем VNN, чтобы у группы доступности было две разные точки подключения: одна использует имя прослушивателя VNN (и порт, если не по умолчанию), а другой — с именем прослушивателя DNN и портом. 

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить действия, описанные в этой статье, необходимо следующее:

- SQL Server 2019 в CU8 или более поздней версии, на Windows Server 2016 и более поздних версиях
- Принято решение о том, что имя распределенной сети является подходящим [вариантом подключения для вашего решения HADR](hadr-cluster-best-practices.md#connectivity).
- Настроена [Группа доступности Always on](availability-group-overview.md). 
- Установлена последняя версия [PowerShell](/powershell/azure/install-az-ps). 


## <a name="create-script"></a>Создать скрипт

С помощью PowerShell создайте ресурс с именем распределенной сети (DNN) и свяжите его с группой доступности. 

Для этого выполните следующие действия. 

1. Откройте текстовый редактор, например Блокнот. 
1. Скопируйте и вставьте следующий скрипт. 

   ```powershell
   param (
      [Parameter(Mandatory=$true)][string]$Ag,
      [Parameter(Mandatory=$true)][string]$Dns,
      [Parameter(Mandatory=$true)][string]$Port
   )
   
   Write-Host "Add a DNN listener for availability group $Ag with DNS name $Dns and port $Port"
   
   $ErrorActionPreference = "Stop"
   
   # create the DNN resource with the port as the resource name
   Add-ClusterResource -Name $Port -ResourceType "Distributed Network Name" -Group $Ag 
   
   # set the DNS name of the DNN resource
   Get-ClusterResource -Name $Port | Set-ClusterParameter -Name DnsName -Value $Dns 
   
   # start the DNN resource
   Start-ClusterResource -Name $Port
   
   
   $Dep = Get-ClusterResourceDependency -Resource $Ag
   if ( $Dep.DependencyExpression -match '\s*\((.*)\)\s*' )
   {
   $DepStr = "$($Matches.1) or [$Port]"
   }
   else
   {
   $DepStr = "[$Port]"
   }
   
   Write-Host "$DepStr"
   
   # add the Dependency from availability group resource to the DNN resource
   Set-ClusterResourceDependency -Resource $Ag -Dependency "$DepStr"
   
   
   #bounce the AG resource
   Stop-ClusterResource -Name $Ag
   Start-ClusterResource -Name $Ag
   ```

1. Сохраните скрипт в виде `.ps1` файла, например `add_dnn_listener.ps1` . 


## <a name="execute-script"></a>Выполнить сценарий

Чтобы создать прослушиватель DNN, выполните скрипт, передав параметры для имени группы доступности, имени прослушивателя и порта. 

Например, если имя группы доступности `ag1` , имя прослушивателя `dnnlsnr` и порт прослушивателя в качестве `6789` , выполните следующие действия. 

1. Откройте средство интерфейса командной строки, например Командная строка или PowerShell. 
1. Перейдите к месту сохранения `.ps1` скрипта, например к:\документс. 
1. Выполните скрипт: ```add_dnn_listener.ps1 <ag name> <listener-name> <listener port>``` . Пример: 

   ```console
   c:\Documents> add_dnn_listener.ps1 ag1 dnnlsnr 6789
   ```

## <a name="verify-listener"></a>Проверка прослушивателя

Чтобы убедиться, что прослушиватель DNN успешно создан, используйте либо SQL Server Management Studio, либо Transact-SQL. 

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

Разверните узел **прослушиватели группы доступности** в [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) , чтобы просмотреть прослушиватель DNN: 

:::image type="content" source="media/availability-group-distributed-network-name-dnn-listener-configure/dnn-listener-in-ssms.png" alt-text="Просмотр прослушивателя DNN в разделе Прослушиватели группы доступности в SQL Server Management Studio (SSMS)":::

### <a name="transact-sql"></a>Transact-SQL

Чтобы просмотреть состояние прослушивателя DNN, используйте Transact-SQL: 

```sql
SELECT * FROM SYS.AVAILABILITY_GROUP_LISTENERS
```

Значение `1` для `is_distributed_network_name` указывает, что прослушиватель является прослушивателем имени распределенной сети (DNN): 

:::image type="content" source="media/availability-group-distributed-network-name-dnn-listener-configure/dnn-listener-tsql.png" alt-text="Просмотр прослушивателя DNN в разделе Прослушиватели группы доступности в SQL Server Management Studio (SSMS)":::


## <a name="update-connection-string"></a>Обновление строки подключения

Обновите строки подключения для приложений, чтобы они подключались к прослушивателю DNN. Чтобы обеспечить быстрое подключение при отработке отказа, добавьте `MultiSubnetFailover=True` в строку подключения, если клиент SQL его поддерживает. 

## <a name="test-failover"></a>Тестовая отработка отказа

Выполните тестовую отработку отказа группы доступности, чтобы обеспечить функционирование. 

Чтобы протестировать отработку отказа, выполните следующие действия. 

1. Подключитесь к прослушивателю DNN или к одной из реплик с помощью [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms). 
1. Разверните **Always on группа доступности** в **обозревателе объектов**. 
1. Щелкните правой кнопкой мыши группу доступности и выберите пункт **отработка отказа** , чтобы открыть **Мастер отработки отказа**. 
1. Следуйте инструкциям на экране, чтобы выбрать цель отработки отказа и выполнить сбой группы доступности во вторичной реплике. 
1. Убедитесь, что база данных находится в состоянии SYNCHRONIZED в новой первичной реплике. 
1. Используемых Восстановить подключение к исходной основной или другой вторичной реплике. 

## <a name="test-connectivity"></a>Проверка подключения

Проверьте подключение к прослушивателю DNN, выполнив следующие действия.

1. Откройте [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms).
1. Подключитесь к прослушивателю DNN. 
1. Откройте новое окно запроса и проверьте, к какой реплике вы подключены, выполнив `SELECT @@SERVERNAME` . 
1. Сбой группы доступности в другой реплике.
1. По истечении разумного времени выполните команду, `SELECT @@SERVERNAME` чтобы подтвердить, что ваша группа доступности теперь размещена в другой реплике. 


## <a name="limitations"></a>Ограничения

- В настоящее время прослушиватель DNN для группы доступности поддерживается только для SQL Server 2019 CU8 и более поздних версий на Windows Server 2016 и более поздних версиях. 
- При работе с другими SQL Serverными функциями и группой доступности могут возникнуть дополнительные соображения, если вы используете DNN. Дополнительные сведения см. в статье группа [доступности с DNN взаимодействием](availability-group-dnn-interoperability.md). 

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о SQL Serverии функций HADR в Azure см. в разделе [группы доступности](availability-group-overview.md) и [экземпляр отказоустойчивого кластера](failover-cluster-instance-overview.md). Вы также [можете ознакомиться с рекомендациями](hadr-cluster-best-practices.md) по настройке среды для обеспечения высокого уровня доступности и аварийного восстановления. 


