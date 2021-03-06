---
title: Вход по ключу безопасности без пароля (Предварительная версия) — Azure Active Directory
description: Включение входа с использованием ключа безопасности без пароля в Azure AD с помощью ключей безопасности FIDO2 (Предварительная версия)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/14/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d9c4dff1e4a3ba7c7a2b11311e97eb5e66a1585
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94839307"
---
# <a name="enable-passwordless-security-key-sign-in-preview"></a>Включить вход в систему без пароля (Предварительная версия)

Для предприятий, использующих пароли сегодня и имеющих общую среду ПК, ключи безопасности обеспечивают простой способ проверки подлинности для работников без ввода имени пользователя или пароля. Ключи безопасности обеспечивают повышенную производительность для рабочих ролей и имеют более высокий уровень безопасности.

В этом документе основное внимание уделяется включению проверки подлинности с использованием пароля на основе ключа безопасности. В конце этой статьи вы сможете войти в веб-приложения с помощью учетной записи Azure AD, используя ключ безопасности FIDO2.

> [!NOTE]
> Ключи безопасности FIDO2 — это общедоступная Предварительная версия функции Azure Active Directory. Дополнительные сведения о предварительных версиях см. в разделе Дополнительные  [условия использования для предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="requirements"></a>Требования

- [Многофакторная идентификация Azure AD](howto-mfa-getstarted.md)
- Разрешить [объединенную предварительную версию регистрации сведений о безопасности](concept-registration-mfa-sspr-combined.md)
- Совместимые [ключи безопасности FIDO2](concept-authentication-passwordless.md#fido2-security-keys)
- Для WebAuthN требуется Windows 10 версии 1903 или более поздней * *

Чтобы использовать ключи безопасности для входа в веб-приложения и службы, необходимо иметь браузер, поддерживающий протокол WebAuthN. К ним относятся Microsoft ребро, Chrome, Firefox и Safari.

## <a name="prepare-devices-for-preview"></a>Подготовка устройств для предварительной версии

Устройства, присоединенные к Azure AD, для которых выполняется Пилотная версия, должны работать под управлением Windows 10 версии 1909 или выше. В Windows 10 версии 1903 или более поздней.

Гибридные устройства, присоединенные к Azure AD, должны работать под управлением Windows 10 версии 2004 или более поздней.

## <a name="enable-passwordless-authentication-method"></a>Включение метода проверки подлинности с паролем

### <a name="enable-the-combined-registration-experience"></a>Включение объединенного процесса регистрации

Функции регистрации для методов проверки подлинности, не защищенных паролем, зависят от функции общей регистрации. Выполните действия, описанные в статье [Включение общей регистрации сведений о безопасности (Предварительная версия)](howto-registration-mfa-sspr-combined.md), чтобы включить объединенную регистрацию.

### <a name="enable-fido2-security-key-method"></a>Включить метод ключа безопасности FIDO2

1. Войдите на [портал Azure](https://portal.azure.com).
1. Перейдите к **Azure Active Directory**  >  **Security**  >  **методы проверки подлинности** безопасности  >  **метод проверки подлинности (Предварительная версия)**.
1. В разделе метод **FIDO2 ключ безопасности** выберите следующие параметры.
   1. **Включить** -да или нет
   1. **Target** — все пользователи или выбранные пользователи
1. **Сохраните** конфигурацию.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Регистрация пользователей и управление ключами безопасности FIDO2

1. Перейдите по ссылке [https://myprofile.microsoft.com](https://myprofile.microsoft.com).
1. Войдите, если он еще не создан.
1. Щелкните **сведения о безопасности**.
   1. Если у пользователя уже есть по крайней мере один зарегистрированный метод многофакторной идентификации Azure AD, он может немедленно зарегистрировать ключ безопасности FIDO2.
   1. Если у них нет хотя бы одного метода многофакторной проверки подлинности Azure AD, он должен добавить один из них.
1. Добавьте ключ безопасности FIDO2, нажав кнопку **Добавить метод** и выбрав **ключ безопасности**.
1. Выберите **USB-устройство** или **устройство NFC**.
1. Получите ключ и нажмите кнопку **Далее**.
1. Появится окно с просьбой создать или ввести ПИН-код для ключа безопасности, а затем выполнить требуемый жест для ключа — Биометрия или сенсорного ввода.
1. Пользователь вернется к объединенному интерфейсу регистрации и попросит ввести понятное имя для ключа, чтобы пользователь мог указать, какой из них имеет несколько. Щелкните **Далее**.
1. Нажмите кнопку **Готово** , чтобы завершить процесс.

## <a name="sign-in-with-passwordless-credential"></a>Вход с использованием учетных данных без пароля

В примере ниже пользователь уже предоставил свой ключ безопасности FIDO2. Пользователь может выбрать вход в Интернет с помощью ключа безопасности FIDO2 в поддерживаемом браузере в Windows 10 версии 1903 или более поздней.

![Вход с ключом безопасности в Microsoft ребро](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="troubleshooting-and-feedback"></a>Устранение неполадок и обратная связь

Если вы хотите поделиться с отзывами или столкнуться с проблемами при предварительном просмотре этой функции, предоставьте общий доступ через приложение центра отзывов Windows, выполнив следующие действия:

1. Запустите **центр отзывов** и убедитесь, что вы вошли в него.
1. Отправьте отзыв по следующей категории:
   - Категория: безопасность и конфиденциальность
   - Подкатегория: FIDO
1. Для записи журналов используйте параметр, чтобы **повторно создать мою проблему**

## <a name="known-issues"></a>Известные проблемы

### <a name="security-key-provisioning"></a>Подготовка ключа безопасности

В общедоступной предварительной версии недоступна подготовка администратора и Отмена подготовки ключей безопасности.

### <a name="upn-changes"></a>Изменения имени субъекта-пользователя

Мы работаем над поддержкой функции, которая разрешает изменение имени участника-пользователя для гибридного присоединения к Azure AD и устройств, присоединенных к Azure AD. При изменении имени участника-пользователя вы больше не сможете изменить ключи безопасности FIDO2, чтобы учитывать изменения. Решение заключается в сбросе устройства и повторной регистрации пользователя.

## <a name="next-steps"></a>Дальнейшие действия

[FIDO2 безопасность ключ безопасности Windows 10 вход](howto-authentication-passwordless-security-key-windows.md)

[Включение проверки подлинности FIDO2 для локальных ресурсов](howto-authentication-passwordless-security-key-on-premises.md)

[Дополнительные сведения о регистрации устройств](../devices/overview.md)

[Дополнительные сведения о многофакторной идентификации Azure AD](../authentication/howto-mfa-getstarted.md)
