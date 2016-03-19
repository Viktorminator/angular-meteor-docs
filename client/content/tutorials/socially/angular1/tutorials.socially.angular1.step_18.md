{{#template name="tutorials.socially.angular1.step_18.md"}}
{{> downloadPreviousStep stepName="step_17"}}


На этом этапе мы переключимся с *Twitter Bootstrap* на [*angular-material*](https://material.angularjs.org/#/).

Angular-material - это Angular 1 реализация Google [Material Design спецификации](http://www.google.com/design/spec/material-design/introduction.html). Material Design  - это mobile-first язык разработки используемый во многих приложениях Google. Особенно на платформах Android.

Удалим из прилоежния bootstrap. Наберите в консоли:

    meteor remove twbs:bootstrap

Теперь добавим пакет angular-material Meteor:

    meteor add angular:angular-material

Далее нужно вставить angular-material модуль в наше Angular 1 приложение. Отредактируйте `client/lib/app.js` и добавьте `ngMaterial`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.3"}}

> Не забудьте удалить зависимость `ui.bootstrap`! Она больше не нужна!

Вот и всё, теперь мы можем использовать `angular-material` в макете нашего приложения.

Сейчас приложение будет выдавать ошибки, так как мы использовали такие службы как `$modal` которые принадлежат к ядру bootstrap.

Давайте починим это используя `$mdDialog` вместо `$modal`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.4"}}

Нам нужно модифицировать логику `close` модуля:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.5"}}

Отлично! Теперь, чтобы избавится от всяких bootstrap элементов, нам нужно кое-что удалить и поменять CSS и LESS.

> Обратите внимание, что изменения в этом коммите включают удаление правил CSS, которые использовались для перезаписи дизайна Bootstrap.

Angular-material использует декларативный синтак, например директивы для использования элементов Материального дизайна в HTML документах.

Во-первых нам нужно изменить наш `index.html` для использования макета flex сетки Материального дизайна. Приведём `client/index.html` к виду:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.6"}}

Также нужно поменять наши `.less` файлы. Я это сделал, можете посмотреть [этот коммит](https://github.com/Urigo/meteor-angular-socially/commit/0eb86d862ad703f5887fee3d558414ac14873a5c);

Как вы заметили мы используем `layout="column"` в теге `body`, который сообщает angular-material разместить все внутренние теги `body` вертикально.

Далее мы используем удобную директиву `md-toolbar` как обёртку для тулбара нашего приложения.
Мы сообщаем ей обрезать вертикальный скрол с помощью аттрибута `md-scroll-shrink` и разместить все внутренние элементы в ряд.

Также мы сообщаем `layout-align="start center"` разместить внутренние элементы начиная с первичного направления (ряд), что значит что элемент должен начинаться с левого угла и расположить их в `центре` вторичного направления (колонки), так они расположаться посередине в вертикальном направлении.
Также придадим отступы вокруг внутренних элементов с помощью `layout-padding`.

Всередине `md-toolbar` вы увидите, что мы использовали

    <span flex></span>

элемент, который на самом деле является - пустым элементом, который используется для заливки пустого пространства между вторым и третьим элементов в тулбаре.

Таким образом у нас есть ссылка на вечеринки слева, span для заполнения пространства и кнопка входа.
Элемент макета flex сетки - очень простой и интуитивный в `angular-material`, можете детальнее познакомиться [здесь](https://material.angularjs.org/#/layout/grid).

Далее нам нужно преобразовать отображения списка вечеринок и деталей вечеринки в `angular-material`.

Заменим вначале код в нашем `parties-list.html` на код:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.8"}}

Что мы сделали:

* Обернули всё в тег `md-content`
* Заменили все кнопки на теги `md-button`
* Обернули поля ввода формы в теги `md-input-container`, что даст нам материальные ярлыки для полей ввода
* Добавили иконки из Материального дизайна
* Use `md-card-content` to display each item in the list

## Иконки

Ещё в наше приложение нужно добавить использование иконок Материального дизайна. Google предоставляет бесплатные иконки. Вы можете их установить набрав:

    meteor add planettraining:material-design-icons

Нам нужно определить провайдер иконок `$mdIconProvider` в `client/lib/app.js`. Вставьте эти строки сразу после декларации `angular.module`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.10"}}

Вам не нужно определять все эти иконки.
Вам нужно просто определить те, которые будете использовать.
Список всех доступных иконок можно найти [тут](http://google.github.io/material-design-icons/).
Выводить иконки нужно следующим образом:

    <md-icon md-svg-icon="content:ic_clear_24px"></md-icon>

В аттрибуте `md-svg-icon` вы указываете значения `<iconset>:<icon_name>` в нашем случае `content:ic_clear_24px`.

Теперь заменим код в `party-details.html` на следующий:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.11"}}

Замените теперь HTML нового модального окна вечеринки:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.12" filename="client/parties/add-new-party-modal/add-new-party-modal.html"}}

Как видите, мы используем специальный тип кнопки `angular-material`. Наша кнопка со ссылкой:

    <md-button ng-href="/parties">Cancel</md-button>

Angular-Material создаёт обычные кнопки, которые направляют по ссылке используя `ng-href`.

## Создание основной страницы компонента

Теперь нужно закончить с основной странице - мы не использовали там компонента, поэтому закончим с этим.

Заменим весь HTML на новый тег - `socially`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.13"}}

Создадим отображение к этому компоненту всередине `client/socially` папки (создайте её вначале):

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.14"}}

Теперь создадим компонент с JS кодом:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.15"}}

## Angular 1 формы и Accounts-UI Материальный дизайн

Далее сменим дизайн управления страниц на материальный.

Чтобы это сделать, мы определим наши маршруты к аккаунтам вручную и будем использовать Meteor и Accounts API в нашем коде.

Сменим код всередине `socially.html` странице и используем кастомные кнопки вместо `login-buttons`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.16"}}

Выведем `Login` и `Sign up` кнопки для незалогиненного пользователя и мы покажем пользовательское имя и `Logout` кнопку для залогиненного пользователя.

После создания этих кнопок, нам нужно назначить для них соответственные маршруты в аттрибутах `ng-href` этих кнопок.

Откройте `client/routes.js` и вставьте следующие маршруты внизу под строкой `$stateProvider` и выше существующих маршрутов:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.17"}}

Мы определили маршруты и состояния и давайте продолжим и создадим их. Создайте подпапку `users` в папке `client/` и в подпапке `users` для каждого компонента мы создадим: логин, сброс паролей и выход.

Создайте файл `.js` для компонента login:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.18"}}

Мы создали объект `credentials`, содержащий email и пароль, которые будут передаваться нам от отображения. Мы создали метод `login`, который будет выполнятся для залогиненного пользователя.

Мы использовали `Meteor.loginWithPassword()`, который принимает 3 аргумента

1. Имя пользователя / E-mail
2. Пароль
3. Колбек результата

В колбеке результата мы обработам успешный ответ и перенаправим пользователя в состояние `parties` или выдадим сообщение об ошибке для нашего свойства `this.error` компонента.

И конечно же, отображение компонента login:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.19"}}

Теперь у нас есть login, и нам нужно сделать logout. 

Создайте метод в компонентe `socially`, который использует Meteor API:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.20"}}

Процедура такая же для `register` и `resetpw` маршрутов!

Это логика компонента подписки:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.21"}}

И его отображение:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.22"}}

И логика компонента сброса пароля:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.23"}}

И, наконец, создание соответствующего отображения:

{{> DiffBox tutorialName="meteor-angular1-socially" step="18.24"}}

# Итоги

В этой главе мы разобрались с двумя вещами:

1. Как работать с angular-material-design в нашем angular-meteor приложении
2. Как создавать кастомные Angular 1 формы для авторизации в нашем приложении

Надеюсь кто-то из вас создаст пакет accounts-ui основываясь на этом коде и сохранит наш код!

{{/template}}