---
title: Синхронизация LDAP с Azure Active Directory
description: Руководство по архитектуре для достижения синхронизации LDAP с Azure Active Directory.
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
ms.openlocfilehash: e617d7ccc14e65c18eb86877b1c7fb1aeef74cd0
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578898"
---
# <a name="ldap-synchronization-with-azure-active-directory"></a>Синхронизация LDAP с Azure Active Directory

Протокол LDAP — это протокол службы каталогов, который работает в стеке TCP/IP. Он предоставляет механизм, используемый для подключения к каталогам Интернета, поиска и изменения каталогов. Служба каталогов LDAP основана на модели "клиент-сервер", и ее функция предназначена для обеспечения доступа к существующему каталогу. Многие компании зависят от локальных серверов LDAP для хранения пользователей и групп для критически важных бизнес-приложений. 

Azure Active Directory (Azure AD) может заменить синхронизацию LDAP Azure AD Connect. Служба синхронизации Azure AD Connect выполняет все операции, связанные с синхронизацией данных удостоверений между локальными средами и Azure AD. 

## <a name="use-when"></a>Используется, если

Необходимо синхронизировать данные удостоверений между локальными каталогами LDAP V3 и Azure AD. 

![Схема архитектуры](./media/authentication-patterns/ldap-sync.png)

## <a name="components-of-system"></a>Компоненты системы

* **Пользователь** : обращается к приложению, использующему каталог LDAP V3 для сортировки пользователей и паролей.

* **Веб-браузер** : компонент, с которым взаимодействует пользователь для доступа к внешнему URL-адресу приложения.

* **Веб-приложение** : приложение с зависимостями от каталогов LDAP V3.

* **Azure AD**. Azure AD синхронизирует сведения об удостоверении (пользователи, группы, пароли) из ЛОКАЛЬНЫХ каталогов LDAP организации с помощью Azure AD Connect. 

* **Azure AD Connect** : — это инструмент для подключения локальных инфраструктур удостоверений к Microsoft Azure AD. Мастер и интерактивные возможности помогут развернуть и настроить предварительные требования и компоненты, необходимые для подключения. 

* **Настраиваемый соединитель**. универсальный соединитель LDAP позволяет интегрировать службу синхронизации Azure AD Connect с сервером LDAP V3. Он располагается на Azure AD Connect.

* **Active Directory**. Active Directory — это служба каталогов, входящая в большинство операционных систем Windows Server. Серверы, на которых работают службы Active Directory Directory, называются контроллерами домена, они проходят проверку подлинности и авторизацию всех пользователей и компьютеров в домене Windows.

* **Сервер LDAP V3** : Каталог, совместимый с протоколом LDAP, в котором хранятся корпоративные пользователи и пароли, используемые для проверки подлинности служб каталогов.

## <a name="implement-ldap-synchronization-with-azure-ad"></a>Реализация синхронизации LDAP с Azure AD

* [Средства интеграции каталогов гибридных удостоверений](https://docs.microsoft.com/azure/active-directory/hybrid/plan-hybrid-identity-design-considerations-tools-comparison) 

* [План установки Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-roadmap) 

* [Обзор и создание соединителя LDAP](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap) 

   > [!NOTE]
   > Соединители LDAP — это расширенная конфигурация, которая требует некоторого опыта работы с Forefront Identity Manager и (или) Microsoft Identity Manager. Если используется в рабочей среде, мы рекомендуем ознакомиться с вопросами об этой конфигурации с помощью [Поддержка Premier](https://support.microsoft.com/premier) или Microsoft Partner Network.

 
