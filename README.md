[![Version][version-shield]][version]
[![License][license-shield]][license]
[![ESPHome release][esphome-release-shield]][esphome-release]
[![Telegram][telegram-shield]][telegram]
[![Support author][donate-tinkoff-shield]][donate-tinkoff]
[![Support author][donate-boosty-shield]][donate-boosty]
[![Open in Visual Studio Code][open-in-vscode-shield]][open-in-vscode]

[version-shield]: https://img.shields.io/static/v1?label=Версия&message=next&color=green
[version]: https://github.com/dentra/esphome-tion/releases/

[license-shield]: https://img.shields.io/static/v1?label=Лицензия&message=MIT&color=orange&logo=license
[license]: https://opensource.org/licenses/MIT

[esphome-release-shield]: https://img.shields.io/static/v1?label=ESPHome&message=2024.5.5&color=green&logo=esphome
[esphome-release]: https://github.com/esphome/esphome/releases/

[open-in-vscode-shield]: https://img.shields.io/static/v1?label=+&message=Открыть+в+VSCode&color=blue&logo=visualstudiocode
[open-in-vscode]: https://open.vscode.dev/dentra/esphome-tion

[telegram-shield]: https://img.shields.io/static/v1?label=Поддержка&message=Телеграм&logo=telegram&color=blue
[telegram]: https://t.me/esphome_tion

[donate-tinkoff-shield]: https://img.shields.io/static/v1?label=Поддержать+автора&message=Тинькофф&color=yellow
[donate-tinkoff]: https://www.tinkoff.ru/cf/3dZPaLYDBAI

[donate-boosty-shield]: https://img.shields.io/static/v1?label=Поддержать+автора&message=Boosty&color=red
[donate-boosty]: https://boosty.to/dentra

# Tion

English version of this page is available via [google translate](https://github-com.translate.goog/dentra/esphome-tion?_x_tr_sl=ru&_x_tr_tl=en&_x_tr_hl=en&_x_tr_pto=wapp).

Компонет ESPHome для управления бризерами `Tion` с помощью ESP в вашей системе управления
умным домом. Поддерживаются Home Asistant [API](https://esphome.io/components/api.html) и
любые другие системы управления умным домом через протокол
[MQTT](https://esphome.io/components/mqtt.html).

Поддерживаемые модели и протоколы:
 - Tion 4S (BLE/UART)
 - Tion Lite (BLE)
 - Tion 3S (BLE/UART)
 - Tion O2 Mac (UART) (бризер должен иметь возможность подключения RF-модуля, наличие самого модуля не обязательно)

Интеграция позволяет управлять следующими функциями:
* Включение/Выключение
* Обогрев
* Целевая температуры нагрева
* Скорость притока воздуха
* Звуковое оповещение (для O2 только если физически включено на бризере)
* Световое оповещение (только для 4S и Lite)
* Режим приток/рециркуляция (только для 4S и 3S)
* Режим приток/рециркуляция/смешанный (только для 3S)
* Гибко настриваемые пресеты
* Режим "Турбо"
* Настройка времени пресета "Турбо"
* Сброс ресурса фильтров (кроме O2)
* Подтверждение сброса ресурса фильтра (кроме O2)

Дополнительно осуществляется мониториг следующих показателей:
* Температура снаружи
* Температура внутри
* Текущая потребяемая мощность нагревателя (только для 4S и Lite)
* Оставшееся время жизни фильтра (кроме O2)
* Индикация о требущейся замене/очистке фильтра (кроме O2)
* Оставшееся время работы режима "Турбо"
* Счетчик прошедшего воздуха (только для 4S и Lite)
* Текущая производительностью бризера
* Версия програмного обеспечения бризера
* Измерение энергопотребления (только для 4S и Lite)
* Контроль состояния заслонки (только для Lite)
* Внутренняя температрура (только для 4S и Lite)

А так же:
* Конфигурация пресетов сервисом
* Задание максимальной температуры нагрева 30°C, в том числе для Lite
* Дамп и сброс внутренних таймеров (только для 4S)
* Защита от обмерзания (только для 3S и O2, в 4S и Lite это неотключаемая встроенная возможность)
* Контроль и зависимость состояния нагревателя от соостояния заслонки (только для 3S и 4S)


Полный список возможностей смотрите в [инструкции по конфигурации](CONFIGURATION.md).

> [!CAUTION]
> ## ⚠️ **ВНИМАНИЕ: Все что вы делаете, вы делаете исключительно на свой страх и риск!**
>

## Подключение

Доступно два вида подключения BLE и UART.

BLE подключение работает так же как ваш пульт или официальное приложение.
UART подключние различно для разных моделей бризеров.

### UART-подключение Tion 4S

Для UART-подключения `Tion 4S` используется штатный интеграционный разъем бризера.
Рекомендуется приобрести стик [Lilygo T-Dongle S3](https://github.com/Xinyuan-LilyGO/T-Dongle-S3),
проще всего это сделать на Aliexpress. Или собрать самостоятельно на базе ESP32.
> [!IMPORTANT] Собрка и работа компонента на ESP8266 возможна, но стабильность не
> гарантируется, так же, в этом направлении, не будет оказана никакая поддержка,
> используйте только ESP32!

### UART-подключение Tion 3S

Для UART-подключения `Tion 3S` использется штатный, но доступный только при небольшой
разборке бризера, разъем. Здесь рекумендуется использовать любую ESP8266
(ESP32 не тянет по питанию) используя [схему подключения](hardware/3s/NodeMCUv3-Tion.pdf).
При использовании ESP-01S не будет доступено подключение штатного модуля BLE,
в остальных случаях подключение будет полным.
> Буду благодарен за любые экперименты по использованию совремнных модулей ESP32,
например, серий S2, S3 или C3.

### UART-подключение Tion O2 Mac

Для UART-подключения `Tion O2 Mac` использется штатный, но доступный только при небольшой
разборке бризера, разъем. Требуются начальные навыки работы с паяльником для подключения
разъема типа XH2.54 4pin male к ESP (распиновку см. [здесь](https://github.com/dentra/esphome-tion/issues/4#issuecomment-1910494611))
или можно приобрести готовый переходник для подключения стика [Lilygo T-Dongle S3](https://github.com/Xinyuan-LilyGO/T-Dongle-S3)
у меня (доступность утоняйте).
> [!IMPORTANT] Собрка и работа компонента на ESP8266 возможна, но стабильность не
> гарантируется, так же, в этом направлении, не будет оказана никакая поддержка,
> используйте только ESP32!

По вопросам подключения велкам в [чат Telegram][telegram].

## Прошивка

Вы можете загрузить и использовать примеры конфигурации для
[Tion 4S BLE](tion-4s-ble.yaml), [Tion 4S UART](tion-4s-uart.yaml),
[Tion Lite BLE](tion-lt-ble.yaml), [Tion 3S BLE](tion-3s-ble.yaml),
[Tion 3S UART](tion-3s-uart.yaml) и [Tion O2 UART](tion-o2-uart.yaml)
все файлы с подробным описанием (на английском).
В примерах базовой сущностью выступает компонет Climate,
вы можете изменить его на Fan или даже обычный Switch и Number в качестве
управляющих элеменов, смотрите [инструкцию по конфигурации](CONFIGURATION.md).

* Скачайте конфигурацию соотствующую модели вашего бризера
* Измените секцию `substitutions` согласно вашим предпочтениям
* Измените набор подключаемых пакетов по вашему вкусу
* Добавьте или удалите необходимы сущности согласно [инструкции по конфигурации](CONFIGURATION.md)
* Поместите модифицированный файл в директорию с конфигурацией ESPHome
* Запустите сборку и прошивку вашей конфигурации
* Добавьте появившееся устройство в Home Assistant

> [!TIP]
> MAC-адрес для работы в режиме BLE можно посмотреть в приложении Tion Remote,
> в приложении MagicAir может отображаться другой адрес.

> [!IMPORTANT]
> Первая прошивка обязательно должна быть по проводу, прошивка пустышкой esphome не считается таковой!

> [!IMPORTANT]
> Для модели 4S, прошивка бризера обязательно должна быть не ниже 02D2, посмотреть ее можно в приложении Tion Remote.

## Использование в режиме BLE

После [прошивки](#прошивка) и перед первым использование вам необъходимо ввести
свой бризер в режим сопряжения (см. инструкцию) и только потом включать ESP или
провести перезагрузку ESP с помощью фукции `Restart`.

Дополнительно, только для `Tion 3S`, необходимо нажать кнопку `Pair` в Home Assistant.

> [!NOTE]
> Рекомендуется испольовать ESP c двумя ядрами, например из линейки ESP32 или ESP32-S3.

## Использование в режиме UART

Никаких дополнительный действий не требуется.

## OTA-обновление

В режиме BLE - нет никаких ограничений.

В режиме UART для всех бризеров кроме Tion 4S - нет никаких ограничений.

В режиме UART только для Tion 4S - строго обязательно использовать быструю WiFi сеть в диапазоне 5GHz,
в противном случае возможно отключение стика бризером. Если это произошло, то рекомендуется извлечть
стик из бризера, перепрошить его обычным способом и после это испольовать как обычно.

## Полезности
### Просмотр и/или сброс внутренних таймеров Tion 4S

Добавьте в свою конфигурацию пакет `tion_4s_timers.yaml`:
```yaml
packages:
  tion:
...
  files:
...
    - packages/tion_4s_timers.yaml
```

## Использование с системами УД отличными от Home Assistant

Вы можете испольовать этот компонент с любой системой УД через протокол MQTT.
Для этого в файле конфигурации найдите и раскомментуйте секцию `mqtt`, дополнительные параметры
broker, port и т.д. установите согласно настройкам вашей системы УД.
Со списком всех параметров можно ознакомиться на странице [документации](https://esphome.io/components/mqtt.html).

### Испольование с УД Sprut.Hub

Наобходимо настроить MQTT согласно описанию из секции выше и установить
[шаблон](https://github.com/l0rda/sprut-tion) от `@l0rda`.

Дополнительную помощь всегда можно получить в [группе Telegram](https://t.me/esphome_tion).

## Принцип работы и тонкая настройка

### Получение состояния

Состояние бризера храниться исключительно в самом бризере и синхронизируется
раз в настраиваемый интервал времени `tion.update_interval` или сразу же
после отработки запроса на изменение. Если состояние получить невозможно, то об этом
информирует бинарный сенсор с типом `state`, таймаут получения ответа на запрос
состояния настраивается `tion.state_timeout` и должен быть меньше
интервала опроса `tion.update_interval`.


### Изменение состояния

Изменение состояния происходит относительно последнего ответа на запрос состояния
или предыдущего ответа запроса на изменение. Так как ответ может прийти медленней
чем придет новый запрос (актуально для скриптов) реализован механизм пакетной
отправки команд на изменение. Поступивший запрос на изменение стартует таймер,
время сработки которого задается параметром `tion.batch_timeout`, каждый последующий
запрос до сработки таймера объединяется с предыдущим и рестартует таймер, после
сработки таймера объединенный запрос отправляется бризеру на выполнение.

### Отправка команд

Все запросы выстраиваются в очередь и выполняются с интервалом `vport.command_interval`.
Размер очереди задается параметром `vport.command_size`. Минимальный интервал `0s` будет
срабатывать один раз в итерацию основного цикла.

## Планы на будущее

* ~~Поддержка UART-подключения через интеграционный разъем для `Tion 4S`~~
* ~~Поддержка UART-подключения `Tion 3S`~~
* ~~Поддержка UART-подключения `Tion O2 Mac`~~
* Поддержка UART-подключения для `Tion Lite`
* ~~Управление сбросом ресура фильтров~~
* Автоматическое управление притоком от внешнего датчика CO2 (работает в эксериментальном режиме)
* Возможность прошивки готового образа
* Автоматическое обновление


## Решение проблем и поддержка новых функций

Не стесняйтесь открывать [задачи](https://github.com/dentra/esphome-tion/issues) для сообщений об ошибках и запросов новых функций.

Так же вы можете воспользоваться [группой в Telegram](https://t.me/esphome_tion).

## Ваша благодарность

Если этот проект оказался для вас полезен и/или вы хотите поддержать его дальнейше
развитие, то всегда можно оставить вашу благодарность
[переводом на карту](https://www.tinkoff.ru/cf/3dZPaLYDBAI) или
[подпиской](https://boosty.to/dentra) и обычной звездой проекту.

## Коммерческое использование

По вопросам коммерческого использования или заказной разработки/доработки,
обращийтесь по почте dennis.trachuk на gmail.com
