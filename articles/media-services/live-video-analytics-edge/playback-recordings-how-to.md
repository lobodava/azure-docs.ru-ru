---
title: Воспроизведение записей в Azure
description: Вы можете использовать интерактивную аналитику видео на IoT Edge для непрерывной записи видео, позволяя записывать видео в облако для недель или месяцев. Вы также можете ограничить запись клипами, которые представляют интерес, с помощью записи на основе событий. В этой статье рассказывается о том, как воспроизводить записи.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: 6222d2c05b2fe05945d4bcbef6dbb0d64bd4726a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84260387"
---
# <a name="playback-of-recordings"></a>Воспроизведение записей 

## <a name="pre-read"></a>Предварительное чтение  

* [Воспроизведение видео](video-playback-concept.md)
* [Непрерывная запись видео](continuous-video-recording-concept.md)
* [Запись видео на основе событий](event-based-video-recording-concept.md)

## <a name="background"></a>История  

Вы можете использовать интерактивную аналитику видео на IoT Edge для [непрерывной записи видео](continuous-video-recording-concept.md) (КВР), что позволяет записывать видео в облако для недель или месяцев. Вы также можете ограничить запись клипами, которые представляют интерес, с помощью [записи видео на основе событий](event-based-video-recording-concept.md) (Евр). 

Ваша учетная запись службы мультимедиа связана с учетной записью хранения Azure, и при записи видео в облако содержимое записывается в [ресурс-контейнер службы мультимедиа](terminology.md#asset). Службы мультимедиа позволяют выполнять [потоковую передачу содержимого](terminology.md#streaming), как только после завершения записи, так и во время записи. Службы мультимедиа предоставляют необходимые возможности для доставки потоков через протоколы HLS или MPEG-ТИРЕ для воспроизведения устройств (клиентов). Дополнительные сведения см. в статье о [воспроизведении видео](video-playback-concept.md) . Интерфейсы API службы мультимедиа используются для создания URL-адресов для потоковой передачи реквизитов, используемых клиентами для воспроизведения видео & аудио. Сведения о создании URL-адреса потоковой передачи для ресурса см. в разделе [Создание указателя потоковой передачи и создание URL-адресов сборки](../latest/create-streaming-locator-build-url.md). После создания URL-адреса потоковой передачи для ресурса можно и сохранить сопоставление URL-адреса с источником видео (то есть с камерой) в системе управления содержимым.

Например, при потоковой передаче через HLS URL-адрес потоковой передачи будет выглядеть следующим образом `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl).m3u8` . Для MPEG-ТИРЕ он будет выглядеть так `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=mpd-time-cmaf).mpd` :.

Он возвращает файл манифеста, который, среди прочего, описывает общую длительность доставляемого потока, а также то, представляет ли он предварительно записанное содержимое, или активный веб-канал.

### <a name="live-vs-vod"></a>Динамическое сравнение с VoD  

Такие протоколы потоковой передачи, как HLS и MPEG-ТИРЕ, созданы для обработки таких сценариев, как потоковая передача видеороликов, а также потоковая передача и предварительно записанного содержимого, например ТЕЛЕВИЗИОНных передач и фильмов. В реальном времени клиенты HLS и MPEG-ТИРЕ предназначены для начала воспроизведения с самого последнего. Тем не менее, с помощью фильмов читатели хотят начать с начала и перейти к тому, если они решили. Манифесты HLS и MPEG-ТИРЕ имеют флаги, указывающие клиентам, что видео представляет собой динамический поток, или это предварительно записанное содержимое.
Эта концепция также применяется к потокам HLS и MPEG-ТИРЕ из ресурсов, которые содержат видео, записанное с помощью функции Live Video Analytics, на IoT Edge.

## <a name="browsing-and-selective-playback-of-recordings"></a>Просмотр и выборочное воспроизведение записей  

Рассмотрим ситуацию, когда вы использовали Live Video Analytics на IoT Edge для записи видео только из 8:00 в 10:00 в течение дня, когда была открыта школа, для всего учебного года. Или, возможно, вы записываете видео только из 7:00 в 19:00 на национальных праздниках. В любом из этих двух сценариев при попытке просмотра и просмотра записи видео потребуется:

* Способ определить, какие даты и часы видео вы используете в записи.
* Способ выбора части (например, 9:00 в 11:00 в течение нового года) записи для воспроизведения.

Службы мультимедиа предоставляют API запросов (Аваилаблемедиа) для устранения первой проблемы, а также фильтры диапазона времени (startTime, endTime) для устранения второй ошибки.

## <a name="query-api"></a>API запроса 

При использовании КВР устройства воспроизведения (клиенты) не могут запрашивать манифест, охватывающий воспроизведение всей записи.  При записи в несколько лет создается файл манифеста, который был слишком большим для воспроизведения, и было бы неудобно разбираться в пригодных для использования частях на стороне клиента.  Клиенту необходимо знать, какие диапазоны DateTime содержат данные в записи, чтобы они могли выполнять допустимые запросы без значительного предположения. Именно здесь поступает новый API запроса. Теперь клиенты могут использовать этот API на стороне сервера для обнаружения содержимого.

Значение точности может быть одним из следующих: год, месяц, день или полная (как показано ниже). 

|Точность|year|month|day|переполненные|
|---|---|---|---|---|
|Запрос|`/availableMedia?precision=year&startTime=2018&endTime=2019`|`/availableMedia?precision=month& startTime=2018-01& endTime=2019-02`|`/availableMedia?precision=day& startTime=2018-01-15& endTime=2019-02-02`|`/availableMedia?precision=full& startTime=2018-01-15T10:08:11.123& endTime=2019-01-015T12:00:01.123`|
|Ответ|`{  "timeRanges":[{ "start":"2018", "end":"2019" }]}`|`{  "timeRanges":[{ "start":"2018-03", "end":"2019-01" }]}`|`{  "timeRanges":[    { "start":"2018-03-01", "end":"2018-03-07" },    { "start":"2018-03-09", "end":"2018-03-31" }  ]}`|Полная точность ответа. При отсутствии пробелов начальное значение будет равно startTime, а End — endTime.|
|Ограничивает|&#x2022;startTime <= endTime<br/>&#x2022;оба значения должны быть в формате гггг, в противном случае возвращает ошибку.<br/>Значение &#x2022;может быть любым числом лет друг от друга.<br/>Значения &#x2022;являются инклюзивными.|&#x2022;startTime <= endTime<br/>&#x2022;оба значения должны быть в формате гггг-мм, в противном случае возвращает ошибку.<br/>Значения &#x2022;могут быть не более 12 месяцев друг от друга.<br/>Значения &#x2022;являются инклюзивными.|&#x2022;startTime <= endTime<br/>&#x2022;оба значения должны быть в формате гггг-мм-дд, в противном случае возвращает ошибку.<br/>Значения &#x2022;могут быть не более 31 дня.<br/>Значения являются инклюзивными.|&#x2022;startTime < endTime<br/>&#x2022;значения могут составлять не более 25 часов.<br/>Значения &#x2022;являются инклюзивными.|

#### <a name="additional-request-format-considerations"></a>Дополнительные рекомендации по формату запроса

* Все значения времени задаются в формате UTC
* Параметр строки запроса точности является обязательным.  
* Параметры строки запроса startTime и endTime необходимы для значений месяца, дня и полной точности.  
* Параметры строки запроса startTime и endTime являются необязательными для значения точности года (не поддерживается ни один, ни и то, и другое).  

    * Если параметр startTime опущен, предполагается, что это первый год записи.
    * Если значение endTime опущено, то предполагается, что это последний год записи.
    * Например, если запись началась в 2011 и продолжается до 2020, то:

        * /Аваилаблемедиа? точность = год совпадает с/Аваилаблемедиа? точность = год&startTime = 2011&endTime = 2020
        * /Аваилаблемедиа? точность = год&startTime = 2015 совпадает с/Аваилаблемедиа? точность = год&startTime = 2015&endTime = 2020
        * /Аваилаблемедиа? точность = год&endTime = 2018 совпадает с/Аваилаблемедиа? точность = год&startTime = 2011&endTime = 2018

Если для диапазонов времени нет доступных данных мультимедиа, возвращается пустой список.

Если ресурс не содержит записи из графа мультимедиа, будет возвращен ответ HTTP 400 с сообщением об ошибке, объясняющим, что эта функция доступна только для содержимого, записанного с помощью графа мультимедиа.

Если длительность времени, заданная параметрами, превышает максимальный диапазон времени для заданного типа запроса, возвращается HTTP 400 с сообщением об ошибке, объясняющим максимально допустимый диапазон времени для запрошенного типа запроса.

Предполагая, что диапазон времени допустим для заданных параметров, для запросов, находящихся за пределами временного окна записи, ошибки не возвращаются.  Если запись начата 7 часов назад, но клиент запрашивает доступный носитель с {UtcNow – 24 часа} до UtcNow, то мы бы возвращали данные {UtcNow – 7} в часах.

### <a name="response-format"></a>Формат ответа  

Вызов Аваилаблемедиа возвращает набор значений времени.

Ответ будет являться телом JSON, где значения Тимеранжес представляют собой массив дат ISO 8601 UTC для года (в формате гггг), месяц (в формате гггг-мм) или Day (гггг-мм-дд) в зависимости от запрошенной точности.  Полная Тимеранжес будет содержать начальное и конечное значения, отформатированное как время в формате ISO 8601 UTC (в формате гггг-мм-DDThh: мм: СС. СССЗ).

Для запроса Аваилаблемедиа API будет отключаться от временной шкалы видео. Все несоответствия на временной шкале будут отображаться как пропуски в ответе.

#### <a name="response-example-for-availablemedia"></a>Пример ответа для Аваилаблемедиа

##### <a name="example-1-live-recording-with-no-gaps"></a>Пример 1. динамическая запись без пробелов

Ниже приведен ответ, показывающий все доступные носители на 21 декабря 2019, где запись была 100% завершено. Ответ имеет только одну пару "начало-конец".

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
   {
         "start":"2019-12-21T00:00:00.000Z", 
         "end":"2019-12-22T00:00:00.000Z"
    }
   ]
}
```

##### <a name="example-2-live-recording-with-a-2-second-gap"></a>Пример 2. динамическая запись с зазором в 2 секунды

Предположим, что по какой-то причине камере не удалось отправить допустимое видео в течение интервала в 2 секунды в день. Ниже приведен ответ, показывающий все доступные носители 21 декабря 2019:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:01:08Z"
    },
    {
         "start":"2019-12-21T04:01:10Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

##### <a name="example-3-live-recording-with-an-8-hour-gap"></a>Пример 3. динамическая запись с пропускной ошибкой в 8 часов

Предположим, что камера и (или) локальная система потеряют питание в течение 8-часового периода в течение дня, с 4 по UTC до полудня в формате UTC и не существовало источника питания для резервного копирования. Ниже приведен ответ, показывающий все доступные носители за такой день

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:00:00Z"
    },
    {
         "start":"2019-12-21T12:00:00Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

#### <a name="example-4-live-recording-where-no-video-is-recorded-over-summer-holidays"></a>Пример 4. динамическая запись, в которой нет видео, записанных в течение лета праздников

Предположим, что видео записано только в том случае, если учебное заведение находится в сеансе, а запись была остановлена с 17 июня по 6 сентября. Запрос на доступ к месяцам будет выглядеть следующим образом:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=month&startTime=2019-01&endTime=2019-12
{
   "timeRanges":[
    {
         "start":"2019-01", 
         "end":"2019-06"
    },
    {
         "start":"2019-09", 
         "end":"2019-12"
    }
   ]
}
```

Если вы запросили доступ к доступным дням в июне, вы увидите следующее:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=day&startTime=2019-06-01&endTime=2019-06-30
{
   "timeRanges":[
    {
         "start":"2019-06-01", 
         "end":"2019-06-17"
    }
   ]
}
```

##### <a name="example-5-live-recording-where-no-video-is-recorded-over-weekends-or-holidays"></a>Пример 5. динамическая запись, в которой нет видео, записываемых в выходные или праздничные дни

Предположим, вы записали видео только в рабочее время. Запрос на доступ к доступным дням будет выглядеть следующим образом:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=day&startTime=2020-02-01&endTime=2020-02-29
{
   "timeRanges":[
    {
         "start":"2020-02-03", 
         "end":"2020-02-07"
    },
    {
         "start":"2020-02-10", 
         "end":"2020-02-14"
    },
    {
         "start":"2020-02-18", // Monday Feb 17th was a holiday
         "end":"2020-02-21"
    },
    {
         "start":"2020-02-24", 
         "end":"2020-02-28"
    }
   ]
}
```

## <a name="time-range-filters"></a>Фильтры диапазона времени

Как упоминалось выше, эти фильтры помогают выбрать фрагменты записи (например, от 9:00 до 11:00 в течение нового года) для воспроизведения. При потоковой передаче через HLS URL-адрес потоковой передачи будет выглядеть следующим образом `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl).m3u8` . Чтобы выбрать часть записи, необходимо добавить параметр startTime и endTime, например: `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T08:00:00Z,endTime=2019-12-21T10:00:00Z).m3u8` . Таким образом, фильтры диапазона времени являются модификаторами URL-адресов, используемыми для описания части временной шкалы записи, включенной в манифест потоковой передачи:

* `starttime` — Это метка даты и времени ISO 8601, описывающая требуемое время начала временной шкалы видео в возвращенном манифесте.
* `endtime` — Это метка даты и времени ISO 8601, описывающая требуемое время окончания временной шкалы видео, возвращаемого в манифесте.

Максимальная длина такого манифеста не может превышать 24 часа.

Ниже приведены ограничения на фильтры.

|startTime| endTime |Результат|
|---|---|---|
|Absent |Absent |Возвращает последнюю часть видео в ресурсе до максимальной длины в 4 часа.<br/><br/>Если ресурс не был записан в (в последнее время поступают новые данные видео), то возвращается манифест по запросу (VoD). В противном случае возвращается динамический манифест.|
|Присутствует|Absent|    Возврат манифеста с любым доступным видео, более новым по сравнению с startTime, если такой манифест будет короче 24 часов.<br/>Если длина манифеста превысит 24 часа, то возвращается ошибка.<br/>Если ресурс не был записан в (в последнее время поступают новые данные видео), то возвращается манифест по запросу (VoD). В противном случае возвращается динамический манифест.
|Absent|Присутствует |Ошибка — если параметр startTime имеется, то параметр endTime является обязательным.|
|Присутствует|Присутствует|Возврат манифеста VoD с любым доступным видео между startTime и endTime.<br/>Диапазон (startTime, endTime) не может превышать 24 часа.|

### <a name="response-examples"></a>Примеры ответов  

#### <a name="example-1-live-recording-with-no-gaps"></a>Пример 1. динамическая запись без пробелов

Ниже приведен ответ, показывающий все доступные носители на 21 декабря 2019, где запись была 100% завершено. Ответ имеет только одну пару "начало-конец".

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
   {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

Если запись непрерывна, можно запросить манифесты HLS или ТИРЕ для любой части времени в пределах пары "начало-конец", если диапазон составляет 24 часа или меньше. Ниже приведен пример для 4-часового запроса манифеста HLS, приведенного выше.

`GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T14:00:00.000Z,endTime=2019-12-21T18:00:00.000Z).m3u8`

#### <a name="example-2-live-recording-with-a-2-second-gap"></a>Пример 2. динамическая запись с зазором в 2 секунды

Предположим, что по какой-то причине камере не удалось отправить допустимое видео в течение интервала в 2 секунды в день. Ниже приведен ответ, показывающий доступный носитель для 21 декабря 2019:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:01:08Z"
    },
    {
         "start":"2019-12-21T04:01:10Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

С записью, такой как приведенный выше, опять же, можно было бы запросить манифесты HLS и ТИРЕ с любыми парами startTime/endTime, если диапазон был меньше 24 часов. Если эти значения приводят к пропуску в 04:01:08AM UTC, возвращаемый манифест будет сообщать о прекращении непрерывности на временной шкале мультимедиа в соответствии с соответствующими характеристиками этих протоколов.

#### <a name="example-3-live-recording-with-an-8-hour-gap"></a>Пример 3. динамическая запись с 8-часовым пропуском

Предположим, что камера и (или) локальная система потеряют питание в течение 8-часового периода в течение дня, с 4 по UTC до полудня в формате UTC и не существовало источника питания для резервного копирования. Ниже приведен ответ, показывающий все доступные носители в течение дня.

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:00:00Z"
    },
    {
         "start":"2019-12-21T12:00:00Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

С такой записью:

* Если вы запрашиваете манифест, где оба фильтра диапазона startTime/endTime находятся внутри частей "Available" временной шкалы, а именно от полуночи до 4AM или от полудня до полуночи, служба вернет манифест, охватывающий время от startTime до endTime, например:

    `GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T14:01:00.000Z,endTime=2019-12-21T03:00:00.000Z).m3u8`
* Если вы запрашиваете манифест, где время startTime и endTime находились внутри отверстия в середине, скажем, из 8:00 в 10:00 UTC, служба будет вести себя так же, как если бы фильтр ресурсов мог привести к пустому результату.

    [Это запрос, который получает пустой ответ] `GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T08:00:00.000Z,endTime=2019-12-21T10:00:00.000Z).m3u8`
* Если вы запрашиваете манифест, в котором только один из времени startTime или endTime находится внутри отверстия, возвращаемый манифест будет включать только часть этого временного интервала. Он привязывает значение startTime или endTime к ближайшей допустимой границе. Например, если вы запросили поток из 3 часов из 10:00 в 13:00-14:30, ответ будет содержать по 1-часовому носителю для 12-полудня до 13:00-14:30

    `GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T10:00:00.000Z,endTime=2019-12-21T13:00:00.000Z).m3u8`
    
    Возвращаемый манифест будет начинаться с 2019-12-21T12:00:00.000 Z, даже если запрос запросил начало 10:00. При создании системы управления видео с подключаемым модулем проигрывателя необходимо соблюдать осторожность, чтобы сообщить об этом средству просмотра. Неосведомленное средство просмотра будет путать с тем, почему временная шкала воспроизведения начинается с полудня и не 10:00 по запросу.

## <a name="recording-and-playback-latencies"></a>Задержка записи и воспроизведения

При использовании функции Live Video Analytics на IoT Edge для записи в ресурс необходимо указать свойство Сегментленгс, которое сообщает модулю о том, что необходимо выполнить статистическую обработку минимальной длительности видео (в секундах) перед его записью в облако. Например, если для Сегментленгс задано значение 300, то модуль будет накапливаться в течение 5 минут перед отправкой фрагмента 1 5 минут, затем перейдет в режим накопления в течение следующих 5 минут и отправит еще раз. Увеличение Сегментленгс имеет преимущество снижения затрат на транзакции в службе хранилища Azure, так как количество операций чтения и записи будет выполняться не чаще одного раза в секунду каждые Сегментленгс секунд.

Следовательно, потоковая передача видео из служб мультимедиа будет отложена по крайней мере на столько времени. 

Другой фактор, определяющий задержку воспроизведения (задержка между временем возникновения события перед камерой, до момента, когда оно может быть просмотрено на устройстве воспроизведения) — это [GOP](https://en.wikipedia.org/wiki/Group_of_pictures) длительность группы изображений. Так как [уменьшение задержки потоков в реальном времени с помощью 3 простых метода](https://medium.com/vrt-digital-studio/reducing-the-delay-of-live-streams-by-using-3-simple-techniques-e8e028b0a641) объясняет, что превышает GOP длительность, что превышает задержку. Обычно IP-камеры используются в сценариях наблюдения и безопасности, настроенных для использования группы GOP дольше 30 секунд. Это оказывает значительное влияние на общую задержку.

## <a name="next-steps"></a>Дальнейшие действия

[Руководство по непрерывной записи видео](continuous-video-recording-tutorial.md)
