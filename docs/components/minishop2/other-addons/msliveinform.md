# msLiveInform

## Описание

**msLiveInform** - компонент отслеживания Заказов магазина MiniShop2 при помощи сервиса [LiveInform][001].

![msLiveInform](https://file.modx.pro/files/0/4/6/046f774ad0906fa37732868978226c1b.png)

## Особенности

- работа только с **новым miniShop2** (version =>2.4.0-beta2)
- отслеживание заказов при помощи сервиса [LiveInform][001]
- сниппет для вывода информации отслеживания заказа
- проверка клиента заказа по **Черному списку** ненадежных клиентов при помощи сервиса [LiveInform][001]

## Установка

- [Подключите наш репозиторий][002]
- Установите **miniShop2** - это магазин на основе которого реализован функционал заказа c оплатой
- Установите **msLiveInform**

Для тестирования можно использовать [наш хостинг][002], на нём эти дополнения можно выбрать прямо при создании сайта.
После установки компонента необходимо:

- прописать ключ для доступа к АПИ сервиса [LiveInform][001]

## Настройка

Все сниппеты **msLiveInform** работают при помощи pdoTools и рассчитывают на использование [Fenom][010103] в чанках.
Это позволяет:

- сократить общее количество чанков
- повысить удобство работы
- ускорить работу
- делать более сложные чанки, за счёт продвинутой проверки условий через функции Fenom

## Создание отслеживания

В окне редактирования заказа [MiniShop2][01020103]

![Создание отслеживания - 1](https://file.modx.pro/files/8/d/7/8d75d9656092ad99601e10253d83639f.png)

Нажимаем **+** создать отслеживание. Заполняем необходимые поля:

- трек-номер отправления
- телефон клиента

![Создание отслеживания - 2](https://file.modx.pro/files/1/8/9/1895f4eec3345b16ce27b6c554d75a99.png)

Сохраняем. Отслеживание будет синхронизировано с сервисом [LiveInform][001].
Информацию об ослеживании можно обновить тут же в админке, либо настроив скрипт на `cron`.

Пример скрипта синхронизации в папке `core/components/msliveinform/cron/`.

По умолчанию активирована настройка `change_order_status` - разрешающая менять статус заказа по смене статуса отслеживания. Каждому статусу отслеживания можно задать соответсвующий статус заказа.

На вкладке **Трекинг** доступна актуальная информация о перемещении отслеживания

![Создание отслеживания - 3](https://file.modx.pro/files/a/9/7/a97479501e859dba0f3ba3d160da45ff.png)

## Вывод отслеживаний на фронте

Сниппет для вывода отслеживаний `msLiveInform.tracking`

![Вывод отслеживаний на фронте](https://file.modx.pro/files/e/c/2/ec25ccb251ffc95245151986512c6fea.png)

### Параметры

| Параметр    | По умолчанию          | Описание                                                            |
| ----------- | --------------------- | ------------------------------------------------------------------- |
| **order**   |                       | Идентификатор заказа. Если поставить 0 - выборка не ограничивается. |
| **tpl**     | `msLiveInform.tracking` | Чанк оформления                                                     |
| **limit**   | `10`                    | Лимит выборки результатов                                           |
| **sortby**  | `id`                    | Сортировка выборки.                                                 |
| **sortdir** | `ASC`                   | Направление сортировки                                              |

:::tip
Можно использовать и другие [общие параметры pdoTools][0104]
:::

пример вызова отслеживаний конкретного заказа:

``` modx
[[!msLiveInform.tracking?
    &order=`2`
]]
```

Вы можете увидеть все доступные плейсхолдеры отслеживаний просто не указывая чанк оформления:

``` modx
<pre>
[[!msLiveInform.tracking?
    &order=`2`
    &tpl=``
]]
</pre>
```

Вывод отслеживаний с постраничной разбивкой:

```modx
[[!pdoPage?
    &element=`msLiveInform.tracking`
]]
[[!+page.nav]]
```

## Проверка по базе заказов сервиса LiveInform

Доступна проверка клиента заказа на предмет количества заказов:

- Количество возвратов
- Количество врученных заказов
- Общее число заказов

![Проверка по базе заказов сервиса LiveInform - 1](https://file.modx.pro/files/c/f/8/cf8bbcbda0f39ae7e500c0254c0caa84.png)

![Проверка по базе заказов сервиса LiveInform - 2](https://file.modx.pro/files/9/f/3/9f30f7d46f18f5588eb7fd76f039a95d.png)

## Настройки msLiveInform

Настройки **msLiveInform** расположены в **Системные настройки** > **msLiveInform**

Содержит основные настройки:

- `api_id` - Идентификатор для отправки запроса к LiveInform API
- `order_id_key` - Идентификатор заказа MiniShop в системе LiveInform. По умолчанию "num"
- `change_order_status` - Флаг разрешающий менять статус заказа по смене статуса отслеживания
- `order_status_2` - Идентификатор статуса заказа соответствующий статусу "Вручен" отслеживания
- `order_status_3` - Идентификатор статуса заказа соответствующий статусу "Возврат" отслеживания
- `delivery_service` - Список служб доставки. Доступен для выбора при создании отслеживания

Для удобства введена настройка `change_order_status` позволяющая автоматически менять статус заказа в зависимости от статуса отслеживания.
Возможны следующие статусы отслеживания:

- 0 - в пути
- 1 - в ПВЗ
- 2 - Вручен
- 3 - Возврат

Для каждого статуса отслеживания можно создать/ указать свою настройку. Например статус отслеживания - **2 - Вручен** определяется соответствующей настройкой `order_status_2`
Если вы новый клиент **LiveInform**, настройте тексты сообщений для каждой из служб доставок в разделе [Настройки][00101].

Можно отключить смену заказов по всем статусам задав настройку `change_order_status` в `нет`.

## Настройка Callback

**LiveInform** может оповещать ваш серевер при изменении статусов заказа. Для этого нужно [указать адрес скрипта][00102], на который будут отправляться оповещения.

Пример скрипта callback `http://site.ru/assets/components/msliveinform/callback.php`.

[010103]: /components/pdotools/parser
[01020103]: /components/minishop2/interface/orders
[0104]: /components/pdotools/general-parameters

[001]: https://liveinform.ru/?partner=166
[00101]: https://www.liveinform.ru/account/settings?partner=166
[00102]: https://liveinform.ru/account/integration/callback?partner=166
[002]: https://modhost.pro