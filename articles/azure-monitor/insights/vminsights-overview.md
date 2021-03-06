---
title: Общие сведения об Azure Monitor для виртуальных машин
description: Обзор Azure Monitor для виртуальных машин, который отслеживает работоспособность и производительность виртуальных машин Azure и автоматически обнаруживает и сопоставляет компоненты приложений и их зависимости.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2020
ms.openlocfilehash: e5eaf2d7075ca09aeb3cfaa2dfea81fd0f8d65ad
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685313"
---
# <a name="overview-of-azure-monitor-for-vms"></a>Общие сведения об Azure Monitor для виртуальных машин

Azure Monitor для виртуальных машин отслеживает производительность и работоспособность виртуальных машин и масштабируемых наборов виртуальных машин, включая выполняющиеся процессы и зависимости от других ресурсов. Это помогает обеспечить предсказуемую производительность и доступность важнейших приложений за счет выявления узких мест производительности и сетевых проблем, а также помогает понять, связана ли проблема с другими зависимостями.

Azure Monitor для виртуальных машин поддерживает следующие операционные системы Windows и Linux:

- Виртуальные машины Azure
- Масштабируемые наборы виртуальных машин Azure
- Гибридные виртуальные машины, подключенные к службе "Дуга Azure"
- Локальные виртуальные машины
- Виртуальные машины, размещенные в другой облачной среде
  

Azure Monitor для виртуальных машин сохраняет свои данные в журналах Azure Monitor, что позволяет ему обеспечивать эффективную статистическую обработку и фильтрацию и анализировать тенденции данных с течением времени. Вы можете просмотреть эти данные на одной ВИРТУАЛЬНОЙ машине непосредственно из виртуальной машины или использовать Azure Monitor для предоставления агрегированного представления нескольких виртуальных машин.

![Перспектива полезных сведений о виртуальной машине на портале Azure](media/vminsights-overview/vminsights-azmon-directvm.png)


## <a name="pricing"></a>Цены
Нет прямой оплаты за Azure Monitor для виртуальных машин, но плата взимается за ее деятельность в Log Analytics рабочей области. Плата в Azure Monitor для виртуальных машин выставляется за следующие элементы (на основе цен, опубликованных на [странице цен на Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/)):

- Данные, полученные от агентов и хранимые в рабочей области.
- Данные о состоянии работоспособности, собранные из работоспособности гостя (Предварительная версия)
- Правила генерации оповещений на основе данных журнала и работоспособности.
- Уведомления, отправленные из правил генерации оповещений.

Размер журнала зависит от длины строк счетчиков производительности и может увеличиваться с учетом количества логических дисков и сетевых адаптеров, выделенных для виртуальной машины. Если вы уже используете Сопоставление служб, единственное изменение, которое вы увидите, — это дополнительные данные о производительности, которые отправляются в Azure Monitorный `InsightsMetrics` тип данных.


## <a name="configuring-azure-monitor-for-vms"></a>Настройка Azure Monitor для виртуальных машин
Ниже приведены действия по настройке Azure Monitor для виртуальных машин. Чтобы получить подробные инструкции по каждому шагу, выполните следующие действия.

- [Создайте рабочую область Log Analytics.](vminsights-configure-workspace.md#create-log-analytics-workspace)
- [Добавьте решение Вминсигхтс в рабочую область.](vminsights-configure-workspace.md#add-vminsights-solution-to-workspace)
- [Установите агенты на виртуальной машине и масштабируемом наборе виртуальных машин для мониторинга.](vminsights-enable-overview.md)



## <a name="next-steps"></a>Дальнейшие действия

- Требования и методы, обеспечивающие мониторинг виртуальных машин, см. в статье [развертывание Azure Monitor для виртуальных машин](vminsights-enable-overview.md) .

