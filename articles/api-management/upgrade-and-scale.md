---
title: Обновление и масштабирование экземпляра службы управления API Azure | Документация Майкрософт
description: В этой статье описано, как обновлять и масштабировать экземпляры службы управления API Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 626f5b67905e5dd89cf8f12460bc2378451614de
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078312"
---
# <a name="upgrade-and-scale-an-azure-api-management-instance"></a>Обновление и масштабирование экземпляра управления API Azure  

Клиенты могут масштабировать экземпляр службы управления API Azure, добавляя и удаляя единицы. **Единицы** состоят из выделенных ресурсов Azure c определенной способностью выдерживать нагрузку, представленную числом вызовов API в месяц. Это число никак не связано с предельным числом вызовов, а представляет собой допустимое значение максимальной пропускной способности при предварительном планировании ресурсов. Фактическая пропускная способность и задержка могут сильно отличаться в зависимости от многих факторов, включая число и частоту параллельных подключений, виды и число настроенных политик, размеры запросов и ответов, а также задержку серверной части.

Емкость и цена каждой единицы зависят от **уровня**, на котором существует эта единица. Вы можете использовать один из четырех уровней: **Разработка**, **Базовый**, **Стандартный** или **Премиум**. Если вы хотите увеличить производительность службы, не изменяя уровень, добавьте новую единицу. Если уровень, выбранный в экземпляре управления API, не допускает добавление дополнительных единиц, необходимо выполнить обновление до уровня более высокого уровня.

Цена каждой единицы и доступные функции (например, развертывание в нескольких регионах) зависит от уровня, выбранного для экземпляра управления API. В [этой статье](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) указаны цены за единицу и описаны доступные функции для каждого уровня. 

>[!NOTE]
>В [этой статье](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) представлены примерные данные о производительности единиц для каждого уровня. Чтобы получить более точные сведения, изучите приближенный к реальному сценарий использования API. См. статью о [емкости экземпляра службы Azure "Управление API"](api-management-capacity.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить шаги из этой статьи, понадобится следующее:

+ Наличие активной подписки Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Иметь экземпляр управления API. Дополнительные сведения см. в статье о [создании экземпляра управления API Azure](get-started-create-service-instance.md).

+ Понимание концепции [емкости экземпляра службы управления API Azure](api-management-capacity.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="upgrade-and-scale"></a>Обновление и масштабирование  

Можно выбрать один из четырех уровней: **Developer**, **Basic**,  **Standard**и **Premium**. Уровень **Developer** предназначен только для оценки службы. Не используйте его в рабочей среде. Уровень **Developer** не предусматривает соглашение об уровне обслуживания и не поддерживает масштабирование (не позволяет добавлять или удалять единицы). 

" **Базовый**", " **стандартный**" и " **премиум** " — это производственные уровни, которые имеют соглашение об уровне обслуживания и могут масштабироваться. Уровень " **базовый** " — самый дешевый уровень с соглашением об уровне обслуживания. его можно масштабировать до двух единиц, а уровень " **стандартный** " можно масштабировать до четырех единиц. На уровне **Premium** вы можете добавлять неограниченное число единиц.

Уровень **Премиум** позволяет распределить один экземпляр управления API Azure между любым числом регионов Azure на выбор. Экземпляр создаваемой службы управления API Azure содержит только одну единицу. Он размещается в одном регионе Azure. Этот начальной регион считается для экземпляра **основным** регионом. Вы можете легко добавить дополнительные регионы. При добавлении региона укажите число выделяемых для него единиц. Например, вы можете использовать одну единицу в **основном** регионе и пять единиц в любом другом регионе. Можно распределить число единиц с учетом характеристик трафика в каждом регионе. Дополнительные сведения см. в статье [Развертывание экземпляра службы управления Azure API в различных регионах Azure](api-management-howto-deploy-multi-region.md).

Вы всегда можете повысить или понизить уровень в любых комбинациях. Обновление или переход на более раннюю версию может удалить некоторые функции, например виртуальных сетей или развертывание в нескольких регионах, при переходе на уровень "Премиум" до уровня "Стандартный" или "базовый".

> [!NOTE]
> Процесс обновления или масштабирования может занять от 15 до 45 минут. По завершении вы получите соответствующее извещение.

> [!NOTE]
> Служба управления API на уровне **потребления** масштабируется автоматически на основе трафика.

## <a name="scale-your-api-management-service"></a>Масштабирование службы управления API

![Масштабирование службы управления API в портал Azure](./media/upgrade-and-scale/portal-scale.png)

1. Перейдите к службе управления API в [портал Azure](https://portal.azure.com/).
2. Выберите **расположения** в меню.
3. Щелкните строку с расположением, которое нужно масштабировать.
4. Укажите новое число **единиц** — используйте ползунок или введите число.
5. Нажмите кнопку **Применить**.

## <a name="change-your-api-management-service-tier"></a>Изменение уровня службы управления API

1. Перейдите к службе управления API в [портал Azure](https://portal.azure.com/).
2. Щелкните **ценовую** категорию в меню.
3. Выберите нужный уровень службы из раскрывающегося списка. Используйте ползунок, чтобы указать масштаб службы управления API после изменения.
4. Выберите команду **Сохранить**.

## <a name="downtime-during-scaling-up-and-down"></a>Время простоя при масштабировании вверх и вниз
При масштабировании от или до уровня разработчика будет просто. В противном случае время простоя отсутствует. 


## <a name="next-steps"></a>Дальнейшие действия

- [Развертывание экземпляра службы управления Azure API в различных регионах Azure](api-management-howto-deploy-multi-region.md)
- [Автоматическое масштабирование экземпляра службы управления API Azure](api-management-howto-autoscale.md)
- [Оптимизируйте и сохраняйте расходы на облачные технологии](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)