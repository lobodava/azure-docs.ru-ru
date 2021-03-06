---
title: Что такое Виртуальный рабочий стол Windows — Azure
description: Общие сведения о Виртуальном рабочем столе Windows.
author: Heidilohr
ms.topic: overview
ms.date: 09/14/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 03566dccbb453aa06a2b5f86bd02b86d85d61b28
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91322059"
---
# <a name="what-is-windows-virtual-desktop"></a>Что такое Виртуальный рабочий стол Windows

Виртуальный рабочий стол Windows — это облачное решение для виртуализации рабочих столов и приложений.

При запуске Виртуального рабочего стола Windows в Azure доступны следующие возможности:

* Настройка развертывания Windows 10 с поддержкой нескольких сеансов со всеми соответствующими возможностями и функцией масштабируемости.
* Виртуализация приложений Microsoft 365 для предприятий с последующей оптимизацией включения поддержки виртуальных сценариев с несколькими пользователями.
* Предоставление виртуальных рабочих столов Windows 7 с бесплатными расширенными обновлениями системы безопасности.
* Перенос существующих служб удаленных рабочих столов, а также рабочих столов и приложений Windows Server на любой компьютер.
* Виртуализация рабочих столов и приложений.
* Управление рабочими столами и приложениями Windows 10, Windows Server и Windows 7 с помощью унифицированных средств управления.

## <a name="introductory-video"></a>Вступительное видео

Узнайте о Виртуальных рабочих столах Windows, их уникальности и новых возможностях из этого видео:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Дополнительные видеоролики о Виртуальных рабочих столах Windows см. в [списке воспроизведения](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev).

## <a name="key-capabilities"></a>Ключевые возможности

С помощью Виртуального рабочего стола Windows можно настроить масштабируемую и гибкую среду, используя следующие возможности:

* Создание среды виртуализации рабочих столов со всеми возможностями в подписке Azure без необходимости запускать дополнительные серверы шлюзов.
* Публикация требуемого количества пулов узлов для размещения различных рабочих нагрузок.
* Возможность использования собственных образов для рабочих нагрузок или тестирования из коллекции Azure.
* Минимизация расходов благодаря использованию ресурсов в составе пула с поддержкой нескольких сеансов. Благодаря поддержке нескольких сеансов в Windows 10 Корпоративная, реализуемой исключительно в рамках роли Виртуального рабочего стола Windows и узла сеансов удаленных рабочих столов в Windows Server, можно значительно уменьшить количество виртуальных машин и снизить нагрузку на ОС, предоставляя пользователям те же ресурсы.
* Назначение отдельных ролей владельца через личные рабочие столы (постоянные).

Развертывание и администрирование виртуальных рабочих столов предусматривает следующие задачи:

* Использование портала Azure, интерфейсов REST и PowerShell Виртуального рабочего стола Windows для настройки пулов узлов, создания групп приложений, назначения пользователей и публикации ресурсов.
* Публикация полных классических или отдельных удаленных приложений из одного пула узлов, создание отдельных групп приложений для разных групп пользователей и даже назначение пользователям нескольких групп приложений для минимизации количества используемых образов.
* Использование при управлении средой функции встроенного делегированного доступа для назначения ролей и сбора диагностики с последующим анализом различных пользовательских конфигураций и ошибок.
* Использование новой службы диагностики для устранения неполадок.
* Управление только образами и виртуальными машинами, а не инфраструктурой. Отсутствие необходимости лично управлять ролями удаленного рабочего стола, как при использовании служб удаленных рабочих столов; осуществляется управление только виртуальными машинами в подписке Azure.

Назначение пользователям виртуальных рабочих столов и подключение к ним.

* После назначения пользователи могут запускать любой клиент Виртуального рабочего стола Windows для подключения пользователей к опубликованным рабочим столам и приложениям Windows. Подключение с любого устройства через собственное приложение на устройстве или веб-клиент HTML5 Виртуального рабочего стола Windows.
* Безопасное обратное подключение пользователей к службе для предотвращения работы с открытыми входящими портами.

## <a name="requirements"></a>Требования

Есть несколько вещей, которые необходимо настроить для Виртуального рабочего стола Windows, чтобы успешно подключить пользователей к рабочим столам и приложениям Windows.

Мы поддерживаем приведенные ниже операционные системы. Поэтому убедитесь, что у вас есть [необходимые лицензии](https://azure.microsoft.com/pricing/details/virtual-desktop/) для пользователей, соответствующие компьютеру и приложениям, которые вы планируете развернуть.

|OS|Требуемая лицензия|
|---|---|
|Windows 10 Корпоративная с поддержкой нескольких сеансов или Windows 10 Корпоративная|Microsoft 365 E3, E5, A3, A5, F3, бизнес премиум<br>Windows E3, E5, A3, A5|
|Windows 7 Корпоративная |Microsoft 365 E3, E5, A3, A5, F3, бизнес премиум<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Клиентская лицензия служб удаленных рабочих столов с поддержкой программы Software Assurance|

Требования к инфраструктуре для включения поддержки Виртуального рабочего стола Windows следующие:

* Наличие [Azure Active Directory](/azure/active-directory/).
* Windows Server Active Directory с синхронизацией с Azure Active Directory. Ее можно настроить с помощью Azure AD Connect (для гибридных организаций) или доменных служб Azure AD (для гибридных или облачных организаций).
  * Windows Server AD с синхронизацией с Azure Active Directory. Пользователь извлекается из Windows Server AD, а виртуальная машина из Виртуального рабочего стола Windows присоединяется к домену Windows Server AD.
  * Windows Server AD с синхронизацией с Azure Active Directory. Пользователь извлекается из Windows Server AD, а виртуальная машина из Виртуального рабочего стола Windows присоединяется к домену доменных служб Azure AD.
  * Домен в доменной службе Azure AD. Пользователь извлекается из Azure Active Directory, а виртуальная машина из Виртуального рабочего стола Windows присоединяется к домену доменных служб Azure AD.
* Подписка Azure с тем же родительским клиентом Azure AD, в котором находится виртуальная сеть, содержащая экземпляр Windows Server Active Directory или Azure AD DS либо подключенная к нему.

Для подключения к Виртуальному рабочему столу Windows к пользователю выдвигаются следующие требования:

* Пользователь должен быть получен из той же службы Active Directory, которая подключена к Azure AD. Виртуальный рабочий стол Windows не поддерживает учетные записи B2B или MSA.
* Имя участника-пользователя, используемое для подписки на Виртуальный рабочий стол Windows, должно существовать в домене Active Directory, к которому присоединена виртуальная машина.

Требования к виртуальным машинам Azure, созданным для Виртуального рабочего стола Windows, следующие:

* [Стандартное присоединение к домену](../active-directory-domain-services/active-directory-ds-comparison.md) или [гибридное присоединение к AD](../active-directory/devices/hybrid-azuread-join-plan.md). Виртуальные машины не могут быть присоединены к Azure AD.
* Использование одного из следующих [поддерживаемых образов ОС](#supported-virtual-machine-os-images).

>[!NOTE]
>Если у вас нет подписки Azure, можно [зарегистрироваться и получить бесплатную пробную версию для использования в течение месяца](https://azure.microsoft.com/free/). Если вы используете бесплатную пробную версию Azure, следует использовать доменные службы Azure AD для синхронизации Windows Server Active Directory с Azure Active Directory.

Список URL-адресов, которые нужно разблокировать для развертывания Виртуального рабочего стола Windows, см. в статье [Список надежных URL-адресов](safe-url-list.md).

Виртуальный рабочий стол Windows включает рабочие столы и приложения Windows, предоставляемые пользователям, а также решение для управления, размещаемое как услуга в Azure корпорацией Майкрософт. Рабочие столы и приложения можно развертывать на виртуальных машинах в любом регионе Azure. При этом решение для управления и данные для этих виртуальных машин будут размещаться в США. Это может привести к переносу данных в США.

Для обеспечения оптимальной производительности убедитесь, что сети соответствует следующим требованиям:

* Задержка кругового пути при передаче данных из сети клиента в регион Azure, где были развернуты пулы узлов, должна составлять менее 150 мс. Воспользуйтесь [оценщиком удобства работы](https://azure.microsoft.com/services/virtual-desktop/assessment), чтобы просмотреть сведения о работоспособности подключения и рекомендуемом регионе Azure.
* Сетевой трафик может передаваться за пределы страны или региона, когда виртуальные машины, на которых размещены рабочие столы и приложения, подключаются к службе управления.
* Чтобы оптимизировать производительность сети, мы рекомендуем, чтобы виртуальные машины узла сеансов совместно размещались в том же регионе Azure, что и служба управления.

Стандартные варианты реализации службы "Виртуальный рабочий стол Windows" для предприятий см. в статье [Виртуальный рабочий стол Windows для предприятий](https://docs.microsoft.com/azure/architecture/example-scenario/wvd/windows-virtual-desktop).

## <a name="supported-remote-desktop-clients"></a>Поддерживаемые клиенты удаленного рабочего стола

Виртуальный рабочий стол Windows поддерживают следующие клиенты удаленного рабочего стола:

* [Классические приложения Windows](connect-windows-7-10.md)
* [Web](connect-web.md)
* [macOS](connect-macos.md)
* [iOS](connect-ios.md)
* [Android](connect-android.md)
* Клиент Microsoft Store

> [!IMPORTANT]
> Виртуальный рабочий стол Windows не поддерживает клиент Подключения к удаленным рабочим столам и приложениям RemoteApp (RADC) или клиент Подключения к удаленному рабочему столу (MSTSC).

Дополнительные сведения об URL-адресах, которые нужно разблокировать для использования клиентов, см. в статье [Список надежных URL-адресов](safe-url-list.md).

## <a name="supported-virtual-machine-os-images"></a>Поддерживаемые образы ОС виртуальной машины

Виртуальный рабочий стол Windows поддерживает следующие образы 64-разрядных операционных систем:

* Windows 10 Корпоративная с поддержкой нескольких сеансов, 1809 или более поздней версии
* Windows 10 Корпоративная, 1809 или более поздней версии
* Windows 7 Корпоративная
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Виртуальный рабочий стол Windows не поддерживает образы операционных систем x86 (32-разрядная), Windows 10 Корпоративная N или Windows 10 Корпоративная KN. Windows 7 также не поддерживает решения профилей на основе VHD или VHDX, размещенные в управляемой службе хранилища Azure, из-за ограничения размера сектора.

Доступные варианты автоматизации и развертывания зависят от выбранной ОС и версии, как показано в приведенной ниже таблице.

|Операционная система|Коллекция образов Azure|Развертывание ВМ вручную|Интеграция шаблона Azure Resource Manager|Подготовка пулов узлов в Azure Marketplace|
|--------------------------------------|:------:|:------:|:------:|:------:|
|Windows 10 Корпоративная (с поддержкой нескольких сеансов), версия 2004|Да|Да|Да|Да|
|Windows 10 Корпоративная (с поддержкой нескольких сеансов), версия 1909|Да|Да|Да|Да|
|Windows 10 Корпоративная (с поддержкой нескольких сеансов), версия 1903|Да|Да|Нет|Нет|
|Windows 10 Корпоративная (с поддержкой нескольких сеансов), версия 1809|Да|Да|Нет|Нет|
|Windows 7 Корпоративная|Да|Да|Нет|Нет|
|Windows Server 2019|Да|Да|Нет|Нет|
|Windows Server 2016|Да|Да|Да|Да|
|Windows Server 2012 R2|Да|Да|Нет|Нет|

## <a name="next-steps"></a>Дальнейшие действия

Если вы используете Виртуальный рабочий стол Windows (классический), работу с учебником можно начать со статьи [Создание клиента в виртуальном рабочем столе Windows](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).

Если вы используете Виртуальный рабочий стол Windows с интеграцией с Azure Resource Manager, вам необходимо создать пул узлов. Чтобы приступить к работе, перейдите к следующему учебнику.

> [!div class="nextstepaction"]
> [Создание пула узлов на портале Azure](create-host-pools-azure-marketplace.md)
