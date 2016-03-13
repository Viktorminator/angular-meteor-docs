{{#template name="tutorials.socially.angular1.step_05.md"}}
{{> downloadPreviousStep stepName="step_04"}}

На этом этапе вы научитесь создавать шаблон макета и как строить приложение, которое имеет различные отображения через добавление маршрутов используя Angular 1 модуль называемый `ui-router`.

Цели этого этапа:

* Когда вы переходите к `index.html`, вы будете перенаправлены к `index.ng.html/parties` и список вечеринок должен появится в браузере.
* Когда вы нажимаете ссылку вечеринки URL должен поменятся на соответствующий этой вечеринке иstub of a party detail page is displayed.

# Зависимости

Функциональность маршрутов добавленная на этом этапе обеспечена модулем [ui-router](https://github.com/angular-ui/ui-router), который распространяется отдельно от ядра Angular 1.

Мы установим ui-router с помощью [официального пакета Meteor](https://atmospherejs.com/angularui/angular-ui-router).

Наберите это в командной строке:

    meteor add angularui:angular-ui-router

Далее добавьте ui-router как зависимость к нашему приложению в `app.js`:

{{> DiffBox tutorialName="angular-meteor" step="5.2"}}

# Множественные отображения, маршрутизация и шаблон макета

Наше приложение медленно растёт и стаёт более сложным.
До этого момента приложение выдавало пользователям только одно отображение (список всех вечеринок) и весь код шаблона находился в файле `index.ng.html`.

Следующим шагом добавим отображение, которое покажет детальную информацию про каждую вечеринку в нашем списке.

Для добавления детального вида, мы могли бы расширить файл `index.ng.html` и поместить туда код обеих отображений, но всё станет очень быстро запутанным.

Вместо этого, мы переделаем шаблон `index.html` в так называемый "шаблон макета". Это шаблон общий для всех отображений в нашем приложении.

Другие "частные шаблоны" включаются в этот шаблон макета в зависимости от текущего "маршрута" -  отображения, которое выводится в данный момент пользователю.

Маршруты приложения в Angular 1 декларируются через [$stateProvider](https://github.com/angular-ui/ui-router/wiki), который является провайдером службы $state.
Эта служба облегчает соединение контроллеров, шаблонов отображений и текущее URL положение в браузере.
Используя эту функцию мы можем развернуть глубокую перелинковку, которая позволит нам использовать историю браузера (навигацию вперёд-назад) и закладки.


# Шаблон

Служба $state обычно используется вместе с директивой uiView.
Роль директивы uiView - включить шаблон отображения для текущего маршрута в шаблон макета.
Это отлично подходит для нашего `index.ng.html` шаблона.

Давайте создадим новый html-файл `parties-list.ng.html` и вставим существующий код списка из `index.ng.html` в него:

{{> DiffBox tutorialName="angular-meteor" step="5.3"}}

Код почти такой же за исключением двоих изменений:

- Добавленна ссылка с названиями вечеринок (эта ссылка ведёт на детальную страницу вечеринки)
- Удаление div с ng-controller информацией будет обработан определением маршрутов (см. дальше)

Вернёмся к `index.html` и заменим содержимое с директивой `ui-view`:

{{> DiffBox tutorialName="angular-meteor" step="5.4"}}

Обратите внимание - мы сделали 3 вещи:

1. Заменили весь контенк с ui-view (это будет отвечать за включение правильного содержимого при нажатии на текущий URL).
2. Добавили `h1` header with a link to the main parties page.
3. We also added a `base` tag in the head (required when using HTML5 location mode).

Now we can delete the `index.ng.html` file, it's not used any more.

Let's add a placeholder to the new party details page.
Create a new html file called `party-details.ng.html` and paste in the following code:

{{> DiffBox tutorialName="angular-meteor" step="5.6"}}

This code can serve as a placeholder for now. We'll get back to filling out the details later on.

# Routes definition

Now let's configure our routes.
Add this config code in `app.js`, after the Angular 1 app has been defined:

{{> DiffBox tutorialName="angular-meteor" step="5.7"}}

Using the Angular 1 app's .config() method, we request the `$stateProvider` to be injected into our config function and use the state method to define our routes.

Our application routes are defined as follows:

* **('/parties')**: The parties list view will be shown when the URL hash fragment is /parties. To construct this view, Angular 1 will use the parties-list.ng.html template and the PartiesListCtrl controller.
* **('/parties/:partyId')**: The party details view will be shown when the URL hash fragment matches '/parties/:partyId', where :partyId is a variable part of the URL. To construct the party details view, Angular will use the party-details.ng.html template and the PartyDetailsCtrl controller.
* **$urlRouterProvider.otherwise('/parties')**: Triggers a redirection to /parties when the browser address doesn't match either of our routes.
* **$locationProvider.html5Mode(true)**: Sets the URL to look like a regular one. more about it [here](https://docs.angularjs.org/guide/$location#hashbang-and-html5-modes).
* Each template gets loaded by it's **absolute path** to the project's top folder ('party-details.ng.html').  this is done by angular-meteor's build process which loads the templates into a cache and names them according to their paths.
If the templates are coming from a package, they will get a prefix of the package name like so - 'my-app_my-package_client/views/my-template.ng.html'.
You can read more about the templating build process in [our code](https://github.com/Urigo/angular-meteor/blob/master/plugin/handler.js).

Note the use of the `:partyId` parameter in the second route declaration.
The $state service uses the route declaration — `/parties/:partyId` — as a template that is matched against the current URL.
All variables defined with the : notation are extracted into the $stateParams object.

# Controllers

As you might have seen we removed the controller definition from the ng-controller directive in the `index.ng.html` and moved it into the routes definitions.

But we still need to define our `PartyDetailsCtrl` controller.
Add this code under the existing controller:

{{> DiffBox tutorialName="angular-meteor" step="5.8"}}

Now all is in place.  Run the app and you'll notice a few things:

* Click on the link in the name of a party - notice that you moved into a different view and that the party's id appears in both the browser's url and in the template.
* Click back - you are back to the main list, this is because of ui-router's integration with the browser's history.
* Try to put arbitrary text in the URL - something like http://localhost:3000/strange-url.  You should to be automatically redirected to the main parties list.


#### Common Mistakes

If you haven't entered the correct absolute path when defining the routes (e.g. by accident adding a relative one), then you might get the following error:

WARNING: Tried to load angular more than once.

If that's the case, double check your paths and remember to use the file extension `.ng.html`.


# Summary

With the routing set up and the parties list view implemented, we're ready to go to the next step and implement the party details view.

{{/template}}
