{{#template name="tutorials.socially.angular1.step_19.md"}}
{{> downloadPreviousStep stepName="step_18"}}

В этой части мы разберёмся с использованием библиотек 3х сторон вместе с angular-meteor.

Часть этого занятия относится к пользователям, которые используют только Meteor, без angular-meteor, так как решение использованиz библиотек 3х сторон пришло из системы пакетов Meteor - **Atmosphere**.

В этом уроке рассмотрим как можно одну проблему решить несколькими способами - используя библиотеки 3х строн вместе с Meteor и angular-meteor.


Каждый разработчик Angular 1 знает и использует библиотеки 3х сторон (такие как angular-ui-bootstrap, ui-router, и др.), но так как у нас нет возможности легко включать ".js" файл в наш тег "head", то нам нужно другое решение.

С целью добавления библиотек 3х сторон к вашему Meteor проекту - вы можете использовать `meteor add PACKAGE_NAME` командну для добавления пакета.

Также для **большинства** библиотек 3х сторон Angular 1 - уже существует пакет Meteor package (эквивалент bower или NPM).

Вы можете найти эти пакеты используя [Atmosphere](https://atmospherejs.com/) сайт.

Например, для использования**[angular-ui-router](https://atmospherejs.com/angularui/angular-ui-router)** в вашем проекте просто запустите команду:
```
meteor add angularui:angular-ui-router
```
А далее просто используйте angular-ui-router в вашем angular-meteor проекте, добавив инициализацию модуля Angular 1:
```
angular.module('myModule', [
  'angular-meteor',
  'ui.router'
]);
```
**Но это лёгких случай** - так как angular-ui-router - это самая часто используемая библиотека Angular 1.

В некоторых случаях, нам нужно использовать некоторые пакеты, для которых нет Atmosphere пакета - вы можете использовать следующие инструкции для решения вашей проблемы.

**Но вначале**, нам нужно понять как Meteor и его пакетный менеджер - Atmosphere, работает: любой пакет, зарегистрированный в Atmosphere имеет некоторые мета-данные и реальные JS и CSS файлы, что записано в файле "**package.js**".

Мета-данные содержат эти параметры:

* Название пакета
* Версия пакета (базируясь на [semver](http://semver.org/) стандарте)
* Итоги пакета
* GIT репозиторий пакета
* Файл документации пакета (обычно это README.md файл)

Также каждый пакет декларирует:

* Зависимости пакета (от других библиотек 3х сторон).
* Минимальную версию Meteor.
* Файлы, которые используются (обычно CSS и JS файлы библиотеки).
* JavaScript экспорты (если есть таковые).

Подробную информацию по созданию пакетов Meteor и их версий можете узнать [здесь](https://meteorhacks.com/meteor-packaging-system-understanding-versioning) статью от MeteorHacks.

Это пример файла package.js для angular-ui-router:
```
// файл метаданных пакета Meteor.js
var packageName = 'angularui:angular-ui-router'; // https://atmospherejs.com/angularui/angular-ui-router
var where = 'client'; // куда устанавливать: 'client' или 'server'. Для обеих, оставляйте пустым.
var version = '0.2.14';

// Мета-данные
Package.describe({
  name: packageName,
  version: version,
  summary: 'angular-ui-router (official): Flexible routing with nested views in Angular 1',
  git: 'git@github.com:angular-ui/ui-router.git',
  documentation: null
});

Package.onUse(function(api) {
  api.versionsFrom(['METEOR@0.9.0', 'METEOR@1.0']); // Meteor версия

  api.use('angular:angular@1.0.8', where); // Зависимости

  api.addфайлы('release/angular-ui-router.js', where); // Используемые файлы
});
```

# Решение #1
Это решение наиболее полезное решение из всех, так как в конце этого решения вы получите эти результаты:

* Пакет появится в списке менеджеров пакетов Atmosphere и вы сможете использовать его с помощью команды "meteor add".
* Вы создадите пул-запрос к автору пакета с целью обеспечить поддержку Meteor для пакета.

**Если вы не заинтересованы в таком решении и просто хотите делать "обезъяний патч" - тогда используйте Решение #2.**

С целью обучения, будем использовать [angularjs-dropdown-multiselect](https://github.com/dotansimha/angularjs-dropdown-multiselect) как пример пакета.

Сперва нужно добавить форк репозитория и клонировать его наш компьютер используя команду `git clone FORKED_REPOSITORY_LINK`.

Далее, создайте файл "`package.js`" в корневой директории проекта и используйте этот базовый шаблон:
```
// файл метаданных пакета для Meteor.js
var packageName = 'PACKAGE_NAME';
var where = 'client'; // куда устанавливать: 'client' или 'server'. Для обеих оставляйте пустым.
var version = 'PACKAGE_VERSION';
var summary = 'PACKAGE_SUMMARY';
var gitLink = 'GIT_LINK';
var documentationFile = 'README.md';

// Мета-данные
Package.describe({
  name: packageName,
  version: version,
  summary: summary,
  git: gitLink,
  documentation: documentationFile
});

Package.onUse(function(api) {
  api.versionsFrom(['METEOR@0.9.0', 'METEOR@1.0']); // Meteor версия

  api.use('DEPENDENCY_NAME', where); // Зависимости

  api.addфайлы('FILE_NAME', where); // используемые файлы
});   
```

Теперь просто запишите информацию в файл, согластно информации пакета, файл `bower.json` может быть полезен для заполнения недостающей информации.

В нашем случае окончательный файл `package.js` примет вид:
```
// файл метаданных пакета для Meteor.js
var packageName = 'dotansimha:angularjs-dropdown-multiselect';
var where = 'client'; // куда устанавливать: 'client' или 'server'. Для обеих оставляйте пустым.
var version = '1.5.2';
var summary = 'Angular 1 Dropdown Multiselect directive';
var gitLink = 'https://github.com/dotansimha/angularjs-dropdown-multiselect';
var documentationFile = 'README.md';

// Meta-data
Package.describe({
  name: packageName,
  version: version,
  summary: summary,
  git: gitLink,
  documentation: documentationFile
});

Package.onUse(function(api) {
  api.versionsFrom(['METEOR@0.9.0', 'METEOR@1.0']); // Meteor версии

  api.use('angular:angular@1.2.0', where); // Зависимости
  api.use('stevezhu:lodash@3.5.0', where); // Удивлены?! Читайте файл bower.json!

  api.addфайлы('src/angularjs-dropdown-multiselect.js', where); // используемые файлы
});   
```

**Важные замечания:**

* Префикс для названия пакета должен быть именем автора пакета - вы можете использовать ВАШЕ имя пользователя или создать Метеор организацию и использовать её имя.
* Зависимости строятся в таком виде: AUTHOR\_NAME:PACKAGE\_NAME@VERSION, вы также можете СПЕЦИАЛЬНО указать особенную версию, добавив "=" перед номером версии.
* В этом случае нужна зависимость от *Lodash*, поэтому мы поищем по сайту Atmposhere пакет Lodash и найдём полное имя пакета.
* Вы спокойно можете использовать файл-источник и неминифицированную версию пакета, так как на продакшене Метеор минифицирует всё за вас!

После создания файла `package.js`, нам нужно запустить команду publish Метеор в папке с файлом пакета:
```
meteor publish
```

Если пакет не существует, то нам необходимо добавить флаг `--create`:
```
meteor publish --create
```

Теперь пакет будет зарегистрирован в менеджере пакетов Atmosphere и вы сможете его использовать.

Последней частью этого решение является коммит файла `package.js` в форкнутый вами репозитории и далее создание пул-запроса для помощи автору пакета для поддержки пакета Метеор на регулярной основе.

**Если вы хотите быть ещё более полезным** - можете использвать [эти инструкции](https://github.com/MeteorPackaging/grunt-gulp-meteor) для создания Gulp/Grunt задачи, которая автоматически опубликует новую версию пакета каждый раз, когда разработчик делает новый релиз.

# Решение #2
Это более простое решение - это решение будет полезным в случае, если вы просто хотите создать Atmosphere пакет не помогая разработчику интегрировать Meteor в его процес публикации разных версий.

ВЫ можете просто использовать `bower`, установить пакет локально на свой компьютер, дальше создать `package.js` файл и запустить `meteor publish`.

Можно ещё больше полениться! Есть инструмент с названием [angular-meteor-publisher](https://github.com/dotansimha/meteor-package-publisher), который обеспечивает полное решение передачи пакета от `bower` (будет поддерживать NPM в будущем) в `Atmosphere` просто предоставив метаданные.

Вам нужно просто предоставить базовую информацию о исходном пакете (метаданные `bower`, обычно это `bower install angular-ui-router@0.2.14` и метаданные цели (информация, которую требует Atmosphere), добавить JS и CSS файлы и зависимости пакета и запустить команду:
```
node meteor-package-publisher myJSONfile.json
```

Например, вот как пакет ngInfiniteScroll будет опубликован используя этот инструмент (JSON файл метаданных):
```
{
  "source": {
    "type": "bower",
    "name": "ngInfiniteScroll",
    "version": "1.2.1"
  },
  "destination": {
    "atmospherePackageName": "ng-infinite-scroll",
    "atmosphereOrganizationName": "srozegh",
    "summary": "Infinite Scrolling for Angular 1",
    "git": "https://github.com/sroze/ngInfiniteScroll",
    "documentation": null,
    "versionsFrom": "METEOR@0.9.0.1",
    "файлыToAdd": [
      {"name": "build/ng-infinite-scroll.js", "platforms": ["client"]}
    ],
    "use": [
      {"name": "jquery@1.11.3_2", "platforms": ["client"]},
      {"name": "angular:angular@1.3.15_1", "platforms": ["client"]}
    ]
  }
}
```

# Итоги
Как видите, очень просто использовать существующие пакеты в вашем angular-meteor приложении и когда вы сталкиваетесь со сторонними библиотеками без поддержки через Метеор пакет, то можно просто создать файл "package.js" и сделать пул-запрос (Решение #1).

Также вы можете использвоать автопубликатор, который помогает очень быстро публиковать пакеты (Решение #2).

**Мы предпочитаем первое решение, так как оно обогащает Meteor и angular-meteor сообщество и помогает другим разработчикам использвоать пакеты 3х сторон.**

{{/template}}
