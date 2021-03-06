---
title: Руководство по настройке Box для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Box.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/20/2020
ms.author: jeedes
ms.openlocfilehash: e22738f1fff813e5a928b76f8049e810847fe548
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358156"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>Руководство по настройке Box для автоматической подготовки пользователей

Цель этого руководства — показать, как настроить автоматическую подготовку и отмену подготовки учетных записей из Azure AD в Box.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Box, вам потребуется:

- клиент Azure AD;
- план Business для Box или выше.

> [!NOTE]
> Мы *не рекомендуем* использовать рабочую среду для тестирования действий, описанных в этом руководстве.

> [!NOTE]
> Сначала необходимо включить приложения в приложении Box.

При проверке действий в этом руководстве соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-box"></a>Назначение пользователей в Box 

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи будут синхронизированы только те записи и группы, которые были "назначены" приложению в Azure AD.

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи или группы в Azure AD представляют пользователей, которым требуется доступ к приложению Box. После этого можно назначить этих пользователей для приложения Box, выполнив следующие действия:

[Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="assign-users-and-groups"></a>Назначение пользователей и групп
В портале Azure на вкладке **Box > Пользователи и группы** можно указать, каким пользователям и группам следует предоставить доступ к Box. Назначение пользователей или группы приводит к указанным далее результатам.

* Azure AD позволяет назначенному пользователю (путем прямого назначения или членства в группе) проходить проверку подлинности в Box. Если пользователь не назначен, Azure AD запретит этому пользователю вход в Box, а на странице входа Azure AD будет показано сообщение об ошибке.
* В [средство запуска приложений](../manage-apps/end-user-experiences.md)будет добавлена плитка приложения Box.
* Если включена автоматическая подготовка, назначенные пользователи и (или) группы добавляются в очередь подготовки.
  
  * Если для подготовки были настроены только объекты пользователей, то в очередь подготовки помещаются все напрямую назначенные пользователи и все пользователи, входящие в назначенные группы. 
  * Если для подготовки были настроены объекты групп, то подготовку в Box проходят все назначенные объекты групп, а также пользователи, входящие в эти группы. Членство пользователей и групп сохраняется после записи в Box.

На вкладке **Атрибуты > Единый вход** можно указать, какие атрибуты пользователя (или утверждения) будут представлены в Box при проверке подлинности на основе SAML. На вкладке **Атрибуты > Подготовка** можно указать, как атрибуты пользователей и групп будут передаваться из Azure AD в Box во время операций подготовки.

### <a name="important-tips-for-assigning-users-to-box"></a>Важные рекомендации по назначению пользователей в Box 

*   Рекомендуется назначить одного пользователя Azure AD в Box для проверки конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

*   При назначении пользователя Box необходимо выбрать допустимую роль пользователя. Роль "Доступ по умолчанию" не работает для подготовки.

## <a name="enable-automated-user-provisioning"></a>Включение автоматической подготовки пользователей

В этом разделе описывается подключение к API подготовки учетной записи пользователя Azure AD в Box и настройка подготовки службы для создания, обновления и отмены назначения учетных записей пользователей в Box на основе назначения пользователей и групп в Azure AD.

Если включена автоматическая подготовка, назначенные пользователи и (или) группы добавляются в очередь подготовки.
    
 * Если для подготовки были настроены только объекты пользователей, то в очередь подготовки помещаются все напрямую назначенные пользователи и все пользователи, входящие в назначенные группы. 
    
 * Если для подготовки были настроены объекты групп, то подготовку в Box проходят все назначенные объекты групп, а также пользователи, входящие в эти группы. Членство пользователей и групп сохраняется после записи в Box.

> [!TIP] 
> Для Box можно также включить единый вход на основе SAML. Для этого следуйте инструкциям на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-account-provisioning"></a>Настройка автоматической подготовки учетных записей пользователей:

В этом разделе показано, как включить подготовку учетных записей пользователей Active Directory для Box.

1. На [портале Azure](https://portal.azure.com) перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

2. Если для Box уже настроен единый вход, найдите свой экземпляр Box с помощью поиска. В противном случае щелкните **Добавить** и найдите **Box** в коллекции приложений. Выберите Box в результатах поиска и добавьте его в список приложений.

3. Выберите экземпляр Box, а затем перейдите на вкладку **Подготовка**.

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически). 

    ![Снимок экрана: вкладка "Подготовка" для Box на портале Azure. Для параметра "Режим подготовки" задано значение "Автоматически". Выделена кнопка "Авторизовать" в разделе "Учетные данные администратора".](./media/box-userprovisioning-tutorial/provisioning.png)

5. В разделе **Учетные данные администратора** щелкните **Авторизация**, чтобы открыть диалоговое окно входа в систему в новом окне браузера.

6. На странице **Войдите в систему, чтобы предоставить доступ к Box** введите необходимые учетные данные и нажмите кнопку **Авторизовать**. 
   
    ![Снимок экрана: окно Log in to grant access to Box (Войти для предоставления доступа к Box) с полями для ввода адреса электронной почты и пароля и кнопкой Authorize (Авторизовать).](./media/box-userprovisioning-tutorial/IC769546.png "Включить автоматическую подготовку пользователей")

7. Нажмите кнопку **Предоставить доступ к Box**, чтобы авторизовать операцию и вернуться на портал Azure. 
   
    ![Снимок экрана: окно авторизации доступа в Box с пояснительным сообщением и кнопкой Grant access to Box (Предоставить доступ к Box).](./media/box-userprovisioning-tutorial/IC769549.png "Включить автоматическую подготовку пользователей")

8. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению Box. Если подключение отсутствует, убедитесь, что у учетной записи Box есть права администратора группы, и повторите шаг **"Авторизовать"**.

9. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок.

10. Нажмите кнопку **Сохранить**.

11. В разделе "Сопоставления" выберите **Синхронизировать пользователей Azure Active Directory с Box**.

12. В разделе **Сопоставления атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в Box. Атрибуты, выбранные как свойства **сопоставления**, используются для сопоставления учетных записей пользователей в Box для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

13. Чтобы включить службу подготовки Azure AD для Box, в разделе "Параметры" измените значение параметра **Состояние подготовки** на **Включено**.

14. Нажмите кнопку **Сохранить**.

После этого будет запущена начальная синхронизация всех пользователей и групп, назначенных для Box в разделе "Пользователи и группы". Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра журналов действий по подготовке, в которых зафиксированы все действия, выполняемые службой подготовки в приложении Box.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

В клиенте Box синхронизированные пользователи выводятся в списке **Управляемые пользователи** в **Консоли администрирования**.

![Состояние интеграции](./media/box-userprovisioning-tutorial/IC769556.png "Состояние интеграции")


## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Настройка единого входа](box-tutorial.md)