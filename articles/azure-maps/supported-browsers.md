---
title: Поддерживаемые браузеры веб-пакета SDK | Карты Microsoft Azure
description: Узнайте, как проверить, поддерживает ли веб-пакет SDK Azure Maps браузер. Просмотрите список поддерживаемых браузеров. Узнайте, как использовать службы Map Services с устаревшими браузерами.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 43bf70d66c42bc0ecd5e26e4cc724456bd4bf84e
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92896928"
---
# <a name="web-sdk-supported-browsers"></a>Браузеры, поддерживаемые в веб-пакетах SDK

Веб-пакет SDK Azure Maps предоставляет вспомогательную функцию с именем [Atlas. support](/javascript/api/azure-maps-control/atlas#issupported-boolean-). Эта функция определяет, имеет ли веб-браузер минимальный набор функций WebGL, необходимых для поддержки загрузки и отрисовки элемента управления картой. Ниже приведен пример использования функции.

```JavaScript
if (!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if (!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Классические приложения

Веб-пакет SDK Azure Maps поддерживает следующие браузеры для настольных систем:

- Microsoft ребро (Текущая и Предыдущая версии)
- Google Chrome (Текущая и Предыдущая версии)
- Mozilla Firefox (Текущая и Предыдущая версии)
- Apple Safari (Mac OS X) (Текущая и Предыдущая версии)

См. также в разделе [устаревшие браузеры](#Target-Legacy-Browsers) ниже в этой статье.

## <a name="mobile"></a>Мобильные приложения

Веб-пакет SDK Azure Maps поддерживает следующие мобильные браузеры:

- Android
  - Текущая версия Chrome на Android 6,0 и более поздних версиях
  - Chrome WebView в Android 6,0 и более поздних версиях
- iOS
  - Mobile Safari в текущей и предыдущей основной версии iOS
  - Уивебвиев и Вквебвиев в текущей и предыдущей основной версии iOS
  - Текущая версия Chrome для iOS

> [!TIP]
> При внедрении схемы в мобильное приложение с помощью элемента управления WebView можно использовать [пакет NPM веб-пакета sdk Azure Maps](https://www.npmjs.com/package/azure-maps-control) вместо ссылки на версию пакета SDK, размещенного в сети доставки содержимого Azure. Такой подход сокращает время загрузки, так как пакет SDK уже находится на устройстве пользователя и не требуется загружать его во время выполнения.

## <a name="nodejs"></a>Node.js

В Node.js также поддерживаются следующие модули веб-пакета SDK:

- Модуль служб ([Документация](how-to-use-services-module.md)  |  [NPM Module](https://www.npmjs.com/package/azure-maps-rest))

## <a name="target-legacy-browsers"></a><a name="Target-Legacy-Browsers"></a>Целевые браузеры прежних версий

Вы можете использовать более старые браузеры, которые не поддерживают WebGL или имеют только ограниченную поддержку. В таких случаях рекомендуется использовать службы Azure Maps вместе с элементом управления картой с открытым кодом, например [леафлет](https://leafletjs.com/). Ниже приведен пример:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps + Леафлет" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
См. раздел "перо <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + леафлет</a> by Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) в <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о веб-пакете SDK для Azure Maps:

[Элемент управления картой](how-to-use-map-control.md)

[Модуль служб](how-to-use-services-module.md)