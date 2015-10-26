# API WAPCLICK.ONLINE

**версия 1.08**

## 1. Описание

WapClick – новая прогрессивная технологий мобильных подписок. Абоненту достаточно нажать одну кнопку для получения доступа к платному контенту и совершения подписки. Абонент получает качественный контент, списания с его счёта происходит ежедневно!


### 1.1 Особенности:

1. Абонент соглашается на подписку на странице оператора связи (landing page).
2. Оператор связи списывает средства со счёта абонента. Списание средств и предоставление доступа прекращается при выполнении одного из следующих условий – абонент отписался, закончился срок подписки, нет достаточного колличества средств на счету.
3. Тарификация подписавшегося абонента происходит ежедневно.


## 2. Подключение

Для подключения партнёр предоставляет:

1. Адрес уведомлений.
2. Адрес возврата на сайт (может быть не задан, если партнёр будет задавать адрес возврата для каждого абонента при инициации подписки).

Менеджер сообщает партнёру:

1. Идентификатор заведённой подписки (service_id).
2. Секретный ключ (secret_key).

### 2.1 Стандартная схема

Партнёр размещает на страницах сайта код загрузки скрипта

```html
<script type="text/javascript" src="//wapclick.mobi/script/[идентификатор подписки].js" async></script>
```

 Пример

```html
<script type="text/javascript" src="//wapclick.mobi/script/13511.js" async></script>
```

Партнёр размещает на страницах сайта элемент для перехода к активации подписки. Это может быть кнопка или любой другой элемент

```html
<div id="subscribe_btn"></div>
```

Так же необходимо внизу страницы разместить элемент для отображения правил подписки

```html
<div id="footer_frame"></div>
```

### 2.2 Упрощённая схема

Партнёр размещает ссылку на баннере или кнопке

```
https://wapclick.mobi/init/[идентификатор подписки].html
```

Пример

```html
<a href="https://wapclick.mobi/init/13511.html">Оформите подписку!</a>
```

Упрощённая схема может быть использована только после согласования с менеджером.

## 3. Дополнительные параметры подключения

Если требуется предоставлять абоненту несколько видов контента или требуется большая безопасность взаимодействия, то возможно задавать дополнительные параметры при использовании как стандартной, так и упрощённой схем

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| back_url | varchar(1000) | Адрес возврата абонента | https://site.com/content

Параметр p_data будет в дальнейшем передан партнёру в уведомлении об успешной тарификации.

back_url будет использован при возврате абонента вместо адреса, прописанного по умолчанию.

Данные параметры не являются обязательными.

Пример

```html
<script type="text/javascript" src="https://wapclick.mobi/script/13511.js?p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&back_url=https%3A%2F%2Fsite.com%2Fcontent" async></script>
```

## 4. Возврат абонента на сайт партнёра

После операции подписки абонент возвращается на back_url, переданный в дополнительных параметрах кода загрузки скрипта или на адрес возврата на сайт, который прописывается при заведении подписки. При возврате к адресу добавляются следующие параметры

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| code | int | Статус подписки | 0
| error | varchar(30) | Расшифровка статуса подписки | ok

Получение данных параметров (успешного статуса подписки) не является гарантией успешного статуса подписки на стороне оператора, т.к. может быть подделано абонентом. Рекомендуется опираться на данные параметры только в простейших сервисах или для предварительного уведомления абонента об успехе или неуспехе.

Для гарантии подписки абонента на стороне оператора требуется обрабатывать уведомление о тарификации и предоставлять абоненту контент после получения успешного уведомления.

Статусы подписок

| code | error | Описание
| --- | --- | ---
| 0 | ok | Подписка оформлена
| 1 | invalid params | Неверные параметры
| 2 | invalid auth | Неверный проверочный код
| 3 | invalid provider auth (please, contact us) | Изменились настройки подключения
| 4 | operator is not supported | Оператор не поддерживается
| 5 | service not found | Сервис не определен или нет такого сервиса
| 6 | service disabled | Сервис отключен
| 7 | already | Абонент уже был подписан или отписан
| 8 | billing failed: no money / fraud | У абонента нет денег / фрод
| 9 | refuse | Абонент отказался от подписки
| 10 | disabled by operator options for abonent / subscriptions banned | У абонента отключены опции подписок на стороне оператора
| 11 | operator subscription service internal error | Ошибка на стороне оператора связи
| 12 | pending | Ожидается действие от абонента
| 13 | service internal error | Ошибка на стороне wapclick.online
| 14 | invalid phone number / cannot find | Оператор на своей стороне не может определить номер телефона абонента
| 15 | activation not finished | Активация не окончена
| 16 | incorect activation code | Неверный код подтверждения от абонента
| 17 | attempt count exceeded | Превышено количество попыток получения временной ссылки от оператора
| 18 | expired | Превышено время ожидания действий от абонента
| 19 | pending (wait confirmaton) | Абонент не завершил действие на стороне оператора
| 20 | cannot find source | Источник не зарегистрирован
| 21 | cannot find oferta | Оферта не найдена
| 22 | device block | Устройство абонента не поддерживает подписки
| 23 | service blacklisted | Чёрный список

## 5. Уведомления о тарификациях

wapclick.online уведомляет партнера об успешных списаниях денежных средств со счета абонента.

Уведомление - GET запрос с параметрами

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| action | varchar(30) | Тип уведомления - тарификация | charge_report
| status | int | Статус тарификации. 0 - успешная, -1 - неуспешная | 0
| phone | bigint | Номер абонента | 79031234567
| op | varchar(30) | Оператор | beeline
| c_amount | numeric(18,2) | Сумма тарификации в валюте абонента | 100.01
| amount | numeric(18,2) | Сумма тарификации в валюте расчётов с партнёром | 100.01
| c_pay | numeric(18,2) | Сумма выплаты партнёру в валюте абонента | 50.01
| pay | numeric(18,2) | Сумма выплаты партнёру в валюте расчётов с партнёром | 50.01
| c_curr | character(3) | Буквенный код валюты абонента (ISO 4217) | RUB
| curr | character(3) | Буквенный код валюты расчётов с партнёром (ISO 4217) | RUB
| tid | varchar(100) | Уникальный идентификатор транзакции | 08057700-690d-11e5-b610-321018f8ac82
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82

Пример уведомления

```
https://site.com/subscriptions?action=charge_report&status=0&phone=79031234567&op=beeline&c_amount=100.01&amount=100.01&c_pay=50.01&pay=50.01&c_curr=RUB&curr=RUB&tid=08057700-690d-11e5-b610-321018f8ac82&p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82
````

## 6. Уведомления об отписках

wapclick.online уведомляет партнера об отписке абонента.

Уведомление - GET запрос с параметрами

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| action | varchar(30) | Тип уведомления - отписка | unsubscribe_report
| service_id | integer | Идентификатор подписки | 1234
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| sign | char(64) | Подпись запроса sha256(action+service_id+p_data+secret_key) | 68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728

Пример уведомления

```
https://site.com/subscriptions?action=unsubscribe_report&service_id=1234&p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&sign=68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728
````

## 7. Оценка конверсии

Система поддерживает подключение UTM меток к элементам активации подписки. Для их использования нужно добавить метки в виде атрибута data. Метки будут доступны для выборки в личном кабинете.

Поддерживаемые метки:

* utm_source
* utm_medium
* utm_campaign
* utm_term
* utm_content

Пример

```html
<div id="subscribe_btn" data-utm_source="1" data-utm_medium="2" data-utm_campaign="3" data-utm_term="4" data-utm_content="5"></div>
```

## 8. Контакты

С вопросами обращайтесь по почте [support@wapclick.online](mailto:support@wapclick.online)

