{{#template name="tutorials.socially.angular1.step_11.md"}}
{{> downloadPreviousStep stepName="step_11"}}

Таким образом мы создали наше приложение и тестировали его только в веб-браузере, но Meteor был разработан кроссплатформенным - ваш социальный вебсайт может стать iOS или Android приложением с помощью всего лишь пары команд.

## Angular 1 инициализация

Перед тем, как мы установим PhoneGap (что очень просто в Meteor) нам нужно сделать кое-какие изменения в нашей Angular 1 инициализации приложения:

В файле `app.js` мы вручную отбутстрапим наше Angular 1 приложение к правильной платформе:

{{> DiffBox tutorialName="meteor-angular1-socially" step="11.1"}}

И далее мы уберём

    ng-app="socially"

из `index.html`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="11.2"}}

## PhoneGap

Meteor упрощает установку всех необходимых инструментов для  создания мобильных приложений, но загрузка всех програм займёт много времени - Android загрузка будет около 300MB и для iOS вам нужно установить Xcode, который занимает 2GB.
Если вы не хотите ждать загрузки этих инструментов - можете пропустить и перейти к [следующему шагу](/tutorial/step_12).

### Запуск на эмуляторе Android

Следуйте этим [инструкциям](https://github.com/meteor/meteor/wiki/Mobile-Development-Install:-Android-on-Mac) для установки всех необходимых инструментов для построения Android приложения из вашего проекта.

Когда вы закончили всю установку, наберите:

    meteor add-platform android

Когда вы согласитесь с правилами использования лицензии, запустите:

    meteor run android

После всяких инициализаций, вы увидите как появится окно эмулятора Android, запустите ваше приложение всередине нативной Android оболочки.
Эмулятор может немного тормозить, поэтому если вы хотите увидеть как всё работает в реальности, то вам нужно использовать реальное устройство.

### Запуск приложения Android

Во-первых, завершите все ваши шаги приведеные выше для установки Android инструментов на вашей системе.
Далее убедитесь, что у вас разрешена USB Debugging на вашем телефоне и телефон подключён к компьютеру USB кабелем.
Также вы должы выйти из Android эмулятора перед запуском на устройстве.

Дальше запустите команду:

    meteor run android-device

Приложение будет построено и установлено на ваше устройство. Если вы хотите перенаправить его на сервер, который вы запустили в предыдущем этапе, запустите:

    meteor run android-device --mobile-server my_app_name.meteor.com

### Запуск симулятора iOS (только для Mac)

Если у вас есть Mac, то вы можете запустить iOS симулятор.

Следуйте этой [инструкции](https://github.com/meteor/meteor/wiki/Mobile-Development-Install:-iOS-on-Mac) для построения iOS приложения из вашего проекта.

Как закончите наберите:

    meteor add-platform ios
    meteor run ios

Вы увидите как запустится iOS симулятор с вашим приложением внутри.

### Запуск iPhone или iPad (Только Mac; требует аккаунт Apple developer)

Если у вас есть Apple developer аккаунт, то вы можете также запустить ваше приложение на iOS устройстве. Запустите следующую команду:

    meteor run ios-device

Это откроет Xcode с проектом для вашего iOS приложения. Вы можете использовать Xcode для запуска приложения на любом устройстве или симуляторе, который поддерживает Xcode.

Если вы хотите направить ваше приложение на уже размещённый сервер, то запустите:

    meteor run ios-device --mobile-server my_app_name.meteor.com

Теперь видите как легко размещать наше приложение и запускать его на мобильных устройствах. Давайте добавим пару функций!

### Отправка вашего Android приложения в Play Store:

[https://github.com/meteor/meteor/wiki/How-to-submit-your-Android-app-to-Play-Store](https://github.com/meteor/meteor/wiki/How-to-submit-your-Android-app-to-Play-Store)

### Отправка вашего iOS приложения в App Store:

[https://github.com/meteor/meteor/wiki/How-to-submit-your-iOS-app-to-App-Store](https://github.com/meteor/meteor/wiki/How-to-submit-your-iOS-app-to-App-Store)

# Итоги

Теперь вы знаете как легко работать с Meteor и PhoneGap.

Это только начало, так как существует ещё горячий пуш кода уже после того как вы разместили ваше приложение в магазины, после того, как вы обновили ваш код, все приложения немедленно обновляются, нет необходимости снова проходить процесс обновления в магазинах!

Более детальная информация:

* [https://github.com/meteor/meteor/wiki/Meteor-Cordova-Phonegap-integration](https://github.com/meteor/meteor/wiki/Meteor-Cordova-Phonegap-integration)

# Решение проблем

Если ваше приложение не работает по какой-то причине, попытайтесь запустить meteor с флагом verbose с целью получить больше информации про ваш запуск. С этой целью запустите следующую команду:

    meteor run ios --verbose

Заметка: Вы тоже саоме можете сделать с платформой "android".

Дальше идут решения общих проблем, которые у нас возникли:

### Эмулятор стартует, но приложение не запускается
Эта проблема возникает из-за проблем в доступах, которое мешает Cordova запустить APK/APP на симуляторе.
Обычно увидите это в логе ошибок:

    "Timed out waiting for device to boot"
    "You may not have the required environment or OS to run this project"
In order to fix that issue you will need to fix the permission by running these commands:

    sudo chown -R YOUR_USERNAME /usr/local/lib/node_modules/
    sudo chmod -R 777 /usr/local/lib/node_modules/
    sudo chown -r YOUR_USERNAME ~/.meteor/
    sudo chmod -R 777 ~/.meteor/
    sudo chown -r YOUR_USERNAME YOUR_PROJECT_FOLDER
    sudo chmod -R 777 YOUR_PROJECT_FOLDER

    Replace YOUR_PROJECT_FOLDER with your project folder and YOUR_USERNAME with the user you use to run the "meteor run" command.
    
### ERROR whitelist rejection при запуске на iOS

* Create `mobile-config.js` and add `App.accessRule('*');`
* Re-run meteor

{{/template}}
