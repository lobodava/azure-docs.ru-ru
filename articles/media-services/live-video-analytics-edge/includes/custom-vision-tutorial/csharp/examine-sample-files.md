---
author: russell-cooks
ms.service: media-services
ms.topic: include
ms.date: 10/05/2020
ms.author: russellcooks
ms.openlocfilehash: 359c5f93516ea6f0561865bd86e4f51dedb4c3a5
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358289"
---
1. В Visual Studio Code перейдите в папку src/edge. Здесь вы увидите созданный вами ENV-файл и несколько файлов шаблонов развертывания.

    Шаблон развертывания — это манифест развертывания для пограничного устройства, где используются некоторые значения заполнителя. В файле .env указаны значения таких переменных.
1. Теперь перейдите в папку src/cloud-to-device-console-app. Здесь мы найдем файл appsettings.json, который вы создали, и еще несколько файлов:

    * c2d-console-app.csproj Это файл проекта для Visual Studio Code.
    * operations.json Это файл со списком операций, которые будут выполняться в программе.
    * Program.cs: Этот пример кода программы выполняет следующее:

        * загружает параметры приложения;
        * вызывает Аналитику видеотрансляций в прямых методах модуля IoT Edge, чтобы создать топологию и экземпляр графа, а также и активировать граф;
        * приостанавливает выполнение графа в окне **Терминал** и событий, отправленных в Центр Интернета вещей в окне **Выходные данные** ;
        * отключает и удаляет экземпляр графа, а также удаляет топологию графа.
