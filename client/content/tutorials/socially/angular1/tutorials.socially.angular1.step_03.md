{{#template name="tutorials.socially.angular1.step_03.md"}}
{{> downloadPreviousStep stepName="step_02"}}

Теперь у нас есть отличное клиентское приложение, которое создаёт и рендерит собственные данные.

Если бы у нас был не Meteor, а другой фреймворк, то мы бы начали с создания REST конечных точек для соединения сервера с клиентом.
Также, нам нужно было бы создать базу данных и функции для соединения с ними.

И мы не говорили пока что о реальном времени, когда нам нужно было бы добавить сокеты и локальную базу данных для кеша и управлять компенсацией задержки (или просто проигнорировать такие функции и создать не настолько хорошее и современное приложение...)

К счастью, мы используем Meteor!


Meteor делает написание распределённого клиентского кода простым как разговор с локальной базой данных.

Каждый Meteor клиент включает кеш памяти базы данных. Для управления клиентским кешем, сервер публикует множество JSON документов и клиент подписывается на эти множества. При смене документов в множестве, сервер патчит кеш клиента автоматически. 

Это представляется для нас как новая концепция - **Полностековая реактивность**.

В ангуляроподобном языке мы бы это назвали **связывание 3 способами**.

В Метеоре управление данными осуществляется через `Mongo.Collection` класс. Он используется для определения MongoDB коллекций и для их манипуляции. 

Благодаря minimongo, Meteor клиенский эмулятор Mongo, `Mongo.Collection` может быть использован в клиентском и серверном коде.

# Определение коллекции

Во-первых, определим коллекцию вечеринок, которая будет хранить все наши вечеринки

Добавьте:

{{> DiffBox tutorialName="meteor-angular1-socially" step="3.1"}}

в начало нашего `app.js` файла.

Обратите внимание, что этот код запускается вне `isClient` выражения.

Это значит, что эта коллекция и действия над ней запустятся на обеих клиенте (minimongo) и сервере (Mongo), вам нужно будет написать их единожды и Meteor позаботится о синхронизации обоих.

# Связывание с Angular 1

Мы создали коллекцию и нашему клиенту нужно подписаться на её изменения и связать её с нашим массивом вечеринок.

Для связывания их мы будем использовать встроенный angular-meteor сервис под названием [$meteor.collection](/api/meteorCollection).

Мы собираемся заменить описание `$scope.parties` на следующую команду всередине контроллера `PartiesListCtrl`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="3.2"}}

Эта строка описывает новую переменную `$scope.parties` (поэтому нам не нужно делать что-то наподобие `$scope.parties = [];` ) и далее связывать это с коллекцией Parties Mongo.

Также нам нужно добавить сервис `$meteor` к контроллеру с инъекцией зависимости.

Наш `app.js` файл должен выглядеть так:

{{> DiffBox tutorialName="meteor-angular1-socially" step="3.3"}}

Теперь при каждом изменении в `$scope.parties` переменная будет автоматически сохранена в локальную minimongo и синхронизированна с сервером MongoDB и другими клиентами в реальном времени!

Но у нас по-прежнему нет данных в этой коллекции, поэтому добавим некоторые данные о вечеринках, что были ранее.

Создаддим файл `server.js` и добавим следующее содержимое:

{{> DiffBox tutorialName="meteor-angular1-socially" step="3.4" filename="server.js"}}

> Обратите внимание, что мы также убрали строку `Mongo.Collection` из `app.ng.js`, так как мы хотим, чтобы эта строка работала в сервере и клиенте  и `.ng.js` файлы загружаются только в клиенте.

Как вы могли бы уже понять, этот код запускается только в сервере и тогда Meteor начинает инициализацию базы данных с этими примерами вечеринок.

Запустите приложение и вы увидите список вечеринок на экране.

В следующей главе мы увидим как просто манипулировать данными, сохранять и публиковать изменения на сервер и как делать это так, чтобы все соединённые клиенты автоматически обновлялись.

# Вставка вечеринок из консоли

Элементы всередине коллекций называются документами. Давайте используем консоль сервера базы данных для вставки документов в нашу коллекцию.
В новой вкладке терминала, перейдите в папку приложения и наберите:

    meteor mongo

Это откроет консоль в локальной базе данных вашего приложения. В промпте введите:

    db.parties.insert({ name: "A new party", description: "From the mongo console!" });

В вашем браузере, вы увидите UI вашего приложения немедленно обновится и отобразит новую вечеринку.
Как вы видите нам не пришлось писать какой-то код для соединения сервера с фронт-эндом - это случилось автоматически. 

Вставим ещё несколько вечеринок в нашей консоли базы данных с другим текстом.

Давайте проделаем то же самое с удалением. В промте, напишите следующую команду для просмотра всех вечеринок и их свойств:

    db.parties.find({});

Теперь выберите одну вечеринку для удаления и скопируйте её свойство `id`.
Удалите её используя этот id (замените `N4KzMEvtm4dYvk2TF` на ваше id значение):

    db.parties.remove( {"_id": "N4KzMEvtm4dYvk2TF"});

Снова, вы увидите как UI приложения мгновенно обновился после удаления вечеринки.

Попробуйте проделать ещё пару действий, например, обновите объект из консоли и так далее. Почитайте документацию mongodb по ссылке <a href="http://docs.mongodb.org/manual/tutorial/getting-started-with-the-mongo-shell/">the mongodb shell</a>.


# Итоги

В этой главе вы увидели как легко и быстро можно создавать полное соединение между нашими клиентскими данными и сервером и всеми подсоединёнными клиентами.

На следующем шаге, вы увидите как добавлять функциональность к нашему UI приложения, чтобы мы могли добавлять вечеринки без использования консоли базы данных.

{{/template}}
