---
title: Условный доступ — требовать соответствующие требованиям устройства — Azure Active Directory
description: Создание настраиваемой политики условного доступа для требования соответствующих устройств
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: c98269f9851272e8caa9b26ae0c57ed13e9a99f2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89049134"
---
# <a name="conditional-access-require-compliant-devices"></a>Условный доступ: требовать соответствующие требованиям устройства

Организации, развернутые Microsoft Intune, могут использовать информацию, возвращенную с устройств, для обнаружения устройств, соответствующих требованиям, таким как:

* Требуется ПИН-код для разблокировки
* Обязательное шифрование устройств
* Требовать минимальную или максимальную версию операционной системы
* Обязательное устройство не имеет защиты или корневого каталога

Эта информация о соответствии политики пересылается в Azure AD, где условный доступ может принимать решения о предоставлении или блокировании доступа к ресурсам. Дополнительные сведения о политиках соответствия устройств можно найти в статье [Настройка правил на устройствах для разрешения доступа к ресурсам в Организации с помощью Intune](/intune/protect/device-compliance-get-started) .

## <a name="create-a-conditional-access-policy"></a>Создание политики условного доступа

Следующие шаги помогут создать политику условного доступа, чтобы требовать, чтобы устройства, обращающиеся к ресурсам, были помечены как соответствующие политикам соответствия Организации Intune.

1. Войдите на **портал Azure** с учетными данными глобального администратора, администратора безопасности или администратора условного доступа.
1. Выберите **Azure Active Directory** > **Безопасность** > **Условный доступ**.
1. Выберите **Новая политика**.
1. Присвойте политике имя. Мы рекомендуем организациям присваивать политикам понятные имена.
1. В разделе **Назначения** выберите **Пользователи и группы**.
   1. В разделе **Включить** выберите **Все пользователи**.
   1. В разделе **Исключить** выберите **Пользователи и группы**, а затем выберите учетные записи для аварийного доступа или для обхода стандартной системы контроля доступа в вашей организации. 
   1. Нажмите кнопку **Готово**.
1. В разделе **облачные приложения или действия**  >  **Include**укажите **все облачные приложения**.
   1. Если необходимо исключить из политики определенные приложения, их можно выбрать на вкладке **исключение** в разделе **выберите Исключенные облачные приложения** и нажмите **кнопку Выбрать**.
   1. Нажмите кнопку **Готово**.
1. В разделе **условия**  >  **клиентские приложения (Предварительная версия)**  >  **выберите клиентские приложения, к которым будет применена эта политика**, оставьте все значения по умолчанию и нажмите кнопку **Готово**.
1. В разделе **Управление доступом**  >  **предоставление**выберите **требовать, чтобы устройство было помечено как соответствующее**.
   1. Щелкните **Выбрать**.
1. Подтвердите параметры и задайте для параметра **Включить политику** значение **Включить**.
1. Нажмите **Создать**, чтобы создать и включить политику.

> [!NOTE]
> Вы можете зарегистрировать новые устройства в Intune, даже если вы выбрали параметр **требовать, чтобы устройство было помечено как соответствующее требованиям** для **всех пользователей** и **всех облачных приложений** , выполнив описанные выше действия. **Требовать, чтобы устройство было помечено как соответствующее** контролю, не блокирует регистрацию в Intune. 

### <a name="known-behavior"></a>Известное поведение

В Windows 7, iOS, Android, macOS и некоторых сторонних веб-браузерах Azure AD определяет устройство с помощью сертификата клиента, подготовленного при регистрации устройства в Azure AD. При первом входе пользователя в систему через браузер пользователю предлагается выбрать сертификат. Конечный пользователь должен выбрать этот сертификат, прежде чем он сможет продолжить работу с браузером.

## <a name="next-steps"></a>Дальнейшие действия

[Распространенные политики условного доступа](concept-conditional-access-policy-common.md)

[Определение влияния условного доступа в режиме "только отчет"](howto-conditional-access-insights-reporting.md)

[Моделирование поведения входа с помощью средства What If условного доступа](troubleshoot-conditional-access-what-if.md)

[Политики соответствия устройства работают с Azure AD](/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
