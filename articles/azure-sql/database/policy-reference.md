---
title: Встроенные определения политик для Базы данных SQL Azure
description: Здесь приведены встроенные определения политик в Политике Azure для Базы данных SQL Azure и Управляемого экземпляра SQL. Эти встроенные определения политик предоставляют популярные подходы к управлению ресурсами Azure.
ms.date: 11/20/2020
ms.topic: reference
author: stevestein
ms.author: sstein
ms.service: sql-database
ms.custom: subject-policy-reference
ms.openlocfilehash: ce12ab6056d0b609a60a4a17f44fb085cb1ba675
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94988642"
---
# <a name="azure-policy-built-in-definitions-for-azure-sql-database--sql-managed-instance"></a>Встроенные определения в Политике Azure для Базы данных SQL Azure и Управляемого экземпляра SQL
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Эта страница представляет собой индекс встроенных определений политик в [Политике Azure](../../governance/policy/overview.md) для Базы данных SQL Azure и Управляемого экземпляра SQL. Дополнительные встроенные компоненты Политики Azure для других служб см. в статье [Встроенные определения Политики Azure](../../governance/policy/samples/built-in-policies.md).

Имя каждого встроенного определения политики связано с определением политики на портале Azure. Перейдите по ссылке в столбце **Версия**, чтобы просмотреть исходный код в [репозитории GitHub для службы "Политика Azure"](https://github.com/Azure/azure-policy).

## <a name="azure-sql-database--sql-managed-instance"></a>База данных SQL Azure и Управляемый экземпляр SQL 

[!INCLUDE [azure-policy-reference-service-sqldatabase](../../../includes/policy/reference/byrp/microsoft.sql.md)]

## <a name="limitations"></a>Ограничения
- Политика Azure, применимая к созданию базы данных SQL Azure, не применяется при использовании T-SQL или SSMS. 

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь со встроенными инициативами в [репозитории GitHub для Политики Azure](https://github.com/Azure/azure-policy).
- Изучите статью о [структуре определения Политики Azure](../../governance/policy/concepts/definition-structure.md).
- Изучите [сведения о действии политик](../../governance/policy/concepts/effects.md).
