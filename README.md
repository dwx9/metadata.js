# metadata.js: 1С-подобный движок данных и метаданных

[English version](README.en.md)

## Предпосылки
Metadata.js - это лёгкий javascript клиент 1С (в дополнение к толстому, тонкому и веб-клиентам), предназначенный для чтения и редактирования данных, расположенных на [сервере 1С](http://v8.1c.ru/overview/Term_000000033.htm) с большим числом подключений (дилеры или интернет-витрина с сотнями анонимных либо авторизованных внешних пользователей).

Стандартный web-клиент 1С для реализации проекта [Заказ дилера](https://light.oknosoft.ru/) не проходил по ряду ограничений:
- **Медленно** на реальных (плохих) интернет-каналах
- **Скудная функциональность** в работе с данными традиционного клиента 1С (на стороне клиента 1С недоступны объекты данных и метаданных - только _ДанныеФормыСтруктура_ и _ДанныеФормыКоллекция_)
- **Интерфейсные ограничения** традиционного клиента 1С
- **Небезопасно** пускать гостей и дилеров в управленческую учетную базу. RLS и группы доступа никто не отменял, но система, в которой к критичным данным нельзя обратиться в принципе, будет надежнее системы, где доступ к этим данным ограничен паролем.

Из ограничений вытекали цели:
- Не нарушая лицензионного соглашения с [фирмой 1С](http://www.1c.ru/eng/title.htm), предоставить дешевые подключения из расчета 300-500 пользователей на один физический [сервер 1С](http://1c-dn.com/1c_enterprise/what_is_1c_enterprise/)
- Обеспечить приемлемое быстродействие за счет:
   + Отказа от избыточной для большинства проектов функциональности стандартного клиента 1С
   + Кеширования html5
   + Оптимизации вычислений на стороне браузера
   
## Концепция и философия
> В metadata.js предпринята попытка дополнить лучшее из современных технологий обработки данных инструментами, которых нам не хватало в повседневной работе

![Структура фреймворка metadata.js](examples/imgs/metadata_structure.png)
 
### Используем самое ценное от 1С
- Эффективная модель *Метаданных* со *ссылочной типизацией* и *подробным описанием реквизитов*
- Высокоуровневая объектная модель данных. Предопределенное (при необходимости, переопределяемое) поведение *Документов*, *Регистров*, *Справочников* и *Менеджеров объектов*, наличие *стандартных реквизитов* и *событий*, повышает эффективность разработки *в разы* по сравнению с фреймворками, оперирующими записями реляционных таблиц
- Автогенерация форм и элементов управления
 
Чтобы предоставить разработчику на javascript инструментарий, подобный 1С-ному, на верхнем уровне фреймворка реализованы классы:
- [AppEvents](http://www.oknosoft.ru/upzp/apidocs/classes/AppEvents.html), обслуживающий события при старте программы, авторизацию пользователей и состояния сети
- [Meta](http://www.oknosoft.ru/upzp/apidocs/classes/Meta.html) - хранилище метаданных конфигурации
- [DataManager](http://www.oknosoft.ru/upzp/apidocs/classes/DataManager.html) с наследниками `RefDataManager`, `EnumManager`, `InfoRegManager`, `CatManager`, `DocManager` - менеджеры объектов данных - аналоги 1С-ных `ПеречислениеМенеджер`, `РегистрСведенийМенеджер`, `СправочникМенеджер`, `ДокументМенеджер`
- [DataObj](http://www.oknosoft.ru/upzp/apidocs/classes/DataObj.html) с наследниками `CatObj`, `DocObj`, `EnumObj`, `DataProcessorObj` - аналоги 1С-ных `СправочникОбъект`, `ДокументОбъект`, `ОбработкаОбъект`

### Дополняем возможностями ES6 и Web UI
- При разработке фреймворка, было решено отказаться от поддержки устаревших браузеров и смело использовать `ServiceWorkers`, `Promises` и `Observers`. Это позволило сократить объём кода javascript, улучшить его структуру и повысить эффективность
- Для визуализации данных, в текущей реализации, использованы компоненты [dhtmlx](http://dhtmlx.com/). Любители ExtJS, Angular, Dojo, Webix, SAP UI5 и т.д. - могут при необходимости подключить нужные визуальные компоненты к нашим объектам данных
 
### Отличия от конкурентов
Metadata.js не конкурирует с клиентскими [Web UI](https://ru.wikipedia.org/wiki/%D0%A1%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5_%D0%BA%D0%B0%D1%80%D0%BA%D0%B0%D1%81%D0%BE%D0%B2_%D0%B2%D0%B5%D0%B1-%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B9) и клиент-серверными (в том числе, реактивными) фреймворками, а дополняет их новой абстракцией в виде [Объектов](http://www.oknosoft.ru/upzp/apidocs/classes/DataObj.html) и [Менеджеров](http://www.oknosoft.ru/upzp/apidocs/classes/DataManager.html) данных. Использование этих классов упрощает разработку сложных интерфейсов бизнес-приложений

## Установка и подключение библиотеки

```bash
npm install --save metadata-js  # node
bower install --save metadata   # bower
```

Для браузера, подключите таблицы стилей `fontawesome`, `dhtmlx`, `metadata` и скрипты `alasql`, `dhtmlx`, `metadata`  

```html
<link rel="stylesheet" type="text/css" href="//cdn.jsdelivr.net/fontawesome/latest/css/font-awesome.min.css">
<link rel="stylesheet" type="text/css" href="//cdn.jsdelivr.net/g/metadata(dhx_web.css+metadata.css)">
<script src="//cdn.jsdelivr.net/g/alasql,metadata(dhtmlx.min.js+metadata.min.js)"></script>
```

## Web-приложение к серверу 1С - это просто
- Подключите скрипт с файлами описания метаданных ([см. демо](examples/unf)) и получите полнофункциональное приложение с бизнес-логикой, реализованной средствами 1С в конфигураторе 1С и отзывчивым интерфейсом, который автоматически сгенерирует библиотека metadata.js
- С фреймворком metadata.js легко создавать системы на сотни и даже тысячи рабочих мест, используя высокоуровневые инструменты платформы 1С на сервере, сочетая их с гибкостью, эффективностью и доступностью браузерных технологий

### Для типовых конфигураций на полной поддержке используется rest-сервис odata
Файлы описания метаданных, в этом случае, формируются внешней обработкой, входящей в комплект поставки

### Если внесение изменений в типовую конфигурацию допустимо, используется http-сервис библиотеки интеграции
На клиенте и сервере в этом случае, доступны дополнительные функции оптимизации вычислений, трафика и кеширования

## Презентация
[![Обзор metadata.js](examples/imgs/metadata_slideshare.jpg)](http://www.slideshare.net/ssuser7ad218/metadatajs)

## Примеры
- Приложение [code examples](examples/codex) дополняет документацию простыми примерами использования и настройки компонентов фреймворка а так же, примерами подключения к типовым конфигурациям 1С  
- На примере [Заказа покупателя в УНФ](examples/unf), расмотрено:
   + Как сформировать файлы описания метаданных
   + Как выгрузить статические данные перечислений и справочников для кеширования на клиенте
   + Как по описанию метаданных сформировать SQL-файл для создания таблиц data-объектов в памяти браузера
   + Какие и в какой последовательности возникают события при старте приложения, при авторизации и построении начальной страницы
   + В каких файлах размещать и как подключать модификаторы метаданных и обработчики событий data-объектов
   + Пример обработки события "при изменении номенклатуры или характеристики в строке табличной части заказа" - получает из регистра срез последних цены и подставляет полученную цену в редактируемую строку документа
- Пример [Счет и отгрузка для Бухгалтерии предприятия](examples/accounting), иллюстрирует аналогичное предыдущему подключение к типовой конфигурации БП 3.0. 

## Тесты
Первые [автоматические тесты](spec) добавлены к проекту в Августе 2015. Покрытие кода тестами пока составляет менее 1%, но начало положено.<br />Разработку нового функционала и работу над ошибками планируется вести _через тестирование_ 

## Благодарности
- Andrey Gershun, author of [AlaSQL](https://github.com/agershun/alasql) - Javascript SQL database library
- Авторам [dhtmlx](http://dhtmlx.com/) - a beautiful set of Ajax-powered UI components
- Прочим авторам за их замечательные инструменты, упрощающие нашу работу

## Лицензия
Библиотека metadata.js имеет две схемы лицензирования:
- Для некоммерческих Open Source проектов доступна лицензия [AGPL-3.0](http://licenseit.ru/wiki/index.php/GNU_Affero_General_Public_License_version_3)
- Коммерческая [лицензия на разработчика](http://www.oknosoft.ru/programmi-oknosoft/metadata.html) позволяет использовать и распространять ПО в любом количестве неконкурирующих продуктов, без ограничений на количество копий

Данная лицензия распространяется на все содержимое репозитория, но не заменеют существующие лицензии для продуктов, используемых библиотекой metadata.js

(c) 2010-2015, компания Окнософт (info@oknosoft.ru)
