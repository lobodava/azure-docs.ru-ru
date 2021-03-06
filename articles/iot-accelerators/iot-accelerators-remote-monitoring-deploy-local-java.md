---
title: Локальное развертывание решения для удаленного мониторинга — IntelliJ IDE в Azure | Документация Майкрософт
description: В этом пошаговом руководство показано, как развернуть средство удаленного мониторинга Solution Accelerator на локальном компьютере с помощью IntelliJ для тестирования и разработки.
author: dominicbetts
ms.custom: devx-track-java
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: 78573cfe00d8e2e7ddcbf705dffdd5530f82c4e0
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91998599"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>Локальное развертывание акселератора решения для удаленного мониторинга IntelliJ

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

В этой статье показано, как развернуть акселератор решения для удаленного мониторинга на локальном компьютере в целях тестирования и разработки. Вы узнаете, как запускать микрослужбы в IntelliJ. При развертывании локальных микрослужб будут использоваться следующие облачные службы: центр Интернета вещей, Azure Cosmos DB, Azure Stream Analytics и аналитика временных рядов Azure.

Если вы хотите запустить акселератор решения для удаленного мониторинга в Docker на локальном компьютере, см. статью [Deploy the Remote Monitoring solution accelerator locally — Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md) (Локальное развертывание акселератора решения для удаленного мониторинга в Docker).

## <a name="prerequisites"></a>Предварительные условия

Для развертывания служб Azure, используемых акселератором решения для удаленного мониторинга, требуется активная подписка Azure.

Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Настройки компьютера

Для завершения локального развертывания необходимо установить следующие средства на локальный компьютер разработчика:

* [Git](https://git-scm.com/);
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html);
* [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/);
* [Подключаемый модуль IntelliJ Scala](https://plugins.jetbrains.com/plugin/1347-scala)
* [Подключаемый модуль IntelliJ SBT](https://plugins.jetbrains.com/plugin/5007-sbt)
* [Подключаемый модуль выполнителя IntelliJ SBT](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js V8](https://nodejs.org/)

Node.js V8 является необходимым условием для интерфейса командной строки для ПК, который используются скриптами для создания ресурсов Azure. Не используйте Node.js версии 10.

> [!NOTE]
> Служба IntelliJ IDE доступна для Windows и Mac.

## <a name="download-the-source-code"></a>Скачивание исходного кода

Репозиторий исходного кода для удаленного мониторинга включает в себе исходный код и файлы конфигурации Docker, необходимые для запуска образов Docker микрослужб.

Чтобы клонировать и создать локальную версию репозитория, используйте среду командной строки для перехода в подходящую папку на локальном компьютере. Затем выполните один из следующих наборов команд, чтобы клонировать репозиторий Java:

* Чтобы скачать последнюю версию реализаций микрослужб Java, выполните следующую команду:

  ```cmd/sh
  git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git
  ```

* Чтобы получить последние подмодули, выполните следующие команды:

   ```cmd/sh
   cd azure-iot-pcs-remote-monitoring-java
   git submodule foreach git pull origin master
    ```

> [!NOTE]
> Эти команды загружают исходный код для всех микрослужб в дополнение к сценариям, которые используются для локального запуска микрослужб. Исходный код для запуска микрослужб в DOCKER не требуется. Но исходный код полезен, если позже планируется изменить средство Solution Accelerator и проверить изменения локально.

## <a name="deploy-the-azure-services"></a>Развертывание служб Azure

Несмотря на то что в этой статье показано, как запускать микрослужбы локально, они зависят от служб Azure, выполняемых в облаке. Воспользуйтесь приведенным ниже скриптом для развертывания служб Azure. В примерах сценария предполагается, что вы используете репозиторий Java на компьютере Windows. Если вы работаете в другой среде, измените пути, расширения файлов и разделители путей соответствующим образом.

### <a name="create-new-azure-resources"></a>Создание ресурсов Azure

Если вы еще не создали необходимые ресурсы Azure, выполните следующие действия:

1. В среде командной строки перейдите в папку **\сервицес\скриптс\локал\лаунч** в клонированной копии репозитория.

1. Выполните следующие команды для установки инструмента CLI **pcs** и войдите в вашу учетную запись Azure.

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Запустите скрипт **start.cmd**. Скрипт запрашивает следующие сведения:

   * имя решения;
   * Подписка Azure, которую нужно использовать.
   * расположение центра обработки данных Azure, который будет использоваться.

   Скрипт создает группу ресурсов в Azure с именем вашего решения. Эта группа ресурсов содержит ресурсы Azure, используемые ускорителем решений. Эту группу ресурсов можно удалить после того, как вам больше не понадобятся соответствующие ресурсы.

   Скрипт также добавляет набор переменных среды на локальный компьютер. Каждое имя переменной имеет префиксные **Компьютеры**. Эти переменные среды содержат сведения, которые позволяют удаленному мониторингу считывать значения конфигурации из ресурса Azure Key Vault.

   > [!TIP]
   > По завершении скрипта переменные среды сохраняются в файл с именем ** \<your home folder\> \\ . PCS \\ \<solution name\> . env**. Их можно использовать для будущих развертываний Solution Accelerator. Обратите внимание, что любые переменные среды, заданные на локальном компьютере, переопределяют значения в файле ** \\ \\ Local \\ . env скриптов служб** при выполнении **создания DOCKER**.

1. Закройте среду командной строки.

### <a name="use-existing-azure-resources"></a>Использование существующих ресурсов Azure

Если вы уже создали необходимые ресурсы Azure, задайте соответствующие переменные среды на локальном компьютере:
* **PCS_KEYVAULT_NAME**— имя ресурса Key Vault.
* **PCS_AAD_APPID**: идентификатор приложения Azure Active Directory (Azure AD).
* **PCS_AAD_APPSECRET**: секрет приложения Azure AD.

Значения конфигурации будут считываться из этого Key Vault ресурса. Эти переменные среды можно сохранить в файле ** \<your home folder\> \\ . PCS \\ \<solution name\> . env** из развертывания. Обратите внимание, что при запуске **docker-compose** набор переменных среды на вашем компьютере переопределяет значения в файле **services\\scripts\\local\\.env**.

Часть конфигурации, необходимая микрослужбе, хранится в экземпляре Key Vault, созданном при первоначальном развертывании. При необходимости соответствующие переменные в хранилище ключей следует изменить.

## <a name="run-the-microservices"></a>Запуск микрослужб

В этом разделе вы запустите микрослужбы для удаленного мониторинга. Тогда вам нужно выполнить такую команду:

* Собственный веб-интерфейс.
* Службы моделирования устройств Интернета вещей Azure, проверки подлинности и Azure Stream Analytics Manager в DOCKER.
* Микрослужбы в IntelliJ.

### <a name="run-the-device-simulation-service"></a>Запуск службы моделирования устройств

Откройте новое окно командной строки. Убедитесь, что у вас есть доступ к переменным среды, заданным в сценарии **Start. cmd** в предыдущем разделе.

Выполните следующую команду, чтобы открыть контейнер DOCKER для службы моделирования устройств. Служба имитирует устройства для решения для удаленного мониторинга.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Запуск службы "Аутентификация"

Откройте новое окно командной строки и выполните следующую команду, чтобы открыть контейнер DOCKER для службы проверки подлинности. С помощью этой службы можно управлять пользователями, которым разрешен доступ к решениям Azure IoT.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-stream-analytics-manager-service"></a>Запуск службы Stream Analytics Manager

Откройте новое окно командной строки и выполните следующую команду, чтобы открыть контейнер DOCKER для службы Stream Analytics Manager. С помощью этой службы можно управлять заданиями Stream Analytics. Такие задачи управления включают настройку конфигурации задания и запуск, остановку и мониторинг состояния задания.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-your-local-machine"></a>Развертывание всех других микрослужб на локальном компьютере

Ниже описано, как запустить микрослужбы удаленного мониторинга в IntelliJ.

#### <a name="import-a-project"></a>Импорт проекта

1. Откройте интегрированную среду разработки IntelliJ.
1. Выберите **Импорт проекта**.
1. Выберите **Азуре-ИОТ-ПКС-ремоте-мониторинг-жава\сервицес\буилд.Сбт**.

#### <a name="create-run-configurations"></a>Создание конфигураций запуска

1. Выберите команду **выполнить**  >  **Изменение конфигураций**.
1. Выберите **Добавить новую конфигурацию**  >  **SBT задача**.
1. Введите **имя**, а затем введите **задачи** в качестве **запуска**.
1. Выберите **Рабочий каталог** , основанный на службе, которую необходимо запустить.
1. Нажмите кнопку **Применить**  >  **ОК** , чтобы сохранить выбранные параметры.
1. Создайте конфигурации запуска для следующих веб-служб:
    * WebService (services\config);
    * WebService (services\device-telemetry);
    * WebService (services\iothub-manager);
    * WebService (services\storage-adapter).

Например, на следующем рисунке показано, как добавить конфигурацию для службы.

[![Снимок экрана: окно "конфигурации запуска/отладки" IntelliJ IDE, в котором отображается параметр Сторажеадаптер, выделенный в списке задач SBT в левой области, и записи в полях "имя", "задачи", "Рабочий каталог" и "Параметры виртуальной машины" в правой области.](./media/deploy-locally-intellij/run-configurations.png)](./media/deploy-locally-intellij/run-configurations.png#lightbox)

#### <a name="create-a-compound-configuration"></a>Создание составной конфигурации

1. Чтобы выполнить все службы вместе, выберите **Добавить новую конфигурацию**  >  **составной**.
1. Введите **имя**, а затем выберите **добавить задачи SBT**.
1. Нажмите кнопку **Применить**  >  **ОК** , чтобы сохранить выбранные параметры.

Например, на следующем рисунке показано, как добавить все задачи SBT в одну конфигурацию:

[![Снимок экрана: окно конфигураций запуска или отладки интегрированной среды разработки IntelliJ, в котором показан параметр Аллсервицес, выделенный в списке составных элементов на левой панели, и параметр SBT задачи "Девицетелеметри", выделенный на правой панели.](./media/deploy-locally-intellij/all-services.png)](./media/deploy-locally-intellij/all-services.png#lightbox)

Выберите **выполнить** , чтобы создать и запустить веб-службы на локальном компьютере.

Каждая веб-служба открывает окно командной строки и окно веб-браузера. В командной строке отобразятся выходные данные выполняющейся службы. Окно браузера позволяет отслеживать состояние. Не закрывайте окна командной строки, так как эти действия останавливают веб-службу.

Чтобы получить доступ к состоянию служб, перейдите по следующим URL-адресам:

* Диспетчер IoT-Hub: `http://localhost:9002/v1/status`
* Телеметрию устройства: `http://localhost:9004/v1/status`
* файле `http://localhost:9005/v1/status`
* адаптер хранилища: `http://localhost:9022/v1/status`

### <a name="start-the-stream-analytics-job"></a>Запуск задания Stream Analytics

Чтобы запустить задание Stream Analytics, выполните следующие действия:

1. Перейдите на [портал Azure](https://portal.azure.com).
1. Перейдите к **группе ресурсов** , созданной для решения. Имя группы ресурсов — это имя, выбранное для решения при запуске скрипта **Start. cmd** .
1. Выберите **Stream Analytics задание** в списке ресурсов.
1. На странице **Обзор** задания Stream Analytics нажмите кнопку **Пуск** и выберите **Запуск** , чтобы запустить задание.

### <a name="run-the-web-ui"></a>Запуск веб-интерфейса

На этом этапе вы запустите веб-интерфейс. Откройте новое окно командной строки. Убедитесь, что у вас есть доступ к переменным среды, заданным сценарием **Start. cmd** . Перейдите в папку **WebUI** локальной копии репозитория и выполните следующие команды:

```cmd
npm install
npm start
```

После завершения команды **запуска** браузер отобразит страницу по адресу `http://localhost:3000/dashboard` . На этой странице могут возникать ошибки. Чтобы просмотреть приложение без ошибок, выполните следующие действия.

### <a name="configure-and-run-nginx"></a>Настройка и запуск nginx

Настройте обратный прокси-сервер, связывающий веб-приложение с микрослужбами, работающими на локальном компьютере:

1. Скопируйте файл **nginx. conf** из папки **вебуи\скриптс\локалхост** локальной копии репозитория в каталог установки **нгинкс\конф** .
1. Запустите nginx.

Дополнительные сведения о запуске nginx см. в разделе [nginx for Windows](https://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Подключение к панели мониторинга

Чтобы получить доступ к панели мониторинга решения для удаленного мониторинга, перейдите `http://localhost:9000` в браузере.

## <a name="clean-up"></a>Очистка

Чтобы избежать ненужных расходов, удалите облачные службы из подписки Azure после завершения тестирования. Чтобы удалить службы, перейдите в [портал Azure](https://ms.portal.azure.com)и удалите группу ресурсов, созданную с помощью скрипта **Start. cmd** .

Также можно удалить локальную копию репозитория удаленного мониторинга, созданного при клонировании исходного кода из GitHub.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы развернули решение для удаленного мониторинга, ознакомьтесь со статьей [Краткое руководство. Использование облачного решения для удаленного мониторинга](quickstart-remote-monitoring-deploy.md).
