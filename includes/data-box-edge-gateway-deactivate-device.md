---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: c1b56cfb85595b8a17dc18f69a0b162d504c04ec
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95556425"
---
Чтобы сбросить настройки устройства, необходимо безопасно очистить все данные на диске данных и загрузочном диске устройства. 

Используйте `Reset-HcsAppliance` командлет, чтобы очистить диски данных и загрузочный диск или только диски данных. `ClearData`Параметры и `BootDisk` позволяют очистить диски данных и загрузочный диск соответственно.

`BootDisk`Параметр очищает загрузочный диск и делает его непригодным для использования. Этот параметр следует применять, только если вы собираетесь вернуть устройство в Майкрософт. Дополнительные сведения см. в [статье Возврат устройства в корпорацию Майкрософт](../articles/databox-online/azure-stack-edge-return-device.md).

Если воспользоваться сбросом настроек устройства в локальном пользовательском веб-интерфейсе, безопасно очистятся только диски данных, но загрузочный диск останется нетронутым. На загрузочном диске содержится конфигурация устройства.

1. [Подключитесь к интерфейсу PowerShell](#connect-to-the-powershell-interface).
2. В командной строке введите:

    `Reset-HcsAppliance -ClearData -BootDisk`

    В следующем примере показано, как использовать этот командлет:

    ```powershell
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk

    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```