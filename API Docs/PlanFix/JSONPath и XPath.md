# JSONPath и XPath

← [[PlanFix API — Index]]

Инструменты для парсинга ответов от внешних API в автоматических сценариях ПланФикс.

---

## JSONPath

Онлайн-тестирование: https://jsonpath.com/

Для отладки и устранения ошибок парсинга используйте специальный онлайн сервис: [JSONPath Online Evaluator](https://jsonpath.com/)

Приходящие в ПланФикс письма по E-mail или вебхуки можно разбирать, используя **JSON**. JavaScript Object Notation — это формат для хранения и обмена информацией, доступной для чтения человеком. Есть два основных элемента объекта JSON: ключи и значения.

Пример:

```
{"city":"New York", "country":"United States"}
```

- ключи — city и country;
- значения — New York и United States.

ПланФикс умеет разбирать письма в таком формате и добавлять нужные данные в инфоблоки. Для этого используется библиотека [JsonPath](https://github.com/json-path/JsonPath).

Обратите внимание, если при разборе вы используете функцию, необходимо указывать круглые скобки в конце, иначе функция работать не будет:

## Пример № 1

### Передаём в ПланФикс E-mail с текстом в формате JSON

- Для примера будем использовать такой JSON:

{

"firstName": "Иван",

"lastName": "Иванов",

"address": {

"streetAddress": "Московское ш., 101, кв.101",

"city": "Ленинград",

"postalCode": 101101

},

"phoneNumbers": \[\
\
"812 123-1234",\
\
"916 123-4567"\
\
\]

}

- Например, из JSON нам необходимы следующие данные:

{

"firstName": "Иван",

"lastName": "Иванов",

"address": {

"streetAddress": "Московское ш., 101, кв.101",

"city": "Ленинград",

"postalCode": 101101

},

"phoneNumbers": \[\
\
"812 123-1234",\
\
"916 123-4567"\
\
\]

}

- Настраиваем такое правило для разбора почты:

- Далее создаём нового клиента по шаблону, указываем какие значения полей из каких инфоблоков устанавливать и добавляем контакт в созданную из письма задачу:

- В итоге необходимые данные из письма в формате JSON переданы в ПланФикс:

## Пример № 2

### Добавляем параметры JSON в аналитику ПланФикса

- Для примера будем использовать JSON заказа товаров в интернет-магазине. Данные, которые нужны для добавления аналитики отмечены красным цветом:

{

"site": "mysite.shop",

"name": "Елизавета Бояринцева",

"id": "6598634",

"date": "26.04.2021 09:16:05",

"status": "В работе",

"email": "liz82@gmail.com",

"phone": "+7 921 625-30-24",

"address": "Павла Усова 23, корпус 1",

"price\_total": "13146.00",

"item\_order": \[\
\
{\
\
"sku": "598623",\
\
"name": "Скрепки, 28 мм, металлические, 100 шт., в картонной коробке",\
\
"barcode": "596325689126",\
\
"price": "14.60",\
\
"quantity": 10\
\
},\
\
{\
\
"sku": "589624",\
\
"name": "Бумага офисная CLASSIC, А4, 80 г/м2, 500 л., марка С",\
\
"barcode": "5963256858963",\
\
"price": "250.00",\
\
"quantity": 25\
\
},\
\
{\
\
"sku": "579824",\
\
"name": "Бумага офисная LUX, А4, 80 г/м2, 500 л., марка С",\
\
"barcode": "5963256889634",\
\
"price": "270.00",\
\
"quantity": 25\
\
}\
\
\]

}

- Настраиваем отдельное правило для задач по почте. Разбираем JSON:

- Создаем новую задачу и оповещаем нужного сотрудника о её появлении:

- Добавляем в созданную задачу аналитику. Обратите внимание, в этом пункте важно указать параметр **Добавить столько строк, сколько значений в инфоблоке**, чтобы в аналитику добавились все значения:

- В итоге в задаче видим оповещение и аналитику:

---

## XPath

Онлайн-тестирование: http://xpather.com/

Для отладки и устранения ошибок парсинга используйте специальный онлайн сервис: [XPath Online tester](http://xpather.com/)

Данные в формате XML можно отправить в ПланФикс и разобрать их, используя [XPath](https://www.w3schools.com/xml/xpath_intro.asp).

Используйте ChatGPT, чтобы составить XPath-запрос для парсинга нужных данных.

## Пример разбора XML

Сайт FloatRates предоставляет [XML](https://www.floatrates.com/daily/usd.xml) с актуальными курсами валют. Их можно получить в ПланФикс и затем распарсить значение нужной валюты. Для этого настройте кнопку:

- Отправьте запрос для получения данных XML:

- Разберите полученный ответ в формате XML:

- В итоге после нажатия на кнопку в задаче появится текущий курс валюты:

Аналогично можно разобрать любые XML-данные, отправленные в ПланФикс.

---

## XPath с ChatGPT

Используйте ChatGPT, чтобы составить XPath-запрос для парсинга нужных данных. Для примера рассмотрим вариант получения запроса для парсинга данных из поля ID 123.

## Пример промта

```
you are also an xpath expert
write xpath to get value on field with id 123, being given xml of the following type
<response>
<task>
<customData>
      <customValue>
        <field>
          <id>123</id>
          <name>qasdasdasd</name>
        </field>
        <value>111111</value>
        <text></text>
      </customValue>
      <customValue>
        <field>
          <id>456</id>
          <name>wdqweqw</name>
        </field>
        <value>222222</value>
        <text></text>
      </customValue>
    </customData>
  </task>
  <signature></signature>
</response>
```

## XPath-запрос в ответе ChatGPT

```
In XPath, you can use the following expression to get the value for the field with id 123:

xpath
//customValue[field/id='123']/value/text()

Here's the breakdown of this XPath expression:
//customValue: Searches for all customValue elements at any depth in the XML document.
[field/id='123']: Filters the customValue elements to only those that have a field child with an id child whose text value is '123'.
/value/text(): Fetches the text value of the value child element of the filtered customValue element.
You can use this XPath expression in any XPath-compatible XML parser to retrieve the value you're looking for.
```

---

## Связанные темы

- [[Вебхуки и HTTP]]
- [[PlanFix API — Index]]
