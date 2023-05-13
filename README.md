# Автобусы на карте Москвы

Веб-приложение показывает передвижение автобусов на карте Москвы.

<img src="screenshots/buses.gif">

## Как запустить

### Клонируйте репозиторий

```
git clone https://github.com/EshiNanase/async-bus-map.git
```

### Установите python 3.9 и необходимые библиотеки
```
pip install -r requirements.txt
```

### Запустите сервер

Возможные флаги:
```
--browser_port - кастомный порт для принятия сообщений из браузера (по дефолту 8000)
--bus_port - кастомный порт для принятия сообщений об автобусах (по дефолту 8080)
--verbose - необходимо ли логирование в консоль
```

Теперь точно запустите сервер:
```
python server.py + флажки
```

Откройте index.html в браузере и наслаждайтесь пустой картой Москвы!

## Как запустить имитатор автобусов

Флаги:
```
--server - адрес сервера, на котором будет имитатор (по дефолту 127.0.0.1:8080)
--routes_number - кол-во маршрутов (по дефолту 500, максимум 900)
--buses_per_route - кол-во автобусов на маршруте (по дефолту 5)
--websockets_number - кол-во каналов для передачи информации об автобусах (по дефолту 5, лучше ставить пропорционально кол-ву автобусов на маршруте)
--emulator_id - префикс к id автобусов, если будет запущено несколько имитаторов (по дефолту текущий timestamp)
--refresh_timeout - задержка обновления координат (по дефолту 1)
--verbose - необходимо ли логирование в консоль
```

Теперь точно запустите имитатор:
```
python fake_bus.py + флажки
```

Теперь в index.html появится много автобусов!

## Как запустить тесты

```
pytest -m server_tests.py
```

## Настройки

Внизу справа на странице можно включить отладочный режим логгирования и указать нестандартный адрес веб-сокета.

<img src="screenshots/settings.png">

Настройки сохраняются в Local Storage браузера и не пропадают после обновления страницы. Чтобы сбросить настройки удалите ключи из Local Storage с помощью Chrome Dev Tools —> Вкладка Application —> Local Storage.

Если что-то работает не так, как ожидалось, то начните с включения отладочного режима логгирования.

## Формат данных

Фронтенд ожидает получить от сервера JSON сообщение со списком автобусов:

```js
{
  "msgType": "Buses",
  "buses": [
    {"busId": "c790сс", "lat": 55.7500, "lng": 37.600, "route": "120"},
    {"busId": "a134aa", "lat": 55.7494, "lng": 37.621, "route": "670к"},
  ]
}
```

Те автобусы, что не попали в список `buses` последнего сообщения от сервера будут удалены с карты.

Фронтенд отслеживает перемещение пользователя по карте и отправляет на сервер новые координаты окна:

```js
{
  "msgType": "newBounds",
  "data": {
    "east_lng": 37.65563964843751,
    "north_lat": 55.77367652953477,
    "south_lat": 55.72628839374007,
    "west_lng": 37.54440307617188,
  },
}
```



## Используемые библиотеки

- [Leaflet](https://leafletjs.com/) — отрисовка карты
- [loglevel](https://www.npmjs.com/package/loglevel) для логгирования


## Цели проекта

Код написан в учебных целях — это урок в курсе по Python и веб-разработке на сайте [Devman](https://dvmn.org).
