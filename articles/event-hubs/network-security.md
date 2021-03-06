---
title: Сетевая безопасность для концентраторов событий Azure
description: В этой статье описывается настройка доступа из частных конечных точек
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: 9503fc26c22d7dbff13c5754288f577b7bb3242f
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331317"
---
# <a name="network-security-for-azure-event-hubs"></a>Сетевая безопасность для концентраторов событий Azure 
В этой статье описывается, как использовать следующие функции безопасности для концентраторов событий Azure. 

- Теги служб
- Правила брандмауэра IP-адресов
- Конечные точки сетевой службы
- Частные конечные точки


## <a name="service-tags"></a>Теги служб
Тег службы представляет группу префиксов IP-адресов из определенной службы Azure. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов, сводя к минимуму сложность частых обновлений правил сетевой безопасности. Дополнительные сведения о тегах служб см. в статье [Общие сведения о тегах служб](../virtual-network/service-tags-overview.md).

Теги службы можно использовать для определения элементов управления доступом к сети для [групп безопасности сети](../virtual-network/network-security-groups-overview.md#security-rules) или [брандмауэра Azure](../firewall/service-tags.md). Теги служб можно использовать вместо определенных IP-адресов при создании правил безопасности. Указав имя тега службы (например, **EventHub**) в соответствующем поле *источника* или *назначения* правила, можно разрешить или запретить трафик для соответствующей службы.

| Тег службы | Назначение | Может ли использовать входящий или исходящий трафик? | Может быть региональным? | Можно ли использовать с Брандмауэром Azure? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **EventHub** | . | Исходящие | Да | Да |


## <a name="ip-firewall"></a>Брандмауэр IP-адресов 
По умолчанию пространства имен Центров событий доступны из Интернета при условии, что запрос поступает с действительной аутентификацией и авторизацией. С помощью IP-брандмауэра такой доступ можно дополнительно ограничить набором или диапазоном IPv4-адресов, введя их в нотации [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Эта возможность полезна в сценариях, в которых Центры событий Azure должны быть доступны только из определенных хорошо известных сайтов. Правила брандмауэра позволяют настроить правила приема трафика, поступающего с определенных IPv4-адресов. Например, при использовании Центров событий с помощью [Azure ExpressRoute](../expressroute/expressroute-faqs.md#supported-services) можно создать **правило брандмауэра**, разрешающее трафик только с IP-адресов в локальной инфраструктуре. 

Правила брандмауэра для IP-адресов применяются на уровне пространства имен Центров событий. Поэтому они действуют для всех клиентских подключений по любым поддерживаемым протоколам. Любые попытки подключения с IP-адреса, который не соответствует правилу разрешения IP-адресов для пространства имен Центров событий, отклоняются. В ответе клиенту правило фильтрации IP-адресов не упоминается. Правила фильтрации IP-адресов применяются по порядку, поэтому первое правило, которое соответствует IP-адресу, определяет действие (принять или отклонить).

Дополнительные сведения см. [в статье Настройка брандмауэра IP для концентратора событий](event-hubs-ip-filtering.md) .

## <a name="network-service-endpoints"></a>Конечные точки сетевой службы
Интеграция Центров событий с [конечными точками службы виртуальной сети](../virtual-network/virtual-network-service-endpoints-overview.md) обеспечивает безопасный доступ к возможностям обмена сообщениями из рабочих нагрузок, таких как виртуальные машины, привязанные к виртуальным сетям, где трафик защищен с обеих сторон.

После настройки привязки хотя бы к одной конечной точке службы подсети виртуальной сети соответствующее пространство имен концентраторов событий больше не будет принимать трафик из любого места, но с полномочными подсетями в виртуальных сетях. С точки зрения виртуальной сети привязка пространства имен Центров событий к конечной точке службы настраивает изолированный сетевой туннель от подсети виртуальной сети к службе обмена сообщениями. 

Результатом является частная и изолированная взаимосвязь между рабочими нагрузками, привязанными к подсети, и соответствующим пространством имен Центров событий, несмотря на то что наблюдаемый сетевой адрес конечной точки службы обмена сообщениями находится в общедоступном диапазоне IP-адресов. Исключение этого поведения. Включение конечной точки службы по умолчанию включает `denyall` правило в [IP-брандмауэре](event-hubs-ip-filtering.md) , связанном с виртуальной сетью. Вы можете добавить определенные IP-адреса в брандмауэре IP-адресов, чтобы разрешить доступ к общедоступной конечной точке концентратора событий. 

> [!IMPORTANT]
> Виртуальные сети поддерживаются в **стандартном** и **выделенном** уровнях Центра событий. Он не поддерживается на уровне **Basic** .

### <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Расширенные сценарии обеспечения безопасности, доступные при интеграции с виртуальной сетью 

Решения, требующие тесной и подразделяетсяй безопасности, а также, где подсети виртуальной сети обеспечивают сегментацию между службами подразделяется, по-прежнему нуждаются в путях обмена данными между службами, находящимися в этих секциях.

Любой прямой IP-маршрут между секциями, включая передачу трафика HTTPS через TCP/IP, несет риск использования уязвимостей на сетевом уровне. Службы обмена сообщениями предоставляют изолированные пути взаимодействия, где сообщения даже записываются на диск при переходе между сторонами. Рабочие нагрузки в двух разных виртуальных сетях, которые связаны с одним и тем же экземпляром Центров событий, могут эффективно и надежно связываться через сообщения, в то время как целостность границ изолированной сети сохраняется.
 
Это означает, что ваши конфиденциальные облачные решения, влияющие на безопасность, не только получают доступ к ведущим в отрасли надежным и масштабируемым асинхронным службам обмена сообщениями Azure, но теперь они могут использовать обмен сообщениями для создания путей передачи данных между защищенными секциями решений, которые по своей сути более безопасны, чем степень безопасности в любом режиме одноранговой связи, включая HTTPS и другие протоколы сокетов TLS.

### <a name="bind-event-hubs-to-virtual-networks"></a>Привязка концентраторов событий к виртуальным сетям

**Правила виртуальной сети** — это функция безопасности брандмауэра, которая контролирует, принимает ли пространство имен Центров событий Azure соединения из определенной подсети виртуальной сети.

Привязка пространства имен Центров событий к виртуальной сети состоит из двух этапов. Сначала необходимо создать **конечную точку службы виртуальной сети** в подсети виртуальной сети и включить ее для **Microsoft. EventHub** , как описано в статье [Обзор конечной точки службы](../virtual-network/virtual-network-service-endpoints-overview.md) . После добавления конечной точки службы вы привязываете пространство имен Центров событий к ней с помощью **правила виртуальной сети**.

Правило виртуальной сети — это связь пространства имен Центров событий с подсетью виртуальной сети. Пока правило существует, всем рабочим нагрузкам, привязанным к подсети, предоставляется доступ к пространству имен Центров событий. Концентраторы событий никогда не устанавливают исходящие подключения, поэтому им не нужно получать доступ и поэтому никогда не предоставляется доступ к подсети, включая это правило.

Дополнительные сведения см. [в статье Настройка конечных точек службы виртуальной сети для концентратора событий](event-hubs-service-endpoints.md) .

## <a name="private-endpoints"></a>Частные конечные точки

[Служба "Частная связь Azure](../private-link/private-link-overview.md) " позволяет получать доступ к службам Azure (например, концентраторам событий Azure, службе хранилища azure и Azure Cosmos DB) и службам клиентов и партнеров Azure, размещенным в **частной конечной точке** в виртуальной сети.

Частная конечная точка — это сетевой интерфейс, который защищенно и надежно подключается к службе через Приватный канал Azure. Частная конечная точка использует частный IP-адрес из виртуальной сети, по сути перемещая службу в виртуальную сеть. Весь трафик к службе может маршрутизироваться через частную конечную точку, поэтому шлюзы, устройства преобразования сетевых адресов (NAT), подключения ExpressRoute и VPN, а также общедоступные IP-адреса не требуются. Трафик между виртуальной сетью и службой проходит через магистральную сеть Майкрософт, что позволяет избежать рисков общедоступного Интернета. Вы можете подключиться к экземпляру ресурса Azure, обеспечивая наивысшую степень детализации в управлении доступом.

> [!IMPORTANT]
> Эта функция поддерживается как для **стандартных** , так и для **выделенных** уровней. Он не поддерживается на уровне **Basic** .

Дополнительные сведения см. [в статье Настройка частных конечных точек для концентратора событий](private-link-service.md) .


## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи:

- [Настройка брандмауэра IP для концентратора событий](event-hubs-ip-filtering.md)
- [Настройка конечных точек службы виртуальной сети для концентратора событий](event-hubs-service-endpoints.md)
- [Настройка частных конечных точек для концентратора событий](private-link-service.md)