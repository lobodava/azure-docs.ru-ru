---
title: Вычисления оценки AVS в службе "миграция Azure" | Документация Майкрософт
description: Содержит обзор вычислений оценки AVS в службе "миграция Azure".
author: rashi-ms
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/25/2020
ms.author: mahain
ms.openlocfilehash: 400c2d91383b5f21fcd40fdbbe279bd83fcef51a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91576546"
---
# <a name="server-assessment-overview-migrate-to-azure-vmware-solution"></a>Обзор оценки сервера (миграция в решение VMware для Azure)

Служба " [Миграция Azure](migrate-services-overview.md) " предоставляет центральный концентратор для наблюдения за обнаружением, оценкой и миграцией локальных приложений и рабочих нагрузок. Он также отслеживает экземпляры частных и общедоступных облачных служб в Azure. Центр предлагает средства миграции Azure для оценки и миграции, а также предложения независимых поставщиков программного обеспечения (ISV) сторонних разработчиков.

Оценка сервера — это средство в службе "миграция Azure", которое оценивает локальные серверы для миграции на виртуальные машины Azure IaaS и решение Azure VMware (AVS). В этой статье содержатся сведения о вычислении оценок решения Azure VMware (AVS).

> [!NOTE]
> Оценка решения Azure VMware (AVS) сейчас доступна в предварительной версии и может быть создана только для виртуальных машин VMware.

## <a name="types-of-assessments"></a>Типы оценок

Оценки, создаваемые с помощью оценки сервера, — это моментальный снимок данных на момент времени. Существует два типа оценки, которые можно создать с помощью средства "Оценка серверов" службы "Миграция Azure".

**Тип оценки** | **Сведения**
--- | --- 
**Azure** | Оценки для переноса локальных серверов на виртуальные машины Azure. <br/><br/> Вы можете оценить локальные [виртуальные машины VMware](how-to-set-up-appliance-vmware.md), [виртуальные машины Hyper-V](how-to-set-up-appliance-hyper-v.md)и [физические серверы](how-to-set-up-appliance-physical.md) для миграции в Azure с помощью этого типа оценки. Дополнительные [сведения](concepts-assessment-calculation.md)
**Решение Azure VMware (AVS)** | Оценки для переноса локальных серверов в [Решение Azure VMware (AVS)](../azure-vmware/introduction.md). <br/><br/> С помощью этого типа оценки вы можете оценить возможность миграции на Решение Azure VMware (AVS) для локальных [виртуальных машин VMware](how-to-set-up-appliance-vmware.md).[Подробнее](concepts-azure-vmware-solution-assessment-calculation.md)

Оценка решения Azure VMware (AVS) в оценке сервера предоставляет два варианта критериев изменения размера:

**Оценка** | **Сведения** | **Data**
--- | --- | ---
**На основе производительности** | Оценки на основе собранных данных о производительности локальных виртуальных машин. | **Рекомендуемый размер узла**: на основе данных об использовании ЦП и памяти вместе с типом узла, типом хранилища и параметром ФТТ, выбранным для оценки.
**Как в локальной среде** | Оценки на основе определения размера в локальной среде. | **Рекомендуемый размер узла**: на основе размера локальной виртуальной машины, а также типа узла, типа хранилища и параметра ФТТ, выбранного для оценки.

## <a name="how-do-i-run-an-assessment"></a>Разделы справки выполнить оценку?

Существует несколько способов выполнения оценки.

- Оценка компьютеров с помощью метаданных сервера, собранных упрощенным устройством миграции Azure. Устройство обнаруживает локальные компьютеры. Затем он отправляет метаданные и данные о производительности компьютера в службу "миграция Azure".
- Оценка компьютеров с помощью метаданных сервера, импортированных в формате значений с разделителями-запятыми (CSV).

## <a name="how-do-i-assess-with-the-appliance"></a>Разделы справки оценить с помощью устройства?

Если вы развертываете устройство миграции Azure для обнаружения локальных серверов, выполните следующие действия.

1. Настройте Azure и локальную среду для работы с оценкой сервера.
2. Для первой оценки создайте проект Azure и добавьте в него средство оценки сервера.
3. Разверните упрощенное устройство миграции Azure. Устройство постоянно обнаруживает локальные компьютеры и отправляет метаданные и данные о производительности компьютера в службу "миграция Azure". Разверните устройство как виртуальную машину. Вам не нужно ничего устанавливать на компьютерах, которые вы хотите оценить.

После того как устройство начнет обнаружение компьютера, можно собрать компьютеры, которые нужно оценить в группу, и выполнить оценку группы с типом оценки **Azure VMware Solution (AVS)**.

Создайте первую оценку решения Azure VMware (AVS), выполнив действия, описанные [здесь](how-to-create-azure-vmware-solution-assessment.md).

## <a name="how-do-i-assess-with-imported-data"></a>Разделы справки оценить с помощью импортированных данных?

Если вы оцениваете серверы с помощью CSV-файла, устройство не требуется. Вместо этого выполните следующие действия.

1. Настройте Azure для работы с оценкой сервера.
2. Для первой оценки создайте проект Azure и добавьте в него средство оценки сервера.
3. Скачайте шаблон CSV-файла и добавьте в него данные сервера.
4. Импортируйте шаблон в средство оценки серверов.
5. Откройте серверы, добавленные с помощью импорта, соберите их в группу и выполните оценку для группы с типом оценки **Azure VMware Solution (AVS)**.

## <a name="what-data-does-the-appliance-collect"></a>Какие данные собираются устройством?

Если вы используете устройство "миграция Azure" для оценки, ознакомьтесь с метаданными и данными о производительности, собранными для [VMware](migrate-appliance.md#collected-data---vmware).

## <a name="how-does-the-appliance-calculate-performance-data"></a>Как устройство вычисляет данные о производительности?

Если вы используете устройство для обнаружения, оно собирает данные о производительности для параметров вычислений, выполнив следующие действия.

1. Устройство собирает образец точки в режиме реального времени.

    - **Виртуальные машины VMware**. выборка собирается каждые 20 секунд.

2. Устройство объединяет образцы точек для создания одной точки данных каждые 10 минут. Чтобы создать точку данных, устройство выбирает пиковые значения из всех выборок. Затем она отправляет точку данных в Azure.
3. В ходе оценки сервера хранятся все 10-минутные точки данных за прошлый месяц.
4. При создании оценки Серверная Оценка определяет соответствующую точку данных, которая будет использоваться для оптимизации дается задание. Идентификация основана на значениях процентиля для *журнала производительности* и *использования процентилей*.

    - Например, если журнал производительности составляет одну неделю, а заданный процентиль — 95 процентиль процентиль, то оценка сервера сортирует 10-минутные точки выборки за последнюю неделю. Он сортирует их в возрастающем порядке и выбирает значение 95 процентиль процентиль для оптимизации дается задание.
    - Значение 95 процентиль процентиль гарантирует игнорирование любых выбросов, которые могут быть добавлены, если вы выбрали 99-м процентиль.
    - Если вы хотите выбрать пиковое использование для периода и не хотите пропускать выбросы, выберите 99-м процентиль для использования процентилей.

5. Это значение умножается на эффективный фактор, чтобы получить эффективные данные об использовании производительности для метрик, собираемых устройством:

    - загрузка ЦП;
    - Использование ОЗУ
    - Дисковые операции ввода-вывода (чтение и запись)
    - Пропускная способность диска (чтение и запись)
    - Пропускная способность сети (в и в)

Следующие данные о производительности собираются, но не используются в рекомендациях по размерам для оценки AVS:
  - Данные дискового ввода-вывода и пропускной способности для каждого диска, подключенного к виртуальной машине.
  - Сетевой ввод-вывод для управления размером на основе производительности для каждого сетевого адаптера, подключенного к виртуальной машине.

## <a name="how-are-avs-assessments-calculated"></a>Как рассчитываются оценки AVS?

При оценке сервера для вычисления оценок используются метаданные и данные производительности локальных компьютеров. При развертывании устройства "миграция Azure" для оценки используются данные, собираемые устройством. Но если вы выполняете оценку, импортированную с помощью CSV-файла, вы предоставляете метаданные для вычисления.

Вычисления выполняются в следующих трех этапах:

1. **Вычислите готовность решения VMware для Azure (AVS)**: какие локальные виртуальные машины подходят для миграции в решение Azure VMware (AVS).
2. **Вычислите количество узлов и использование экземпляров AVS на разных узлах**: предполагаемое количество узлов AVS, необходимых для запуска виртуальных машин и планируемого использования ресурсов ЦП, памяти и хранилища на всех узлах.
3. **Ежемесячная оценка затрат**: приблизительные ежемесячные затраты на все узлы решения Azure VMware (AVS), на которых выполняются локальные виртуальные машины.

Вычисления выполняются в предыдущем порядке. Сервер машины переходит на более позднюю стадию, только если он проходит предыдущее. Например, если сервер не проходит этап готовности AVS, он помечается как неподходящий для Azure. Расчет размера и затрат для этого сервера не выполняется

## <a name="whats-in-an-azure-vmware-solution-avs-assessment"></a>Что такое оценка решения Azure VMware (AVS)?

Ниже приведена именно, входящая в оценку AVS при оценке сервера:


| **Свойство** | **Сведения** 
| - | - 
| **Целевое расположение** | Указывает расположение частного облака AVS, в которое требуется выполнить миграцию.<br/><br/> Оценка "AVS" в настоящее время оценка серверов поддерживает следующие целевые регионы: Восточная часть США, Западная Европа, Западная часть США. 
| **Тип хранилища** | Указывает подсистему хранения, используемую в AVS.<br/><br/> Оценки AVS поддерживают только vSAN в качестве типа хранилища по умолчанию. 
**Зарезервированные экземпляры (RIs)** | Это свойство помогает указать зарезервированные экземпляры в AVS. Сейчас службы RIs не поддерживаются для узлов AVS. 
**Тип узла** | Указывает [тип узла AVS](../azure-vmware/concepts-private-clouds-clusters.md) , используемый для отображения локальных виртуальных машин. Тип узла по умолчанию — AV36. <br/><br/> Служба "миграция Azure" рекомендует для виртуальных машин, которые будут перенесены в AVS, необходимое число узлов. 
**Параметр ФТТ, уровень RAID** | Указывает применимую ошибку для комбинаций и томов RAID. Выбранный параметр ФТТ в сочетании с требованиями к локальному диску виртуальной машины определит общее хранилище vSAN, необходимое для AVS. 
**Условия определения размера** | Задает критерии для использования в качестве критериев для виртуальных машин, *Размер* которых составляет AVS. Вы можете выбрать размер на *основе производительности* или *локально,* не учитывая историю производительности. 
**Журнал производительности** | Задает продолжительность, которую следует учитывать при оценке данных производительности компьютеров. Это свойство применимо, только если критерии изменения размера *основаны на производительности*. 
**Использование процентиля** | Указывает значение процентиля для набора данных "выборка производительности", которое будет учитываться при правильном изменении размера. Это свойство применимо только в том случае, если размер зависит от производительности.
**Фактор комфорта** | При оценке сервера "миграция Azure" в процессе оценки учитывается буфер (недостаточный коэффициент). Этот буфер применяется на основе данных об использовании компьютера для виртуальных машин (показателей ЦП, памяти, диска и сети). Фактор комфорта учитывается, например, для сезонного использования и малого количества записей в журнале с потенциальным повышением в будущем.<br/><br/> Например, если виртуальная машина с 10 ядрами загружена на 20 %, обычно в результате оценки определяется виртуальная машина с двумя ядрами. Но с фактором комфорта 2.0x в результате определяется машина с 4 ядрами. 
**Предложение** | Отображает [предложение Azure](https://azure.microsoft.com/support/legal/offer-details/) , в котором вы зарегистрированы. Служба "Миграция Azure" оценивает стоимость соответствующим образом.
**Валюта** | Показывает валюту выставления счетов для вашей учетной записи. 
**Скидка (%)** | Список всех скидок, относящихся к подписке, которые вы получаете поверх предложения Azure. Значение по умолчанию — 0 %. 
**Преимущество гибридного использования Azure** | Указывает, есть ли у вас программа Software Assurance и что она подходит для [преимущество гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Несмотря на то, что оно не влияет на цены на решения VMware для Azure в соответствии с ценой на основе узлов, клиенты по-прежнему могут применять локальные лицензии ОС (на основе Майкрософт) в AVS, используя преимущества гибридного использования Azure. Другим поставщикам программной ОС потребуется предоставить собственные условия лицензирования, например RHEL. 
**Виртуальных ЦП превышение лимита подписки** | Указывает соотношение числа виртуальных ядер, привязанных к одному физическому ядру в узле AVS. Значение по умолчанию для вычислений — 4 виртуальных ЦП: 1 физическое ядро в AVS. <br/><br/> Пользователи API могут задать это значение как целое число. Обратите внимание, что виртуальных ЦП "превышение лимита подписки" > 4:1 может повлиять на рабочие нагрузки в зависимости от использования ЦП. 


## <a name="azure-vmware-solution-avs-suitability-analysis"></a>Анализ пригодности для решения VMware Azure (AVS)

Оценки AVS в оценке сервера оценивают каждую локальную виртуальную машину на предмет пригодности для AVS, просмотрев свойства компьютера. Он также назначает каждому компьютеру с оценкой одну из следующих категорий пригодности:

- **Готово**к установке AVS. компьютер можно перенести как есть в Azure (AVS) без каких-либо изменений. Он будет запущен в AVS с полной поддержкой AVS.
- **Готовность к работе**. возможно, у виртуальной машины есть проблемы совместимости с текущей версией vSphere, а также требуются средства VMware и другие параметры, прежде чем можно будет достичь полной функциональности виртуальной машины в AVS.
- **Не готово для AVS**: виртуальная машина не запустится в AVS. Например, если в локальной виртуальной машине VMware имеется подключенное внешнее устройство, например компакт-диск, то операция VMware VMotion завершится неудачей (при использовании VMware VMotion).
- **Неизвестная готовность**: службе миграции Azure не удалось определить готовность компьютера из-за недостатка метаданных, собранных из локальной среды.

Оценка сервера проверяет свойства компьютера, чтобы определить готовность Azure к работе с локальным компьютером.

### <a name="machine-properties"></a>Свойства виртуальной машины

Оценка сервера проверяет следующее свойство локальной виртуальной машины, чтобы определить, можно ли ее запустить в решении Azure VMware (AVS).


| **Свойство** | **Сведения** | **Состояние готовности AVS** 
| - | - | - 
| **Протокол Интернета** | AVS в настоящее время не поддерживает адресацию по IPv6.<br/><br/> Обратитесь к местной группе MSFT AVS GBB, чтобы получить рекомендации по исправлению ошибок, если для вашей виртуальной машины определяется поддержка IPv6.| Условно готовый протокол IP


### <a name="guest-operating-system"></a>Операционная система на виртуальной машине

В настоящее время оценки AVS не просматривают операционную систему в ходе анализа пригодности. Все операционные системы, работающие на локальных виртуальных машинах, скорее всего, будут работать в решении Azure VMware (AVS).

Наряду со свойствами виртуальной машины, Служба оценки серверов просматривает операционную систему на виртуальных машинах, чтобы определить, может ли она работать в Azure.


## <a name="sizing"></a>Определение размера

После того как компьютер помечается как готовый для AVS, оценка числа AVS в оценке сервера делает рекомендации по выбору размера узла, которые позволяют определить необходимые требования к локальной виртуальной машине и найти общее число требуемых узлов AVS. Эти рекомендации различаются в зависимости от указанных свойств оценки.

- Если для оценки используется *Размер на основе производительности*, служба "миграция Azure" считает журнал производительности компьютера, чтобы сделать соответствующую рекомендацию по изменению размера для AVS. Этот метод особенно полезен при чрезмерном выделении локальной виртуальной машины, но при этом используется низкий уровень использования, и для экономии издержек нужно щелкнуть правой кнопкой мыши размер виртуальной машины в AVS. Этот метод поможет вам оптимизировать размеры во время миграции.
- Если вы не хотите учитывать данные о производительности для изменения размера виртуальной машины и хотите перевести локальные компьютеры в режим "без изменений" на "AVS", вы можете установить критерии изменения размера *как локальные*. Затем Оценка сервера будет масштабировать виртуальные машины на основе локальной конфигурации без учета данных об использовании. 


### <a name="ftt-sizing-parameters"></a>Параметры размера ФТТ

Подсистема хранилища, используемая в AVS, — это vSAN. политики хранилища vSAN определяют требования к хранилищу для виртуальных машин. Они гарантируют требуемый уровень обслуживания для виртуальных машин, так как определяют способ выделения ресурсов хранилища для виртуальной машины. Доступные сочетания FTT-Raid: 

**Отказоустойчивость (FTT)** | **Конфигурация RAID** | **Минимальное требуемое число узлов** | **Рекомендации по размеру**
--- | --- | --- | --- 
1 | RAID-1 (зеркальное отображение) | 3 | Виртуальная машина размером 100 ГБ будет потреблять 200 ГБ.
1 | RAID-5 (удаляющее кодирование) | 4 | Виртуальная машина размером 100 ГБ будет потреблять 133,33 ГБ.
2 | RAID-1 (зеркальное отображение) | 5 | Виртуальная машина размером 100 ГБ будет потреблять 300 ГБ.
2 | RAID-6 (удаляющее кодирование) | 6 | Виртуальная машина размером 100 ГБ будет потреблять 150 ГБ.
3 | RAID-1 (зеркальное отображение) | 7 | Виртуальная машина размером 100 ГБ будет потреблять 400 ГБ.

### <a name="performance-based-sizing"></a>Выбор размера на основе производительности

При изменении размера на основе производительности, Оценка сервера начинается с дисков, подключенных к виртуальной машине, за которыми следуют сетевые адаптеры. Затем требования к виртуальной машине сопоставляются с соответствующим числом узлов для AVS. Устройство "миграция Azure" проводит профили в локальной среде для получения данных о производительности ЦП, памяти, дисков и сетевого адаптера.

**Этапы сбора данных о производительности.**

1. Для виртуальных машин VMware устройство миграции Azure собирает образец точки в режиме реального времени каждые 20 секунд. 
2. Устройство выполняет сведение образцов точек, собираемых каждые 10 минут, и отправляет максимальное значение за последние 10 минут до оценки сервера.
3. В ходе оценки сервера хранятся все 10-минутные точки выборки за последний месяц. Затем, в зависимости от свойств оценки, заданных для *журнала производительности* и *использования процентилей*, он определяет соответствующую точку данных для использования при правильном размере. Например, если для журнала производительности задано значение 1 день, а в качестве показателя использования процентиля является 95 процентиль процентиль, то Серверная Оценка использует 10-минутные точки выборки за последний день, сортирует их в возрастающем порядке и выбирает значение 95 процентиль процентиля для правильного изменения размера.
4. Это значение умножается на эффективный фактор, чтобы получить эффективные данные об использовании производительности для каждой метрики (загрузка ЦП, использование памяти, операции ввода-вывода на диск (чтение и запись), пропускная способность диска (чтение и запись), а также пропускная способность сети (in и out), собираемой устройством.

После определения фактического использования значение размера хранилища, сети и вычислений обрабатывается следующим образом.

**Размер хранилища**. Служба "миграция Azure" использует общее дисковое пространство локальной виртуальной машины в качестве параметра вычисления, чтобы определить требования к хранилищу vSAN AVS в дополнение к параметру ФТТ, выбранному пользователем. ФТТ — сбои для допуска, а также требование минимального числа узлов на ФТТ, определяет общее хранилище vSAN, необходимое в сочетании с требованием диска виртуальной машины.

**Определение размера сети:** инструмент "Оценка сервера" в настоящее время не учитывает параметры сети при выполнении оценок для Решения AVS.

**Вычисление размера**. После вычисления требований к хранилищу Серверная Оценка считает требования к ЦП и памяти для определения количества узлов, необходимых для AVS на основе типа узла.

- Основываясь на критериях изменения размера, серверная Оценка проверяет либо данные виртуальной машины на основе производительности, либо локальную конфигурацию виртуальной машины. Параметр "комфортный фактор" позволяет указать коэффициент роста кластера. В настоящее время технология Hyper-Threading включена, поэтому узлы с 36 ядрами будут иметь 72 виртуальных ядра. Значение в 4 виртуальных ядра на физический компьютер используется для определения пороговых значений ЦП одного кластера с использованием стандарта VMware по недопущению превышения загрузки в 80 %, что позволяет выполнять обслуживание или обрабатывать ошибки без снижения уровня доступности кластера. В настоящее время переопределение недоступно для изменения значений превышения лимита подписки, и это может быть в будущих версиях.

### <a name="as-on-premises-sizing"></a>Определение размера как для локальной виртуальной машины

Если используется *как локальное изменение размера*, то при оценке сервера не учитывается журнал производительности виртуальных машин и дисков. Вместо этого он выделяет узлы AVS на основе размера, выделенного в локальной среде. Тип хранилища по умолчанию — vSAN в AVS.

## <a name="confidence-ratings"></a>Оценки достоверности

Каждая оценка на основе производительности в службе "миграция Azure" связана с рейтингом достоверности, который находится в диапазоне от одного (низший) до пяти звезд (самый высокий).

- Оценка достоверности назначается на основе доступности точек данных, необходимых для вычисления оценки.
- Оценка достоверности помогает определить надежность рекомендаций по выбору размера, предоставленных службой "Миграция Azure".
- Оценки достоверности не применяются *в качестве локальных* оценок.
- Для изменения размера на основе производительности для оценки AVS в оценке сервера требуются данные об использовании ЦП и памяти виртуальной машины. Следующие данные собираются, но не используются в рекомендациях по размерам для AVS:
  - Данные дискового ввода-вывода и пропускной способности для каждого диска, подключенного к виртуальной машине.
  - Сетевой ввод-вывод для управления размером на основе производительности для каждого сетевого адаптера, подключенного к виртуальной машине.

  Если любое из этих номеров использования недоступно в vCenter Server, рекомендация по размеру может быть ненадежной.

В зависимости от количества доступных точек данных Оценка достоверности для оценки выглядит следующим образом.


| **Уровень доступности точек данных** | **Оценка достоверности** |
| - | - |
| 0-20% | 1 звезда |
| 21-40% | 2 звезды |
| 41-60% | 3 звезды |
| 61-80% | 4 звезды |
| 81-100% | 5 звезд |

### <a name="low-confidence-ratings"></a>Низкая оценка достоверности

Ниже приведены несколько причин, по которым оценка может иметь низкую степень достоверности.

- Вы не проделали Профилирование среды в течение времени, в течение которого вы создаете оценку. Например, если вы создаете оценку с длительностью производительности в один день, необходимо подождать не менее одного дня после начала обнаружения всех точек данных для сбора.
- В течение периода, для которого рассчитывалась оценка, некоторые виртуальные машины были отключены. Если какие бы то ни было виртуальные машины отключаются в течение некоторого времени, при оценке сервера не удается получить данные о производительности за этот период.
- Некоторые виртуальные машины были созданы в течение периода, для которого была рассчитана оценка. Например, если вы создали оценку для журнала производительности за прошлый месяц, но некоторые виртуальные машины были созданы в среде только неделю назад, журнал производительности новых виртуальных машин не будет существовать в течение всего времени.

> [!NOTE]
> Если оценка достоверности для какой-либо оценки меньше пяти звезд, рекомендуется подождать по крайней мере один день, чтобы устройство пропрофилировать среду, а затем повторно вычислить оценку. В противном случае размер на основе производительности может быть ненадежным. В этом случае рекомендуется переключить оценку на локальное изменение размера.

## <a name="monthly-cost-estimation"></a>Примерная ежемесячная стоимость

После того как рекомендации по изменению размера завершены, служба "миграция Azure" вычисляет общую стоимость выполнения локальных рабочих нагрузок в AVS, умножая число узлов AVS, необходимых для цены узла. Стоимость каждой виртуальной машины вычисляется путем деления общей стоимости на число виртуальных машин в оценке. 
- Вычисление принимает необходимое количество узлов, тип узла и расположение.
- В нем суммируются затраты на все узлы, чтобы вычислить общую месячную стоимость.
- Цены отображаются в валюте, заданной в настройках оценки.

Так как ценообразование для решения Azure VMware (AVS) относится к каждому узлу, Общая стоимость не имеет стоимости вычислений и распределения затрат на хранение. [Подробнее](../azure-vmware/introduction.md)

Обратите внимание, что как решение Azure VMware (AVS) находится на этапе предварительной версии, цены на узлы в оценке являются ценами на предварительную версию. Обратитесь к локальной группе MSFT AVS ГББ для получения инструкций.

## <a name="migration-tool-guidance"></a>Руководство по средствам миграции

В отчете о готовности к работе в Azure для оценки Решения Azure VMware приведены следующие рекомендуемые инструменты: 
- **VMware хккс или Enterprise**: для виртуальных машин VMware — это рекомендуемое средство миграции, которое позволяет перенести локальную рабочую нагрузку в частное облако решения Azure VMware (AVS). [Подробнее](../azure-vmware/tutorial-deploy-vmware-hcx.md)
- **Неизвестно**: для виртуальных машин, импортированных с помощью CSV-файла, инструмент миграции по умолчанию неизвестен. Однако для компьютеров VMware рекомендуется использовать решение гибридного облака VMware (ХККС).

## <a name="next-steps"></a>Дальнейшие шаги

Создайте оценку [виртуальных машин VMware для AVS](how-to-create-azure-vmware-solution-assessment.md).
