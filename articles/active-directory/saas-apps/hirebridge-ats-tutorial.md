---
title: Руководство по интеграции единого входа Azure Active Directory с Hirebridge ATS | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Hirebridge ATS.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/29/2020
ms.author: jeedes
ms.openlocfilehash: 68ebd88be1a8c68df65557ae29fd50639df0aef5
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133372"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hirebridge-ats"></a>Руководство по интеграции единого входа Azure Active Directory с Hirebridge ATS

В этом учебнике описано, как интегрировать Hirebridge ATS с Azure Active Directory (Azure AD). Интеграция Hirebridge ATS с Azure AD обеспечивает следующие возможности.

* Контроль доступа к Hirebridge ATS с помощью Azure AD.
* Автоматический вход пользователей в Hirebridge ATS с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Hirebridge ATS с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Hirebridge ATS поддерживает единый вход, инициируемый **поставщиком удостоверений**.


## <a name="adding-hirebridge-ats-from-the-gallery"></a>Добавление Hirebridge ATS из коллекции

Чтобы настроить интеграцию Hirebridge ATS с Azure AD, необходимо добавить Hirebridge ATS из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Hirebridge ATS**.
1. Выберите **Hirebridge ATS** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-hirebridge-ats"></a>Настройка и проверка единого входа Azure Active Directory для Hirebridge ATS

Настройте и проверьте единый вход Azure AD в Hirebridge ATS с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в приложении Hirebridge ATS.

Чтобы настроить и проверить единый вход Azure AD в Hirebridge ATS, выполните действия из следующих стандартных блоков:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Hirebridge ATS](#configure-hirebridge-ats-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя приложения Hirebridge ATS](#create-hirebridge-ats-test-user)** требуется для того, чтобы в Hirebridge ATS существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Hirebridge ATS** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** приложение предварительно настроено и ему заданы требуемые URL-адреса. Пользователь должен сохранить конфигурацию, нажав кнопку **Сохранить**.


1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Hirebridge ATS**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)
### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к Hirebridge ATS.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Hirebridge ATS**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-hirebridge-ats-sso"></a>Настройка единого входа Hirebridge ATS

Чтобы настроить единый вход на стороне **Hirebridge ATS**, нужно отправить скачанный **сертификат (Base64)** и соответствующие URL-адреса, скопированные на портале Azure, в [группу поддержки Hirebridge ATS](mailto:support@hirebridge.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-hirebridge-ats-test-user"></a>Создание тестового пользователя в Hirebridge ATS

В этом разделе описано, как создать пользователя Britta Simon в приложении Hirebridge ATS. Чтобы добавить пользователей на платформу Hirebridge ATS, обратитесь в [группу поддержки Hirebridge ATS](mailto:support@hirebridge.com). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов.

1. На портале Azure выберите "Тестировать приложение", и вы автоматически войдете в приложение Hirebridge ATS, для которого настроен единый вход.

1. Вы можете использовать Панель доступа (Майкрософт). Щелкнув плитку Hirebridge ATS на Панели доступа, вы автоматически войдете в приложение Hirebridge ATS, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="next-steps"></a>Дальнейшие действия

После настройки Hirebridge ATS вы сможете применить функцию управления сеансом, которая защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа в режиме реального времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


