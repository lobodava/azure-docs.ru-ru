---
title: Защита паролем в Azure Active Directory
description: Узнайте, как динамически запретить слабые пароли в вашей среде с помощью Azure Active Directory защиты паролем
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/16/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 026f45e715f6d442b27cdd0274f029a68330f7ee
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94839834"
---
# <a name="eliminate-bad-passwords-using-azure-active-directory-password-protection"></a>Устранение неверных паролей с помощью Azure Active Directory защиты паролем

Многие рекомендации по безопасности рекомендуют не использовать один и тот же пароль в нескольких местах, чтобы сделать его сложным и избежать простых паролей, таких как *Password123*. Вы можете предоставить пользователям [рекомендации по выбору паролей](https://www.microsoft.com/research/publication/password-guidance), но при этом часто используются слабые или небезопасные пароли. Служба защиты паролей Azure AD обнаруживает и блокирует известные ненадежные пароли и их варианты, а также может блокировать дополнительные слабые термины, характерные для вашей организации.

При использовании защиты паролей Azure AD списки паролей по умолчанию с глобальным запрещением автоматически применяются ко всем пользователям в клиенте Azure AD. Для поддержки собственных потребностей в бизнесе и безопасности можно определить записи в списке запрещенных паролей. Когда пользователь изменяет или сбрасывает пароли, эти списки запрещенных паролей проверяются на принудительное использование надежных паролей.

Следует использовать дополнительные функции, такие как [многофакторная идентификация Azure AD](concept-mfa-howitworks.md), а не только надежные пароли, принудительно защищенные с помощью защиты ПАРОЛЕЙ Azure AD. Дополнительные сведения об использовании нескольких уровней безопасности для событий входа см. в статье [PA $ $Word не имеет значения](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984).

> [!IMPORTANT]
> В этой статье объясняется администратором, как работает защита паролей Azure AD. Если вы уже зарегистрировались для самостоятельного сброса пароля и хотите вернуться в учетную запись, перейдите по адресу [https://aka.ms/sspr](https://aka.ms/sspr) .
>
> Если ваша группа ИТ пока не включила возможность самостоятельно сбрасывать пароль, обратитесь за помощью в службу поддержки.

## <a name="global-banned-password-list"></a>Список глобально заблокированных паролей

Группа защита идентификации Azure AD постоянно анализирует данные телеметрии безопасности Azure AD, поискя часто используемых слабых или скомпрометированных паролей. В частности, анализ ищет базовые термины, которые часто используются в качестве основы для ненадежных паролей. При обнаружении слабых терминов они добавляются в *глобальный список запрещенных паролей*. Содержимое списка глобальных запрещенных паролей не основано на любом внешнем источнике данных, но в результатах телеметрии и анализа безопасности Azure AD.

При изменении или сбросе пароля для любого пользователя в клиенте Azure AD для проверки надежности пароля используется текущая версия списка глобальных паролей с запрещением. Эта проверка приводит к более надежным паролям для всех клиентов Azure AD.

Список глобальных запрещенных паролей автоматически применяется ко всем пользователям в клиенте Azure AD. Нет ничего для включения или настройки, и его нельзя отключить. Этот список глобальных паролей с запрещением применяется к пользователям при изменении или сбросе собственного пароля через Azure AD.

> [!NOTE]
> Кибератак-преступники также используют аналогичные стратегии в их атаках для обнаружения распространенных ненадежных паролей и вариаций. Для повышения безопасности корпорация Майкрософт не публикует содержимое списка глобальных запрещенных паролей.

## <a name="custom-banned-password-list"></a>пользовательский список заблокированных паролей

Некоторые организации хотят улучшить безопасность и добавить собственные настройки поверх глобального списка запрещенных паролей. Чтобы добавить собственные записи, можно использовать *настраиваемый список запрещенных паролей*. Термины, добавленные в список пользовательских запрещенных паролей, должны быть сосредоточены на терминах, относящихся к Организации, например в следующих примерах:

- названия торговых марок;
- названия продуктов;
- расположения подразделений компании, например штаб-квартиры;
- характерные для организации слова внутреннего употребления;
- аббревиатуры, имеющие важное значение для конкретной организации.

Когда термины добавляются в список настраиваемых паролей с запрещенными паролями, они объединяются с терминами в глобальном списке запрещенных паролей. Затем события изменения или сброса пароля проверяются по Объединенному набору этих списков запрещенных паролей.

> [!NOTE]
> Длина пользовательского списка заблокированных паролей ограничена значением 1000 элементов. Он не предназначен для блокирования чрезвычайно больших списков паролей.
>
> Чтобы полностью использовать преимущества списка пользовательских запрещенных паролей, сначала изучите, [как оцениваются пароли](#how-are-passwords-evaluated) , прежде чем добавлять термины в список запрещенных. Такой подход позволяет эффективно обнаруживать и блокировать большое количество ненадежных паролей и их разновидностей.

![Изменение настраиваемого списка запрещенных паролей в разделе методы проверки подлинности](./media/tutorial-configure-custom-password-protection/enable-configure-custom-banned-passwords-cropped.png)

Давайте рассмотрим клиента с именем *contoso*. Компания основана на Лондоне и делает продукт с именем *Widget*. В этом примере клиент был бы непроизводительна и менее безопасным, чтобы попытаться заблокировать определенные разновидности этих терминов, например следующие:

- "Contoso! 1"
- "Contoso@London"
- "Контосовиджет"
- "! Компанией
- "Лондонхк"

Вместо этого гораздо эффективнее и безопасно блокировать только основные термины, такие как следующие примеры:

- "Contoso"
- Лондон
- »

Затем алгоритм проверки пароля автоматически блокирует ненадежные варианты и сочетания.

Чтобы приступить к работе с пользовательским списком запрещенных паролей, выполните следующие действия.

> [!div class="nextstepaction"]
> [Учебник. Настройка пользовательских запрещенных паролей](tutorial-configure-custom-password-protection.md)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Атаки на распыление паролей и списки скомпрометированных паролей сторонних разработчиков

Защита паролем Azure AD помогает защититься от таких атак. Большинство атак типа «распыление паролей» не пытаются атаковать какую-либо отдельную учетную запись несколько раз. Такое поведение может повысить вероятность обнаружения с помощью блокировки учетной записи или других средств.

Вместо этого большинство атак типа «распыление паролей» отправляют только небольшое количество известных слабых паролей для каждой учетной записи предприятия. Этот метод позволяет злоумышленнику быстро найти скомпрометированную учетную запись и избежать возможных пороговых значений обнаружения.

Защита паролем в Azure AD позволяет эффективно блокировать все известные слабые пароли, которые, скорее всего, будут использоваться при атаках распылителя паролей. Эта защита основана на реальных данных телеметрии безопасности из Azure AD для создания списка глобальных паролей с запрещением.

Есть некоторые сторонние веб-сайты, которые перечисляют миллионы паролей, которые были скомпрометированы в предыдущих общеизвестных уязвимостях безопасности. Обычно продукты для проверки пароля от сторонних производителей основаны на сравнении с этими миллионами паролей. Однако эти приемы не являются лучшим способом улучшить общую надежность пароля с учетом типичных стратегий, используемых злоумышленниками в области паролей.

> [!NOTE]
> Глобальный список запрещенных паролей основан не на источниках данных сторонних производителей, включая скомпрометированные списки паролей.

Несмотря на то, что глобальный список запрещен в сравнении с некоторыми небольшими групповыми списками, он получает данные из реальной телеметрии безопасности на основе фактических паролей. Такой подход повышает общую безопасность и эффективность работы, а алгоритм проверки пароля также использует интеллектуальные методы нечеткого сопоставления. В результате защита паролей Azure AD эффективно обнаруживает и блокирует миллионы наиболее распространенных слабых паролей, используемых в Организации.

## <a name="on-premises-hybrid-scenarios"></a>Локальные гибридные сценарии

Во многих организациях используется гибридная модель идентификации, которая включает локальные среды домен Active Directory служб (AD DS). Чтобы расширить преимущества безопасности защиты паролей Azure AD в среде AD DS, можно установить компоненты на локальных серверах. Эти агенты нуждаются в событиях смены пароля в локальной среде AD DS, чтобы соответствовать той же политике паролей, что и в Azure AD.

Дополнительные сведения см. в статье [Принудительная защита паролей Azure AD для AD DS](concept-password-ban-bad-on-premises.md).

## <a name="how-are-passwords-evaluated"></a>Оценка паролей

Когда пользователь изменяет или сбрасывает пароль, новый пароль проверяется на надежность и сложность путем его проверки по Объединенному списку терминов из глобальных и настраиваемых списков запрещенных паролей.

Даже если пароль пользователя содержит запрещенный пароль, пароль может быть принят, если общий пароль достаточно надежен. Недавно настроенный пароль проходит следующие шаги, чтобы оценить его общую надежность, чтобы определить, следует ли его принять или отклонить.

### <a name="step-1-normalization"></a>Шаг 1. Нормализация

Новый пароль сначала проходит через процесс нормализации. Этот метод позволяет соотнесению небольшого набора запрещенных паролей с большим набором потенциально ненадежных паролей.

Нормализация состоит из двух следующих частей:

* Все прописные буквы изменяются на нижний регистр.
* Затем выполняются подстановки общих символов, как в следующем примере:

   | Исходный символ | Замененный символ |
   |-----------------|--------------------|
   | 0               | o                  |
   | 1               | l                  |
   | $               | s                  |
   | \@              | а                  |

Рассмотрим следующий пример.

* Пароль "blank" запрещен.
* Пользователь пытается изменить пароль на " Bl@nK ".
* Несмотря Bl@nk на то, что "" не заблокирован, процесс нормализации преобразует этот пароль в "blank".
* Этот пароль будет отклонен.

### <a name="step-2-check-if-password-is-considered-banned"></a>Шаг 2. Проверка того, что пароль считается запрещенным

Затем пароль проверяется для другого поведения сопоставления, и формируется оценка. Эта окончательная оценка определяет, принимается ли запрос на изменение пароля или отклоняется.

#### <a name="fuzzy-matching-behavior"></a>Режим работы нечеткого соответствия

Нечеткое сопоставление используется для нормализованного пароля, чтобы определить, содержит ли он пароль, найденный либо в глобальном, либо в пользовательском списках запрещенных паролей. Процесс сопоставления основан на расстоянии редактирования одного (1) сравнения.

Рассмотрим следующий пример.

* Пароль "abcdef" запрещен.
* Пользователь пытается изменить пароль одним из следующих способов:

   * "абкдег" — *последний символ изменен с "f" на "g"*
   * "абкдефг"- *"g" добавляется к концу*
   * "abcd"- *замыкающий "f" был удален с конца*

* Каждый из перечисленных выше паролей не соответствует запрещенному паролю "abcdef".

    Однако, поскольку каждый пример находится в пределах расстояния, равного 1 из запрещенного термина "abcdef", все они считаются совпадением с "abcdef".
* Эти пароли будут отклонены.

#### <a name="substring-matching-on-specific-terms"></a>Совпадение строк (на определенных условиях)

Сопоставление подстроки используется в нормализованном пароле для проверки имени и фамилии пользователя, а также имени клиента. Сопоставление имен клиентов не выполняется при проверке паролей на AD DS контроллере домена для локальных гибридных сценариев.

> [!IMPORTANT]
> Сопоставление подстрок применяется только к именам и другим терминам, которые имеют длину не менее четырех символов.

Рассмотрим следующий пример.

* Пользователь с именем poll, желающий сбросить пароль до "p0LL23fb".
* После нормализации этот пароль станет «poll23fb».
* Подстрока Match определяет, что пароль содержит имя пользователя "poll".
* Несмотря на то что "poll23fb" не был специально в списке запрещенных паролей, подстрока, соответствующая условиям, в пароле обнаружено "poll".
* Этот пароль будет отклонен.

#### <a name="score-calculation"></a>Оценки безопасности

Следующим шагом является идентификация всех экземпляров запрещенных паролей в новом нормализованном пароле пользователя. Точки назначаются на основе следующих критериев:

1. Каждый запрещенный пароль, найденный в пароле пользователя, предоставляется одной точкой.
1. Каждый оставшийся уникальный символ получает один балл.
1. Пароль должен быть не менее пяти (5) баллов, которые должны быть приняты.

В следующих двух примерах сценариев Contoso использует защиту паролем Azure AD и содержит "contoso" в списке настраиваемых паролей с запрещенными паролями. Также предположим, что "blank" находится в глобальном списке.

В следующем примере сценария пользователь изменяет свой пароль на "C0ntos0Blank12":

* После нормализации этот пароль превращается в "contosoblank12".
* Процесс сопоставления определяет, что этот пароль содержит два запрещенных пароля: contoso и blank.
* После этого пароль получает следующую оценку:

    *[Contoso] + [пусто] + [1] + [2] = 4 точки*

* Так как этот пароль находится под пяти (5) баллами, он отклоняется.

Рассмотрим немного другой пример, чтобы продемонстрировать, как дополнительная сложность пароля может создать необходимое количество баллов, которые должны быть приняты. В следующем примере сценария пользователь изменяет свой пароль на " ContoS0Bl@nkf9 !":

* После нормализации этот пароль преобразуется в "contosoblankf9!".
* Процесс сопоставления определяет, что этот пароль содержит два запрещенных пароля: contoso и blank.
* После этого пароль получает следующую оценку:

    *[Contoso] + [пусто] + [f] + [9] + [!] = 5 точек*

* Так как этот пароль не менее пяти (5) баллов, он принимается.

> [!IMPORTANT]
> Алгоритм запрещенных паролей, а также список глобальных паролей с запрещенными паролями может и в любое время вносить изменения в Azure на основе текущего анализа безопасности и исследований.
>
> Для локальной службы агента контроллера домена в гибридных сценариях обновленные алгоритмы вступят в силу только после обновления программного обеспечения агента контроллера домена.

## <a name="what-do-users-see"></a>Что видят пользователи?

Когда пользователь пытается сбросить пароль на что-то, которое было бы заблокировано, отображается следующее сообщение об ошибке:

*«Увы, ваш пароль содержит слово, фразу или шаблон, что позволяет легко подобрать пароль. Повторите попытку с другим паролем ".*

## <a name="license-requirements"></a>Требования лицензий

| Пользователи | Защита паролем Azure AD с помощью глобального списка запрещенных паролей | Защита паролем Azure AD с помощью настраиваемого списка запрещенных паролей|
|-------------------------------------------|---------------------------|---------------------------|
| Только пользователи облака                          | Azure AD уровня "Бесплатный"             | Azure AD Premium (P1 или P2) |
| Пользователи, синхронизированные из локальной AD DS | Azure AD Premium (P1 или P2) | Azure AD Premium (P1 или P2) |

> [!NOTE]
> Локальные AD DS пользователи, которые не синхронизируются с Azure AD, также могут воспользоваться преимуществами защиты паролей Azure AD на основе существующих лицензий для синхронизированных пользователей.

Дополнительные сведения о лицензировании, включая расходы, можно найти на [сайте с ценами на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе с пользовательским списком запрещенных паролей, выполните следующие действия.

> [!div class="nextstepaction"]
> [Учебник. Настройка пользовательских запрещенных паролей](tutorial-configure-custom-password-protection.md)

Вы также можете [включить локальную защиту паролей Azure AD](howto-password-ban-bad-on-premises-deploy.md).
