---
title: Настройка параметров автозапуска для виртуальной машины в Azure DevTest Labs | Документация Майкрософт
description: Узнайте, как настроить параметры автозапуска для виртуальных машин в лаборатории. Этот параметр позволяет автоматически запускать виртуальные машины в лаборатории по расписанию.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 83e7b0836273a59eaaf66471bd0cb42d63ccf1c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91328487"
---
# <a name="auto-startup-lab-virtual-machines"></a>Автоматические запуски виртуальных машин лаборатории  
Azure DevTest Labs позволяет настроить автоматический запуск и завершение работы виртуальных машин в лаборатории по расписанию. Сведения о настройке параметров автоматического завершения работы см. [в разделе Управление политиками автоматического завершения лабораторий в Azure DevTest Labs](devtest-lab-auto-shutdown.md). 

В отличие от автоматического завершения работы, когда все виртуальные машины включены, когда политика включена, политика автоматического запуска требует, чтобы пользователь лаборатории явно выберет виртуальную машину и отказались от этого расписания. Таким образом, вы не сможете легко работать в ситуации, когда нежелательные виртуальные машины случайно запускаются и вызывают непредвиденные затраты.

В этой статье показано, как настроить политику автозапуска для лаборатории.

## <a name="configure-autostart-settings-for-a-lab"></a>Настройка параметров автозапуска для лаборатории 
1. Перейдите на домашнюю страницу лаборатории. 
2. В меню слева выберите **Конфигурация и политики** . 

    ![Снимок экрана, показывающий меню "Конфигурация и политики" в лаборатории DevTest.](./media/devtest-lab-auto-startup-vm/configuration-policies-menu.png)
3. На странице **Конфигурация и политики** выполните следующие действия.
    
    1. Выберите **вкл** ., чтобы **Разрешить запланированное автоматическое начало для виртуальных машин** , чтобы включить функцию автоматического запуска для этой лабораторной работы. 
    2. Выберите время начала (например, 8:00:00 AM) для поля **начало расписания** . 
    3. Выберите **Часовой пояс** , который будет использоваться. 
    4. Выберите **дни недели** , на которые должны автоматически запускаться виртуальные машины. 
    5. Затем нажмите кнопку **сохранить** на панели инструментов, чтобы сохранить параметры. 

        ![Параметры автозапуска](./media/devtest-lab-auto-startup-vm/auto-start-configuration.png)

        > [!IMPORTANT]
        > Эта политика не автоматически применяет автоматический запуск к любым виртуальным машинам в лаборатории. Чтобы **принять участие в** отдельных виртуальных машинах, перейдите на страницу виртуальной машины и включите **Автоматический запуск** для этой виртуальной машины.

## <a name="enable-autostart-for-a-vm-in-the-lab"></a>Включение автозапуска для виртуальной машины в лаборатории
Следующая процедура позволяет выполнить действия по отказаться от виртуальной машины в политике автозапуска лаборатории. 

1. На домашней странице лаборатории выберите виртуальную **машину** в списке **мои виртуальные машины** . 

    ![Меню конфигурации и политик](./media/devtest-lab-auto-startup-vm/select-vm.png)
2. На странице **Виртуальная машина** выберите **Автозапуск** в меню слева или в списке **расписания** . 

    ![Выбрать меню автозапуска](./media/devtest-lab-auto-startup-vm/select-auto-start.png)
3. На странице **автозапуска** выберите **вкл** ., **чтобы разрешить запланированный автоматический запуск для этой виртуальной машины** .

    ![Включение автозапуска для виртуальной машины](./media/devtest-lab-auto-startup-vm/auto-start-vm.png)
4. Затем нажмите кнопку **сохранить** на панели инструментов, чтобы сохранить параметр. 


## <a name="next-steps"></a>Дальнейшие шаги
Дополнительные сведения о политике автоматического завершения работы в лаборатории см. [в разделе Управление политиками автоматического завершения лабораторий в Azure DevTest Labs](devtest-lab-auto-shutdown.md)
