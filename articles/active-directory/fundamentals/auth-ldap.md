---
title: Проверка подлинности LDAP с Azure Active Directory
description: Рекомендации по архитектуре для обеспечения аутентификации LDAP с помощью Azure Active Directory.
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
ms.openlocfilehash: d5314758acecae2a9d68f2405fc1c3d2196950b4
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94577062"
---
# <a name="ldap-authentication-with-azure-active-directory"></a>Проверка подлинности LDAP с Azure Active Directory

Протокол LDAP является протоколом приложения для работы с различными службами каталогов. Службы каталогов, такие как Active Directory, [хранят сведения о пользователе и учетной записи](https://www.dnsstuff.com/active-directory-service-accounts), а также сведения о безопасности, такие как пароли. Затем служба позволяет совместно использовать информацию с другими устройствами в сети. Корпоративные приложения, такие как электронная почта, менеджеры по взаимоотношениям с клиентами (Крмс) и программное обеспечение отдела кадров (HR), могут использовать LDAP для проверки подлинности, доступа и поиска информации. 

Azure Active Directory (Azure AD) поддерживает этот шаблон с помощью доменных служб Azure AD (AD DS). Это позволяет организациям, которые принимают облачную стратегию, модернизировать свою среду, перемещая свои локальные ресурсы LDAP в облако. Преимущества немедленного использования: 

* Интеграция с Azure AD. Добавление пользователей и групп, а также изменение атрибутов их объектов автоматически синхронизируются с клиентом Azure AD в AD DS. Изменения объектов в локальных Active Directory синхронизируются с Azure AD, а затем для AD DS.

* Упростите операции. Снижается необходимость сохранения и исправления локальных инфраструктур вручную. 

* Надежность. Вы получаете управляемые службы высокой доступности 

## <a name="use-when"></a>Используется, если

Для приложения или службы необходимо использовать проверку подлинности LDAP.

![Схема архитектуры](./media/authentication-patterns/ldap-auth.png)

## <a name="components-of-system"></a>Компоненты системы

* **Пользователь** : обращается к приложениям, зависящим от LDAP, через браузер.

* **Веб-браузер** : интерфейс, с которым пользователь взаимодействует для доступа к внешнему URL-адресу приложения.

* **Виртуальная сеть**. Частная сеть в Azure, с помощью которой устаревшее приложение может использовать службы LDAP. 

* **Устаревшие приложения**. рабочие нагрузки приложений или серверов, для которых требуется LDAP, развернутые в виртуальной сети в Azure или имеющие видимость IP-адресов AD DS через сетевые маршруты. 

* **Azure AD** : синхронизирует сведения об удостоверениях из локального каталога организации с помощью Azure AD Connect.

* **Доменные службы Azure AD (AD DS)** : выполняет одностороннюю синхронизацию из Azure AD для предоставления доступа к центральному набору пользователей, групп и учетных данных. Экземпляр AD DS назначен виртуальной сети. Приложения, службы и виртуальные машины в Azure, подключающиеся к виртуальной сети, назначенной AD DS, могут использовать общие функции AD DS, такие как LDAP, присоединение к домену, групповая политика, Kerberos и проверка подлинности NTLM.
   > [!NOTE]
   >  В средах, где Организация не может синхронизировать хэши паролей или пользователи выполняют вход с использованием смарт-карт, рекомендуется использовать лес ресурсов в AD DS. 

* **Azure AD Connect** : средство для синхронизации сведений о локальных удостоверениях для Microsoft Azure AD. Мастер развертывания и интерактивные возможности помогут вам настроить необходимые компоненты для подключения, включая синхронизацию и вход из Active Directory в Azure AD. 

* **Active Directory** : Служба каталогов, в которой хранятся [Локальные сведения об удостоверениях, такие как сведения о пользователе и учетной записи](https://www.dnsstuff.com/active-directory-service-accounts), а также сведения о безопасности, такие как пароли.

## <a name="implement-ldap-authentication-with-azure-ad"></a>Реализация аутентификации LDAP с помощью Azure AD

* [Создание и настройка экземпляра AD DS Azure](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance) 

* [Настройка виртуальных сетей для экземпляра AD DS Azure](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-configure-networking) 

* [Настройка защищенный протокол LDAP для управляемого домена AD DS Azure](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-configure-ldaps) 

* [Создание исходящего доверия лесов для локального домена в Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-forest-trust)

 
