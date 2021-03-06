---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: df572b88cf8a67163b28ec87154dcea01646b9a2
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "95555487"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Установка обновлений с помощью портала Azure

1. Откройте диспетчер устройств StorSimple и выберите **Устройства**. Из списка устройств, подключенных к службе, выберите то, которое требуется обновить. 

    ![В последней версии Device Manager Миссдевманажер выделены и выбраны, устройства выделяются и выбираются, а устройство MYSSIS1103 выделяется и выбирается.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate1m.png) 

2. В колонке **Параметры** щелкните **Обновления устройства**. 

    ![Появится информация для MYSSIS1103. В меню Параметры обновления устройства выделены.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate2m.png)  

3. При наличии доступных обновлений вы увидите сообщение об этом. Чтобы проверить наличие обновлений, можно щелкнуть элемент **Проверить**.

    ![На панели обновления устройства выделена кнопка Scan (сканирование). Имеется сообщение из четырех строк: Последняя просмотренная/не проверенная/программная версия/10.0.10280.0.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate3m.png)

    Вы получите уведомления о начале и успешном завершении проверки.

    ![В уведомлении отображается сообщение "Проверка обновлений для устройства миссис 1103.2:12 PM/успешно завершена операция".](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate5m.png)

4. После проверки наличия обновлений щелкните **Скачать обновления**. 

    ![На панели "обновления устройства" отображается надпись "новые обновления доступны" и "последний сканированный/11/3/2016 2:12 программный/версия/10.0.10280.0". Версия выделена.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate6m.png)

5. В колонке **Свежие обновления** просмотрите сообщение о подтверждении установки после загрузки обновлений. Нажмите кнопку **ОК**.

    ![В области новые обновления появится сообщение "после загрузки обновлений необходимо подтвердить установку". Кнопка ОК выделяется.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate7m.png)

6. Вы получите уведомления о начале и успешном завершении загрузки.

     ![В уведомлении указано "скачивание обновлений для Миссисов устройств... 2:13 РМ.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate8m.png)

5. В колонке **Обновления устройства** щелкните **Установить**.

     ![На панели "обновления устройства" отображается сообщение "подтверждение установки обновлений". Кнопка "установить" выделена.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate11m.png)   

6. В колонке **Свежие обновления** вы увидите предупреждение о том, что обновление нарушит работу. Так как виртуальный массив является устройством с одним узлом, после обновления он будет перезагружен. Все текущие операции ввода-вывода будут прерваны. Щелкните **ОК**, чтобы установить обновления. 

    ![Сообщение в области "новые обновления": "устройство будет иметь время простоя при установке этих обновлений". Кнопка ОК выделяется.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate12m.png) 

7. Вы получите уведомление о запуске задания установки. 

    ![В уведомлении указано "Установка обновлений для MYSSIS1103ов устройств". 2:15 РМ.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate13m.png)

8.  Когда задание установки успешно завершится, выберите ссылку **Просмотреть задание** в колонке **Обновление устройства**, чтобы проверить выполнение установки. 

    ![В области обновления устройства щелкните ссылку "Установка обновлений. Просмотр задания "выделен.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate15m.png)

    Вы перейдете к колонке **Установка обновлений**. Здесь вы увидите подробные сведения о выполненном задании.

    ![На панели "Установка обновлений" находятся сведения об установке, включая устройство, состояние, длительность, время начала и время окончания.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate16m.png)

9. После успешной установки обновлений вы увидите сообщение об этом в колонке обновлений устройства. Также изменится версия программного обеспечения на **10.0.10288.0**. 

    ![На панели "обновления устройства" отображается "устройство находится в актуальном состоянии" и предоставляется версия программного обеспечения (10.0.10288.0).](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate17m.png)
