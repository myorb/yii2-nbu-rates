# DONT WORK !!! JUST TEST !!! 
# TODO REWRITE NBU.php

# yii2-nbu-rates

Экстеншен yii2 для получения курса валют.
Источник данных [Национального Банка Украины](http://www.bank.gov.ua/)

Установка
=========

Предпочтительный способ установки [composer](http://getcomposer.org/download/).

Запустите комманду

```
php composer.phar require --prefer-dist myorb/yii2-nbu-rates "dev-master"
```

или добавьте

```json
"myorb/yii2-nbu-rates": "dev-master"
```

в раздел `require` в вашем composer.json файле.

В конфигурационном файле добавляем:
```php
$config = [
    ...
    'components' => [
        ...
        'NBU' => [
            'class' => 'myorb\NBURates\NBU',
            'defaultCurrency' => "EUR"
        ],
        ...
    ]
    ...
]
```


Примеры работы
========

```php
// ------------------------------------------------------------------------------------------------------------------------
// Вызов метода all() без вызова других методов возврошает все курсы на текущее время
print_r(Yii::$app->NBU->all());
/*
Результат работы

Array
(
    ...
    [USD] => Array
        (
            [name] => Доллар США
            [value] => 59.7665
            [char_code] => USD
            [num_code] => 840
            [nominal] => 1
            [id] => R01235
        )
    ...
)
*/
// ------------------------------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------------------------------
// Вызов метода one() без вызова других методов возврошает курс по умолчаню, указанный в конфиге
print_r(Yii::$app->NBU->one());

/*
//Результат работы
Array
(
    [name] => Евро
    [value] => 65.9882
    [char_code] => EUR
    [num_code] => 978
    [nominal] => 1
    [id] => R01239
)
*/
// ------------------------------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------------------------------
// Возвращает все курсы валют для указанной даты
print_r(Yii::$app->NBU->filter(['date' => time()-(86400*7)]))->all(); 
// ------------------------------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------------------------------
// Возвращает курс валюты по умолчанию для указанной даты
print_r(Yii::$app->NBU->filter(['date' => time()-(86400*7)]))->one(); 
// ------------------------------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------------------------------
// Возврощает все курсы валют указанный в параметре currency
print_r(Yii::$app->NBU->filter(['currency' => 'usd, aud, eur']))->all(); 
// ------------------------------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------------------------------

//Вызов метода short()
print_r(Yii::$app->NBU->short()->all());
/*
Резлуьтат запроса
Array
(
    ...
    [HUF] => 0.21349
    [DKK] => 8.8581
    [USD] => 59.7665
    [EUR] => 65.9882
    [INR] => 0.93502
    [KZT] => 0.318891
    [CAD] => 46.1304
    ...
)
*/
// ------------------------------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------------------------------
// Пример получения динамики котировок 
print_r(
    Yii::$app
        ->NBU
        ->filter(['currency' => 'usd, aud, eur']))
        ->withDynamic(['date_from' => '20.05.2015', 'date_to' => '25.05.2015'])
        ->all()
); 
/*
//Резлуьтат запроса
Array
(
    ...
    [USD] => Array
        (
            [name] => Доллар США
            [value] => 59.7665
            [char_code] => USD
            [num_code] => 840
            [nominal] => 1
            [id] => R01235
            [dynamic] => Array
                (
                    [0] => Array
                        (
                            [date] => 1432080000
                            [value] => 49.1777
                        )

                    [1] => Array
                        (
                            [date] => 1432166400
                            [value] => 49.7919
                        )

                    [2] => Array
                        (
                            [date] => 1432252800
                            [value] => 49.9204
                        )

                    [3] => Array
                        (
                            [date] => 1432339200
                            [value] => 49.7901
                        )

                )

        )
    ...
)
*/
// ------------------------------------------------------------------------------------------------------------------------
```