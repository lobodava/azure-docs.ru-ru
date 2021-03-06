---
title: Автомасштабирование в Microsoft Azure
description: Автомасштабирование в Microsoft Azure
ms.subservice: autoscale
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: bd7c1582cdb4b2b1b72d3f969ad08879d208785f
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94505843"
---
# <a name="overview-of-autoscale-in-microsoft-azure"></a>Общие сведения об автомасштабировании в Microsoft Azure
В этой статье объясняется, что такое автомасштабирование Microsoft Azure, каковы преимущества этой функции и как начать ее использовать.  

Автомасштабирование Azure Monitor используется только с [масштабируемыми наборами виртуальных машин](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [облачными службами](https://azure.microsoft.com/services/cloud-services/), [веб-приложениями службы приложений](https://azure.microsoft.com/services/app-service/web/), [службами управления API](../../api-management/api-management-key-concepts.md) и [кластерами Azure Data Explorer](/azure/data-explorer/).

> [!NOTE]
> В Azure доступно два способа автомасштабирования. Более старый способ работает с виртуальными машинами (группами доступности). Для него предлагается ограниченная поддержка, поэтому мы рекомендуем перейти к масштабируемым наборам виртуальных машин, чтобы обеспечить более быстрое и надежное автомасштабирование. В этой статье есть ссылка на сведения об использовании более старой технологии.  
>

## <a name="what-is-autoscale"></a>Основные сведения об автомасштабировании
Благодаря автомасштабированию вы получаете именно тот объем ресурсов, который нужен для обработки нагрузки в вашем приложении. Эта функция позволяет добавлять ресурсы для обработки дополнительной нагрузки и удалять неиспользуемые ресурсы для экономии средств. Вы указываете минимальное и максимальное количество экземпляров, которые должны работать, и система на основании набора правил автоматически добавляет или удаляет виртуальные машины. Если указано минимальное количество, приложение работает всегда, даже при отсутствии нагрузки. Максимальное количество, в свою очередь, ограничивает общую возможную почасовую стоимость. Используя созданные вами правила, система автоматически масштабирует ресурсы в диапазоне двух предельных значений.

 ![Объяснение автомасштабирования. Добавление и удаление виртуальных машин](./media/autoscale-overview/AutoscaleConcept.png)

Когда условия правил выполняются, активируется одно или несколько действий автомасштабирования. К ним относятся добавление и удаление виртуальных машин, а также другие настраиваемые действия. Этот процесс показан на концептуальной схеме ниже.  

 ![Схема потока автомасштабирования](./media/autoscale-overview/Autoscale_Overview_v4.png)

Приведенные ниже сведения относятся к показанной выше схеме.   

## <a name="resource-metrics"></a>Метрики ресурсов
Ресурсы выдают метрики, которые позже обрабатываются с помощью правил. Метрики можно получить различными способами.
В масштабируемых наборах виртуальных машин используются данные телеметрии из агентов диагностики Azure, а телеметрия для веб-приложений и облачных служб поступает непосредственно из инфраструктуры Azure. К некоторым часто используемым статистическим сведениям относятся загрузка ЦП, использование памяти, число потоков, длина очереди и использование диска. Список данных телеметрии, которые можно использовать, см. в статье об [общих метриках автомасштабирования в Azure Monitor](autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Пользовательские метрики
Вы также можете использовать собственные пользовательские метрики, выдаваемые приложениями. Если вы настроили приложения для отправки метрик в Application Insights, вы сможете использовать эти метрики для принятия решений о том, нужно ли выполнять масштабирование.

## <a name="time"></a>Time
В правилах на основе расписания используется формат UTC. При настройке правил необходимо правильно задать свой часовой пояс.  

## <a name="rules"></a>Правила
На схеме показано только одно правило автомасштабирования. Однако можно использовать и сразу несколько. При необходимости для конкретной ситуации можно создать сложные перекрывающиеся правила.  Правила бывают следующих типов:  

* **на основе метрики** , например выполнение определенного действия при загрузке ЦП более чем на 50 %;
* **на основе времени** , например активация webhook каждую субботу в 8:00 в заданном часовом поясе.

Правила на основе метрик измеряют нагрузку приложений и в зависимости от нагрузки добавляют или удаляют виртуальные машины. Правила, в которых используется расписание, позволяют масштабировать ресурсы, когда в изменении нагрузки просматриваются закономерности. Например, вы хотите добавить или удалить ресурсы до того, как нагрузка увеличится или уменьшится.  

## <a name="actions-and-automation"></a>Действия и автоматизация
Правила могут вызвать одно или несколько типов действий:

* **Масштабирование** — свертывание или развертывание виртуальных машин.
* **Электронная почта** — отправка письма по электронной почте администраторам и соадминистраторам подписки, а также на указанные дополнительные адреса электронной почты.
* **Автоматизация с помощью объектов webhook** — вызов объектов webhook, с помощью которых можно активировать несколько сложных действий в среде Azure и за ее пределами. В Azure можно запустить Runbook службы автоматизации Azure, функции Azure или приложения Azure Logic. К примерам сторонних служб можно отнести Slack и Twilio.

## <a name="autoscale-settings"></a>Параметры автомасштабирования
Автомасштабирование имеет определенную структуру, для описания которой используется соответствующая терминология.

- Подсистема автомасштабирования считывает **параметр автомасштабирования** и определяет, что нужно делать с масштабом: увеличивать или уменьшать. Этот параметр содержит один или несколько профилей, информацию о целевом ресурсе и настройки уведомлений.

  - **Профиль автомасштабирования**  — это комбинация следующих компонентов:

    - **параметр емкости** , который определяет минимальное, максимальное и стандартное значение числа экземпляров;
    - **набор правил,** , каждый из которых включает триггер (время или метрика) и действие масштабирования (увеличение или уменьшение);
    - сведения о **повторении** , которые определяют, когда именно функция автомасштабирования должна активировать тот или иной профиль.

      На случай, если одни требования частично дублируются другими требованиями, можно создать несколько профилей. У вас может быть несколько профилей автомасштабирования, предназначенных, например, для разных дней недели или разного времени суток.

  - **Параметр уведомлений** определяет, какие уведомления должны появляться, когда происходит событие автомасштабирования, отвечающее условиям одного из профилей автомасштабирования. Функция автомасштабирования может отправлять уведомления на электронную почту или вызывать перехватчики webhook.


![Структура правил, профилей и параметров автомасштабирования Azure](./media/autoscale-overview/AzureResourceManagerRuleStructure3.png)

Полный список настраиваемых полей и описаний см. в статье [о REST API для функции автомасштабирования](/rest/api/monitor/autoscalesettings).

Примеры кода см. в следующих статьях:

* [Расширенная настройка автомасштабирования с помощью шаблонов Resource Manager для набора масштабирования виртуальных машин](autoscale-virtual-machine-scale-sets.md)  
* [Create or update an autoscale setting in Azure Insights REST API (Создание и изменение параметров автомасштабирования в REST API Azure Insights)](/rest/api/monitor/autoscalesettings)

## <a name="horizontal-vs-vertical-scaling"></a>Горизонтальное и вертикальное масштабирование
При автомасштабировании ресурсы масштабируются только горизонтально, то есть происходит увеличение (развертывание) или уменьшение (свертывание) количества экземпляров виртуальной машины.  Горизонтальное масштабирование в облаке более гибкое, так как при этом можно потенциально запускать тысячи виртуальных машин для обработки нагрузки.

С вертикальным масштабированием дело обстоит иначе. Имеющееся количество виртуальных машин всегда остается неизменным, а увеличивается или уменьшается их мощность. Мощность измеряется по объему памяти, скорости ЦП, дисковому пространству и т. д.  Вертикальное масштабирование связано с дополнительными ограничениями. Оно зависит от доступности оборудования большего размера, что, в свою очередь, зависит от региона. К тому же при таком масштабировании быстро достигается верхнее ограничение. Обычно при вертикальном масштабировании требуется запускать и останавливать виртуальную машину.

## <a name="methods-of-access"></a>Варианты доступа
Автомасштабирование можно настроить с помощью следующих инструментов:

* [Портал Azure](autoscale-get-started.md)
* [PowerShell](../samples/powershell-samples.md#create-and-manage-autoscale-settings)
* [Кроссплатформенный интерфейс командной строки](../samples/cli-samples.md#autoscale)
* [Azure Monitor REST API](/rest/api/monitor/autoscalesettings)

## <a name="supported-services-for-autoscale"></a>Службы, поддерживающие автомасштабирование
| Служба | Схемы и документы |
| --- | --- |
| Веб-приложения |[Scaling Web Apps (Масштабирование веб-приложения)](autoscale-get-started.md) |
| Облачные службы |[Автомасштабирование облачной службы](../../cloud-services/cloud-services-how-to-scale-portal.md) |
| Виртуальные машины: Классический |[Scaling Classic Virtual Machine Availability Sets (Масштабирование групп доступности классических виртуальных машин)](/archive/blogs/kaevans/autoscaling-azurevirtual-machines) |
| Виртуальные машины: масштабируемые наборы Windows |[Масштабирование и масштабируемые наборы виртуальных машин в Windows](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md) |
| Виртуальные машины: масштабируемые наборы Linux |[Масштабирование и масштабируемые наборы виртуальных машин в Linux](../../virtual-machine-scale-sets/tutorial-autoscale-cli.md) |
| Виртуальные машины: пример Windows |[Расширенная настройка автомасштабирования с помощью шаблонов Resource Manager для набора масштабирования виртуальных машин](autoscale-virtual-machine-scale-sets.md) |
| Служба приложений Azure |[Масштабирование приложения в службе приложений Azure](../../app-service/manage-scale-up.md)|
| Служба управления API|[Автоматическое масштабирование экземпляра службы управления API Azure](../../api-management/api-management-howto-autoscale.md)
| Кластеры Azure Data Explorer|[Управление кластерами Azure Data Explorer с учетом меняющихся потребностей](/azure/data-explorer/manage-cluster-horizontal-scaling)|
| Logic Apps |[Добавление емкости среды службы интеграции (ISE)](../../logic-apps/ise-manage-integration-service-environment.md#add-ise-capacity)|
| Spring Cloud |[Настройка автомасштабирования для приложений, состоящих из микрослужб](../../spring-cloud/spring-cloud-tutorial-setup-autoscale.md)|
| Cлужебная шина |[Автоматическое обновление единиц обмена сообщениями в пространстве имен Служебной шины Azure](../../service-bus-messaging/automate-update-messaging-units.md)|

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения об автомасштабировании см. в пошаговых руководствах по автомасштабированию выше, а также в следующих статьях:

* [Общие метрики автомасштабирования Azure Monitor](autoscale-common-metrics.md)
* [Рекомендации по автомасштабированию в Azure Monitor](autoscale-best-practices.md)
* [Использование действий автомасштабирования для отправки электронной почты и уведомлений об оповещениях веб-перехватчика в Azure Insights](autoscale-webhook-email.md)
* [Create or update an autoscale setting in Azure Insights REST API (Создание и изменение параметров автомасштабирования в REST API Azure Insights)](/rest/api/monitor/autoscalesettings)
* [Устранение неполадок при автомасштабировании масштабируемых наборов виртуальных машин](../../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
