---
title: Устранение неполадок при входе в доменные службы Azure AD | Документация Майкрософт
description: Сведения об устранении распространенных проблем с входом пользователя и ошибках в доменных службах Azure Active Directory.
services: active-directory-ds
author: MicrosoftGuyJFlo
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: joflore
ms.openlocfilehash: 9343af5b29289a152db84e64f81fa8ca74ce7bc3
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91967398"
---
# <a name="troubleshoot-account-sign-in-problems-with-an-azure-active-directory-domain-services-managed-domain"></a>Устранение неполадок при входе в учетную запись с управляемым доменом доменных служб Azure Active Directory

Наиболее распространенными причинами для учетной записи пользователя, которая не может войти в управляемый домен доменных служб Azure Active Directory (Azure AD DS), являются следующие сценарии.

* [Учетная запись еще не синхронизирована с Azure AD DS.](#account-isnt-synchronized-into-azure-ad-ds-yet)
* [AD DS Azure не имеет хэшей паролей, чтобы разрешить вход в учетную запись.](#azure-ad-ds-doesnt-have-the-password-hashes)
* [Учетная запись заблокирована.](#the-account-is-locked-out)

> [!TIP]
> AD DS Azure не может синхронизировать учетные данные для учетных записей, которые являются внешними для клиента Azure AD. Внешние пользователи не могут войти в управляемый домен AD DS Azure.

## <a name="account-isnt-synchronized-into-azure-ad-ds-yet"></a>Учетная запись еще не синхронизирована с Azure AD DS

В зависимости от размера каталога может потребоваться некоторое время, чтобы учетные записи пользователей и хэши учетных данных были доступны в управляемом домене. Для больших каталогов Эта начальная односторонняя синхронизация из Azure AD может занять несколько часов и до одного дня или двух. Убедитесь, что вы ожидаете достаточно времени, прежде чем повторять проверку подлинности.

Для гибридных сред, которые пользователь Azure AD Connect для синхронизации локальных данных каталога с Azure AD, убедитесь, что вы используете последнюю версию Azure AD Connect и [настроили Azure AD Connect для выполнения полной синхронизации после включения Azure AD DS][azure-ad-connect-phs]. Если вы отключаете AD DS Azure, а затем включаете его повторно, необходимо выполнить эти действия еще раз.

Если по-прежнему возникают проблемы с учетными записями, которые не синхронизируются с помощью Azure AD Connect, перезапустите службу Azure AD Sync. На компьютере с установленным Azure AD Connect откройте окно командной строки и выполните следующие команды:

```console
net stop 'Microsoft Azure AD Sync'
net start 'Microsoft Azure AD Sync'
```

## <a name="azure-ad-ds-doesnt-have-the-password-hashes"></a>AD DS Azure не имеет хэшей паролей

Пока вы не включите Azure AD DS для клиента, Azure AD не будет создавать и хранить хэши паролей в формате, требуемом для аутентификации NTLM или Kerberos. Из соображений безопасности Azure AD никогда не хранит пароли в виде открытого текста. Поэтому Azure AD не может автоматически создать хэши паролей в формате NTLM или Kerberos по уже существующим учетным данным пользователей.

### <a name="hybrid-environments-with-on-premises-synchronization"></a>Гибридные среды с локальной синхронизацией

Для гибридных сред, использующих Azure AD Connect для синхронизации из локальной AD DS среды, вы можете локально создавать и синхронизировать необходимые хэши паролей NTLM или Kerberos в Azure AD. После создания управляемого домена [включите синхронизацию хэшей паролей для Azure Active Directory доменных служб][azure-ad-connect-phs]. Без завершения этого шага синхронизации хэша паролей вы не сможете войти в учетную запись с помощью управляемого домена. Если вы отключаете AD DS Azure, а затем включаете его повторно, необходимо выполнить эти действия еще раз.

Дополнительные сведения см. в статье [как работает синхронизация хэша паролей для Azure AD DS][phs-process].

### <a name="cloud-only-environments-with-no-on-premises-synchronization"></a>Облачные среды без локальной синхронизации

Управляемые домены без локальной синхронизации, учетные записи в Azure AD, Кроме того, необходимо создать необходимые хэши паролей NTLM или Kerberos. Если учетная запись, доступная только для облака, не может войти в систему, для учетной записи после включения Azure AD DS был успешно выполнен процесс изменения пароля?

* **Нет, пароль не был изменен.**
    * [Измените пароль для учетной записи][enable-user-accounts] , чтобы создать необходимые хэши паролей, а затем подождите 15 минут, прежде чем пытаться войти снова.
    * Если вы отключаете AD DS Azure, а затем включаете его повторно, каждая учетная запись должна следовать инструкциям, чтобы изменить пароль и создать необходимые хэши паролей.
* **Да, пароль изменен.**
    * Попробуйте выполнить вход с использованием формата *имени участника-пользователя* , например `driley@aaddscontoso.com` , вместо формата *SamAccountName* , такого как `AADDSCONTOSO\deeriley` .
    * *SamAccountName* может автоматически создаваться для пользователей, чей префикс имени участника-пользователя слишком длинный или совпадает с другим пользователем в управляемом домене. Формат *имени участника-пользователя* гарантированно уникален в пределах клиента Azure AD.

## <a name="the-account-is-locked-out"></a>Учетная запись заблокирована

Учетная запись пользователя в управляемом домене блокируется, если выполнено определенное пороговое значение неудачных попыток входа. Это поведение блокировки учетной записи предназначено для защиты от повторных попыток входа методом подбора, которые могут указывать на автоматическую цифровую атаку.

По умолчанию при наличии 5 попыток ввода пароля в течение 2 минут учетная запись блокируется в течение 30 минут.

Дополнительные сведения и способы устранения проблем с блокировкой учетной записи см. [в статье Устранение неполадок блокировки учетных записей в Azure AD DS][troubleshoot-account-lockout].

## <a name="next-steps"></a>Дальнейшие действия

Если у вас по-прежнему возникли проблемы при присоединении виртуальной машины к управляемому домену, [найдите справку и отправьте запрос в службу поддержки для Azure Active Directory][azure-ad-support].

<!-- INTERNAL LINKS -->
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[azure-ad-connect-phs]: ./tutorial-configure-password-hash-sync.md
[enable-user-accounts]:  tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[phs-process]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md