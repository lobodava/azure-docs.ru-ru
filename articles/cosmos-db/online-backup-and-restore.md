---
title: Оперативное резервное копирование и восстановление данных по запросу в Azure Cosmos DB
description: В этой статье описывается, как работает автоматическое резервное копирование, восстановление данных по запросу, как настроить интервал и период хранения резервных копий, как использовать службу поддержки для восстановления данных в Azure Cosmos DB.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/13/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 43625a80df76ff35b8bb1804df5f5fd1524326c5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097538"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Оперативное резервное копирование и восстановление данных по запросу в Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB автоматически копирует все ваши данные с регулярными интервалами. Автоматическое резервное копирование не влияет на производительность или доступность операций баз данных. Для обеспечения устойчивости в случае региональной аварии все резервные копии хранятся отдельно в службе хранилища, а также глобально реплицируются. Автоматические резервные копии позволяют восстановить данные при случайном удалении или обновлении учетной записи, базы данных или контейнера Azure Cosmos.

## <a name="automatic-and-online-backups"></a>Автоматическое и оперативное резервное копирование

Azure Cosmos DB обеспечивает высокую избыточность и устойчивость к региональным авариям не только ваших данных, но и их резервных копий. Ниже описано, как Azure Cosmos DB выполняет резервное копирование данных.

* Azure Cosmos DB автоматически создает полную резервную копию базы данных каждые 4 часа и в любой момент времени по умолчанию сохраняются только последние две резервные копии. Если интервалы по умолчанию недостаточно для рабочих нагрузок, можно изменить интервал резервного копирования и срок хранения из портал Azure. Конфигурацию резервного копирования можно изменить во время или после создания учетной записи Azure Cosmos. Если контейнер или база данных удалены, Azure Cosmos DB оставляет существующие моментальные снимки определенного контейнера или базы данных в течение 30 дней.

* Azure Cosmos DB хранит эти резервные копии в хранилище BLOB-объектов Azure, а фактические данные находятся локально в Azure Cosmos DB.

* Чтобы гарантировать низкую задержку, моментальный снимок резервной копии хранится в хранилище BLOB-объектов Azure в том же регионе, что и текущий регион записи (или **один** из регионов записи, если у вас есть конфигурация записи в нескольких регионах). Чтобы можно было обеспечить устойчивость к региональным авариям, каждый моментальный снимок данных резервной копии в хранилище BLOB-объектов Azure повторно реплицируется в другой регион через геоизбыточное хранилище (GRS). Регион, в который реплицируется резервная копия, зависит от региона источника и региона, который составляет с ним пару. Чтобы узнать больше, ознакомьтесь со [списком геоизбыточных пар регионов Azure](../best-practices-availability-paired-regions.md). У вас нет прямого доступа к этой резервной копии. Команда Azure Cosmos DB восстановит резервную копию при получении запроса на поддержку.

   Ниже показано, как контейнер Azure Cosmos со всеми тремя основными физическими разделами в регионе "западная часть США" копируется в удаленную учетную запись хранилища BLOB-объектов Azure в регионе "западная часть США", а затем реплицируется в регион "восточная часть США".

  :::image type="content" source="./media/online-backup-and-restore/automatic-backup.png" alt-text="Периодически выполняемая полная архивация всех сущностей Cosmos DB в геоизбыточное хранилище Azure" border="false":::

* Резервное копирование не влияет на производительность или доступность приложения. Azure Cosmos DB выполняет резервное копирование данных в фоновом режиме, не используя дополнительную пропускную способность (ЕЗ) и не снижая производительность и доступность базы данных.

## <a name="modify-the-backup-interval-and-retention-period"></a><a id="configure-backup-interval-retention"></a>Изменение интервала резервного копирования и срока хранения

Azure Cosmos DB автоматически выполняет полную архивацию данных каждые 4 часа и в любой момент времени сохраняются две последние резервные копии. Эта конфигурация является вариантом по умолчанию, и она предоставляется без каких-либо дополнительных затрат. Вы можете изменить интервал резервного копирования по умолчанию и период хранения во время создания учетной записи Azure Cosmos или после ее создания. Конфигурация резервного копирования задается на уровне учетной записи Azure Cosmos, и ее необходимо настроить для каждой учетной записи. После настройки параметров резервного копирования для учетной записи она применяется ко всем контейнерам в этой учетной записи. В настоящее время параметры резервного копирования можно изменить только на портале Azure.

Если вы случайно удалили или повредили данные, **прежде чем создавать запрос в службу поддержки для восстановления данных, не забудьте увеличить срок хранения резервной копии для вашей учетной записи, не менее семи дней. Лучше увеличить срок хранения в течение 8 часов этого события.** Таким образом, у команды Azure Cosmos DB будет достаточно времени для восстановления вашей учетной записи.

Чтобы изменить параметры резервного копирования по умолчанию для существующей учетной записи Azure Cosmos, выполните следующие действия.

1. Войдите в [портал Azure.](https://portal.azure.com/)
1. Перейдите к учетной записи Azure Cosmos и откройте панель **архивация & восстановление** . Обновите интервал резервного копирования и срок хранения резервных копий по мере необходимости.

   * **Интервал резервного копирования** — это интервал, с которым Azure Cosmos DB пытается создать резервную копию данных. Резервное копирование занимает ненулевое количество времени. в некоторых случаях это может привести к сбою из-за подчиненных зависимостей. Azure Cosmos DB пытается создать резервную копию с заданным интервалом, однако это не гарантирует, что резервное копирование завершится в течение этого интервала времени. Это значение можно настроить в часах или минутах. Интервал резервного копирования не может быть меньше 1 часа и больше 24 часов. При изменении этого интервала новый интервал вступает в силу с момента, когда была создана последняя резервная копия.

   * **Хранение резервных копий** — указывает период хранения каждой резервной копии. Его можно настроить в часах или днях. Минимальный срок хранения не может превышать интервал резервного копирования (в часах) в два раза и не может превышать 720 часов.

   * **Копии хранящихся данных** . по умолчанию предлагаются две резервные копии данных без оплаты. Дополнительная плата взимается, если требуется более двух копий. Точную стоимость дополнительных копий можно просмотреть в разделе "Использованное хранилище" на [странице цен](https://azure.microsoft.com/pricing/details/cosmos-db/).

   :::image type="content" source="./media/online-backup-and-restore/configure-backup-interval-retention.png" alt-text="Периодически выполняемая полная архивация всех сущностей Cosmos DB в геоизбыточное хранилище Azure" border="true":::

При настройке параметров резервного копирования во время создания учетной записи можно настроить **политику резервного копирования** , которая является **периодической** или **непрерывной** . Периодическая политика позволяет настроить интервал резервного копирования и срок хранения резервных копий. Постоянная политика в настоящее время доступна только для регистрации. Команда Azure Cosmos DB выполнит оценку рабочей нагрузки и утвердит ваш запрос.

:::image type="content" source="./media/online-backup-and-restore/configure-periodic-continuous-backup-policy.png" alt-text="Периодически выполняемая полная архивация всех сущностей Cosmos DB в геоизбыточное хранилище Azure" border="true":::

## <a name="request-data-restore-from-a-backup"></a>Запрос восстановления данных из резервной копии

Если вы случайно удалили базу данных или контейнер, вы можете отправить запрос в [службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) или [обратиться в службу поддержки Azure](https://azure.microsoft.com/support/options/) для восстановления данных из автоматической оперативной архивации. Поддержка Azure доступна только для выбранных планов, таких как **стандартные** , **разработчики** и планы выше. Для плана **Базовый** поддержка Azure не предоставляется. Дополнительные сведения о различных планах поддержки см. на странице [Планы поддержки Azure](https://azure.microsoft.com/support/plans/).

Чтобы восстановить определенный моментальный снимок архива, службе Azure Cosmos DB требуется, чтобы данные были доступны в течение цикла архивации для данного снимка.
Прежде чем запрашивать восстановление, нужно подготовить следующие сведения:

* Идентификатор подписки.

* В зависимости от того, каким способом были случайно удалены или изменены ваши данные, может понадобиться подготовить дополнительные сведения. Рекомендуем заранее приготовить информацию, чтобы свести к минимуму время на ее передачу, особенно в тех случаях, когда проблему нужно устранить как можно быстрее.

* Если учетная запись Azure Cosmos DB удалена полностью, необходимо указать имя удаленной учетной записи. Если вы создаете другую учетную запись с тем же именем, что и у удаленной, сообщите об этом в службу поддержки, так как это поможет определить правильную учетную запись для выбора. Рекомендуется использовать разные билеты в службу поддержки для каждой удаленной учетной записи, так как это снизит путаницу в состоянии восстановления.

* Если удалена одна или больше баз данных, необходимо указать учетную запись Azure Cosmos и имена баз данных Azure Cosmos, а также сообщить, существует ли новая база данных с тем же именем.

* Если удален один или больше контейнеров, следует указать имя учетной записи Azure Cosmos, имена баз данных и контейнеров. Сообщите также, существует ли контейнер с тем же именем.

* Если вы случайно удалили или повредили данные, обратитесь в [службу поддержки Azure](https://azure.microsoft.com/support/options/) в течение 8 часов, чтобы группа Azure Cosmos DB могла помочь вам восстановить данные из резервных копий. **Перед созданием запроса на поддержку для восстановления данных необходимо [увеличить срок хранения резервной копии](#configure-backup-interval-retention) для вашей учетной записи, не менее семи дней. Лучше увеличить срок хранения в течение 8 часов этого события.** Таким образом, Группа поддержки Azure Cosmos DB будет иметь достаточно времени для восстановления вашей учетной записи.

В дополнение к имени учетной записи Azure Cosmos, именам баз данных, именам контейнеров следует указать момент времени, в который можно восстановить данные. Важно действовать с максимальной точностью, чтобы помочь нам определить самые лучшие резервные копии, доступные на этот момент. **Важно также указать время в формате UTC.**

На снимке экрана ниже показано, как создать запрос в службу поддержки для контейнера (коллекции, графа, таблицы) на восстановление данных с помощью портала Azure. Предоставьте дополнительные сведения, например тип данных, цель восстановления и время удаления данных, чтобы помочь нам назначить приоритет для запроса.

:::image type="content" source="./media/online-backup-and-restore/backup-support-request-portal.png" alt-text="Периодически выполняемая полная архивация всех сущностей Cosmos DB в геоизбыточное хранилище Azure":::

## <a name="considerations-for-restoring-the-data-from-a-backup"></a>Рекомендации по восстановлению данных из резервной копии

Вы можете случайно удалить или изменить данные в одном из следующих сценариев:  

* Удалите всю учетную запись Azure Cosmos.

* Удалите одну или несколько баз данных Azure Cosmos.

* Удалите один или несколько контейнеров Azure Cosmos.

* Удаление или изменение элементов Azure Cosmos (например, документов) в контейнере. Этот конкретный случай обычно называется повреждением данных.

* База данных общего предложения или контейнеры в общей базе данных предложений удалены или повреждены.

Azure Cosmos DB может восстановить данные во всех указанных выше сценариях. При восстановлении создается новая учетная запись Azure Cosmos для хранения восстановленных данных. Имя новой учетной записи, если она не указана, будет иметь формат `<Azure_Cosmos_account_original_name>-restored1` . Последняя цифра увеличивается при попыток нескольких восстановлений. Вы не можете восстановить данные в предварительно созданную учетную запись Azure Cosmos.

При случайном удалении учетной записи Cosmos Azure можно восстановить данные в новую учетную запись с тем же именем, если имя учетной записи не используется. Поэтому не рекомендуется повторно создавать учетную запись после ее удаления. Так как он не только предотвращает использование восстановленных данных одним и тем же именем, но также обнаруживает правильную учетную запись для восстановления из сложностей.

При случайном удалении базы данных Cosmos Azure можно восстановить всю базу данных или подмножество контейнеров в этой базе данных. Кроме того, можно выбрать определенные контейнеры в базах данных и восстановить их в новой учетной записи Azure Cosmos.

При случайном удалении или изменении одного или нескольких элементов в контейнере (случай повреждения данных) необходимо указать время восстановления. При повреждении данных важно иметь значение времени. Так как контейнер находится в реальном времени, резервная копия все еще выполняется, поэтому, если вы ждете превышение срока хранения (значение по умолчанию составляет восемь часов), резервные копии будут перезаписаны. Чтобы предотвратить перезапись резервной копии, увеличьте срок хранения резервной копии для своей учетной записи, не менее семи дней. Лучше увеличить срок хранения в течение 8 часов после повреждения данных.

Если вы случайно удалили или повредили данные, обратитесь в [службу поддержки Azure](https://azure.microsoft.com/support/options/) в течение 8 часов, чтобы группа Azure Cosmos DB могла помочь вам восстановить данные из резервных копий. Таким образом, Группа поддержки Azure Cosmos DB будет иметь достаточно времени для восстановления вашей учетной записи.

> [!NOTE]
> После восстановления данных не все исходные возможности и параметры переносятся в восстановленную учетную запись. Следующие параметры не переносятся на новую учетную запись:
> * Списки управления доступом к виртуальной сети
> * Хранимые процедуры, триггеры и определяемые пользователем функции
> * Параметры для нескольких регионов  

При подготовке пропускной способности на уровне базы данных процесс резервного копирования и восстановления в этом случае выполняется на уровне всей базы данных, а не на уровне отдельных контейнеров. В таких случаях нельзя выбрать подмножество восстанавливаемых контейнеров.

## <a name="options-to-manage-your-own-backups"></a>Способы управления резервными копиями

Учетные записи API SQL Azure Cosmos DB предоставляют следующие способы управления резервными копиями:

* периодическое перемещение данных в собственное хранилище с помощью [Фабрики данных Azure](../data-factory/connector-azure-cosmos-db.md);

* Используйте Azure Cosmos DB [канал изменений](change-feed.md) , чтобы периодически считывать данные для полных резервных копий или для добавочных изменений, а также сохранять их в собственном хранилище.

## <a name="post-restore-actions"></a>Действия после восстановления

Основной целью восстановления данных является восстановление данных, которые были случайно удалены или изменены. Поэтому мы рекомендуем сначала проверить содержимое восстановленных данных. Если все выглядит хорошо, можно перенести данные обратно в основную учетную запись. Вы можете использовать восстановленную учетную запись в качестве новой активной учетной записи, но это не рекомендуется делать при наличии производственных рабочих нагрузок. 

После восстановления данных вы получите уведомление с указанием имени новой учетной записи (обычно в формате `<original-name>-restored1`) и времени ее восстановления. У восстановленной учетной записи будет такая же подготовленная пропускная способность и политики индексирования, она будет находиться в том же регионе, что и исходная учетная запись. Пользователь, который является администратором подписки или соадминистратором, может просмотреть восстановленную учетную запись.

### <a name="migrate-data-to-the-original-account"></a>Перенос данных в исходную учетную запись

Ниже приведены различные способы переноса данных обратно в исходную учетную запись.

* Используйте [средство переноса данных Azure Cosmos DB](import-data.md).
* Используйте [фабрику данных Azure](../data-factory/connector-azure-cosmos-db.md).
* Используйте [веб-канал изменений](change-feed.md) в Azure Cosmos DB.
* Вы можете написать собственный пользовательский код.

Рекомендуем удалить контейнер или базу данных сразу после переноса данных. Если восстановленные базы данных или контейнеры не будут удалены, то затраты на единицы запросов, хранение и исходящий трафик увеличатся.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как восстановить данные из учетной записи Azure Cosmos или перенести данные в учетную запись Azure Cosmos.

* [Запрос на восстановление можно отправить с портала Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), обратившись в службу поддержки Azure.
* Перемещение данных в Azure Cosmos DB с помощью [канала изменений Cosmos DB](change-feed.md).
* Перемещение данных в Azure Cosmos DB с помощью [Фабрики данных Azure](../data-factory/connector-azure-cosmos-db.md).

