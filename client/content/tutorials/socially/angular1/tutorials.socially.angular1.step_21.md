{{#template name="tutorials.socially.angular1.step_21.md"}}
{{> downloadPreviousStep stepName="step_20"}}

На этом этапе мы научимся добавлять поддержку мобильных устройств: iOS и Android и как элегантно использовать код с помощью системы пакетов Meteor.

Давайте разберёмся, что такое Meteor пакеты: 
- Папка `packages` в корне опрпделяет *локальный* пакет нашего проекта. "Локальный" значит пакеты, которые мы разрабатываем самостоятельно, а не используем существующие. 
- Мы используем пакеты для разделения кода, который используется для мобильных устройств и веба.

В этом примере урока мы разделим часть с login проекта: в браузере пользователи будут логинится используя email и password и в мобильном приложении пользователи будут входить через SMS верификацию.

Мы создадим отдельные AngularJS модули для мобильных устройств и веба с целью разделить эту часть кода - это отличная альтернатива использованию `Meteor.isCordova` множественное количество раз!

### Поддержка мобильных устройств в проекте:

Для добавления поддержки выберите желаемую платформу(ы) с помощью команды:

    $ meteor add-platform ios
    # ИЛИ / И
    $ meteor add-platform android

И для запуска в эмуляторе запустите команду:

    $ meteor run ios
    # ИЛИ
    $ meteor run android

Вы также можете запустить в мобильном устройстве, почитайте [интеграция Meteor и Cordova](https://github.com/meteor/meteor/wiki/Meteor-Cordova-integration).

### создание пакетов

Для работы с пакетами нам нужно создать папку `packages`:

    $ mkdir packages

Мы вначале начнём работу с пакетом, который содержит логику мобильного приложения.. Давайте создадим мобильный пакет запустив команду:

    $ meteor create --package socially-mobile

Meteor создаст для нас пакет по-умолчанию. Главный файл - `package.js` будет выглядеть так:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.2" filename="packages/socially-mobile/package.js"}}

Как и с нашим авто-генерированным кодом, вам не нужно всё. В этом случае мы удалим сгенерированные файлы, которые мы сейчас не используем:

`packages/socially-mobile/socially-mobile-tests.js`

`packages/socially-mobile/socially-mobile.js`

Мы также удалим их из файла `package.js`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.3" filename="packages/socially-mobile/package.js"}}

Пакет готов и может быть добавлен к нашему проекту: 

  `$ meteor add socially-mobile`

Мы убедились, что AngularJS и angular-meteor загружаются перед нашим пакетом добавив эту зависимость с помощью метода `api.use`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.4"}}

### Модуль Angular Mobile 

В этой части мы создадим модуль Angular, который содержит маршрут для входа и соответствующий html для мобильных устройств.

Мы имеем ту же файловую структуру как и в основном проекте, поэтому файлы будут размещены в `client/lib/` внутри нашего проекта.

Проведём следующие шаги: 

Создайте модуль AngularJS и назовите его `socially.mobile`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.5"}}

Теперь создадим новый компонент для `login`, внутри нашего пакета:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.6"}}

И его отображение, пока что простой его вид:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.7"}}

### Добавьте файлы модуля к пакету

Нам нужно сообщить нашему `package.js` использовать файлы, которые мы недавно создали, так мы будем использовать `api.addFiles`.

Нам также необходимо добавить описание платформы, используя последний аргумент этих функций.

> Доступные платформы: `web.browser`, `web.cordova`, `client` (обое browser и cordova) и `server`.

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.8"}}

Наконец, нам нужно загрузить модуль `socially.mobile` только в окружении Cordova. Мы можем обновить `app.js` для загрузки модуля только при запуске всередине Cordova (`Meteor.isCordova`):

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.9"}}

Давайте попытаемся запустить его всередине эмулятора (я использовал iOS Simulator на Mac OS X), он должен выглядеть так:

{{tutorialImage 'angular1' 'step21_1.png' 500}}

Отлично! Похоже, что всё работает!

### Модуль Angular Browser 

Сделаем то же для добавления поддержки пакета браузера. Вначале загрузим другой модуль AngularJS, когда мы запускаемся в браузере:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.10"}}

Мы создадим пакет снова, с названием `socially-browser`, и удалим файлы по-умолчанию:

    $ meteor create --package socially-browser

Модифицируем файл `package.js` схожим образом как и в случае с мобильным пакетом:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.11" filename="packages/socially-browser/package.js"}}

> Внимание: мы загружаем файлы только в платформе `web.browser`.

Создайте модуль `socially.browser`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.11" filename="packages/socially-browser/client/lib/module.js"}}

Добавьте начальное отображение для логина в браузере:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.11" filename="packages/socially-browser/client/auth/login/login.html"}}

Добавьте начальный компонент:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.11" filename="packages/socially-browser/client/auth/login/login.component.js"}}

Добавьте пакет браузера к нашему проекту:

  $ meteor add socially-browser

### Реализация Browser Login

Мы будем использовать существующий код для веб-логина. Это значит 2 вещи:
- копирование файла оригинального отображения логина из `client/users/login/login.html` в наш пакет отображения (`login.html`):

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.14"}}

- перемещение текущего компонента `login` из `client/users/login/login.component.js` в наш пакет и обновление имени модуля для этого компонента:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.13"}}

> Мы также можем удалить `client/users/login/login.html` и `client/users/login/login.component.js` файлы. Они больше не нужны!

> **Заметка по поводу префикса имени пакета**: Важно знать, что если вы включаете префикс в описании имени пакета (как можно было увидеть в `21.11 Создание пакета socially-browser`) тогда вы должны помнить и включить этот префикс в абсолютных ссылках везде. Например, если имя пакета `name: 'my-prefix:socially-browser'` тогда templateUrl в вышеприведенном коде будет читаться как `templateUrl: '/packages/my-prefix:socially-browser/client/auth/login/login.html'`

### Реализация мобильного логина

Нужно разобраться с отображением логина для мобильных устройств. Мы создадим его в два шага:
- Определим телефонный номер пользователя
- Введём SMS верификацию

Мы будем использовать внешний пакет, который расширяет Meteor аккаунты, под названием `accounts-phone`, который верифицирует номера с помощью SMS, давайте сделаем это:

    $ meteor add okland:accounts-phone

И теперь мы добавим к мобильному пакету логику компонента `login`, котoрая использует пакет Accounts-Phone для верификации телефонного номера:

{{> DiffBox tutorialName="meteor-angular1-socially" step="21.17"}}

> В режиме разработки - СМСки не будут отправляться и верификационный код будет заносится в Meteor log.

Вот и всё! У нас разные компоненты для каждой платформы!

## Итоги

В этом уроке мы показали как сделать так, чтобы наш код вёл себя различным образом в мобильных устройствах и вебе и загружался по-разному. Мы достигли этого используя систему пакетов Meteor. Мы сделали это создав отдельные модули AngularJS с соответствующим кодом для мобильных устройств и веба загружая их в клиент, базируясь на платформе, которая запускает приложение.

{{/template}}
