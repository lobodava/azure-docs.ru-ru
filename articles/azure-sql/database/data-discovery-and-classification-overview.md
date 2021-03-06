---
title: Обнаружение и классификация данных
description: '& классификации данных для базы данных SQL Azure, Управляемый экземпляр SQL Azure и Azure синапсе Analytics'
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.custom: sqldbrb=1
titleSuffix: Azure SQL Database, Azure SQL Managed Instance, and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 09/21/2020
tags: azure-synapse
ms.openlocfilehash: ab974b0f68e831e672329f8af5ae1cb6a5fdbd4c
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92672082"
---
# <a name="data-discovery--classification"></a>Обнаружение и классификация данных
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Классификация & обнаружения данных встроена в базу данных SQL Azure, Управляемый экземпляр SQL Azure и Azure синапсе Analytics. Она предоставляет расширенные возможности для обнаружения, классификации, добавления меток и создания отчетов о конфиденциальных данных в базах данных.

Наиболее конфиденциальные данные могут включать деловые, финансовые, здравоохранение или персональные данные. Обнаружение и классификация этих данных может играть сводную роль в подходе защиты информации вашей организации. Она может использоваться в качестве инфраструктуры для следующего:

- Помощь в соблюдении стандартов конфиденциальности данных и требований к соответствию нормативным требованиям.
- Различные сценарии безопасности, такие как мониторинг (аудит) и оповещение об аномальном доступе к конфиденциальным данным.
- Управление доступом и усилением безопасности баз данных, содержащих высококонфиденциальные данные.

> [!NOTE]
> Сведения о локальной SQL Server см. в разделе [Обнаружение данных SQL & классификация](/sql/relational-databases/security/sql-data-discovery-and-classification).

## <a name="what-is-data-discovery--classification"></a><a id="what-is-dc"></a>Что такое &ная классификация обнаружения данных?

Функция обнаружения данных & классификации предоставляет набор дополнительных служб и новых возможностей в Azure. Она формирует новую парадигму защиты информации для базы данных SQL, SQL Управляемый экземпляр и Azure синапсе, нацеленную на защиту данных, а не только базы данных. Парадигма включает в себя:

- **Обнаружение и рекомендации:** Модуль классификации проверяет базу данных и определяет столбцы, которые содержат потенциально конфиденциальные данные. Затем он предоставляет простой способ просмотра и применения рекомендуемой классификации с помощью портал Azure.

- Добавление **меток:** Метки классификации чувствительности можно применять к столбцам с помощью новых атрибутов метаданных, которые были добавлены в ядро СУБД SQL Server. Эти метаданные затем можно использовать для расширенных сценариев аудита и защиты на основе чувствительности.

- **Результат запроса — задать чувствительность:** Чувствительность результирующего набора запроса вычисляется в режиме реального времени для целей аудита.

- **Видимость:** Состояние классификации базы данных можно просмотреть на подробной панели мониторинга в портал Azure. Кроме того, можно скачать отчет в формате Excel для использования в целях соблюдения требований и аудита и других задач.

## <a name="discover-classify-and-label-sensitive-columns"></a><a id="discover-classify-columns"></a>Обнаружение, классификация и маркировка конфиденциальных столбцов

В этом разделе описаны шаги для:

- Обнаружение, классификация и маркировка столбцов, содержащих конфиденциальные данные в базе данных.
- Просмотр текущего состояния классификации базы данных и экспорт отчетов.

Классификация включает в себя два типа атрибутов метаданных:

- **Метки** : основные атрибуты классификации, используемые для определения уровня чувствительности данных, хранящихся в столбце.  
- **Типы сведений** : атрибуты, предоставляющие более детализированные сведения о типе данных, хранящихся в столбце.

### <a name="define-and-customize-your-classification-taxonomy"></a>Определение и настройка таксономии классификации

& классификация обнаружения данных поставляется со встроенным набором меток чувствительности и встроенным набором информационных типов и логикой обнаружения. Теперь вы можете настраивать таксономию и задавать набор и ранжирование конструкций классификации для своей среды.

Вы определяете и настраиваете таксономию классификации в одном централизованном месте для всей Организации Azure. Это расположение находится в [центре безопасности Azure](../../security-center/security-center-introduction.md)в рамках политики безопасности. Эта задача может выполнять только пользователь с правами администратора в корневой группе управления Организации.

В рамках управления политиками для защиты информации можно определить пользовательские метки, ранжировать их и связать с выбранным набором типов сведений. Можно также добавить собственные пользовательские типы данных и настроить их с помощью строковых шаблонов. Шаблоны добавляются в логику обнаружения для идентификации этого типа данных в базах данных.

Дополнительные сведения см. [в статье Настройка политики SQL Information Protection в центре безопасности Azure (Предварительная версия)](../../security-center/security-center-info-protection-policy.md).

После определения политики для всей Организации можно продолжить классификацию отдельных баз данных с помощью настраиваемой политики.

### <a name="classify-your-database"></a>Классификация базы данных

> [!NOTE]
> В приведенном ниже примере используется база данных SQL Azure, но следует выбрать соответствующий продукт, для которого требуется настроить обнаружение данных & классификации.

1. Перейдите на [портал Azure](https://portal.azure.com).

1. Перейдите к разделу **Обнаружение данных & классификации** под заголовком безопасность в области базы данных SQL Azure. Вкладка Обзор содержит сводку текущего состояния классификации базы данных. Сводка включает подробный список всех классифицированных столбцов, которые можно также отфильтровать, чтобы отображались только определенные части схемы, типы сведений и метки. Если вы еще не классифицируете столбцы, [перейдите к шагу 4](#step-4).

1. Чтобы скачать отчет в формате Excel, выберите **Экспорт** в верхнем меню панели.

1. <a id="step-4"></a>Чтобы начать классификацию данных, выберите вкладку **классификация** на странице **& классификация обнаружения данных** .

    Механизм классификации проверяет базу данных на наличие столбцов, содержащих потенциально конфиденциальные данные, и предоставляет список рекомендуемых классификаций столбцов.

1. Просмотр и применение рекомендаций по классификации:

   - Чтобы просмотреть список рекомендуемых классификаций столбцов, выберите панель рекомендации в нижней части панели.

   - Чтобы принять рекомендации для определенного столбца, установите флажок в левом столбце соответствующей строки. Чтобы пометить все рекомендации как принятые, установите крайний левый флажок в заголовке таблицы рекомендаций.

   - Чтобы применить выбранные рекомендации, выберите **принять выбранные рекомендации** .

1. Можно также классифицировать столбцы вручную в качестве альтернативы или в дополнение к классификации на основе рекомендаций.

   1. Выберите **добавить классификацию** в верхнем меню панели.

   1. В открывшемся контекстном окне выберите схему, таблицу и столбец, которые необходимо классифицировать, а также тип сведений и метку чувствительности.

   1. Щелкните **добавить классификацию** в нижней части контекстного окна.

1. Чтобы завершить классификацию и сохранить метку (тег) столбцов базы данных с новыми метаданными классификации, выберите **сохранить** в верхнем меню окна.

## <a name="audit-access-to-sensitive-data"></a><a id="audit-sensitive-data"></a>Аудит доступа к конфиденциальным данным

Важным аспектом парадигмы защиты информации является возможность отслеживать доступ к конфиденциальным данным. [Аудит SQL Azure](../../azure-sql/database/auditing-overview.md) был улучшен и включает в журнал аудита новое поле с именем `data_sensitivity_information` . В этом поле регистрируются классификации конфиденциальности (метки) данных, возвращенных запросом. Пример:

![Журнал аудита](./media/data-discovery-and-classification-overview/11_data_classification_audit_log.png)

## <a name="permissions"></a><a id="permissions"></a>Permissions

Эти встроенные роли могут считывать классификацию данных базы данных.

- Владелец
- Читатель
- Участник
- Диспетчер безопасности SQL
- Администратор доступа пользователей

Эти встроенные роли могут изменять классификацию данных базы данных.

- Владелец
- Участник
- Диспетчер безопасности SQL

Дополнительные сведения о разрешениях на основе ролей в [Azure RBAC](../../role-based-access-control/overview.md).

## <a name="manage-classifications"></a><a id="manage-classification"></a>Управление классификациями

Для управления классификациями можно использовать T-SQL, REST API или PowerShell.

### <a name="use-t-sql"></a>Использование T-SQL

T-SQL можно использовать для добавления или удаления классификаций столбцов, а также для получения всех классификаций для всей базы данных.

> [!NOTE]
> При использовании T-SQL для управления метками не существует проверки того, что метки, добавляемые в столбец, существуют в политике защиты информации Организации (набор меток, отображаемых в рекомендациях по порталу). Таким образом, вы можете проверить это.

Дополнительные сведения об использовании T-SQL для классификаций см. по следующим ссылкам:

- Добавление или обновление классификации одного или нескольких столбцов: [Добавление классификации чувствительности](/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Удаление классификации из одного или нескольких столбцов: удаление [классификации чувствительности](/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Чтобы просмотреть все классификации в базе данных, выполните следующие действия. [sys.sensitivity_classifications](/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

### <a name="use-powershell-cmdlets"></a>Использование командлетов PowerShell
Управление классификациями и рекомендациями для базы данных SQL Azure и Управляемый экземпляр Azure SQL с помощью PowerShell.

#### <a name="powershell-cmdlets-for-azure-sql-database"></a>Командлеты PowerShell для базы данных SQL Azure

- [Get-Азсклдатабасесенситивитиклассификатион](/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-Азсклдатабасесенситивитиклассификатион](/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-Азсклдатабасесенситивитиклассификатион](/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-Азсклдатабасесенситивитирекоммендатион](/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-Азсклдатабасесенситивитирекоммендатион](/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Disable-Азсклдатабасесенситивитирекоммендатион](/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

#### <a name="powershell-cmdlets-for-azure-sql-managed-instance"></a>Командлеты PowerShell для Управляемый экземпляр Azure SQL

- [Get-Азсклинстанцедатабасесенситивитиклассификатион](/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-Азсклинстанцедатабасесенситивитиклассификатион](/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-Азсклинстанцедатабасесенситивитиклассификатион](/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-Азсклинстанцедатабасесенситивитирекоммендатион](/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-Азсклинстанцедатабасесенситивитирекоммендатион](/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Disable-Азсклинстанцедатабасесенситивитирекоммендатион](/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)

### <a name="use-the-rest-api"></a>Использование API-интерфейса для других функций

Вы можете использовать REST API для программного управления классификациями и рекомендациями. Опубликованная REST API поддерживает следующие операции:

- [Создать или обновить](/rest/api/sql/sensitivitylabels/createorupdate): создает или обновляет метку чувствительности указанного столбца.
- [Удалить](/rest/api/sql/sensitivitylabels/delete): удаляет метку чувствительности указанного столбца.
- [Отключить рекомендацию](/rest/api/sql/sensitivitylabels/disablerecommendation): отключает рекомендации по конфиденциальности для указанного столбца.
- [Включить рекомендацию](/rest/api/sql/sensitivitylabels/enablerecommendation): включает рекомендации по конфиденциальности для указанного столбца. (Рекомендации включены по умолчанию для всех столбцов.)
- [Get](/rest/api/sql/sensitivitylabels/get): Получает метку чувствительности указанного столбца.
- [Перечислить текущие по базе данных](/rest/api/sql/sensitivitylabels/listcurrentbydatabase): Возвращает текущие метки чувствительности указанной базы данных.
- [Список рекомендованных баз данных](/rest/api/sql/sensitivitylabels/listrecommendedbydatabase): получает Рекомендуемые метки чувствительности указанной базы данных.

## <a name="next-steps"></a><a id="next-steps"></a>Следующие шаги

- Рассмотрите возможность настройки [аудита SQL Azure](../../azure-sql/database/auditing-overview.md) для мониторинга и аудита доступа к секретным конфиденциальным данным.
- Для презентаций, включающих & классификацию данных, см. статью обнаружение [, классификация, маркировка & защита данных SQL | Предоставленные данные](https://www.youtube.com/watch?v=itVi9bkJUNc).