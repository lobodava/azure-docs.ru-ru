---
title: Ссылки на странице приложения прокси приложения не работают
description: Как устранить ошибки, связанные с неработающими ссылками на приложения прокси приложения, которые были интегрированы с Azure AD?
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: troubleshooting
ms.date: 09/10/2018
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1922ea9afd69366e534049f5a7a350cf39e52dee
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371585"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Ссылки на странице приложения прокси приложения не работают

Эта статья поможет понять, почему ссылки на приложение Azure Active Directory Proxy работают неправильно.

## <a name="overview"></a>Обзор 
После публикации приложения прокси приложения единственные ссылки, которые работают по умолчанию, — это ссылки на места назначения, которые содержатся в опубликованном корневом URL-адресе. Ссылки в приложениях не работают; внутренний URL-адрес приложения, скорее всего, не включает все места назначения для ссылок в приложении.

**Почему это происходит?** При щелчке по ссылке в приложении прокси приложения пытается разрешить URL-адрес как внутренний URL-адрес, который относится к этому приложению, или как внешний URL-адрес. Если ссылка указывает на внутренний URL-адрес, который не относится к этому приложению, то эта ссылка не принадлежит ни к одному из контейнеров, и возникает ошибка "Не найдено".

## <a name="ways-you-can-resolve-broken-links"></a>Способы решения проблемы с неработающими ссылками

Существует три способа решения этой проблемы. Они перечислены ниже в порядке возрастания сложности.

1.  Убедитесь, что внутренний URL-адрес является корневым URL-адресом, который содержит все соответствующие ссылки для приложения. Благодаря этому все ссылки будут разрешаться как содержимое, опубликованное в том же приложении.

    Если вы хотите изменить внутренний URL-адрес, но хотите оставить прежней целевую страницу для пользователей, измените URL-адрес домашней страницы на ранее опубликованный внутренний URL-адрес. Это можно сделать, перейдя к "Azure Active Directory" — &gt; Регистрация приложений — &gt; выберите &gt; фирменную символику приложения. В разделе фирменной символики отображается поле "URL-адрес домашней страницы", которое можно настроить так, чтобы оно было нужной целевой страницей. Если вы по-прежнему используете устаревшую Регистрация приложений интерфейс, на вкладке Свойства будет отображаться информация "URL-адрес домашней страницы". 
    
    > [!IMPORTANT]
    > Чтобы внести указанные выше изменения, необходимо иметь права на изменение объектов приложения в Azure AD. Пользователю должна быть назначена роль [администратора приложения](../roles/delegate-app-roles.md#assign-built-in-application-admin-roles) , которая предоставляет пользователю права модификаион приложений в Azure AD.
    >

2.  Если в приложениях используются полные доменные имена (FQDN), для публикации приложений следует использовать [пользовательские домены](application-proxy-configure-custom-domain.md). Эта функция позволяет использовать один и тот же URL-адрес и как внутренний, и как внешний.

    В этом случае ссылки в приложении будут доступны извне через прокси приложения, так как ссылки на внутренние URL-адреса в приложении также будут распознаваться извне. Все ссылки по-прежнему должны принадлежать опубликованному приложению. Тем не менее в этом случае ссылки не должны принадлежать одному приложению и могут принадлежать нескольким приложениям.

3.  Если ни один из этих вариантов вам не подходит, можно включить преобразование встроенных ссылок с помощью несколько вариантов. Эти варианты предусматриваются использование Intune Managed Browser, расширение "Мои приложения" или параметра преобразования ссылок в приложении. Дополнительные сведения о каждом из этих вариантов и их применении, см. в разделе [Перенаправление встроенных ссылок для приложений, опубликованных с помощью прокси приложения Azure AD](application-proxy-configure-hard-coded-link-translation.md).

## <a name="next-steps"></a>Дальнейшие шаги
[Работа с имеющимися локальными прокси-серверами](application-proxy-configure-connectors-with-proxy-servers.md)

