---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 46cfb27b8bde95990d13ec4bca4e96f25cfe9dc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "67185802"
---
Из-за постоянной ведущейся разработки версия пакета Android SDK, установленная в Android Studio, может не совпадать с версией в коде. В этом руководстве используется пакет SDK для Android версии 26 (последней на момент написания руководства). Номер версии может увеличиваться по мере выпуска новых пакетов SDK, и мы рекомендуем использовать новейшую доступную версию.

Два признака несоответствия версии:

- При выполнении сборке или перестроении проекта может появиться сообщение об ошибке Gradle, например `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Стандартные объекты Android в коде, которые должны разрешаться на основе инструкций `import` , могут создавать сообщения об ошибках.

Если появляется любой из этих признаков, версия пакета Android SDK, установленная в Android Studio, может не соответствовать целевому SDK загруженного проекта. Чтобы проверить версию, внесите следующие изменения:

1. В Android Studio, нажмите **Инструменты** > **Android** > **SDK Manager**. Если вы не установили последнюю версию платформы SDK, щелкните ее для установки. Запишите номер версии.

2. На вкладке **Обозреватель проектов** в разделе **Gradle Scripts** (Сценарии Gradle) откройте файл **build.gradle (Module: app)**. Для параметров **compileSdkVersion** и **targetSdkVersion** задайте самую последнюю установленную версию пакета SDK. `build.gradle` может выглядеть следующим образом.

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
