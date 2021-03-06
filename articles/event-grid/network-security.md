---
title: Сетевая безопасность для ресурсов службы "Сетка событий Azure"
description: В этой статье описывается, как использовать теги службы для исходящего трафика, правила IP-брандмауэра для входящих и частных конечных точек в службе "Сетка событий Azure".
author: VidyaKukke
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: vkukke
ms.openlocfilehash: 10c9b165041f0a4a1f09511f17bef3629353c3b2
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2020
ms.locfileid: "94917534"
---
# <a name="network-security-for-azure-event-grid-resources"></a>Сетевая безопасность для ресурсов службы "Сетка событий Azure"
В этой статье описывается, как использовать следующие функции безопасности в службе "Сетка событий Azure": 

- Теги службы для исходящего трафика
- Правила брандмауэра IP-адресов для входящих данных
- Частные конечные точки для входящих данных


## <a name="service-tags"></a>Теги служб
Тег службы представляет группу префиксов IP-адресов из определенной службы Azure. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов, сводя к минимуму сложность частых обновлений правил сетевой безопасности. Дополнительные сведения о тегах служб см. в статье [Общие сведения о тегах служб](../virtual-network/service-tags-overview.md).

Теги службы можно использовать для определения элементов управления доступом к сети для [групп безопасности сети](../virtual-network/network-security-groups-overview.md#security-rules) или [брандмауэра Azure](../firewall/service-tags.md). Теги служб можно использовать вместо определенных IP-адресов при создании правил безопасности. Указав имя тега службы (например, **азуривентгрид**) в соответствующем поле *источника* или *назначения* правила, можно разрешить или запретить трафик для соответствующей службы.

| Тег службы | Назначение | Может ли использовать входящий или исходящий трафик? | Может быть региональным? | Можно ли использовать с Брандмауэром Azure? |
| --- | -------- |:---:|:---:|:---:|
| AzureEventGrid | Сетка событий Azure. | both | Нет | Нет |


## <a name="ip-firewall"></a>Брандмауэр IP-адресов 
Служба "Сетка событий Azure" поддерживает управление доступом на основе IP-адресов для публикации разделов и доменов. Элементы управления на основе IP-адресов позволяют ограничить издателей раздел или домен только набором утвержденных компьютеров и облачных служб. Эта функция дополняет [механизмы проверки подлинности](security-authentication.md) , поддерживаемые службой "Сетка событий".

По умолчанию раздел и домен доступны через Интернет, если запрос сопровождается действительной проверкой подлинности и авторизацией. С помощью IP-брандмауэра можно ограничить его применение только к набору IP-адресов или диапазонов IP-адресов в нотации [CIDR (без класса Inter-Domain маршрутизации)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) . Издатели, исходящие из любого другого IP-адреса, будут отклонены и получат ответ 403 (запрещено).

Пошаговые инструкции по настройке брандмауэра IP для разделов и доменов см. в разделе [Настройка брандмауэра IP](configure-firewall.md).

## <a name="private-endpoints"></a>Частные конечные точки
[Частные конечные точки](../private-link/private-endpoint-overview.md) можно использовать, чтобы разрешить прием событий непосредственно из виртуальной сети в разделы и домены, защищенные по [частной ссылке](../private-link/private-link-overview.md) , без использования общедоступного Интернета. Частная конечная точка — это специальный сетевой интерфейс для службы Azure в виртуальной сети. При создании частной конечной точки для раздела или домена обеспечивается безопасное подключение между клиентами в виртуальной сети и ресурсом сетки событий. Частной конечной точке назначается IP-адрес из диапазона IP-адресов виртуальной сети. Соединение между частной конечной точкой и службой сетки событий использует защищенную закрытую ссылку.

![Диаграмма архитектуры](./media/network-security/architecture-diagram.png)

Использование частных конечных точек для ресурса сетки событий позволяет:

- Обеспечьте безопасный доступ к разделу или домену из виртуальной сети через магистральную сеть Майкрософт, а не через общедоступный Интернет.
- Безопасное подключение из локальных сетей, которые подключаются к виртуальной сети с помощью VPN или Express-маршрутов с частными соединениями.

При создании частной конечной точки для раздела или домена в виртуальной сети запрос согласия отправляется на утверждение владельцу ресурса. Если пользователь, запрашивающий создание закрытой конечной точки, также является владельцем ресурса, этот запрос на согласие автоматически утверждается. В противном случае соединение находится в состоянии **ожидания** , пока не утверждено. Приложения в виртуальной сети могут легко подключаться к службе "Сетка событий" через частную конечную точку, используя те же строки подключения и механизмы авторизации, которые в противном случае будут использоваться. Владельцы ресурсов могут управлять запросами на согласие и закрытыми конечными точками через вкладку **частные конечные точки** для ресурса в портал Azure.

### <a name="connect-to-private-endpoints"></a>Подключение к частным конечным точкам
Издатели в виртуальной сети с использованием частной конечной точки должны использовать одну и ту же строку подключения для раздела или домена, когда клиенты подключаются к общедоступной конечной точке. Разрешение DNS автоматически направляет подключения из виртуальной сети в раздел или домен по частному каналу. Служба "Сетка событий" создает [закрытую зону DNS](../dns/private-dns-overview.md) , подключенную к виртуальной сети, с необходимым обновлением для частных конечных точек по умолчанию. Однако если вы используете собственный DNS-сервер, может потребоваться внести дополнительные изменения в конфигурацию DNS.

### <a name="dns-changes-for-private-endpoints"></a>Изменения DNS для частных конечных точек
При создании частной конечной точки запись DNS CNAME для ресурса обновляется до псевдонима в поддомене с префиксом `privatelink` . По умолчанию создается частная зона DNS, соответствующая поддомену частной ссылки. 

При разрешении раздела или URL-адреса конечной точки домена за пределами виртуальной сети с частной конечной точкой она разрешается в общедоступную конечную точку службы. Записи ресурсов DNS для "topic" при разрешении из- **за пределов виртуальной сети** , в которой размещается частная конечная точка, будут:

| Имя                                          | Type      | Значение                                         |
| --------------------------------------------- | ----------| --------------------------------------------- |  
| `topicA.westus.eventgrid.azure.net`             | CNAME     | `topicA.westus.privatelink.eventgrid.azure.net` |
| `topicA.westus.privatelink.eventgrid.azure.net` | CNAME     | \<Azure traffic manager profile\>

Вы можете запретить или управлять доступом клиента за пределами виртуальной сети через общедоступную конечную точку с помощью [брандмауэра IP](#ip-firewall). 

При разрешении из виртуальной сети, в которой размещается частная конечная точка, URL-адрес раздела или конечной точки домена разрешается в IP-адрес частной конечной точки. Записи ресурсов DNS для раздела «Topic», разрешаемые из **виртуальной сети** , в которой размещается частная конечная точка, будут:

| Имя                                          | Type      | Значение                                         |
| --------------------------------------------- | ----------| --------------------------------------------- |  
| `topicA.westus.eventgrid.azure.net`             | CNAME     | `topicA.westus.privatelink.eventgrid.azure.net` |
| `topicA.westus.privatelink.eventgrid.azure.net` | А         | 10.0.0.5

Такой подход обеспечивает доступ к разделу или домену, используя одну и ту же строку подключения для клиентов в виртуальной сети, где размещаются частные конечные точки, и клиентов за пределами виртуальной сети.

Если вы используете пользовательский DNS-сервер в сети, клиенты могут разрешить полное доменное имя раздела или конечной точки домена на IP-адрес частной конечной точки. Настройте DNS-сервер для делегирования поддомена частной ссылки в частную зону DNS для виртуальной сети или настройте записи A для `topicOrDomainName.regionName.privatelink.eventgrid.azure.net` с IP-адресом частной конечной точки.

Рекомендуемое имя зоны DNS — `privatelink.eventgrid.azure.net` .

### <a name="private-endpoints-and-publishing"></a>Частные конечные точки и публикация

В следующей таблице описаны различные состояния подключения к частной конечной точке и влияние на публикацию.

| Состояние соединения   |  Публикация успешно выполнена (да/нет) |
| ------------------ | -------------------------------|
| Approved           | Да                            |
| Отклонено           | Нет                             |
| Ожидает            | Нет                             |
| Отключено       | Нет                             |

Чтобы публикация прошла успешно, состояние подключения к частной конечной точке должно быть **утверждено**. Если подключение отклоняется, оно не может быть утверждено с помощью портал Azure. Единственной возможностью является удаление соединения и создание нового.

## <a name="pricing-and-quotas"></a>Цены и квоты
**Частные конечные точки** доступны как на уровне Basic, так и в категории "Премиум" сетки событий. Сетка событий позволяет создавать подключения к частным конечным точкам до 64 для каждого раздела или домена. 

Функция **брандмауэра IP** доступна как на уровне Basic, так и в категории "Премиум" в службе "Сетка событий". Мы допуским создание до 16 правил брандмауэра IP для каждого раздела или домена.

## <a name="next-steps"></a>Дальнейшие действия
Вы можете настроить брандмауэр IP-адресов для ресурса сетки событий, чтобы ограничить доступ через общедоступный Интернет только из выбранного набора IP-адресов или диапазонов IP-адресов. Пошаговые инструкции см. в разделе [Настройка брандмауэра IP](configure-firewall.md).

Частные конечные точки можно настроить для ограничения доступа только из выбранных виртуальных сетей. Пошаговые инструкции см. в разделе [Настройка частных конечных точек](configure-private-endpoints.md).

Сведения об устранении неполадок с сетевым подключением см. в разделе [Устранение неполадок с сетевым подключением](troubleshoot-network-connectivity.md)