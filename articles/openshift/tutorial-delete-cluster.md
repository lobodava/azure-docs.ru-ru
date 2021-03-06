---
title: Руководство. Удаление кластера Azure Red Hat OpenShift
description: В этом руководстве содержатся сведения об удалении кластера Azure Red Hat OpenShift с помощью Azure CLI.
author: sakthi-vetrivel
ms.custom: fasttrack-edit
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: 254a9737b805aeeae2008923a8178cd971602132
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92677750"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-4-cluster"></a>Руководство по Удаление кластера Azure Red Hat OpenShift 4

В этом руководстве (третья часть из трех) описывается, как удалить кластер Azure Red Hat OpenShift с OpenShift 4. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * удаление кластера Azure Red Hat OpenShift.


## <a name="before-you-begin"></a>Перед началом

В предыдущих руководствах вы создали кластер Azure Red Hat OpenShift и подключились к нему с помощью веб-консоли OpenShift. Если вы не выполнили эти действия, вы можете начать с руководства 1 [Создание кластера Azure Red Hat OpenShift](tutorial-create-cluster.md).

Если вы решили установить и использовать интерфейс командной строки локально, то для работы с этим руководством вам понадобится Azure CLI 2.6.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sign-in-to-azure"></a>Вход в Azure

Если интерфейс Azure CLI запущен локально, выполните командлет `az login`, чтобы войти в Azure.

```bash
az login
```

Если у вас есть доступ к нескольким подпискам, выполните команду `az account set -s {subscription ID}`, заменив `{subscription ID}` необходимой подпиской.

## <a name="delete-the-cluster"></a>Удаление кластера

В предыдущих руководствах были заданы следующие переменные.

```bash
CLUSTER=yourclustername
RESOURCEGROUP=yourresourcegroup
```

Удалите свой кластер, используя эти значения:

```bash
az aro delete --resource-group $RESOURCEGROUP --name $CLUSTER
```

Появится запрос на подтверждение удаления кластера. Подтвердите, введя `y`. Удаление кластера займет несколько минут. После завершения выполнения команды вся группа ресурсов и ресурсы внутри нее, включая кластер, будут удалены.

## <a name="next-steps"></a>Дальнейшие действия

В этой части руководства вы узнали, как выполнить следующие действия:
> [!div class="checklist"]
> * Удаление кластера Azure Red Hat OpenShift 4

Ознакомьтесь с дополнительными сведениями об использовании OpenShift в [документации по Red Hat OpenShift](https://www.openshift.com/try).
