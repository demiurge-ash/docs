---
git: ebe5c98bee0769c1b6c8901007bb0be276446632
---

# Установка

<a name="meet-laravel"></a>
## Встречайте Laravel

Laravel – фреймворк веб-приложения с выразительным, элегантным синтаксисом. Веб-фреймворк предлагает структуру и отправную точку для создания вашего приложения, позволяя вам сосредоточиться на создании чего-то удивительного, но пока мы не будем вдаваться в детали.

Laravel стремится обеспечить потрясающий опыт разработчика, предоставляя при этом мощный функционал: тщательное внедрение зависимостей, выразительный уровень абстракции базы данных, очереди и запланированные задачи, модульное и интеграционное тестирование и многое другое.

Независимо от того, новичок ли вы в PHP, веб-фреймворках или имеете многолетний опыт, Laravel – это фреймворк, который может расти вместе с вами. Мы поможем вам сделать первые шаги в качестве веб-разработчика или подскажем, как вы поднимите свой опыт на новый уровень. Нам не терпится увидеть, что вы построите.

> [!NOTE]
> Новичок в Laravel? Посетите [Laravel Bootcamp](https://bootcamp.laravel.com) для практического тура по фреймворку, во время которого мы проведем вас через создание вашего первого приложения Laravel.

<a name="why-laravel"></a>
### Почему именно Laravel?

При создании веб-приложения вам доступны различные инструменты и фреймворки. Однако мы считаем, что Laravel – лучший выбор для создания современных полнофункциональных веб-приложений.

##### Прогрессивный фреймворк

Нам нравится называть Laravel «прогрессивным» фреймворком. Под этим мы подразумеваем, что Laravel растет вместе с вами. Если вы только делаете первые шаги в веб-разработке, обширная библиотека документации, руководств и [видеоуроков](https://laracasts.com) Laravel поможет вам изучить основы, не перегружая себя.

Если вы старший разработчик, Laravel предлагает вам надежные инструменты для [внедрения зависимостей](/docs/{{version}}/container), [модульного тестирования](/docs/{{version}}/testing), [создания очередей](/docs/{{version}}/queues), [событий в реальном времени](/docs/{{version}}/broadcasting) и многое другое. Laravel оптимизирован для создания профессиональных веб-приложений и готов обрабатывать корпоративные рабочие нагрузки.

##### Масштабируемый фреймворк

Laravel невероятно масштабируем. Благодаря удобному для масштабирования характеру PHP и встроенной поддержке быстрых распределенных систем кеширования, таких как Redis, горизонтальное масштабирование с Laravel очень просто. Фактически, приложения Laravel легко масштабируются для обработки сотен миллионов запросов в месяц.

Требуется экстремальное масштабирование? Такие платформы, как [Laravel Vapor](https://vapor.laravel.com), позволяют запускать приложение Laravel в практически неограниченном масштабе с использованием новейшей бессерверной технологии AWS.

##### Фреймворк сообщества

Laravel объединяет лучшие пакеты в экосистеме PHP, чтобы предложить наиболее надежный и удобный для разработчиков фреймворк. Кроме того, тысячи талантливых разработчиков со всего мира [внесли свой вклад в фреймворк](https://github.com/laravel/framework). Кто знает, возможно, вы даже станете соучастником Laravel.

<a name="creating-a-laravel-project"></a>
## Создание проекта Laravel

Перед созданием вашего первого проекта Laravel убедитесь, что на вашем локальном компьютере установлены PHP и [Composer](https://getcomposer.org). Если вы разрабатываете на macOS, PHP и Composer можно установить всего за несколько минут с помощью [Laravel Herd](https://herd.laravel.com). Кроме того, мы рекомендуем [установить Node и NPM](https://nodejs.org).

После установки PHP и Composer вы можете создать новый проект Laravel с помощью команды `create-project` от Composer:

```shell
composer create-project laravel/laravel example-app
```

Или вы можете создавать новые проекты Laravel, установив глобально [Laravel installer](https://github.com/laravel/installer) via Composer:

```nothing
composer global require laravel/installer:^5.4
laravel new example-app
```

После создания проекта запустите локальный сервер разработки Laravel с помощью команды `serve` в Laravel Artisan:

```shell
cd example-app

php artisan serve
```

После запуска сервера разработки Artisan ваше приложение будет доступно в вашем веб-браузере по адресу [http://localhost:8000](http://localhost:8000). Теперь вы готовы [продолжить свои первые шаги в мире Laravel](#next-steps). Конечно же, вы также можете [настроить базу данных](#databases-and-migrations).

> [!NOTE]
> Если вы хотите начать разработку вашего приложения Laravel с хорошим стартом, рассмотрите использование одного из наших [стартовых комплектов](/docs/{{version}}/starter-kits). Стартовые комплекты Laravel предоставляют инфраструктуру для аутентификации как на сервере, так и на клиенте для вашего нового приложения Laravel.



<a name="initial-configuration"></a>
## Начальная конфигурация

Все файлы конфигурации для фреймворка Laravel хранятся в каталоге `config`. Каждый параметр имеет комментарии, поэтому не стесняйтесь просматривать файлы и знакомиться с доступными вам вариантами.

Laravel практически не требует дополнительной настройки из коробки. Вы можете начать разработку! Однако вы можете просмотреть файл `config/app.php` и его комментарии. Он содержит несколько параметров, таких как часовой пояс и локаль, которые вы можете изменить в соответствии с вашим приложением.

<a name="environment-based-configuration"></a>
### Конфигурация на основе окружения

Поскольку многие значения параметров конфигурации Laravel могут различаться в зависимости от того, работает ли ваше приложение на локальном компьютере или на эксплуатационном веб-сервере, многие важные значения конфигурации определяются с помощью файла `.env`, существующий в корне вашего приложения.

Ваш файл `.env` не должен быть привязан к системе контроля версий вашего приложения, поскольку каждому разработчику / серверу, использующему ваше приложение, может потребоваться другая конфигурация окружения. Более того, это будет угрозой безопасности в случае, если злоумышленник получит доступ к вашему репозиторию системы управления версиями, поскольку любые конфиденциальные учетные данные будут раскрыты.

> [!NOTE]
> Для получения дополнительной информации о конфигурации на основе файла `.env` и окружения ознакомьтесь с полной [документацией по конфигурации](/docs/{{version}}/configuration#environment-configuration).

<a name="databases-and-migrations"></a>
### Базы данных и миграции

Теперь, когда вы создали ваше приложение Laravel, возможно, вы хотите сохранить некоторые данные в базе данных. По умолчанию файл конфигурации вашего приложения `.env` указывает, что Laravel будет взаимодействовать с базой данных MySQL и получит доступ к базе данных по адресу `127.0.0.1`.

> [!NOTE]
> Если вы разрабатываете на macOS и вам нужно установить MySQL, Postgres или Redis локально, рассмотрите использование [DBngin](https://dbngin.com/).

Если вы не хотите устанавливать MySQL или Postgres на своем локальном компьютере, вы всегда можете использовать базу данных [SQLite](https://www.sqlite.org/index.html). SQLite - это небольшая, быстрая, автономная система управления базами данных. Для начала работы обновите файл конфигурации `.env` вашего Laravel, чтобы использовать драйвер базы данных `sqlite` Laravel. Вы можете удалить другие настройки базы данных:

```ini
# Добавьте следующие строки:
DB_CONNECTION=sqlite

# Удалите или закомментируйте следующие строки:
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

После настройки базы данных SQLite вы можете запустить [миграции баз данных](/docs/{{version}}/migrations) вашего приложения, которые создадут таблицы базы данных вашего приложения:

```shell
php artisan migrate
```

Если база данных SQLite еще не существует для вашего приложения, Laravel спросит вас, хотите ли вы, чтобы база данных была создана. Обычно файл базы данных SQLite будет создан в `database/database.sqlite`.

<a name="directory-configuration"></a>
### Конфигурация каталога

Laravel всегда должен обслуживаться из корня «веб-каталога», настроенного для вашего веб-сервера. Вы не должны пытаться обслуживать приложение Laravel из поддиректории относительно «веб-каталога». Такая попытка может открыть доступ к конфиденциальным файлам, существующим в вашем приложении.

<a name="docker-installation-using-sail"></a>
## Установка Docker с использованием Sail

Мы хотим, чтобы начало работы с Laravel было максимально простым, независимо от вашей предпочтительной операционной системы. Поэтому существует несколько вариантов для разработки и запуска проекта Laravel на вашем локальном компьютере. Хотя вы можете изучить эти варианты позже, Laravel предоставляет [Sail](/docs/{{version}}/sail), встроенное решение для запуска проекта Laravel с использованием [Docker](https://www.docker.com).

Docker - это инструмент для запуска приложений и служб в небольших легковесных "контейнерах", которые не вмешиваются в установленное программное обеспечение или конфигурацию вашего локального компьютера. Это означает, что вам не нужно беспокоиться о настройке сложных инструментов разработки, таких как веб-серверы и базы данных на вашем локальном компьютере. Для начала вам нужно только установить [Docker Desktop](https://www.docker.com/products/docker-desktop).

Laravel Sail - это легкий интерфейс командной строки для взаимодействия с конфигурацией Docker по умолчанию Laravel. Sail предоставляет отличную отправную точку для создания приложения Laravel с использованием PHP, MySQL и Redis, не требуя опыта работы с Docker.

> [!NOTE]
> Уже являетесь экспертом по Docker? Не волнуйтесь! Все в Sail можно настроить с помощью файлов `docker-compose.yml`, включенных в Laravel.

<a name="sail-on-macos"></a>
### Sail на macOS

Если вы разрабатываете на Mac, и [Docker Desktop](https://www.docker.com/products/docker-desktop) уже установлен, вы можете использовать простую команду в терминале, чтобы создать новый проект Laravel. Например, чтобы создать новое приложение Laravel в каталоге с именем "example-app", вы можете выполнить следующую команду в терминале:

```shell
curl -s "https://laravel.build/example-app" | bash
```

Конечно, вы можете изменить "example-app" в этом URL на что угодно, лишь бы имя приложения содержало только буквенно-цифровые символы, дефисы и подчеркивания. Директория приложения Laravel будет создана в том каталоге, из которого вы выполнили эту команду.

Установка Sail может занять несколько минут, пока контейнеры приложения Sail будут созданы на вашем локальном компьютере.

После создания проекта вы можете перейти в директорию приложения и запустить Laravel Sail. Laravel Sail предоставляет простой интерфейс командной строки для взаимодействия с конфигурацией Docker по умолчанию:

```shell
cd example-app

./vendor/bin/sail up
```

После запуска контейнеров Docker приложения вы сможете получить доступ к приложению в веб-браузере по адресу: http://localhost.

> [!NOTE]
> Чтобы продолжить изучение Laravel Sail, ознакомьтесь с его [полной документацией](/docs/{{version}}/sail).

<a name="sail-on-windows"></a>
### Sail на Windows

Прежде чем создавать новое приложение Laravel на вашем компьютере с Windows, убедитесь, что установлен [Docker Desktop](https://www.docker.com/products/docker-desktop). Затем убедитесь, что установлен и включен

Windows Subsystem for Linux 2 (WSL2). WSL позволяет запускать исполняемые файлы Linux нативно на Windows 10. Информацию о том, как установить и включить WSL2, можно найти в документации [среды разработки Microsoft](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

> [!NOTE]
> После установки и включения WSL2 убедитесь, что Docker Desktop [настроен для использования бэкенда WSL2](https://docs.docker.com/docker-for-windows/wsl/).

Затем вы готовы создать свой первый проект Laravel. Запустите [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab) и начните новую сессию терминала для вашей операционной системы WSL2 Linux. Затем вы можете использовать простую команду в терминале, чтобы создать новое приложение Laravel. Например, чтобы создать новое приложение Laravel в каталоге с именем "example-app", вы можете выполнить следующую команду в терминале:

```shell
curl -s https://laravel.build/example-app | bash
```

Конечно, вы можете изменить "example-app" в этом URL на что угодно, лишь бы имя приложения содержало только буквенно-цифровые символы, дефисы и подчеркивания. Директория приложения Laravel будет создана в том каталоге, из которого вы выполнили эту команду.

Установка Sail может занять несколько минут, пока контейнеры приложения Sail будут созданы на вашем локальном компьютере.

После создания проекта вы можете перейти в директорию приложения и запустить Laravel Sail. Laravel Sail предоставляет простой интерфейс командной строки для взаимодействия с конфигурацией Docker по умолчанию:

```shell
cd example-app

./vendor/bin/sail up
```

После запуска контейнеров Docker приложения вы сможете получить доступ к приложению в веб-браузере по адресу: http://localhost.

> [!NOTE]
> Чтобы продолжить изучение Laravel Sail, ознакомьтесь с его [полной документацией](/docs/{{version}}/sail).

#### Разработка в WSL2

Конечно, вам потребуется иметь возможность изменять файлы приложения Laravel, созданные в вашей установке WSL2. Для этого мы рекомендуем использовать редактор [Visual Studio Code](https://code.visualstudio.com) от Microsoft и их официальное расширение [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) для удаленной разработки.

После установки этих инструментов вы можете открыть любой проект Laravel, выполнив команду `code .` из корневой директории вашего приложения с использованием Windows Terminal.

<a name="sail-on-linux"></a>
### Sail на Linux

Если вы разрабатываете на Linux и у вас уже установлен [Docker Compose](https://docs.docker.com/compose/install/), вы можете использовать простую команду терминала для создания нового проекта Laravel.

Сначала, если вы используете Docker Desktop для Linux, вам следует выполнить следующую команду. Если вы не используете Docker Desktop для Linux, этот шаг можно пропустить:

```shell
docker context use default
```

Затем, чтобы создать новое приложение Laravel в директории с названием "example-app", вы можете выполнить следующую команду в терминале:

```shell
curl -s https://laravel.build/example-app | bash
```

Конечно, вы можете изменить "example-app" в этом URL на любое другое имя, просто убедитесь, что имя приложения содержит только буквенно-цифровые символы, дефисы и подчеркивания. Директория приложения Laravel будет создана в той директории, из которой вы выполните команду.

Установка Sail может занять несколько минут, пока контейнеры приложения Sail строятся на вашем локальном компьютере.

После создания проекта вы можете перейти в директорию приложения и запустить Laravel Sail. Laravel Sail предоставляет простой интерфейс командной строки для взаимодействия с настройками Docker по умолчанию для Laravel:

```shell
cd example-app

./vendor/bin/sail up
```

После того как контейнеры Docker приложения будут запущены, вы можете получить доступ к приложению в своем веб-браузере по адресу: http://localhost.

> [!NOTE]
> Чтобы узнать больше о Laravel Sail, ознакомьтесь с его [полной документацией](/docs/{{version}}/sail).


<a name="choosing-your-sail-services"></a>
### Выбор сервисов Sail

При создании нового приложения Laravel через Sail вы можете использовать переменную строки запроса `with` для выбора того, какие сервисы должны быть настроены в файле `docker-compose.yml` вашего нового приложения. Доступные сервисы включают `mysql`, `pgsql`, `mariadb`, `redis`, `memcached`, `meilisearch`, `typesense`, `minio`, `selenium` и `mailpit`:

```shell
curl -s "https://laravel.build/example-app?with=mysql,redis" | bash
```

Если вы не укажете, какие сервисы вы хотели бы настроить, будет настроен стандартный стек из `mysql`, `redis`, `meilisearch`, `mailpit` и `selenium`.

Вы можете указать Sail установить стандартный [Devcontainer](/docs/{{version}}/sail#using-devcontainers), добавив параметр `devcontainer` в URL:

```shell
curl -s "https://laravel.build/example-app?with=mysql,redis&devcontainer" | bash
```

<a name="ide-support"></a>
## Поддержка IDE

Вы можете использовать любой редактор кода при разработке приложений Laravel; однако [PhpStorm](https://www.jetbrains.com/phpstorm/laravel/) предлагает обширную поддержку для Laravel и его экосистемы, включая [Laravel Pint](https://www.jetbrains.com/help/phpstorm/using-laravel-pint.html).

Кроме того, поддерживаемый сообществом плагин PhpStorm [Laravel Idea](https://laravel-idea.com/) предлагает различные полезные дополнения для IDE, включая генерацию кода, автодополнение синтаксиса Eloquent, автодополнение правил валидации и многое другое.

<a name="next-steps"></a>
## Следующие шаги

Теперь, когда вы создали свой проект Laravel, возможно, вы задаетесь вопросом, что изучить дальше. Во-первых, мы настоятельно рекомендуем ознакомиться с тем, как работает Laravel, прочитав следующую документацию:

- [Жизненный Цикл Запроса](/docs/{{version}}/lifecycle)
- [Конфигурация](/docs/{{version}}/configuration)
- [Структура Директорий](/docs/{{version}}/structure)
- [Фронтенд](/docs/{{version}}/frontend)
- [Контейнер Сервисов](/docs/{{version}}/container)
- [Фасады](/docs/{{version}}/facades)


Как вы хотите использовать Laravel, также определит следующие шаги на вашем пути. Существует множество способов использования Laravel, и мы рассмотрим два основных варианта использования фреймворка ниже.

> [!NOTE]
> Новичок в Laravel? Ознакомьтесь с [Laravel Bootcamp](https://bootcamp.laravel.com) для практического ознакомления с фреймворком, пока мы проведем вас через создание вашего первого приложения Laravel.

<a name="laravel-the-fullstack-framework"></a>
### Laravel как клиент-серверный фреймворк

Laravel может служить клиент-серверным фреймворком. Под «клиент-серверным фреймворком» мы подразумеваем, что вы собираетесь использовать Laravel для маршрутизации запросов к вашему приложению и отрисовки интерфейса через [шаблоны Blade](/docs/{{version}}/blade) или с использованием гибридной технологии одностраничного приложения, такой как [Inertia.js](https://inertiajs.com). Это наиболее распространенный способ использования фреймворка Laravel.

Если вы планируете использовать Laravel таким образом, вы можете ознакомиться с нашей документацией по [разработке фронтенда](/docs/{{version}}/frontend), [маршрутизации](/docs/{{version}}/routing), [представлениям](/docs/{{version}}/views) или [ORM Eloquent](/docs/{{version}}/eloquent). Кроме того, вам может быть интересно узнать о пакетах сообщества, таких как [Livewire](https://livewire.laravel.com) и [Inertia](https://inertiajs.com). Эти пакеты позволяют использовать Laravel как полноценный фреймворк, наслаждаясь многими преимуществами интерфейса одностраничных JavaScript-приложений.

Если вы используете Laravel как полноценный фреймворк, мы также настоятельно рекомендуем вам научиться компилировать CSS и JavaScript вашего приложения с помощью [Vite](/docs/{{version}}/vite).

> [!NOTE] 
> Если вы хотите получить преимущество перед созданием своего приложения, ознакомьтесь с одним из наших официальных [стартовых комплектов приложений](starter-kits).

<a name="laravel-the-api-backend"></a>
### Laravel в качестве сервера API

Laravel также может служить серверной частью API для одностраничного JavaScript-приложения или мобильного приложения. Например, вы можете использовать Laravel в качестве серверной части API для своего [Next.js](https://nextjs.org) приложения. В этом контексте вы можете использовать Laravel для обеспечения [аутентификации](/docs/{{version}}/sanctum) и хранения / получения данных для вашего приложения, а также пользуясь преимуществами мощных служб Laravel, таких, как очереди, электронная почта, уведомления и многое другое.

Если вы планируете использовать Laravel именно так, то вы можете ознакомиться с нашей документацией по [маршрутизации](/docs/{{version}}/routing), пакету [Laravel Sanctum](/docs/{{version}}/sanctum) и [Eloquent ORM](/docs/{{version}}/eloquent).

> [!NOTE]
> Нужен быстрый старт для создания бэкенда Laravel и фронтенда Next.js? Laravel Breeze предлагает [API stack](/docs/{{version}}/starter-kits#breeze-and-next), а так же [реализацию внешнего интерфейса Next.js](https://github.com/laravel/breeze-next), чтобы вы могли начать работу за считанные минуты.

