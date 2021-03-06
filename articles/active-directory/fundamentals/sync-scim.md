---
title: SCIM синхронизация с Azure Active Directory
description: Рекомендации по архитектуре для достижения SCIM синхронизации с Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f957070ec94fc4c61089f31fe91261a2f52c4ee4
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578864"
---
# <a name="scim-synchronization-with-azure-active-directory"></a>SCIM синхронизация с Azure Active Directory

Система для управления междоменными удостоверениями (SCIM) — это открытый стандартный протокол для автоматизации обмена данными удостоверений пользователей между доменами удостоверений и ИТ-системами. SCIM гарантирует, что сотрудники, добавленные в систему управления человеческим капиталом (HCM), автоматически получают учетные записи, созданные в Azure Active Directory (Azure AD) или Windows Server Active Directory. Атрибуты и профили пользователя синхронизируются между двумя системами, обновляя их при изменении состояния пользователя или роли.

SCIM — это стандартизированное определение двух конечных точек: конечная точка/Users и конечная точка/Граупс. Он использует общие команды RESTFUL для создания, обновления и удаления объектов. Он также использует предварительно определенную схему для общих атрибутов, таких как имя группы, имя пользователя, имя, фамилия и адрес электронной почты. Приложения, которые предлагают SCIM 2,0 REST API, могут сократить или устранить трудности при работе с собственными интерфейсами API управления пользователями или продуктами. Например, любой клиент, совместимый с SCIM, может выполнить HTTP-запись объекта JSON в конечную точку/Users для создания новой записи пользователя. Вместо того, чтобы использовать несколько других API для одних и тех же основных действий, приложения, которые соответствуют стандарту SCIM, могут мгновенно воспользоваться преимуществами существующих клиентов, средств и кода. 

## <a name="use-when"></a>Используется, когда: 

Вы хотите автоматически подготавливать сведения о пользователях из системы HCM в Azure AD и Active Directory Windows Server, а затем при необходимости указать целевые системы. 

![Схема архитектуры](./media/authentication-patterns/scim-auth.png)


## <a name="components-of-system"></a>Компоненты системы 

* **HCM System** : приложения и технологии, обеспечивающие процесс управления человеческим капиталом и методики, которые поддерживают и автоматизируют процессы отдела кадров на протяжении всего жизненного цикла сотрудников. 

* **Служба подготовки Azure AD** : использует протокол scim 2,0 для автоматической подготовки. Служба подключается к конечной точке SCIM для приложения и использует схему объектов пользователей SCIM и интерфейсы API-интерфейса, чтобы автоматизировать подготовку и отмену подготовки пользователей и групп.  

* **Azure AD** : репозиторий пользователей, используемый для управления жизненным циклом удостоверений и их назначениями. 

* **Целевая система** : приложение или система с КОНЕЧНОЙ точкой scim и работает с подготовкой Azure AD, чтобы включить автоматическую подготовку пользователей и групп.  

## <a name="implement-scim-with-azure-ad"></a>Реализация SCIM с помощью Azure AD 

* [Как работает подготовка в Azure AD ](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works)

* [Управление подготовкой учетных записей пользователей для корпоративных приложений в портал Azure ](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

* [Создание конечной точки SCIM и Настройка подготовки пользователей с помощью Azure AD  ](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups)

* [Соответствие протокола SCIM 2,0 службе подготовки Azure AD](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-scim-compatibility)

