---
title: Обновление API Bing для проверки орфографии версии 5 до версии 7
titleSuffix: Azure Cognitive Services
description: Определяет части приложения Проверка орфографии Bing, которые необходимо обновить для использования версии 7.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: ac581b96ba4e29837570ab8c01bf6c927eb34a28
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2020
ms.locfileid: "94367528"
---
# <a name="spell-check-api-upgrade-guide"></a>Руководство по обновлению API для проверки орфографии

> [!WARNING]
> API-интерфейсы поиска Bing перемещаются из Cognitive Services в Поиск Bing службы. Начиная с **30 октября 2020** , все новые экземпляры Поиск Bing должны быть подготовлены, следуя описанному [здесь](https://aka.ms/cogsvcs/bingmove)процессу.
> API-интерфейсы поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до конца Соглашение Enterprise, в зависимости от того, что происходит раньше.
> Инструкции по миграции см. в разделе [Поиск Bing Services](https://aka.ms/cogsvcs/bingmigration).

В этом руководстве по обновлению определены изменения между версиями 5 и 7 API Bing для проверки орфографии. Руководство поможет определить компоненты приложения, которые необходимо обновить для использования версии 7.

## <a name="breaking-changes"></a>Критические изменения

### <a name="endpoints"></a>Конечные точки

- Номер версии конечной точки изменен с 5 на 7. Например, `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Объекты ответов на ошибки и коды ошибок

- Все невыполненные запросы теперь должны включать в тело ответа объект `ErrorResponse`.

- В объект `Error` добавлены следующие поля:  
  - `subCode`&mdash;Разделяет код ошибки на дискретные сегменты, если это возможно
  - `moreDetails`&mdash;Дополнительные сведения об ошибке, описанной в поле `message`
   

- Коды ошибок версии 5 заменены следующими возможными значениями `code` и `subCode`.  
  
|Код|SubCode (дополнительный код)|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing возвращает ServerError (ошибку сервера) каждый раз при возникновении любого из условий дополнительного кода. Ответ включает в себя ошибки, если код состояния HTTP — 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Блокировано|Bing возвращает ошибку InvalidRequest (недопустимый запрос) каждый раз, когда любая часть запроса недопустима. Например, отсутствует обязательный параметр или значение параметра недопустимо.<br/><br/>В случае ошибки ParameterMissing или ParameterInvalidValue возвращается код состояния HTTP 400.<br/><br/>При ошибке HttpNotAllowed (HTTP запрещен) будет наблюдаться код состояния HTTP 410.
|RateLimitExceeded||Bing возвращает ошибку RateLimitExceeded всякий раз при превышении квоты запросов в секунду (QPS) или запросов в месяц (QPM).<br/><br/>Bing возвращает код состояния HTTP 429 при превышении квоты QPS и 403 при превышении QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing возвращает InvalidAuthorization, когда Bing не может проверить подлинность вызывающего объекта. Например, когда заголовок `Ocp-Apim-Subscription-Key` отсутствует или при недопустимом ключе подписки.<br/><br/>Избыточность возникает, если указать более одного способа проверки подлинности.<br/><br/>При ошибке InvalidAuthorization кодом состояния HTTP будет 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing возвращает InsufficientAuthorization, когда вызывающая сторона не имеет разрешений на доступ к ресурсу. Это может произойти, если ключ подписки отключен или срок его действия истек. <br/><br/>При ошибке InsufficientAuthorization возвращается код состояния HTTP 403.

- Ниже представлено сопоставление предыдущих кодов ошибок с новыми. При наличии зависимостей от кодов ошибок версии 5 обновите свой код соответствующим образом.  
  
|Код версии 5|Код версии 7.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Выключено|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Блокировано|InvalidRequest.Blocked

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Требования к использованию и отображению](../bing-web-search/use-display-requirements.md)