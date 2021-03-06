---
title: Мониторинг устройства Azure Stack пограничной Pro с помощью панели мониторинга Kubernetes | Документация Майкрософт
description: В этой статье описывается, как получить доступ к панели мониторинга Kubernetes и использовать ее для мониторинга устройства Azure Stack пограничной Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 09/22/2020
ms.author: alkohli
ms.openlocfilehash: 137cff47d49be1405f60bc47cd16f7f027ab63a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91320835"
---
# <a name="use-kubernetes-dashboard-to-monitor-your-azure-stack-edge-pro-gpu-device"></a>Использование панели мониторинга Kubernetes для мониторинга устройства GPU Azure Stack ребра

В этой статье описывается, как получить доступ к панели мониторинга Kubernetes и использовать ее для наблюдения за устройством GPU Azure Stack ребра Pro. Для мониторинга устройства можно использовать диаграммы в портал Azure, просматривать панель мониторинга Kubernetes или выполнять `kubectl` команды через интерфейс PowerShell устройства. 

Эта статья посвящена только задачам мониторинга, которые можно выполнять на панели мониторинга Kubernetes.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
>
> * Доступ к панели мониторинга Kubernetes на устройстве
> * Просмотр модулей, развернутых на устройстве
> * Получение IP-адреса для приложений, развернутых на устройстве
> * Просмотр журналов контейнеров для модулей, развернутых на устройстве


## <a name="about-kubernetes-dashboard"></a>О панели мониторинга Kubernetes

Панель мониторинга Kubernetes — это веб-интерфейс пользователя, который можно использовать для устранения неполадок в контейнерных приложениях. Панель мониторинга Kubernetes — это альтернатива, основанная на пользовательском интерфейсе, в `kubectl` командной строке Kubernetes. Дополнительные сведения см. в статье [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). 

На устройстве Azure Stack пограничной Pro можно использовать панель мониторинга Kubernetes в режиме *только для чтения* , чтобы получить общие сведения о приложениях, выполняющихся на устройстве Azure Stack. Pro, просмотреть состояние ресурсов кластера Kubernetes и просмотреть ошибки, произошедшие на устройстве.

## <a name="access-dashboard"></a>Панель мониторинга доступа

Панель мониторинга Kubernetes доступна *только для чтения* и выполняется на главном узле Kubernetes по порту 31000. Чтобы получить доступ к панели мониторинга, выполните следующие действия. 

1. В локальном пользовательском интерфейсе устройства перейдите на **устройство** и перейдите в раздел **конечные точки устройства**. 
1. Выберите **скачать конфигурацию** , чтобы скачать `kubeconfig` , что позволяет получить доступ к панели мониторинга. Сохраните `config.json` файл в локальной системе.
1. Выберите URL-адрес панели мониторинга Kubernetes, чтобы открыть панель мониторинга в браузере.

    ![URL-адрес панели мониторинга Kubernetes на странице устройства в локальном пользовательском интерфейсе](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-url-local-ui-1.png)

1. На странице **входа в панель мониторинга Kubernetes** :
    
    1. Выберите **kubeconfig**. 
        ![Выбор параметра kubeconfig](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-sign-in-1.png) 
    1. Нажмите кнопку с многоточием **...**. Просмотрите и укажите `kubeconfig` , что вы ранее скачали в локальной системе. Выберите **Войти**.
        ![Перейти к файлу kubeconfig](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-sign-in-2.png)    

6. Теперь вы можете просмотреть панель мониторинга Kubernetes для устройства Azure Stack ребра Pro в режиме только для чтения.

    ![Главная страница панели мониторинга Kubernetes](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-main-page-1.png)

## <a name="view-module-status"></a>Просмотр состояния модуля

Модули вычислений — это контейнеры, в которых реализована бизнес-логика. Вы можете использовать панель мониторинга для проверки успешности развертывания модуля вычислений на устройстве Azure Stack пограничной Pro.

Чтобы просмотреть состояние модуля, выполните следующие действия на панели мониторинга.

1. В левой области панели мониторинга перейдите к **пространству имен**. Отфильтруйте по пространству имен, где отображаются модули IoT Edge, в данном случае **iotedge**.
1. В левой области перейдите к **рабочей нагрузке > развертывания**.
1. На правой панели вы увидите все модули, развернутые на устройстве. В этом случае модуль Жеттингстартедвисгпу был развернут на Azure Stack пограничных Pro. Вы видите, что модуль был развернут.

    ![Просмотр развертывания модуля](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-module-deployment-1.png)

 
## <a name="get-ip-address-for-services-or-modules"></a>Получение IP-адреса для служб или модулей

Панель мониторинга можно использовать для получения IP-адресов служб или модулей, которые необходимо предоставить за пределами кластера Kubernetes. 

Диапазон IP-адресов для этих внешних служб назначается через локальный веб-интерфейс устройства на странице « **Параметры вычислений сети** ». После развертывания модулей IoT Edge может потребоваться получить IP-адрес, назначенный конкретному модулю или службе. 

Чтобы получить IP-адрес, выполните следующие действия на панели мониторинга:

1. В левой области панели мониторинга перейдите к **пространству имен**. Фильтрация по пространству имен, в котором развернута внешняя служба, в данном случае **iotedge**.
1. В левой области выберите **Обнаружение и балансировка нагрузки > службы**.
1. На правой панели вы увидите все службы, которые выполняются в `iotedge` пространстве имен на устройстве Azure Stack ребра Pro.

    ![Получение IP-адреса для внешних служб](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-get-ip-external-service-1.png)

## <a name="view-container-logs"></a>Просмотр журналов контейнеров

Существуют экземпляры, в которых необходимо просмотреть журналы контейнеров. Панель мониторинга можно использовать для получения журналов для определенного контейнера, развернутого в кластере Kubernetes.

Чтобы просмотреть журналы контейнеров, выполните следующие действия на панели мониторинга.

1. В левой области панели мониторинга перейдите к **пространству имен**. Фильтрация по пространству имен, в котором развернуты IoT Edge модули, в данном случае **iotedge**.
1. В левой области перейдите к **рабочей нагрузке >** модули Pod.
1. На правой панели вы увидите все модули Pod, выполняющиеся на устройстве. Найдите модуль Pod, на котором выполняется модуля, для которого необходимо просмотреть журналы. Выберите вертикальное многоточие для идентифицируемого Pod и в контекстном меню выберите **журналы**.

    ![Просмотр журналов контейнеров 1](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-container-logs-1.png)

1. Журналы отображаются в средстве просмотра журналов, встроенном в панель мониторинга. Кроме того, можно скачать журналы.

    ![Просмотр журналов контейнеров 2](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-container-logs-1.png)
    

## <a name="view-cpu-memory-usage"></a>Просмотр использования ЦП, памяти

На панели мониторинга Kubernetes для устройства Azure Stack пограничной Pro также имеется [надстройка сервера метрик](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/) , которая объединяет использование ЦП и памяти в рамках ресурсов Kubernetes.
 
Например, можно просмотреть ресурсы ЦП и объем памяти, потребляемые во всех развертываниях во всех пространствах имен. 

![Просмотр использования ЦП и памяти во всех развертываниях](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/view-cpu-memory-all-1.png)

Можно также выполнить фильтрацию по определенному пространству имен. В следующем примере можно просмотреть потребление ресурсов ЦП и памяти только для развертываний ARC в Azure.  

![Просмотр использования ЦП и памяти для развертываний ARC в Azure](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/view-cpu-memory-azure-arc-1.png)

Сервер метрик Kubernetes предоставляет конвейеры автомасштабирования, такие как [Горизонтальный Автомасштабирование Pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/).


## <a name="next-steps"></a>Дальнейшие шаги

Сведения об [устранении неполадок с устройствами](azure-stack-edge-gpu-troubleshoot.md).
