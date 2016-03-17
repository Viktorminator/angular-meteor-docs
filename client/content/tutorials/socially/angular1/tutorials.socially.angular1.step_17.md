{{#template name="tutorials.socially.angular1.step_17.md"}}
{{> downloadPreviousStep stepName="step_16"}}

Meteor отлично поддерживает CSS.

Как и другие типы файлов, Meteor собирает все CSS файлы для и клиент получает сборку всех CSS в своём приложении.

Meteor также предобрабатывает CSS файлы и минифицирует их (при разработке это не делается для лёгкой отладки).

Meteor также поддерживает [LESS](http://lesscss.org/) как препроцессор CSS по-умолчанию (пакет поддерживается комадной Meteor лично).

LESS добавляет в СSS динамическое поведение, такое как переменные, миксины, операции и функции. Он позволяет создавать более компактные стили и помогает уменьшать повторения кода  в CSS файлах.

С установленным less пакетом, .less файлы в вашем приложении автоматичесчки компилируются в CSS и результат включается в клиентскую CSS сборку.

# Добавление простых стилей

Создайте новую папку 'styles' в папке 'client' и добавьте новый файл - main.css в этой папке.

Далее смените цвет фона нашего приложения:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.1"}}

Запустите приложение и посмотрите как поменялся цвет фона.

# CSS фреймворки

Существует много CSS фреймворков, которые помогают писать меньше CSS кода.

Наиболее популярным сейчас является [Bootstrap](http://getbootstrap.com/), поэтому мы будем использовать его в этом уроке.

* Также будем  присматриваться к [Polymer](https://www.polymer-project.org/) и [Famo.us/angular](http://famo.us/integrations/angular) и можем обновить уроки с их использованием вместо Bootstrap в будущем.

Добавим Bootstrap Meteor пакет к нашему проекту:

    meteor add twbs:bootstrap


Давайте начнём с создадния ссылки Home и кнопки login всередине панели навигации и остального содержимого всередине Bootstrap класса .container.

Всередине `index.html` окружённого `h1` и `login-buttons` с таким хедером:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.3"}}

Преобразования в Bootstrap не заканчиваются здесь. Применение bootstrap стилей к другим частям нашего приложения Socially, наш сайт будет выглядеть лучше в различных экранах. Посмотрите на [Code Diff](https://github.com/Urigo/meteor-angular-socially/compare/step_16...step_17), чтобы увидеть как мы поменяли структуру основных файлов.

# Less

OK, просто работа со стилями, но нам хотелось бы использовать [LESS](http://lesscss.org/).

Вначале сменим имя файла с main.css на `main.less`. Теперь добавим пакет LESS в командной строке:

    meteor add less

Протестируйте и увидите, что всё работало как и ранее.

Перейдите в `main.less` и добавьте различные цвета фона к панели навигации:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.5"}}


Обратите внимание, что мы включили `.navbar` всередину body. Это часть [LESS синтаксиса для правил включения](http://lesscss.org/features/#features-overview-feature-nested-rules).

# Дизайн списка вечеринок

Сейчас мы поработаем над дизайном списка вечеринок.

Мы можем начать с написания специфического LESS кода всередине этого файла.

Мы сделаем некоторые изменения в нашем коде, чтобы сделать его более читаемым и создадим CSS правила для получения преимущества Bootstrap.

Сперва добавим `angular-ui-bootstrap`, запустив команду:

    meteor add angularui:angular-ui-bootstrap

Теперь добавим классные шрифты и некоторые CSS изменения, чтобы `index.html` выглядел лучше:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.7"}}

Подтвердим, что у нас есть зависимость `ui.bootstrap` в определении модуля, так как мы будем позже использовать службу `$modal`.

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.8"}}

Теперь мы переместим логику создания новой вечеринки в новый компонент, давайте вначале его создадим:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.9"}}

И его отображение:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.10"}}

Теперь мы сделаем некоторые изменения в отображении компонента `partiesList` с целью соответствия новой структуры с `$modal` и добавим пару стилей к объекту Google Maps object.

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.11"}}

Обновим отображение списка вечеринок и соотвествие новой логике - мы используем `$modal` для отрытия новой страницы вечеринки и поменяем макет используя Bootstrap сетку.

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.12"}}

Я добавлю пару правил CSS, чтобы наша структура выглядела лучше.

Добавим некоторые переменные для определения наших цветов в файле `main.less`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.13"}}

Сделаем некоторые изменения в отображении деталей вечеринки используя Bootstrap:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.14"}}

Добавим недостающий хелпер `currentUserId`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.15"}}

Правила стилей размещены в `.less` файлах для каждого компоенента и текущие изменения доступны в [этом коммите](https://github.com/Urigo/meteor-angular-socially/commit/a261d7f1126cdde64194dc2fdcc7946940c9a56d), because the CSS is not the most relevant part in this tutorial.

Давайте включим этот файл в наш `main.less`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="17.17"}}

Вот и всё! Теперь у нас отличный стиль и лучший вид с CSS используя Bootstrap и LESS!

# Итоги

Мы научились использовать CSS, LESS и Bootstrap в Meteor.

{{/template}}