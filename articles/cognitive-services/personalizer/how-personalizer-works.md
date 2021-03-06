---
title: Персонализатор. Принцип его работы
description: В _цикле_ персонализации используется машинное обучение для создания модели, которая прогнозирует наибольшее действие для содержимого. Модель обучена исключительно для ваших данных, отправленных в нее с помощью вызовов ранжирования и наград.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: cfbe5cf8c19bfafb38f6149391e09350785ebf9c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91303613"
---
# <a name="how-personalizer-works"></a>Принцип работы Персонализатора

Ресурс персонализации, _цикл обучения_, использует машинное обучение для создания модели, которая прогнозирует самое частное действие для вашего содержимого. Модель обучена исключительно для ваших данных, отправленных в нее с помощью вызовов **ранжирования** и **наград** . Каждый цикл полностью не зависит друг от друга.

## <a name="rank-and-reward-apis-impact-the-model"></a>API ранжирования и наград влияют на модель

Вы отправляете _действия с функциями_ и _контекстными функциями_ в API ранжирования. API **ранжирования** решает, какую из следующих моделей использовать.

* _Эксплойт_. Текущая модель для выбора оптимального действия на основе прошлых данных.
* _Обзор_. Выберите другое действие вместо самого верхнего действия. Вы [настраиваете этот процент](how-to-settings.md#configure-exploration-to-allow-the-learning-loop-to-adapt) для ресурса персонализации в портал Azure.

Вы определяете оценку награды и отправляете этот рейтинг в API-интерфейс наград. API **вознаграждения**:

* собирает данные для обучения модели, записывая характеристики и награды каждого вызова "Ранг";
* Использует эти данные для обновления модели на основе конфигурации, указанной в _политике обучения_.

## <a name="your-system-calling-personalizer"></a>Ваш системный вызов персонализации

На следующем изображении показан архитектурный процесс вызовов "Ранг" и "Вознаграждение".

![Замещающий текст](./media/how-personalizer-works/personalization-how-it-works.png "Принцип работы персонализации")

1. Вы отправляете _действия с функциями_ и _контекстными функциями_ в API ранжирования.

    * Персонализация определяет, следует ли использовать текущую модель или исследовать новые варианты для модели.
    * Результат ранжирования отправляется в EventHub.
1. Высший рейтинг возвращается в систему в качестве _идентификатора действия наград_.
    Система представляет это содержимое и определяет оценку вознаграждений на основе собственных бизнес-правил.
1. Система возвращает оценку награды в цикл обучения.
    * Когда Персонализатор получает вознаграждение, оно отправляется в EventHub.
    * "Ранг" и "Вознаграждение" связаны друг с другом.
    * Модель ИИ обновляется на основе результатов корреляции.
    * Механизм вывода обновляется с помощью новой модели.

## <a name="personalizer-retrains-your-model"></a>Персонализация повторно обучить модель

Персонализация перестраивает модель на основе параметра **обновления частоты модели** в ресурсе персонализации в портал Azure.

Персонализация использует все текущие сохраняемые данные на основе параметра **хранения данных** в днях на ресурсе персонализации в портал Azure.

## <a name="research-behind-personalizer"></a>Исследование вне Персонализатора

Персонализатор базируется на новейших научных и исследовательских разработках в области [обучения с подкреплением](concepts-reinforcement-learning.md), включая статьи, исследовательскую деятельность и текущие области исследований в Microsoft Research.

## <a name="next-steps"></a>Дальнейшие шаги

Сведения о [наиболее частых сценариях](where-can-you-use-personalizer.md) для персонализации