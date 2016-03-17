{{#template name="tutorials.socially.angular1.step_15.md"}}
{{> downloadPreviousStep stepName="step_14"}}

Angular 1 содержит отличные и очень простые директивы, которые скрывают и показывают DOM элементы исходя из условий.
You can bind them to an expression, variables or functions.

# ng-show и ng-hide

Сперва, познакомимся с [ng-show](https://docs.angularjs.org/api/ng/directive/ngShow) и [ng-hide](https://docs.angularjs.org/api/ng/directive/ngHide).

Мы хотим скрыть и показать форму создания новой вечеринки. Если пользователь не вошёл и он не может создать вечеринку, зачем тогда выводить для него форму?
Если пользователь не вошёл, мы хотели бы вывести сообщение о том, что нужно войти для создания новой вечеринки.

В `parties-list.html` добавьте директиву `ng-show` для показа формы:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.1"}}

Далее нам нужно добавить возможность определения зашёл ли пользователь в данный момент, добавим для этого хелпер:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.2"}}

Сразу после формы, добавьте этот HTML:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.3"}}

Как раз наоборот, если - if `isLoggedIn` true или мы в обработке логина прячем этот div.

Сделаем то же с кнопками RSVP:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.4"}}

Добавим это в конец наших кнопок RSVP:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.5"}}

Следующим скроем опцию 'delete party' на случай, если залогиненный пользователь не является владельцем вечеринки.

Добавим ng-show к кнопке delete следующим образом:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.6"}}

Здесь вы увидите, что `ng-show` может получить оператор, в нашем случае - пользователь существует (залогинен) и является владельцем вечеринки.

Мы пропустили использование хелпера:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.7"}}


# ng-if

[ng-if](https://docs.angularjs.org/api/ng/directive/ngIf) работает почти так же как и `ng-show` с разницей, что `ng-show` скрывает элемент меняя отображение свойство css display и `ng-if` просто удаляет его из DOM.

Давайте используем `ng-if`, чтобы скрыть выдающиеся приглашения из вечеринки, если она публична (все приглашены!):

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.8"}}

# Назначение функции

Давайте скроем 'Users to invite' всередине `party-details.html` на случай, если пользователь не вошёл или не может быть приглашён на вечеринку:

Чтобы сделать это мы создадим функцию окружения, которая возвращает boolean и ассоциирует его с `ng-show`:

Создайте новую функцию всередине компонента `partyDetails` с названием `canInvite`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.9"}}

и добавьте `ng-show` к `ul` в `party-details.html` и добавьте `li`, который сообщает пользователю, что каждый приглашённый, если он в кейсе:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.10"}}

Вот, мы берём результат неприглашённых пользователей и проверяем его длину.

Нам нехватает только хелперов в этом компоненте, добавим же их:

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.11"}}


# ng-disabled

Давайте отключим поля ввода `partyDetails` на случай, если у пользователя нет разрешения на их смену (на данный момент сервер останавливает пользователя, но нет никаких визуальных фидбеков от сервера на перезапись локальных исправлений сразу же после):

{{> DiffBox tutorialName="meteor-angular1-socially" step="15.12" filename="client/parties/party-details/party-details.html"}}

# Итоги

Теперь наш пример выглядит гораздо лучше, после того, как мы скрыли элементы исходя из текущей ситуации.

В следующих главах мы добавим Google Maps и некоторые CSS стили к нашему приложению.

{{/template}}