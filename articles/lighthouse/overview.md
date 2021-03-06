---
title: Что собой представляет Azure Lighthouse?
description: Azure Lighthouse позволяет поставщикам служб доставлять клиентам управляемые службы с высоким уровнем автоматизации и эффективностью в масштабе.
ms.date: 10/19/2020
ms.topic: overview
ms.openlocfilehash: a76606ff48a09c0c31584882e3d2aa164ec97325
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92203795"
---
# <a name="what-is-azure-lighthouse"></a>Что собой представляет Azure Lighthouse?

Azure Lighthouse обеспечивает управление между несколькими арендаторами, обеспечивая более высокую степень автоматизации, масштабируемости и управления ресурсами и арендаторами.

С помощью Azure Lighthouse поставщики служб могут поставлять управляемые службы, используя комплексные и надежные инструменты управления, встроенные в платформу Azure. Клиенты сохраняют контроль над тем, кто имеет доступ к их арендаторам, какие ресурсы они могут получить и какие действия могут быть выполнены. Это предложение также может предоставить преимущества [корпоративным ИТ предприятиям](concepts/enterprise.md), управляющим ресурсами нескольких арендаторов.

![Обзорная схема Azure Lighthouse](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Преимущества

Azure Lighthouse помогает поставщикам услуг эффективно создавать и предоставлять управляемые службы. Доступные преимущества:

- **Управление схемой в масштабе** . Вовлеченность клиентов и операции жизненного цикла для управления ресурсами клиентов являются более простыми и масштабируемыми. Существующие API, средства управления и рабочие процессы можно использовать с делегированными ресурсами, включая компьютеры, размещенные за пределами Azure, независимо от регионов, в которых они находятся.
- **Дополнительные возможности отслеживания и контроля для клиентов** . Клиенты имеют возможность точно контролировать области, которые они делегируют для управления, и разрешенные права доступа. Они могут выполнять аудит действий поставщиков служб и, при необходимости, полностью удалять доступ.
- **Комплексные и унифицированные средства платформы** . Наш инструментарий относится к основным сценариям поставщика служб, включая несколько моделей лицензирования, таких как EA, CSP и оплату по мере использования. Azure Lighthouse работает с существующими инструментами и API-интерфейсами, моделями лицензирования, [приложениями, управляемыми Azure](concepts/managed-applications.md), и партнерским программами, как, например, [Программа для поставщиков облачных решений (CSP)](/partner-center/csp-overview). Вы можете интегрировать Azure Lighthouse в существующие рабочие процессы и приложения, а также отследить влияние на вовлеченность клиентов, [выполнив привязку идентификатора партнера](./how-to/partner-earned-credit.md).

Использование Azure Lighthouse для управления ресурсами Azure не требует дополнительных затрат. Любой клиент или партнер Azure может использовать Azure Lighthouse.

## <a name="capabilities"></a>Возможности

Azure Lighthouse содержит несколько способов оптимизации вовлеченности и управления.

- **Azure delegated resource management** (Делегированное управление ресурсами Azure). [Управляйте клиентскими ресурсами Azure безопасно из вашего арендатора](concepts/azure-delegated-resource-management.md) без необходимости переключения контекста и уровня управления. Клиентские подписки и группы ресурсов можно делегировать указанным пользователям и ролям в управляющем арендаторе с возможностью запрета доступа по мере необходимости.
- **Новые возможности портала Azure** . Просмотреть сведения о различных арендаторах можно на [странице **Мои клиенты**](how-to/view-manage-customers.md) на портале Azure. Соответствующая [страница **Поставщики служб**](how-to/view-manage-service-providers.md) позволяет вашим клиентам просматривать данные о доступе поставщика служб и управлять им.
- **Шаблоны Azure Resource Manager** . Используйте шаблоны ARM, чтобы [подключить делегированные ресурсы клиента](how-to/onboard-customer.md) и [задачи управления между арендаторами](samples/index.md).
- **Предложения управляемых служб в Azure Marketplace:** [Предоставляйте услуги клиентам](concepts/managed-services-offers.md) в виде частных или общедоступных предложений, а также обеспечьте их автоматическое подключение к Azure Lighthouse.

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь со сведениями о [делегированном управлении ресурсами Azure](concepts/azure-delegated-resource-management.md).
- Узнайте больше об [управлении арендаторами](concepts/cross-tenant-management-experience.md).
- Узнайте, как [использовать Azure Lighthouse в корпоративной среде](concepts/enterprise.md).
