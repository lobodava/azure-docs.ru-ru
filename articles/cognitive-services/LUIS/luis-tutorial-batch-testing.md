---
title: Руководство по пакетному тестированию для поиска проблем — LUIS
description: В этом учебнике показано, как использовать пакетное тестирование для проверки качества приложения службы "Распознавания речи" (LUIS).
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 05/07/2020
ms.openlocfilehash: 7dd181f8cd398dd683296b446028b398a9800b53
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91298326"
---
# <a name="tutorial-batch-test-data-sets"></a>Руководство по Пакетное тестирование наборов данных

В этом учебнике показано, как использовать пакетное тестирование для проверки качества приложения службы "Распознавания речи" (LUIS).

Пакетное тестирование позволяет проверить состояние активной, обученной модели с известным набором помеченных высказываний и сущностей. В пакетном файле в формате JSON добавьте высказывания и установите метки сущностей, которые нужно спрогнозировать внутри высказывания.

Требования к пакетному тестированию.

* Не более 1000 высказываний за тест.
* Без дубликатов.
* Разрешенные типы сущностей — только машинно-обученные.

При использовании приложения, которое отличается от описанного в этом учебнике, *не* используйте примеры речевых фрагментов, которые уже добавлены в приложение.

**В этом руководстве рассмотрено, как выполнять следующие задачи.**

<!-- green checkmark -->
> [!div class="checklist"]
> * Импортировать пример приложения
> * Создание файла пакетного теста
> * Выполнение пакетного теста
> * Просмотр результатов теста

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Импортировать пример приложения

Импортируйте приложение для заказа пиццы, например `1 pepperoni pizza on thin crust`.

1.  Загрузите и сохраните [JSON-файл приложения](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json?raw=true).

1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Импортируйте JSON-файл в новое приложение с именем `Pizza app`.


1. Выберите **Обучение** в правом верхнем углу области навигации, чтобы обучить приложение.

## <a name="what-should-the-batch-file-utterances-include"></a>Что должны содержать речевые фрагменты пакетного файла

Пакетный файл должен включать в себя речевые фрагменты с сущностями машинного обучения верхнего уровня, для которых отмечены начальная и конечная позиции. Речевые фрагменты не должны принадлежать к примерам, которые уже есть в приложении. Это должны быть речевые фрагменты, которые необходимо положительно прогнозировать для намерений и сущностей.

Вы можете разделить тестовые элементы по намерению и (или) сущности или расположить все эти элементы (до 1000 речевых фрагментов) в одном файле.

## <a name="batch-file"></a>Пакетный файл

Пример JSON включает в себя один речевой фрагмент с помеченной сущностью, чтобы проиллюстрировать, как выглядит тестовый файл. В собственных тестах у вас должно быть много речевых фрагментов с правильными намерениями и помеченными сущностями машинного обучения.

1. Создайте файл `pizza-with-machine-learned-entity-test.json` в текстовом редакторе или [скачайте](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/batch-tests/pizza-with-machine-learned-entity-test.json?raw=true) его.

2. В пакетный файл в формате JSON добавьте речевой фрагмент с **намерением**, которое должно быть спрогнозировано в тесте.

   [!code-json[Add the intents to the batch test file](~/samples-cognitive-services-data-files/luis/batch-tests/pizza-with-machine-learned-entity-test.json "Add the intent to the batch test file")]

## <a name="run-the-batch"></a>Запуск пакетного теста

1. Нажмите кнопку **Test** (Тестировать) в верхней панели навигации.

2. На правой панели щелкните ссылку **Batch testing panel** (Панель пакетного тестирования).

3. Нажмите кнопку **Import dataset** (Импортировать набор данных).

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана приложения LUIS с выделенной кнопкой "Import dataset" (Импортировать набор данных)](./media/luis-tutorial-batch-testing/import-dataset-button.png)

4. Выберите расположение для файла `pizza-with-machine-learned-entity-test.json`.

5. Назовите набор данных `pizza test` и выберите **Готово**.

    > [!div class="mx-imgBorder"]
    > ![Выбор файла](./media/luis-tutorial-batch-testing/import-dataset-modal.png)

6. Нажмите кнопку **Запустить**.

7. Щелкните **See results** (Просмотреть результаты).

8. Просмотрите результаты на диаграмме и в условных обозначениях.

## <a name="review-batch-results-for-intents"></a>Просмотр намерений в результатах пакетного тестирования

Результаты теста графически показывают, как тестовые речевые фрагменты спрогнозированы для активной версии.

Пакетная диаграмма отображает четыре квадранта результатов. В правой части диаграммы находится фильтр. Фильтр содержит намерения и сущности. При выборе [раздела диаграммы](luis-concept-batch-test.md#batch-test-results) или точки внутри диаграммы соответствующие высказывания отображаются под диаграммой.

При наведении курсора на диаграмму с помощью колеса мыши можно увеличить или уменьшить отображение. Это полезно в тех случаях, когда на диаграмме собрано много плотно сгруппированных точек.

Диаграмма состоит из четырех квадрантов, два из которых выделены красным цветом.

1. В списке фильтров выберите намерение **ModifyOrder**.

    > [!div class="mx-imgBorder"]
    > ![Выбор намерения ModifyOrder в списке фильтров](./media/luis-tutorial-batch-testing/select-intent-from-filter-list.png)

    Речевой фрагмент спрогнозирован как **истинно положительный результат**. Это означает, что речевой фрагмент совпал с его положительным прогнозом, указанным в пакетном файле.

    > [!div class="mx-imgBorder"]
    > ![Речевой фрагмент совпал с его положительным прогнозом](./media/luis-tutorial-batch-testing/intent-predicted-true-positive.png)

    Зеленые флажки в списке фильтров также указывают на успешное выполнение теста для каждого намерения. Все остальные намерения перечислены с положительным результатом 1/1, потому что речевой фрагмент был протестирован для каждого намерения и является отрицательным для любых намерений, не перечисленных в пакетном тесте.

1. Выберите намерение **Confirmation**. Это намерение не указано в пакетном тестировании, поэтому является отрицательным для речевого фрагмента, который указан в пакетном тесте.

    > [!div class="mx-imgBorder"]
    > ![Речевой фрагмент спрогнозирован как отрицательный для неуказанного намерения в пакетном файле](./media/luis-tutorial-batch-testing/true-negative-intent.png)

    Отрицательный тест выполнен успешно, как отмечено зеленым текстом в фильтре и на сетке.

## <a name="review-batch-test-results-for-entities"></a>Проверка результатов пакетного тестирования для сущностей

Сущность ModifyOrder, как сущность компьютера с подсущностями, показывает, соответствует ли ожиданиям сущность верхнего уровня и как прогнозируются подсущности.

1. Выберите сущность **ModifyOrder** в списке фильтров, а затем выберите круг на сетке.

1. Прогнозирование сущности отображается под диаграммой. При этом отображаются сплошные линии для прогнозов, которые соответствуют ожиданиям, и пунктирные линии — для тех, которые не соответствуют.

    > [!div class="mx-imgBorder"]
    > ![Родительская сущность спрогнозирована в пакетном файле](./media/luis-tutorial-batch-testing/labeled-entity-prediction.png)

## <a name="finding-errors-with-a-batch-test"></a>Поиск ошибок с помощью пакетного тестирования

Из этого учебника вы узнали, как выполнять тестирование и интерпретировать результаты. В нем не рассматривались принципы тестирования или способы реагирования на неудачные тесты.

* Обязательно включите в тестирование как положительные, так и отрицательные речевые фрагменты, в том числе те, которые могут быть спрогнозированы для другого, но связанного намерения.
* В случае ошибки речевого фрагмента выполните приведенные ниже задачи, а затем снова тестирование.
    * Просмотрите текущие примеры намерений и сущностей, проверьте правильность примеров речевых фрагментов активной версии как для намерения, так и для меток сущностей.
    * Добавьте функции, которые помогут приложению прогнозировать намерения и сущности.
    * Добавьте больше положительных примеров речевых фрагментов.
    * Проверьте баланс примеров речевых фрагментов в намерениях.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [LUIS How to clean up resources](./includes/cleanup-resources-preview-portal.md)]

## <a name="next-step"></a>Следующий шаг

В этом учебнике вы использовали пакетное тестирование для проверки текущей модели.

> [!div class="nextstepaction"]
> [Tutorial: Improve app with pattern roles](luis-tutorial-pattern.md) (Руководство. Улучшение приложения с ролями шаблонов)

