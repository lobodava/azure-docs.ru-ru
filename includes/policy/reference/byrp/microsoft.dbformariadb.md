---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 11/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a68c3ff276c4bd1a9e33084f9745beac48f4b83d
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94992603"
---
|Имя<br /><sub>(портал Azure)</sub> |Описание |Действие |Версия<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[База данных Azure для MariaDB должна использовать геоизбыточное резервное копирование](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ec47710-77ff-4a3d-9181-6aa50af424d0) |База данных Azure для MariaDB позволяет выбрать вариант избыточности для сервера базы данных. Можно задать геоизбыточное хранилище резервных копий, в котором данные хранятся не только в том регионе, в котором размещен сервер, — они также реплицированы в парный регион для обеспечения восстановления в случае сбоя в регионе. Геоизбыточное хранилище резервных копий можно настроить только на этапе создания сервера. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMariaDB_Audit.json) |
|[Сервер MariaDB должен использовать конечную точку службы виртуальной сети.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdfbd9a64-6114-48de-a47d-90574dc2e489) |Правила брандмауэра на основе виртуальной сети используются для разрешения трафика к Базе данных Azure для MariaDB из определенной подсети, при этом трафик остается в пределах границ Azure. Эта политика позволяет выполнять аудит в случае, если в базе данных Azure для MariaDB используется конечная точка службы виртуальной сети. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_VirtualNetworkServiceEndpoint_Audit.json) |
|[Для серверов MariaDB необходимо включить частную конечную точку.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0a1302fb-a631-4106-9753-f3d494733990) |Подключения к частной конечной точке обеспечивают защищенный обмен данными, предоставляя возможность частного доступа к Базе данных Azure для MariaDB. Настройте подключение к частной конечной точке, чтобы разрешить доступ к трафику, поступающему только из известных сетей, и запретить доступ со всех других IP-адресов, включая адреса в Azure. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_EnablePrivateEndPoint_Audit.json) |
|[Для северов MariaDB должен быть отключен доступ через общедоступную сеть](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffdccbe47-f3e3-4213-ad5d-ea459b2fa077) |Отключение доступа через общедоступную сеть повышает безопасность, гарантируя, что доступ к Базе данных Azure для MariaDB будет возможен только из частной конечной точки. Эта конфигурация строго отключает доступ из любого общедоступного адресного пространства вне диапазона IP-адресов Azure и запрещает все имена входа, соответствующие правилам брандмауэра на основе IP или виртуальной сети. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_DisablePublicNetworkAccess_Audit.json) |
