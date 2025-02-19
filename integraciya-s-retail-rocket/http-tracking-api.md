# API трекинг пользовательского поведения

Данные о пользовательском поведении необходимы для работы многих продуктов Retail Rocket: товарных рекомендаций, автоматизированных коммуникационных кампаний, для отслеживания метрик эффективности коммуникаций и т.д.

## **Общие концепции**

Для передачи пользовательского поведения в систему Retail Rocket необходимо вызывать соответствующие действию методы трекинг API для всех значимых действий пользователя (просмотр товара, добавление в корзину и т.д.). Ниже описаны методы для каждого такого действия с их параметрами, возможными кодами ответов и примерами вызова.

## **Base URL**

`https://apptracking.retailrocket.ru/1.0/`

## Resources

Трекинг API придерживается [общих принципов интеграционных API](obshie-principy-integracii-s-retail-rocket.md).

### Просмотр карточки товара

Должен быть вызван при каждом просмотре посетителем карточки товара.

**Path**

**`view`**

**HTTP-метод**

`POST`

**Параметры строки запроса**

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

**HTTP-заголовки**

`Content-type: application/json`

**Тело запроса**

В теле запроса передается объект типа **`ViewEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                      |
| ------------------- | ------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)                         |
| `productId`         | Да           | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                               |
| `stockId`           | Нет          | string  | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp`         | Да           | string  | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)                             |

**Пример вызова**

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/view?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Просмотр карточки группового товара

Должен быть вызван при каждом просмотре посетителем карточки товара, на которой представлен групповой товар.

#### Path

**`groupView`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`GroupViewEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип          | Описание                                                                                                                                                                      |
| ------------------- | ------------ | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)                         |
| `groupId`           | Да           | integer      | [Идентификатор товарной группы](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                      |
| `productIds`        | Да           | number array | [Список идентификаторов товаров](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                     |
| `stockId`           | Нет          | string       | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp`         | Да           | string       | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)                             |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/groupView?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"groupId\": 654321,
         \"productIds\": [123456, 234567, 345678],
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Добавление товара в корзину

Должен быть вызван при каждом добавлении посетителем товара в корзину.

**Path**

**`addToBasket`**

**HTTP-метод**

`POST`

**Параметры строки запроса**

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

**HTTP-заголовки**

`Content-type: application/json`

**Тело запроса**

В теле запроса передается объект типа **`AddToBasketEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                      |
| ------------------- | ------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)                         |
| `productId`         | Да           | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                               |
| `stockId`           | Нет          | string  | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp`         | Да           | string  | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)                             |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/addToBasket?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Просмотр страницы товарной категории

Должен быть вызван при просмотре посетителем страницы товарной категории.&#x20;

{% hint style="warning" %}
В зависимости от того как выполнена интеграция передачи [товарной базы](../#tovarnaya-baza), необходимо вызывать разные методы для передачи факта просмотра пользователем страницы товарной категории.

[Интеграции через YML-файл](http-tracking-api.md#pri-integracii-cherez-yml-fail)

[Интеграции через Product API](http-tracking-api.md#pri-integracii-cherez-product-api)
{% endhint %}

#### При интеграции через YML-файл

В YML файле доступно дерево категорий с целочисленными идентификаторами категории, поэтому и товары имеют целочисленные идентификаторы категорий, что позволяет использовать такие идентификаторы в трекинге пользовательского поведения.

**Path**

**`categoryView`**

**HTTP-метод**

`POST`

**Параметры строки запроса**

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

**HTTP-заголовки**

`Content-type: application/json`

**Тело запроса**

В теле запроса передается объект типа **`CategoryViewEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                              |
| ------------------- | ------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `categoryId`        | Да           | integer | [Идентификатор категории товара](obshie-principy-integracii-s-retail-rocket.md#sposoby-peredachi-tovarnoi-bazy)                                       |
| `timestamp`         | Да           | string  | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |

Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/categoryView?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"categoryId\": 123456,
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

#### При интеграции через Product API

При использовании product API категория передается как `categoryPath` также она должна быть передана и при просмотре страницы категории.

#### Path

**`categoryViewByCategoryPath`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`CategoryViewEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип    | Описание                                                                                                                                              |
| ------------------- | ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `categoryPath`      | Да           | string | [Путь категории товара](obshie-principy-integracii-s-retail-rocket.md#sposoby-peredachi-tovarnoi-bazy)                                                |
| `timestamp`         | Да           | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/categoryViewByCategoryPath?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"categoryPath\": \"Товары для дома/Кухня/Вилки\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Заказ товара

​Должен быть вызван для каждой товарной позиции в заказе.

#### Path

**`order`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`OrderEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                      |
| ------------------- | ------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)                         |
| `productId`         | Да           | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                               |
| `stockId`           | Нет          | string  | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `price`             | Да           | number  | Цена с учетом скидок за **единицу товара**                                                                                                                                    |
| `quantity`          | Да           | number  | Кол-во единиц товара в заказе                                                                                                                                                 |
| `transaction`       | Да           | string  | Идентификатор покупки                                                                                                                                                         |
| `timestamp`         | Да           | string  | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)                             |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/order?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"Moscow\",
         \"quantity\": 2,
         \"price\": 234,
         \"transaction\": \"135243\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Поисковый запрос

Должен быть вызван при вводе посетителем поисковой фразы.

#### Path

**`search`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`SearchEvent`** со следующими полями:

| Имя поля            | Обязательное | Тип    | Описание                                                                                                                                              |
| ------------------- | ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `searchPhrase`      | Да           | string | Поисковая фраза, которую ввел пользователь                                                                                                            |
| `stockId`           | Нет          | string | [Идентификатор склада, к которому принадлежит пользователь](obshie-principy-integracii-s-retail-rocket.md#podderzhka-regionalnosti-sklad-region)      |
| `timestamp`         | Да           | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/search?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"searchPhrase\": \"подгузник для новорожденных\",
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### **Аутентификация** пользователя

Для того, чтобы пользовательское поведение было правильно учтено для [контакта](../#kontakt), в момент, когда становится известен идентификатор посетителя(логин на сайте), необходимо передать устойчивый идентификатор посетителя в систему Retail Rocket.

{% hint style="info" %}
**`contactExternalId` -**  уникальный идентификатор контакта, по которому пользователь может быть распознан в системе клиента. Служит для обмена информацией о контактах между платформой Retail Rocket и источниками данных клиента.
{% endhint %}

{% hint style="warning" %}
**`phone -`** Номер телефона пользователя следует передавать в формате E.164
{% endhint %}

#### Требования к contactExternalId

* Должен состоять только из цифр и букв;
* Не превышать 50 символов;

#### Path

**`setContact`**

#### HTTP-метод

`POST`

#### HTTP-заголовки

`Content-type: application/json`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### Тело запроса

| Имя поля            | Обязательное | Тип         | Описание                                                                                                                                              |
| ------------------- | ------------ | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string      | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `contactExternalId` | Да           | string      | [Уникальный идентификатор контакта](http-tracking-api.md#ustanovka-sessii)                                                                            |
| `timestamp`         | Да           | string      | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |
| `email`             | Нет          | string      | Адрес почты пользователя                                                                                                                              |
| `stockId`           | Нет          | string      | [Идентификатор склада, к которому принадлежит пользователь](obshie-principy-integracii-s-retail-rocket.md#podderzhka-regionalnosti-sklad-region)      |
| `phone`             | Нет          | string      | [Номер телефона пользователя](http-tracking-api.md#autentifikaciya-polzovatelya)                                                                      |
| `customData`        | Нет          | JSON Object | Пользовательские параметры                                                                                                                            |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/setContact?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"contactExternalId\": \"2342dfgf253454c645346wer3\",
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\",
         \"email\": \"example@email.com\"
      }
   "
```

### Запуск сценария 'Welcome Sequence'

Запускает цепочку email-сценария WelcomeSequence.

{% hint style="info" %}
**Welcome-цепочка** — это приветственное письмо или серия писем, в которых интернет-магазин рассказывает о своем бренде, или об особенностях своей работы.
{% endhint %}

{% hint style="warning" %}
На момент вызова метода, email-адрес должен быть в базе подписчиков партнера.
{% endhint %}

#### Path

**`welcomeSequence`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                              |
| ------------------- | ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `timestamp`         | Да           | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |
| `email`             | Да           | string | Адрес почты пользователя                                                                                                                              |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/welcomeSequence?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"email\": \"example@gmail.com\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Клик по содержимому письма

Событие клика по содержимому письма. Должно вызываться, если пользователь переходит из письма.

{% hint style="info" %}
Все ссылки в письме содержат GET параметр **`MailTrackingId`** с уникальным значением для каждого письма. При каждом переходе из письма на сайт, URL страницы будет содержать параметр **`MailTrackingId`**, значение которого следует передавать в тело запроса.
{% endhint %}

#### Path

**`emailClick`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

| Имя параметра       | Обязательное | Тип         | Описание                                                                                                                                              |
| ------------------- | ------------ | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string      | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `timestamp`         | Да           | string      | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |
| `mailTrackingId`    | Да           | string      | [Метка пользователя из письма](http-tracking-api.md#zapusk-scenariya-welcome-sequence-1)                                                              |
| `customData`        | Нет          | JSON объект | Пользовательские параметры                                                                                                                            |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/emailClick?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"mailTrackingId\": \"5b488f277ecc191c98859541\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

## Передача данных о взаимодействии с рекомендациями

Для оценки эффективности рекомендаций, а также для учета продаж через товарные рекомендации, необходимо передавать соответствующие события взаимодействия с рекомендациями.

{% hint style="info" %}
Идентификатор блока рекомендаций `recomBlockId` **-** любая строка до 50 символов, используется для идентификации каждого блока рекомендаций
{% endhint %}

### Просмотр блока рекомендаций

Должен быть вызван при каждом показе блока рекомендаций пользователю приложения

**Path**

**`recomBlockViewed`**

**HTTP-метод**

`POST`

**Параметры строки запроса**

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

**HTTP-заголовки**

`Content-type: application/json`

**Тело запроса**

| Имя поля            | Обязательное | Тип    | Описание                                                                                                                                              |
| ------------------- | ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `recomBlockId`      | Да           | string | [Идентификатор блока рекомендаций](http-tracking-api.md#peredacha-dannykh-o-vzaimodeistvii-s-rekomendaciyami)                                         |
| `timestamp`         | Да           | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/recomBlockViewed?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"recomBlockId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\", 
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Клик по блоку рекомендаций

Нажатие по рекомендации – это, фактически, переход в карточку товара, рекомендованную в блоке. Элементы, такие как название товара, изображения товара, цена и другие, относящиеся к рекомендуемому товару из рекомендаций, должны содержать обработчик tap и вызывать событие recomTap.

#### Path

**`recomTap`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                      |
| ------------------- | ------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)                         |
| `productId`         | Да           | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                               |
| `stockId`           | Нет          | string  | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp`         | Да           | string  | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)                             |
| `recomBlockId`      | Да           | string  | [Идентификатор блока рекомендаций](http-tracking-api.md#peredacha-dannykh-o-vzaimodeistvii-s-rekomendaciyami)                                                                 |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/recomTap?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\",
         \"recomBlockId\": \"5994d90bc7d0114fa0a26122\"
      }
   "
```

### Добавление в корзину из блока рекомендаций

Если в блоке товарных рекомендаций используется кнопка добавления товара в корзину, то при каждом добавлении товара в корзину из блока рекомендаций (без перехода в карточку товара) необходимо вызывать обработчик добавления товара из блока в корзину.

#### **Path**

**`recomAddToBasket`**

#### HTTP-метод

`POST`

#### HTTP-заголовки

`Content-type: application/json`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### Тело запроса

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                      |
| ------------------- | ------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)                         |
| `productId`         | Да           | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                               |
| `stockId`           | Нет          | string  | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp`         | Да           | string  | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)                             |
| `recomBlockId`      | Да           | string  | [Идентификатор блока рекомендаций](http-tracking-api.md#peredacha-dannykh-o-vzaimodeistvii-s-rekomendaciyami)                                                                 |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/recomAddToBasket?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\",
         \"recomBlockId\": \"5994d90bc7d0114fa0a26122\"
      }
   "
```

## Передача данных о взаимодействии со спонсорским контентом

### Просмотр спонсорского контента

Должен быть вызван при каждом показе посетителю спонсорского контента.

#### Path

**`impressionContentViewed`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`ImpressionContentViewedEvent`** со следующими полями:

| Имя поля              | Обязательное | Тип    | Описание                                                                                                                                              |
| --------------------- | ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId`   | Да           | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `impressionContentId` | Да           | string | [Идентификатор спонсорского контента](instrukciya-po-integracii-retail-rocket-sponsorskoe-razmeshenie.md#integraciya-sistem)                          |
| `timestamp`           | Да           | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/impressionContentViewed?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Клик по спонсорскому контенту

Должен быть вызван при каждом клике посетителем в спонсорский контент.

#### Path

**impressionContentClicked**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`ImpressionContentViewedEvent`** со следующими полями:

| Имя поля              | Обязательное | Тип    | Описание                                                                                                                                              |
| --------------------- | ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId`   | Да           | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `impressionContentId` | Да           | string | [Идентификатор спонсорского контента](instrukciya-po-integracii-retail-rocket-sponsorskoe-razmeshenie.md#integraciya-sistem)                          |
| `timestamp`           | Да           | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova)     |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/impressionContentClicked?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

## Пакетная загрузка пользовательского поведения

API предоставляет возможность пакетной загрузки пользовательского поведения. В теле вызова метода передается список пользовательских событий. Вызов гарантирует сохранение порядка переданных в него событий.

### Специфика использования пакетной загрузки

Метод API `visitorEvents` решает проблему сохранения порядка пользовательских событий. К примеру, при потере соединения с сетью интернет, когда нельзя передать события в режиме реального времени, но действия совершаются в приложении. Как только у приложения появится доступ к сети интернет, нам важно получить эти события, но если отправить их с помощью обычных вызовов `view`, `addToBasket` - есть вероятность потерять порядок, из-за того, что каждый из вызовов `view`, `addToBasket` обработается разным сервером. Для сохранения порядка действий есть метод `visitorEvents`, принимающий набор событий и гарантирующий, что события будут сохранены в том порядке в котором они переданы.

### Особенности использования

{% hint style="danger" %}
Для отправки двух пакетов событий, содержащих разное поведение одного  пользователя, нужно отправить первый, дождаться успешного статуса ответа и после этого отправить второй;
{% endhint %}

{% hint style="danger" %}
Нельзя параллельно совершать более одного вызова, в которых есть события по одному пользователю - это может нарушить порядок;
{% endhint %}

{% hint style="warning" %}
Из-за необходимости вернуть подтверждение сохранения событий и порядка время ответа **`visitorEvents`**, как правило, на порядки больше времени ответа обычных (не пакетных) вызовов трекинга и может составлять порядка 1.5 секунд и более. В связи с этим не следует использовать данный вызов для передачи realtime-событий;
{% endhint %}

{% hint style="info" %}
В вызове можно передать события для разных пользователей (**sessionExternalId**);
{% endhint %}

{% hint style="info" %}
Можно параллельно вызывать **`visitorEvents`** с пакетами для разных пользователей, т.к. порядок важен только в рамках одного пользователя;
{% endhint %}

#### Path

**`visitorEvents`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип    | Описание                                                                                                         |
| ------------- | ------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `apiKey`      | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                   |
| `partnerId`   | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается список пользовательских событий любого из следующих типов:

**`ViewEventEnvelope`**

| Имя поля | Обязательное | Тип         | Описание                                                                                |
| -------- | ------------ | ----------- | --------------------------------------------------------------------------------------- |
| `view`   | Да           | `ViewEvent` | Тип подробно описан в разделе «Просмотр карточки товара» как тип параметра тела запроса |

**`GroupEventViewEnvelope`**

| Имя поля    | Обязательное | Тип              | Описание                                                                                           |
| ----------- | ------------ | ---------------- | -------------------------------------------------------------------------------------------------- |
| `groupView` | Да           | `GroupViewEvent` | Тип подробно описан в разделе «Просмотр карточки группового товара» как тип параметра тела запроса |

**`AddToBasketEventEnvelope`**

| Имя поля      | Обязательное | Тип                | Описание                                                                                   |
| ------------- | ------------ | ------------------ | ------------------------------------------------------------------------------------------ |
| `addToBasket` | Да           | `addToBasketEvent` | Тип подробно описан в разделе «Добавление товара в корзину» как тип параметра тела запроса |

**`OrderEventEnvelope`**

| Имя поля | Обязательное | Тип          | Описание                                                                    |
| -------- | ------------ | ------------ | --------------------------------------------------------------------------- |
| `order`  | Да           | `OrderEvent` | Тип подробно описан в разделе «Заказ товара» как тип параметра тела запроса |

**`CategoryViewEventEnvelope`**

| Имя поля       | Обязательное | Тип                 | Описание                                                                                          |
| -------------- | ------------ | ------------------- | ------------------------------------------------------------------------------------------------- |
| `categoryView` | Да           | `CategoryViewEvent` | Тип подробно описан в разделе «Просмотр страницы категории товара» как тип параметра тела запроса |

**`SearchViewEventEnvelope`**

| Имя поля | Обязательное | Тип               | Описание                                                                                          |
| -------- | ------------ | ----------------- | ------------------------------------------------------------------------------------------------- |
| `search` | Да           | `SearchViewEvent` | Тип подробно описан в разделе «Просмотр страницы категории товара» как тип параметра тела запроса |

**`ImpressionContentViewedEventEnvelope`**

| Имя поля                  | Обязательное | Тип                            | Описание                                                                                      |
| ------------------------- | ------------ | ------------------------------ | --------------------------------------------------------------------------------------------- |
| `impressionContentViewed` | Да           | `ImpressionContentViewedEvent` | Тип подробно описан в разделе «Просмотр спонсорского контента» как тип параметра тела запроса |

**`ImpressionContentClickedEventEnvelope`**

| Имя поля                   | Обязательное | Тип                             | Описание                                                                                     |
| -------------------------- | ------------ | ------------------------------- | -------------------------------------------------------------------------------------------- |
| `impressionContentClicked` | Да           | `ImpressionContentClickedEvent` | Тип подробно описан в разделе «Клик по спонсорскому контенту» как тип параметра тела запроса |

**`EmailClickEventEnvelope`**

| Имя поля     | Обязательное | Тип               | Описание                                                                                  |
| ------------ | ------------ | ----------------- | ----------------------------------------------------------------------------------------- |
| `emailClick` | Да           | `EmailClickEvent` | Тип подробно описан в разделе «Клик по содержимому письма» как тип параметра тела запроса |

**`WelcomeSequenceEventEnvelope`**

| Имя поля          | Обязательное | Тип                    | Описание                                                                                          |
| ----------------- | ------------ | ---------------------- | ------------------------------------------------------------------------------------------------- |
| `welcomeSequence` | Да           | `WelcomeSequenceEvent` | Тип подробно описан в разделе «Запуск сценария 'Welcome Sequence'» как тип параметра тела запроса |

**`SetContactEventEnvelope`**

| Имя поля     | Обязательное | Тип               | Описание                                                                                   |
| ------------ | ------------ | ----------------- | ------------------------------------------------------------------------------------------ |
| `setContact` | Да           | `SetContactEvent` | Тип подробно описан в разделе «Аутентификация пользователя» как тип параметра тела запроса |

**Пример вызова**

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST "https://apptracking.retailrocket.ru/1.0/visitorEvents?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
    [
        {
            \"view\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 123456,
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"groupView\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"groupId\": 654321,
                \"productIds\": [123456, 234567, 345678],
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"addToBasket\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 234567,
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"order\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 234567,
                \"stockId\": \"NewYork\",
                \"quantity\": 3,
                \"price\": 1321.43,
                \"transaction\": \"135243\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"categoryView\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"categoryId\": 123456,                 
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"search\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"searchPhrase\": \"подгузник для новорожденных\",            
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"recomBlockViewed\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"recomBlockId\": \"34453dffg34534te565\",            
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"recomTap\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"recomBlockId\": \"34453dffg34534te55\",  
                \"productId\" : 123,          
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"recomAddToBasket\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"recomBlockId\": \"34453dffg34534te54\",      
                \"productId\" : 123,        
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"impressionContentViewed\": {
              \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
              \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
              \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"impressionContentClicked\": {
              \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
              \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
              \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"emailClick\": {
             \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
             \"mailTrackingId\": \"5b488f277ecc191c98859541\",                          
             \"timestamp\": \"2018-09-15T15:53:00+00:00\"            
           }  
        },
        {
            \"setContact\": {
             \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
             \"contactExternalId\": \"2342dfgf253454c645346wer3\", 
             \"email\": \"example@gmail.com\",
             \"stockId\": \"Moscow\",                         
             \"timestamp\": \"2018-09-15T15:53:00+00:00\"            
           }  
        },
        {
            \"welcomeSequence\": {
               \"sessionExternalId\": \"60842392e4881c65e6c5e423\",       
               \"email\": \"example@gmail.com\",                           
               \"timestamp\": \"2018-09-15T15:53:00+00:00\"                   
            }
        }
    ]
"
```
{% endtab %}
{% endtabs %}

## Коды ответов

| Код ответа | Описание                                                               |
| ---------- | ---------------------------------------------------------------------- |
| 200        | Запрос принят                                                          |
| 400        | Ошибка в запросе, необходимо проверить правильность построения запроса |
| 401        | Ошибка аутентификации, проверьте корректность пары `apiKey`            |
| 403        | Доступ запрещен                                                        |
| 404        | Запрошенный ресурс не найден, необходимо проверить правильность URL    |

## Время ответа

Ожидаемое время ответа – 100 мс для всех ресурсов (может отличаться из-за особенностей сетевой инфраструктуры).

## Ограничения

По умолчанию действует ограничение в 100 запросов/секунду для площадки. При необходимости возможно изменить (с помощью аккаунт-менеджера).
