---
title: Перенос виртуальных машин AWS в Azure с помощью Миграции Azure
description: В этой статье описаны параметры для переноса экземпляров AWS в Azure с рекомендацией использовать Миграцию Azure.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 83d8106b9f206da9095c37b6179bdda8001b439c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87852775"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Перенос виртуальных машин Amazon Web Services (AWS) в Azure

В этой статье описаны параметры для миграции экземпляров Amazon Web Services (AWS).

## <a name="migrate-with-azure-migrate"></a>Миграция с помощью средства "Миграция Azure"

Мы рекомендуем перенести экземпляры AWS EC2 в Azure с помощью службы [Миграция Azure](../migrate/migrate-services-overview.md). Миграция Azure специально предназначена для переноса серверов. Миграция Azure — это центр управления для обнаружения, оценки и переноса локальных компьютеров в Azure.

[Узнайте](../migrate/tutorial-migrate-aws-virtual-machines.md) о миграции экземпляров AWS с помощью Миграции Azure. 


## <a name="migrate-with-site-recovery"></a>Миграция с помощью Site Recovery

Site Recovery следует использовать только для аварийного восстановления, а не для миграции.

Если вы уже используете Azure Site Recovery и хотите продолжить его использование для миграции AWS, выполните те же действия, что и при настройке [аварийного восстановления физических компьютеров](physical-azure-disaster-recovery.md).


> [!NOTE]
> При выполнении отработки отказа для аварийного восстановления в качестве последнего шага вы примените отработку отказа. При миграции экземпляров AWS параметр **Исполнить** не учитывается. Вместо этого следует выбрать параметр **Завершение миграции**. 

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Ознакомьтесь с часто задаваемыми вопросами](../migrate/resources-faq.md) о Миграции Azure.
