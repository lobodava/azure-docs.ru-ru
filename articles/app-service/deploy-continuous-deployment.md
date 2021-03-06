---
title: настройка непрерывного развертывания;
description: Узнайте, как включить CI/CD в службу приложений Azure из GitHub, BitBucket, Azure Repos или других репозиториев. Выберите конвейер сборки, соответствующий вашим потребностям.
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.topic: article
ms.date: 03/20/2020
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: b7730558e2a660b0cf00a5b6962d1e2275dd472c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90984404"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Непрерывное развертывание в службе приложений Azure

[Служба приложений Azure](overview.md) обеспечивает непрерывное развертывание из репозиториев GitHub, BitBucket и [Azure Repos](https://azure.microsoft.com/services/devops/repos/) , потянув за последние обновления. В этой статье показано, как использовать портал Azure для непрерывного развертывания приложения с помощью службы сборки KUDU или [Azure pipelines](https://azure.microsoft.com/services/devops/pipelines/). 

Дополнительные сведения о службах управления версиями см. в статьях [Создание репозитория (GitHub)], [Создание репозитория (BitBucket)]или [создание нового репозитория Git (Azure Repos)].

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Авторизация службы приложений Azure 

Чтобы использовать Azure Repos, убедитесь, что ваша организация Azure DevOps связана с вашей подпиской Azure. Дополнительные сведения см. [в статье Настройка учетной записи Azure DevOps Services для ее развертывания в веб-приложении](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops&preserve-view=true).

Для BitBucket или GitHub разрешите службе приложений Azure подключиться к репозиторию. Только один раз вы должны авторизоваться в службе системы управления версиями. 

1. В [портал Azure](https://portal.azure.com)найдите  **службы приложений** и выберите.

   ![Выполните поиск по запросу службы приложений.](media/app-service-continuous-deployment/search-for-app-services.png)

1. Выберите службу приложений, которую требуется развернуть.

   ![Выберите свое приложение.](media/app-service-continuous-deployment/select-your-app.png)
   
1. На странице приложение в меню слева выберите пункт **центр развертывания** .
   
1. На странице **центр развертывания** выберите **GitHub** или **BitBucket**, а затем щелкните **авторизовать**. 
   
   ![Выберите служба системы управления версиями, а затем выберите авторизовать.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. При необходимости войдите в службу и следуйте запросам авторизации. 

## <a name="enable-continuous-deployment"></a>Включение непрерывного развертывания 

После авторизации службы управления версиями настройте приложение для непрерывного развертывания с помощью встроенного сервера сборки [службы приложений KUDU](#option-1-kudu-app-service) или с помощью [Azure pipelines](#option-2-azure-pipelines). 

### <a name="option-1-kudu-app-service"></a>Вариант 1. служба приложений KUDU

Вы можете использовать встроенный сервер сборки службы приложений KUDU для непрерывного развертывания из GitHub, BitBucket или Azure Repos. 

1. В [портал Azure](https://portal.azure.com)найдите **службы приложений**, а затем выберите службу приложений, которую требуется развернуть. 
   
1. На странице приложение в меню слева выберите пункт **центр развертывания** .
   
1. Выберите свой Полномочный поставщик системы управления версиями на странице **центр развертывания** и нажмите кнопку **продолжить**. Для GitHub или BitBucket можно также выбрать **изменить учетную запись** , чтобы изменить разрешенную учетную запись. 
   
   > [!NOTE]
   > Чтобы использовать Azure Repos, убедитесь, что ваша организация Azure DevOps Services связана с вашей подпиской Azure. Дополнительные сведения см. [в статье Настройка учетной записи Azure DevOps Services для ее развертывания в веб-приложении](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops&preserve-view=true).
   
1. Для GitHub или Azure Repos на странице **поставщик сборки** выберите **Служба сборок службы приложений**, а затем нажмите кнопку **продолжить**. BitBucket всегда использует службу сборки службы приложений.
   
   ![Выберите Служба сборок службы приложений, а затем нажмите кнопку продолжить.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. На странице **Настройка** выполните следующие действия.
   
   - Для GitHub раскрывающийся список выберите **организацию**, **репозиторий**и **ветвь** , которые нужно развернуть непрерывно.
     
     > [!NOTE]
     > Если вы не видите репозиториев, вам может потребоваться авторизовать службу приложений Azure на сайте GitHub. Перейдите к репозиторию GitHub и последовательно выберите **Параметры**  >  **приложения**  >  **Разрешенные приложения OAuth**. Выберите **служба приложений Azure**и щелкните **предоставить**. Чтобы предоставить разрешения для репозиториев организации, необходимо быть владельцем организации.
     
   - Для BitBucket выберите **группу**BitBucket, **репозиторий**и **ветвь** , которые нужно развернуть непрерывно.
     
   - Для Azure Repos выберите **организацию Azure DevOps**, **проект**, **репозиторий**и **ветвь** , которые нужно развернуть непрерывно.
     
     > [!NOTE]
     > Если ваша организация Azure DevOps отсутствует в списке, убедитесь, что она связана с вашей подпиской Azure. Дополнительные сведения см. [в статье Настройка учетной записи Azure DevOps Services для ее развертывания в веб-приложении](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops&preserve-view=true).
     
1. Выберите **Continue** (Продолжить).
   
   ![Заполните сведения о репозитории и нажмите кнопку продолжить.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. После настройки поставщика сборки просмотрите параметры на странице **Сводка** и нажмите кнопку **Готово**.
   
1. Новые фиксации в выбранном репозитории в приложении Службы приложений будут непрерывно развертываться. Фиксации и развертывания можно отслеживать на странице **Центра развертывания**.
   
   ![Мониторинг фиксаций и развертываний в центре развертывания](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-azure-pipelines"></a>Вариант 2. Azure Pipelines 

Если у вашей учетной записи есть необходимые разрешения, можно настроить Azure Pipelines для непрерывного развертывания из GitHub или Azure Repos. Дополнительные сведения о развертывании с помощью Azure Pipelines см. [в статье Развертывание веб-приложения в службах приложений Azure](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

#### <a name="prerequisites"></a>Предварительные требования

Чтобы служба приложений Azure была создана непрерывной поставкой с помощью Azure Pipelines, ваша организация Azure DevOps должна иметь следующие разрешения: 

- Учетная запись Azure должна иметь разрешения на запись в Azure Active Directory и создание службы. 
  
- Ваша учетная запись Azure должна иметь роль **владельца** в подписке Azure.

- Вы должны быть администратором в проекте Azure DevOps, который вы хотите использовать.

#### <a name="github--azure-pipelines"></a>GitHub + Azure Pipelines

1. В [портал Azure](https://portal.azure.com)найдите **службы приложений**, а затем выберите службу приложений, которую требуется развернуть. 
   
1. На странице приложение в меню слева выберите пункт **центр развертывания** .

1. Выберите **GitHub** в качестве поставщика системы управления версиями на странице **центра развертывания** и нажмите кнопку **продолжить**. Для **GitHub**можно выбрать **изменить учетную запись** , чтобы изменить разрешенную учетную запись.

    :::image type="content" source="media/app-service-continuous-deployment/deployment-center-src-control.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::
   
1. На странице **поставщик сборки** выберите **Azure pipelines (Предварительная версия)** и нажмите кнопку **продолжить**.

    :::image type="content" source="media/app-service-continuous-deployment/select-build-provider.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::
   
1. На странице **Настройка** в разделе **код** выберите **организацию**, **репозиторий**и **ветвь** , которые нужно развернуть непрерывно, и выберите **продолжить**.
     
     > [!NOTE]
     > Если вы не видите репозиториев, вам может потребоваться авторизовать службу приложений Azure на сайте GitHub. Перейдите к репозиторию GitHub и последовательно выберите **Параметры**  >  **приложения**  >  **Разрешенные приложения OAuth**. Выберите **служба приложений Azure**и щелкните **предоставить**. Чтобы предоставить разрешения для репозиториев организации, необходимо быть владельцем организации.
       
    В разделе **Build (сборка** ) укажите организацию Azure DevOps, проект, языковую платформу, которую Azure pipelines следует использовать для выполнения задач сборки, а затем нажмите кнопку **продолжить**.

   :::image type="content" source="media/app-service-continuous-deployment/build-configure.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::

1. После настройки поставщика сборки просмотрите параметры на странице **Сводка** и нажмите кнопку **Готово**.

   :::image type="content" source="media/app-service-continuous-deployment/summary.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::
   
1. Новые фиксации в выбранном репозитории и ветви теперь непрерывно развертываются в службе приложений. Фиксации и развертывания можно отслеживать на странице **Центра развертывания**.
   
   ![Мониторинг фиксаций и развертываний в центре развертывания](media/app-service-continuous-deployment/github-finished.png)

#### <a name="azure-repos--azure-pipelines"></a>Azure Repos + Azure Pipelines

1. В [портал Azure](https://portal.azure.com)найдите **службы приложений**, а затем выберите службу приложений, которую требуется развернуть. 
   
1. На странице приложение в меню слева выберите пункт **центр развертывания** .

1. Выберите **Azure Repos** в качестве поставщика системы управления версиями на странице **центра развертывания** и нажмите кнопку **продолжить**.

    :::image type="content" source="media/app-service-continuous-deployment/deployment-center-src-control.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::

1. На странице **поставщик сборки** выберите **Azure pipelines (Предварительная версия)** и нажмите кнопку **продолжить**.

    :::image type="content" source="media/app-service-continuous-deployment/azure-pipelines.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::

1. На странице **Настройка** в разделе **код** выберите **организацию**, **репозиторий**и **ветвь** , которые нужно развернуть непрерывно, и выберите **продолжить**.

   > [!NOTE]
   > Если существующая организация Azure DevOps отсутствует в списке, может потребоваться связать ее с подпиской Azure. Дополнительные сведения см. в разделе [Определение конвейера освобождения компакт-диска](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).

   В разделе **Build (сборка** ) укажите организацию Azure DevOps, проект, языковую платформу, которую Azure pipelines следует использовать для выполнения задач сборки, а затем нажмите кнопку **продолжить**.

   :::image type="content" source="media/app-service-continuous-deployment/build-configure.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::

1. После настройки поставщика сборки просмотрите параметры на странице **Сводка** и нажмите кнопку **Готово**.  
     
   :::image type="content" source="media/app-service-continuous-deployment/summary-azure-pipelines.png" alt-text="Снимок экрана со страницей центра развертывания службы приложений.":::

1. Новые фиксации в выбранном репозитории и ветви теперь непрерывно развертываются в службе приложений. Фиксации и развертывания можно отслеживать на странице **Центра развертывания**.

## <a name="disable-continuous-deployment"></a>Отключение непрерывного развертывания

Чтобы отключить непрерывное развертывание, выберите **Отключить** в верхней части страницы **центра развертывания** приложения.

![Отключение непрерывного развертывания](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="use-unsupported-repos"></a>Использовать неподдерживаемый репозиториев

Для приложений Windows можно вручную настроить непрерывное развертывание из облачного репозитория Git или Mercurial, который не поддерживает напрямую портал, например [GitLab](https://gitlab.com/). Для этого выберите внешнее поле на странице **центра развертывания** . Дополнительные сведения см. в [статье Настройка непрерывного развертывания с помощью ручных действий](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Исследование распространенных проблем с непрерывным развертыванием](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Использование Azure PowerShell](/powershell/azure/)
* [Документация по Git](https://git-scm.com/documentation)
* [Проект Kudu](https://github.com/projectkudu/kudu/wiki)

[Create a repo (GitHub)]: https://help.github.com/articles/create-a-repo (Создание репозитория GitHub)
[Create a repo (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html (Создание репозитория BitBucket)
[Создание нового репозитория Git (Azure Repos)]: /azure/devops/repos/git/creatingrepo