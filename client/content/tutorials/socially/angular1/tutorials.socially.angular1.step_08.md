{{#template name="tutorials.socially.angular1.step_08.md"}}
{{> downloadPreviousStep stepName="step_07"}}

Один из наиболее мощных Meteor пакетов - это система аккаунтов Meteor.

Прямо сейчас наше приложение публикует все вечеринки, всех клиентов и клиенты могут менять эти вечеринки. Изменения отображаются у всех клиентов автоматически. 

Это супермощно и легко, но что по поводу безопастности? Мы не хотим, чтобы любой пользователь менял вечеринку...

Сперва нам унжно убрать небезопасный пакет, который автоматически добавляется к любому приложению Meteor.

'Небезопасный' пакет по-умолчанию предоставляет полный доступ всем к коллекциям Meteor.

Удалив этот пакет по-умолчанию мы запретим всем доступ.

Сделайте это выполнив команду:

    $ meteor remove insecure

Теперь попробуем сменить массив вечеринок или отдельную вечеринку. Мы получим:

    remove failed: Access denied
    
В консоли, так как у нас нет разрешение сделать это.

Теперь, нам нужно написать дополнительное правило безопастности для каждой операции с Mongo коллекцией, которую мы хотим разрешить клиенту выполнять.

Во-первых, добавим пакет `accounts-password`.
Это очень мощный пакет для всех операций пользователя, о которых вы хотите подумать: вход, подписка, смена пароля, восстановление пароля, подтверждение email и остальное.

    $ meteor add accounts-password

Также мы добавим пакет `dotansimha:accounts-ui-angular`. Этот пакет содержит все HTML и CSS, которые нам нужны для операций пользователя с формами.

Позже в этом уроке мы заменим формы по-умолчанию в account-ui формами Angular 1.

    $ meteor add dotansimha:accounts-ui-angular

Давайте добавим зависимость модуля `accounts.ui` к нашему определению модуля.

{{> DiffBox tutorialName="meteor-angular1-socially" step="8.4"}}

Добавим директиву `login-buttons` (часть модуля `accounts.ui` module) в наше приложение в файл index.html.

Теперь наш `index.html` примет вид:

{{> DiffBox tutorialName="meteor-angular1-socially" step="8.5"}}

Запустите код, создайте аккаунт, вход, выход...

Теперь у нас есть аккаунт система, мы можем начать определять наши правила безопастности для вечеринок.

Перейдём к папке моделей и сменим её вид на такой:

{{> DiffBox tutorialName="meteor-angular1-socially" step="8.6"}}

[Функция Meteor collection.allow](http://docs.meteor.com/#/full/allow) определяет разрешения необходимые клиенту для записи напрямую в коллекцию (что мы и делали до сих пор).

В каждом колбеке типов действий (insert, update, remove) функции должны возвращать true, если они думают, что операции должны быть разрешены.
Иначе они должны возвращать false или совсем ничего (undefined).

Доступные колбеки:

* `insert(userId, doc)`

  Пользователь userId хочет вставить документ doc в коллекцию. Возвращает true если это позволено.

  doc будет содержать _id поле, если оно избыточно было установлено клиентом или есть активная трансформация. Вы можете использовать это для защиты пользователей от определения _id полей.

* `update(userId, doc, fieldNames, modifier)`

  Пользователь userId хочет обновить документ doc. (doc - это текущая версия документа из базы данных, без предложенного обновления). Возвращает true при разрешении изменений.

  fieldNames - это массив (top-level) полей в doc that the client wants to modify, for example ['name', 'score'].

  modifier is the raw Mongo modifier that the client wants to execute; for example, {$set: {'name.first': "Alice"}, $inc: {score: 1}}.

  Only Mongo modifiers are supported (operations like $set and $push). If the user tries to replace the entire document rather than use $-modifiers, the request will be denied without checking the allow functions.

* `remove(userId, doc)`

  The user userId wants to remove doc from the database. Return true to permit this.


In our example:

* insert - only if the user who makes the insert is the party owner.
* update - only if the user who makes the update is the party owner.
* remove - only if the user who deletes the party is the party owner.


OK, right now none of the parties has an owner so we can't change any of them.

So let's add the following simple code to define an owner for each party that gets created.

We can use `Meteor.userId()` method which returns the current user id. 

We will use this id that add it to our new party:

{{> DiffBox tutorialName="meteor-angular1-socially" step="8.7"}}

So first we set the new party's owner to our current user's id and then insert it to the parties collection like before.

Now, start the app in 2 different browsers and login with 2 different users.

Test editing and removing your own parties, and try to do the same for parties owned by another user.

# Social login

We also want to let users login with their Facebook and Twitter accounts.

To do this, we simply need to add the right packages in the console:

    meteor add accounts-facebook accounts-twitter

Now run the app.  when you will first press the login buttons of the social login, meteor will show you a wizard that will help you define your app.

You can also skip the wizard and configure it manually like the explanation here: [http://docs.meteor.com/#meteor_loginwithexternalservice](http://docs.meteor.com/#meteor_loginwithexternalservice)

There are more social login services you can use:

* Facebook
* Github
* Google
* Meetup
* Twitter
* Weibo
* Meteor developer account


# Authentication With Routers

Now that we prevented authorized users from changing parties they don't own,
there is no reason for them to go into the party details page.

We can easily prevent them from going into that view using our routes.

We will use Meteor's API again, and we will use AngularJS `$q` to easily create promises.

Our promise will resolve it there is a user logged in, and otherwise we will reject it.

We are going to use the [resolve](https://github.com/angular-ui/ui-router/wiki#resolve) object of ui-router and ngRoute:

{{> DiffBox tutorialName="meteor-angular1-socially" step="8.9"}}

Now, if a user is not logged in to the system, it won't be able to access that route.

We also want to handle that scenario and redirect the user to the main page.

on the top of the routes file, let's add these lines (the `run` block), and we will also add a "reason" for the `reject()` call, to detect if it's our reject that cause the route error.

{{> DiffBox tutorialName="meteor-angular1-socially" step="8.10"}}

# Summary

Amazing, only a few lines of code and we have a secure application!

Please note it is possible for someone with malicious intent to override your route on the client (in the client/routes.js). 
As that is where we are validating the user is authenticated, they can remove the check and get access.
You should never restrict access to sensitive data, sensitive areas, using the client router.  
This is the reason we also made restrictions on the server using the allow/deny functionality, so even if someone gets in they cannot make updates.
While this prevents writes from happening from unintended sources, reads can still be an issue.
The next step will take care of privacy, not showing users parties they are not allowed to see.

{{/template}}