---
title: Синхронизация нескольких устройств Azure Kinect DK
description: В этой статье рассматриваются преимущества синхронизации с несколькими устройствами, а также настройка устройств для синхронизации.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 02/20/2020
ms.topic: article
keywords: Azure, Kinect, спецификации, оборудование, DK, возможности, глубина, цвет, RGB, иму, массив, глубина, несколько, синхронизация
ms.openlocfilehash: 7c79101de5e5455ae2ff9fd8b5d8369a3832631c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91361166"
---
# <a name="synchronize-multiple-azure-kinect-dk-devices"></a>Синхронизация нескольких устройств Azure Kinect DK

Каждое устройство Azure Kinect DK включает порты синхронизации 3,5-mm (**Синхронизация в** и **синхронизацию**), которые можно использовать для связи нескольких устройств друг с другом. После подключения устройств ваше программное обеспечение может координировать время срабатывания триггера между ними. 

В этой статье описывается подключение и синхронизация устройств.

## <a name="benefits-of-using-multiple-azure-kinect-dk-devices"></a>Преимущества использования нескольких устройств Azure Kinect DK

Существует множество причин использования нескольких устройств Azure Kinect DK, включая следующие:

- Заполните окклусионс. Хотя преобразование данных Azure Kinect DK создает один образ, две камеры (глубина и RGB) на самом деле разбиваются на небольшие расстояния. Смещение делает окклусионс возможным. Перекрытия возникает, когда объект переднего плана блокирует представление части объекта фона для одной из двух камер на устройстве. В полученном цветном изображении объект переднего плана по-видимому применяет тень к объекту фона.  
   Например, на следующей диаграмме на левой стороне камеры виден серый пиксель "P2". Однако белый объект переднего плана блокирует правую ИК-форму камеры на стороне. На правой стороне камеры отсутствуют данные для "P2".  
   ![На схеме показаны две камеры, направленные в одну и ту же точку, и одна из них заблокирована.](./media/occlusion.png)  
   Дополнительные синхронизированные устройства могут предоставлять данные перекрыто.
- Просмотр объектов в трех измерениях.
- Увеличьте эффективность частоты кадров до значения, превышающего 30 кадров в секунду (кадров/с).
- Запишите несколько 4-килобайтных цветовых изображений одной сцены, все из которых выровнены в пределах 100 МКС в &mu; центре экспозиции.
- Увеличьте покрытие камеры в пределах пространства.

## <a name="plan-your-multi-device-configuration"></a>Планирование конфигурации для нескольких устройств

Прежде чем начать, убедитесь, что вы просматриваете [спецификации оборудования Azure KINECT DK](hardware-specification.md) и [КАМЕРУ Azure Kinect DK Depth](depth-camera.md).

### <a name="select-a-device-configuration"></a>Выберите конфигурацию устройства

Для конфигурации устройства можно использовать один из следующих подходов.

- **Конфигурация последовательной цепочки**. Синхронизация одного главного устройства и до восьми подчиненных устройств.  
   ![Схема, в которой показано, как подключить устройства Azure Kinect DK в конфигурации последовательной цепочки.](./media/multicam-sync-daisychain.png)
- **Конфигурация Star**. Синхронизируйте одно основное устройство и до двух подчиненных устройств.  
   ![Схема, на которой показано, как настроить несколько устройств Azure DK в конфигурации типа "звезда".](./media/multicam-sync-star.png)

#### <a name="using-an-external-sync-trigger"></a>Использование внешнего триггера синхронизации

В обеих конфигурациях основное устройство предоставляет сигнал активации для подчиненных устройств. Однако для триггера синхронизации можно использовать пользовательский внешний источник. Например, этот параметр можно использовать для синхронизации захвата образов с другим оборудованием. В конфигурации с последовательной цепочкой или в конфигурации "звезда" источник внешнего триггера подключается к главному устройству.

Источник внешнего триггера должен функционировать так же, как и основное устройство. Он должен доставлять сигнал синхронизации со следующими характеристиками:

- Активный высокий
- Ширина импульса: больше 8 &mu; с
- 5-Я TTL/CMOS
- Максимальная мощность: не менее 8 миллиампс (mA)
- Поддержка частоты: ровно 30 кадров/с, 15 кадров/с и 5 кадров/с (частота сигнала главного вертикальной СИНХРОНИЗАЦИИа цветовой камеры)

Источник триггера должен доставить сигнал в **синхронизацию** главного устройства в порте, используя звуковой кабель 3,5-mm. Вы можете использовать стерео или моно кабель. В Azure Kinect DK все закатайте рукава и кольца соединительного кабеля, а также их причины. Как показано на следующей схеме, устройство получает сигнал синхронизации только из Совета соединителя.

![Конфигурации кабелей для сигнала внешнего триггера](./media/resources/camera-trigger-signal.jpg)

Дополнительные сведения о работе с внешним оборудованием см. в статье [Использование средства записи Azure Kinect с внешними синхронизированными устройствами](record-external-synchronized-units.md) .

### <a name="plan-your-camera-settings-and-software-configuration"></a>Планирование параметров камеры и конфигурации программного обеспечения

Сведения о том, как настроить программное обеспечение для управления камерами и использования данных образа, см. в разделе [пакет SDK для датчика Kinect для Azure](about-sensor-sdk.md).

В этом разделе обсуждаются некоторые факторы, влияющие на синхронизированные устройства (но не на отдельные устройства). Программное обеспечение должно учитывать эти факторы.

#### <a name="exposure-considerations"></a>Рекомендации по экспозиции
Если требуется управлять точным временем каждого устройства, рекомендуется использовать параметр раскрытия вручную. В параметре автоматическая экспозиция Каждая цветовая Камера может динамически изменять реальную раскрытие. Так как экспозиция влияет на время, такие изменения быстро отправляют камеры из синхронизации.

В цикле записи образа следует избегать неоднократного задания того же параметра экспозиции. Вызывайте API только один раз, когда он потребуется.

#### <a name="avoiding-interference-between-multiple-depth-cameras"></a>Предотвращение помех между несколькими камерами глубины

Если несколько камер глубины представляют собой перекрывающиеся поля представления, каждая камера должна иметь изображение с соответствующей лазерной камеры. Чтобы предотвратить помехи друг другу лучами, записи камеры должны быть смещены друг от друга на 160 μс или более.

Для каждой записи на камере глубина лазерного принтера включает девять раз и активна только 125 &mu; s раз. В зависимости от режима работы лазерный принтер будет бездействует в 14505 &mu; s или 23905 &mu; s. Такое поведение означает, что начальная точка для вычисления смещения — 125 &mu; s.

Кроме того, различия между часами камеры и часами встроенного по устройства увеличивают минимальное смещение до 160 &mu; с. Чтобы вычислить более точное смещение конфигурации, обратите внимание на режим глубины, который вы используете, и обратитесь к таблице временной шкалы " [необработанный датчик глубины](hardware-specification.md#depth-sensor-raw-timing)". Используя данные из этой таблицы, можно вычислить минимальное смещение (время выдержки каждой камеры), используя следующее уравнение:

> *Время выдержки* = (*IR Pulses* &times; *Ширина импульса*ИК-импульса) + (время простоя в*периодах бездействия* &times; *Idle Time*)

При использовании смещения 160 &mu; s можно настроить до девяти камер глубины, чтобы каждый лазер включится во время бездействия других лучами.

В программном обеспечении используйте ```depth_delay_off_color_usec``` или, ```subordinate_delay_off_master_usec``` чтобы убедиться, что каждый IR-лазер срабатывает в своем собственном &mu; окне 160 s или имеет другое поле представления.

## <a name="prepare-your-devices-and-other-hardware"></a>Подготовка устройств и другого оборудования

Помимо нескольких устройств Azure Kinect DK, для поддержки конфигурации, которую требуется собрать, может потребоваться получить дополнительные узлы и другое оборудование. Используйте сведения в этом разделе, чтобы убедиться, что все устройства и оборудование готовы перед началом настройки.

### <a name="azure-kinect-dk-devices"></a>Устройства Azure Kinect DK

Для каждого устройства Azure Kinect DK, которое необходимо синхронизировать, выполните следующие действия.

- Убедитесь, что на устройстве установлена последняя версия встроенного по. Дополнительные сведения об обновлении устройств см. в [статье обновление встроенного по Azure KINECT DK](update-device-firmware.md). 
- Удалите обложку устройства, чтобы отобразить порты синхронизации.
- Запишите серийный номер для каждого устройства. Этот номер будет использоваться позже в процессе установки.

### <a name="host-computers"></a>Ведущие компьютеры

Как правило, каждый Azure Kinect DK использует собственный главный компьютер. Можно использовать выделенный контроллер узла в зависимости от способа использования устройства и объема данных, передаваемых через USB-подключение. 

Убедитесь, что на каждом узле установлен пакет SDK для датчика Kinect для Azure. Дополнительные сведения об установке пакета SDK для датчика см. в статье [Краткое руководство. Настройка Azure KINECT DK](set-up-azure-kinect-dk.md). 

#### <a name="linux-computers-usb-memory-on-ubuntu"></a>Компьютеры Linux: память USB на Ubuntu

По умолчанию хост-компьютеры под управлением Linux выделяют контроллеру USB только 16 МБ памяти ядра для передачи данных по USB. Обычно этот объем достаточно велик для поддержки одного Azure Kinect DK. Однако для поддержки нескольких устройств USB-контроллеру требуется больше памяти. Чтобы увеличить объем памяти, выполните следующие действия.

1. Edit,**etc, default/grub**.
1. Найдите следующую строку:
   ```cmd
   GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
   ```
   Замените его, используя следующую строку:
   ```cmd
   GRUB_CMDLINE_LINUX_DEFAULT="quiet splash usbcore.usbfs_memory_mb=32"
   ```
   > [!NOTE]  
   > Эти команды устанавливают для USB-памяти значение 32 МБ. Это пример параметра в два раза больше значения по умолчанию. Можно задать гораздо большее значение, соответствующее вашему решению.
1. Выполните команду **sudo Update-GRUB**.
1. Перезагрузите компьютер.

### <a name="cables"></a>Кабели

Чтобы подключить устройства друг к другу и к основным компьютерам, необходимо использовать кабели «штекер-штекер» 3,5-мм (также известный как «звуковой кабель 3,5-мм»). Длина кабелей должна быть меньше 10 метров и может быть стерео или Mono.

Количество используемых кабелей зависит от количества используемых устройств, а также от конкретной конфигурации устройства. В поле Azure Kinect DK не включены кабели. Их необходимо приобрести отдельно.

При подключении устройств в конфигурации "звезда" необходимо также иметь один разделитель наушников.

## <a name="connect-your-devices"></a>Подключение устройств

**Подключение устройств Azure Kinect DK в конфигурации последовательной цепочки**

1. Подключите каждый Azure Kinect DK к Power.
1. Подключите каждое устройство к главному компьютеру. 
1. Выберите одно устройство для главного устройства и подключите звуковой кабель 3,5-mm к его порту **синхронизации** .
1. Подключите другой конец кабеля к **синхронизации в** порте первого подчиненного устройства.
1. Чтобы подключить другое устройство, подключите другой кабель к порту **out** первого подчиненного устройства и **синхронизируйте его с** портом следующего устройства.
1. Повторите предыдущий шаг, пока не будут подключены все устройства. Последнее устройство должно иметь только одно Кабельное подключение. Его порт **синхронизации** должен быть пустым.

**Подключение устройств Azure Kinect DK в конфигурации типа "звезда"**

1. Подключите каждый Azure Kinect DK к Power.
1. Подключите каждое устройство к главному компьютеру. 
1. Выберите одно устройство, чтобы оно было главным, и подключите один конец разделителя наушников к порту **синхронизации** .
1. Подключите аудиоразъемы 3,5-mm к концу "расщепления" разделителя наушников.
1. Подключите другой конец каждого кабеля к **синхронизации через** порт одного из подчиненных устройств.

## <a name="verify-that-the-devices-are-connected-and-communicating"></a>Убедитесь, что устройства подключены и обмениваются данными

Чтобы убедиться, что устройства подключены правильно, используйте [средство просмотра Kinect для Azure](azure-kinect-viewer.md). При необходимости повторите эту процедуру, чтобы протестировать каждое подчиненное устройство в сочетании с главным устройством.

> [!IMPORTANT]  
> Для этой процедуры необходимо получить серийный номер каждого Azure Kinect DK.

1. Откройте два экземпляра средства просмотра Kinect Azure.
1. В разделе **открыть устройство**выберите серийный номер подчиненного устройства, которое требуется протестировать.  
   ![Открыть устройство](./media/open-devices.png)
   > [!IMPORTANT]  
   > Чтобы получить точную рассогласование захвата образа между всеми устройствами, необходимо запустить главное устройство последним.  
1. В разделе **Внешняя синхронизация**выберите **вложенный**.  
   ![Начало подчиненной камеры](./media/sub-device-start.png)
1.  Щелкните **Запуск**.  
    > [!NOTE]  
    > Так как это подчиненное устройство, средство просмотра Azure Kinect Viewer не отображает образ после запуска устройства. Изображение не отображается, пока подчиненное устройство не получит сигнал синхронизации от главного устройства.
1. После запуска подчиненного устройства используйте другой экземпляр средства просмотра Azure Kinect Viewer, чтобы открыть основное устройство.
1. В разделе **Внешняя синхронизация**выберите **master**.
1. Щелкните **Запуск**.

При запуске главного устройства Azure Kinect в обоих экземплярах средства просмотра Azure Kinect должны отображаться образы.

## <a name="calibrate-the-devices-as-a-synchronized-set"></a>Калибровка устройств как синхронизированного набора

Убедившись, что устройства обмениваются данными правильно, вы можете откалибровать их для создания образов в одном домене.

На одном устройстве камеры с глубиной и RGB калибровка загружаются для совместной работы. Однако, если несколько устройств должны работать вместе, их необходимо откалибровать, чтобы определить, как преобразовать образ из домена камеры, который был захвачен в домен камеры, которую вы хотите использовать для обработки образов.

Существует несколько вариантов перекрестной калибровки устройств. Корпорация Майкрософт предоставляет [пример кода зеленого экрана GitHub](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/green_screen), который использует метод opencv. В файле readme для этого примера кода содержатся дополнительные сведения и инструкции по калибровке устройств.

Дополнительные сведения о калибровке см. в статье [Использование функций калибровки Kinect для Azure](use-calibration-functions.md).

## <a name="next-steps"></a>Дальнейшие шаги

После настройки синхронизированных устройств можно также узнать, как использовать
> [!div class="nextstepaction"]
> [API-интерфейс для записи и воспроизведения пакета SDK для датчика Kinect Azure](record-playback-api.md)

## <a name="related-topics"></a>Связанные темы

- [Сведения о пакете SDK для датчика Kinect для Azure](about-sensor-sdk.md)
- [Спецификации оборудования для Azure Kinect DK](hardware-specification.md) 
- [Краткое руководство. Настройка Azure Kinect DK](set-up-azure-kinect-dk.md) 
- [Обновление встроенного по Azure Kinect DK](update-device-firmware.md) 
- [Сброс Azure Kinect DK](reset-azure-kinect-dk.md) 
- [Средство просмотра Kinect Azure](azure-kinect-viewer.md) 
