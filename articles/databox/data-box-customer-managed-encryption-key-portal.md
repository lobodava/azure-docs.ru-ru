---
title: Использование портал Azure для управления ключами, управляемыми клиентом, для Azure Data Box
description: Узнайте, как использовать портал Azure для создания ключей, управляемых клиентом, и управления ими с помощью Azure Key Vault для Azure Data Box. Ключи, управляемые клиентом, позволяют создавать, поворачивать, отключать и отзывать элементы управления доступом.
services: databox
author: alkohli
ms.service: databox
ms.topic: how-to
ms.date: 11/19/2020
ms.author: alkohli
ms.subservice: pod
ms.openlocfilehash: cd9f4ad6b6831b2b15c09b37edc569b3f2d247f7
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94958207"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-azure-data-box"></a>Использование управляемых клиентом ключей в Azure Key Vault для Azure Data Box

Azure Data Box защищает ключ разблокировки устройства (также называемый паролем устройства), который используется для блокировки устройства с помощью ключа шифрования. По умолчанию этот ключ шифрования является управляемым корпорацией Майкрософт ключом. Для дополнительного контроля можно использовать ключ, управляемый клиентом.

Использование ключа, управляемого клиентом, не влияет на шифрование данных на устройстве. Он влияет только на шифрование ключа разблокировки устройства.

Чтобы обеспечить этот уровень контроля в рамках процесса заказа, используйте управляемый клиентом ключ при создании заказа. Дополнительные сведения см. в разделе [учебник. порядок Azure Data Box](data-box-deploy-ordered.md).

В этой статье показано, как включить управляемый клиентом ключ для существующего заказа Data Box в [портал Azure](https://portal.azure.com/). Вы узнаете, как изменить хранилище ключей, ключ, версию или удостоверение для текущего управляемого клиентом ключа или вернуться к использованию управляемого ключа Майкрософт.

Эта статья относится к устройствам Azure Data Box и Azure Data Box Heavy.

## <a name="requirements"></a>Требования

Ключ, управляемый клиентом для Data Box заказа, должен соответствовать следующим требованиям.

- Ключ должен быть создан и сохранен в Azure Key Vault с **обратимым удалением** и **не очищать** включенным. Дополнительные сведения см. в статье [Что такое хранилище ключей Azure?](../key-vault/general/overview.md) Вы можете создать хранилище ключей и ключ при создании или обновлении заказа.

- Ключ должен быть ключом RSA размером 2048 или больше.

## <a name="enable-key"></a>Включить ключ

Чтобы включить управляемый клиентом ключ для существующего заказа Data Box в портал Azure, выполните следующие действия.

1. Перейдите на экран **обзора** для заказа Data Box.

    ![Обзорный экран Data Box заказ-1](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-1.png)

2. Перейдите в раздел **параметры > шифрование** и выберите пункт **управляемый ключ клиента**. Затем выберите **выбрать ключ и хранилище ключей**.

    ![Выбор параметра шифрования ключа, управляемого клиентом](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3.png)

   На экране **Выбор ключа из Azure Key Vault** ваша подписка заполняется автоматически.

 3. Для **хранилища ключей** можно выбрать существующее хранилище ключей из раскрывающегося списка или выбрать **создать** и создать новое хранилище ключей.

     ![Параметры хранилища ключей при выборе управляемого клиентом ключа](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3-a.png)

     Чтобы создать новое хранилище ключей, введите подписку, группу ресурсов, имя хранилища ключей и другие сведения на экране **Создание хранилища ключей** . В **параметрах восстановления** убедитесь, что включена защита **обратимого удаления** и **очистки** . Щелкните **Просмотр и создание**.

      ![Проверка и создание Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-4.png)

      Проверьте сведения о хранилище ключей и нажмите кнопку **создать**. Подождите несколько минут, пока не завершится создание хранилища ключей.

       ![Создание Azure Key Vault с помощью параметров](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-5.png)

4. На экране **Выбор ключа с Azure Key Vault** можно выбрать существующий ключ из хранилища ключей или создать новый.

    ![Выберите ключ из Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6.png)

   Если вы хотите создать новый ключ, выберите **создать**. Необходимо использовать ключ RSA. Размер может быть 2048 или выше.

    ![Создание ключа в Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6-a.png)

    Введите имя нового ключа, примите другие значения по умолчанию и нажмите кнопку **создать**. Вы получите извещение о том, что в хранилище ключей создан ключ.

    ![Имя новый ключ](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-7.png)

5. Для параметра **версия** можно выбрать существующую версию ключа из раскрывающегося списка.

    ![Выберите версию для нового ключа](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8.png)

    Если необходимо создать новую версию ключа, выберите **создать**.

    ![Открытие диалогового окна для создания новой версии ключа](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8-a.png)

    Выберите параметры для новой версии ключа и нажмите кнопку **создать**.

    ![Создание новой версии ключа](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8-b.png)

6. Выбрав хранилище ключей, ключ и версию ключа, нажмите кнопку **выбрать**.

    ![Ключ в Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8-c.png)

    Параметры **типа шифрования** показывают выбранное хранилище ключей и ключ.

    ![Ключ и хранилище ключей для управляемого клиентом ключа](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-9.png)

7. Выберите тип удостоверения, которое будет использоваться для управления управляемым клиентом ключом для этого ресурса. Можно использовать **назначенное системой** удостоверение, созданное во время создания заказа, или выбрать удостоверение, назначенное пользователем.

    Назначенное пользователем удостоверение — это независимый ресурс, который можно использовать для управления доступом к ресурсам. Дополнительные сведения см. в разделе [управляемые типы удостоверений](/azure/active-directory/managed-identities-azure-resources/overview).

    ![Выбор типа удостоверения](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-13.png)

    Чтобы назначить удостоверение пользователя, выберите значение **назначено пользователем**. Затем выберите **выбрать удостоверение пользователя** и выберите управляемое удостоверение, которое вы хотите использовать.

    ![Выберите удостоверение для использования](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-14.png)

    Здесь нельзя создать новый идентификатор пользователя. Сведения о том, как ее создать, см. в разделе [Создание, перечисление, удаление или назначение роли назначенному пользователем управляемому удостоверению с помощью портал Azure](/azure-docs/blob/master/articles/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal).

    Выбранное удостоверение пользователя отображается в параметрах **типа шифрования** .

    ![Выбранное удостоверение пользователя отображается в параметрах типа шифрования](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-15.png)

 9. Нажмите кнопку **сохранить** , чтобы сохранить обновленные параметры **типа шифрования** .

     ![Сохранение ключа, управляемого клиентом](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-10.png)

    URL-адрес ключа отображается в разделе **тип шифрования**.

    ![URL-адрес ключа, управляемого клиентом](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-11.png)<!--Probably need new screen from recent order. Can you provide one? I can't create an order using CMK with the subscription I'm using.-->

## <a name="change-key"></a>Изменить ключ

Чтобы изменить хранилище ключей, ключ или версию ключа для управляемого клиентом ключа, выполните следующие действия.

1. На экране **обзора** для заказа Data Box выберите **Параметры**  >  **Шифрование** и нажмите кнопку **изменить ключ**.

    ![Обзорная страница заказа Data Box с помощью ключа, управляемого клиентом, — 1](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-16.png)

2. Выберите **выбрать другое хранилище ключей и ключ**.

    ![Обзорный экран Data Boxного заказа, выбор другого ключа и хранилища ключей](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-16-a.png)

3. На экране **Выбор ключа из хранилища ключей** отображается подписка, но нет хранилища ключей, ключа или версии ключа. Можно внести любые из следующих изменений:

   - Выберите другой ключ из того же хранилища ключей. Перед выбором ключа и версии необходимо выбрать хранилище ключей.

   - Выберите другое хранилище ключей и назначьте новый ключ.

   - Изменение версии для текущего ключа.
   
    Завершив изменения, нажмите кнопку **выбрать**.

    ![Выбор параметра шифрования — 2](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-17.png)

4. Щелкните **Сохранить**.

    ![Сохранить обновленные параметры шифрования — 1](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-17-a.png)

## <a name="change-identity"></a>Изменить удостоверение

Чтобы изменить удостоверение, используемое для управления доступом к ключу, управляемому клиентом, для этого заказа, выполните следующие действия.

1. На экране **обзора** завершенного заказа Data Box выберите **Параметры**  >  **Шифрование**.

2. Внесите одно из следующих изменений:

     - Чтобы изменить удостоверение другого пользователя, щелкните **выбрать другое удостоверение пользователя**. Затем выберите другое удостоверение на панели в правой части экрана и нажмите кнопку **выбрать**.

       ![Параметр для изменения удостоверения, назначенного пользователем для ключа, управляемого клиентом](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-18.png)

   - Чтобы переключиться на системное удостоверение, созданное во время создания заказа, выберите **система, назначенная** с помощью **параметра Тип удостоверения**.

     ![Возможность перехода на систему, назначенную системой для ключа, управляемого клиентом](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-19.png)

3. Щелкните **Сохранить**.

    ![Сохранить обновленные параметры шифрования — 2](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-17-a.png)

## <a name="use-microsoft-managed-key"></a>Использовать управляемый ключ Майкрософт

Чтобы перейти от использования ключа, управляемого клиентом, к ключу, управляемому корпорацией Майкрософт для вашего заказа, выполните следующие действия.

1. На экране **обзора** завершенного заказа Data Box выберите **Параметры**  >  **Шифрование**.

2. В разделе **Выбор типа** выберите **ключ, управляемый Майкрософт**.

    ![Обзорный экран Data Box заказ-5](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-20.png)

3. Щелкните **Сохранить**.

    ![Сохранить обновленные параметры шифрования для ключа, управляемого Майкрософт](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-21.png)

## <a name="troubleshoot-errors"></a>Устранение неполадок

Если вы получаете ошибки, связанные с ключом, управляемым клиентом, воспользуйтесь следующей таблицей для устранения неполадок.

| Код ошибки| Сведения об ошибке| Восстановимая|
|-------------|--------------|---------|
| ссемусерерроренкриптионкэйдисаблед| Не удалось получить ключ доступа, так как ключ, управляемый клиентом, отключен.| Да, включив версию ключа.|
| ссемусерерроренкриптионкэйекспиред| Не удалось получить ключ доступа, так как истек срок действия управляемого ключа клиента.| Да, включив версию ключа.|
| ссемусерерроркэйдетаилснотфаунд| Не удалось получить ключ доступа, так как не удалось найти ключ, управляемый клиентом.| Если вы удалили хранилище ключей, вы не сможете восстановить ключ, управляемый клиентом.  Если вы перенесли хранилище ключей на другой клиент, см. статью [Изменение идентификатора клиента хранилища ключей после перемещения подписки](../key-vault/general/move-subscription.md). Если вы удалили хранилище ключей:<ol><li>Да, если срок действия защиты от очистки не истек, выполните шаги, описанные в разделе [Восстановление хранилища ключей](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault).</li><li>Нет, если истек срок действия защиты от очистки.</li></ol><br>В противном случае, если хранилище ключей прошло миграцию клиента, да, его можно восстановить, выполнив одно из следующих действий. <ol><li>Верните Key Vault обратно на старый клиент.</li><li>Установите значение `Identity = None`, а затем верните обратно значение `Identity = SystemAssigned`. Это приведет к удалению и повторному созданию удостоверения сразу же после создания удостоверения. Включите для нового удостоверения разрешения `Get`, `Wrap` и `Unwrap` в политике доступа Key Vault.</li></ol> |
| ссемусерерроркэйваултбадрекуестексцептион | Применен ключ, управляемый клиентом, но доступ к ключу не был предоставлен или отменен, или не удается получить доступ к хранилищу ключей из-за включения брандмауэра. | Добавьте удостоверение, выбранное в хранилище ключей, чтобы разрешить доступ к ключу, управляемому клиентом. Если в хранилище ключей включен брандмауэр, переключитесь на назначенное системой удостоверение, а затем добавьте ключ, управляемый клиентом. Дополнительные сведения см. в разделе [Включение ключа](#enable-key). |
| ссемусерерроркэйваултдетаилснотфаунд| Не удалось получить ключ доступа, так как не удалось найти связанное хранилище ключей для управляемого ключа клиента. | Если вы удалили хранилище ключей, вы не сможете восстановить ключ, управляемый клиентом.  Если вы перенесли хранилище ключей на другой клиент, см. статью [Изменение идентификатора клиента хранилища ключей после перемещения подписки](../key-vault/general/move-subscription.md). Если вы удалили хранилище ключей:<ol><li>Да, если срок действия защиты от очистки не истек, выполните шаги, описанные в разделе [Восстановление хранилища ключей](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault).</li><li>Нет, если истек срок действия защиты от очистки.</li></ol><br>В противном случае, если хранилище ключей прошло миграцию клиента, да, его можно восстановить, выполнив одно из следующих действий. <ol><li>Верните Key Vault обратно на старый клиент.</li><li>Установите значение `Identity = None`, а затем верните обратно значение `Identity = SystemAssigned`. Это приведет к удалению и повторному созданию удостоверения сразу же после создания удостоверения. Включите для нового удостоверения разрешения `Get`, `Wrap` и `Unwrap` в политике доступа Key Vault.</li></ol> |
| ссемусереррорсистемассигнедидентитябсент  | Не удалось получить ключ доступа, так как не удалось найти ключ, управляемый клиентом.| Да, убедитесь, что: <ol><li>В Key Vault по-прежнему есть MSI в политике доступа.</li><li>Удостоверение имеет назначенный системой тип.</li><li>Включите разрешения GET, Wrap и Unwrap для удостоверения в политике доступа хранилища ключей.</li></ol>|
| ссемусереррорусерассигнедлимитреачед | Не удалось добавить нового пользователя, которому назначено удостоверение, так как достигнуто ограничение на общее число пользователей, которым можно добавить. | Повторите операцию с меньшим числом удостоверений пользователей или удалите из ресурса некоторые идентификаторы, назначенные пользователем, прежде чем повторять попытку. |
| ссемусерерроркросстенантидентитякцессфорбидден | Сбой операции доступа управляемого удостоверения. <br> Примечание. Это относится к сценарию, когда подписка перемещается в другой клиент. Клиент должен вручную переместить удостоверение в новый клиент. Дополнительные сведения см. в почтовых сообщениях PFA. | Переместите выбранное удостоверение в новый клиент, для которого имеется подписка. Дополнительные сведения см. в разделе [Включение ключа](#enable-key). |
| ссемусерерроркекусеридентитинотфаунд | Применен ключ, управляемый клиентом, но пользователь с таким идентификатором, имеющий доступ к ключу, не найден в Active Directory. <br> Примечание. это происходит в случае, если удостоверение пользователя удаляется из Azure.| Попробуйте добавить к хранилищу ключей другое назначенное пользователем удостоверение, чтобы разрешить доступ к ключу, управляемому клиентом. Дополнительные сведения см. в разделе [Включение ключа](#enable-key). |
| ссемусереррорусерассигнедидентитябсент | Не удалось получить ключ доступа, так как не удалось найти ключ, управляемый клиентом. | Не удалось получить доступ к управляемому ключу клиента. Пользовательское удостоверение (УАИ), связанное с ключом, удалено, или изменился тип УАИ. |
| ссемусерерроркросстенантидентитякцессфорбидден | Сбой операции доступа управляемого удостоверения. <br> Примечание. Это относится к сценарию, когда подписка перемещается в другой клиент. Клиент должен вручную переместить удостоверение в новый клиент. Дополнительные сведения см. в почтовых сообщениях PFA. | Попробуйте добавить к хранилищу ключей другое назначенное пользователем удостоверение, чтобы разрешить доступ к ключу, управляемому клиентом. Дополнительные сведения см. в разделе [Включение ключа](#enable-key).|
| ссемусерерроркэйваултбадрекуестексцептион | Применен ключ, управляемый клиентом, но доступ к ключу не был предоставлен или отменен, или не удается получить доступ к хранилищу ключей из-за включения брандмауэра. | Добавьте удостоверение, выбранное в хранилище ключей, чтобы разрешить доступ к ключу, управляемому клиентом. Если в хранилище ключей включен брандмауэр, переключитесь на назначенное системой удостоверение, а затем добавьте ключ, управляемый клиентом. Дополнительные сведения см. в разделе [Включение ключа](#enable-key). |
| Общая ошибка  | Не удалось получить ключ доступа.| Это общая ошибка. Обратитесь в служба поддержки Майкрософт, чтобы устранить ошибку и определить дальнейшие действия.|

## <a name="next-steps"></a>Следующие шаги

- [Что такое хранилище ключей Azure?](../key-vault/general/overview.md)
- [Краткое руководство. Настройка и получение секрета из Azure Key Vault с помощью портала Azure](../key-vault/secrets/quick-create-portal.md)
