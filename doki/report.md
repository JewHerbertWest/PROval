## Структура проекта, API и политики безопасности

Проект `SevastopolSHIP` представляет собой программную реализацию переработанной архитектуры системы управления шлюзами космической станции «Севастополь». В отличие от исходной версии, здесь элементы архитектуры не собраны в один общий модуль. Каждый значимый элемент вынесен в отдельную папку внутри `services/`.

Такой подход нужен для того, чтобы структура кода соответствовала архитектуре: один элемент на схеме — один сервис в проекте. У каждого сервиса есть собственная реализация, файл инициализации, Dockerfile, список зависимостей и краткое описание.

### Общая структура проекта

```text
SevastopolSHIP/
│
├── app.py
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── pytest.ini
├── README.md
├── ARCHITECTURE_MAP.md
│
├── services/
│   ├── terminal/
│   ├── authorization/
│   ├── crypto_encryptor/
│   ├── management_subsystem/
│   ├── local_operator_terminal/
│   ├── crew_control_panel/
│   ├── earth_interface/
│   ├── ship_interface/
│   ├── crypto_decryptor/
│   ├── command_filter/
│   ├── authorization_check/
│   ├── security_policy_check/
│   ├── command_validator/
│   ├── central_control/
│   ├── security_policies/
│   ├── command_security_monitor/
│   ├── compartment_control_service/
│   ├── metrics_control/
│   ├── sensor_verifier/
│   ├── access_control_subsystem/
│   ├── authentication_system/
│   ├── airlock_control_service/
│   ├── airlock_control/
│   ├── drive_control/
│   ├── airlock_drives/
│   ├── docking_subsystem/
│   ├── ship_telemetry_check/
│   ├── docking_node_control/
│   ├── docking_confirmation/
│   ├── service_monitor/
│   ├── event_analyzer/
│   ├── emergency_module/
│   ├── emergency_response_module/
│   ├── safe_mode_transition/
│   ├── executive_devices/
│   ├── state_db/
│   ├── telemetry_db/
│   ├── logging_service/
│   └── log_integrity_control/
│
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── test_e2e_scenarios.py
│   └── test_negative_scenarios.py
│
├── scenarios/
│   └── open_airlock_success.py
│
└── docs/
    ├── scenario_open_airlock_new.puml
    └── service_structure.md
```

### Назначение основных папок и файлов

| Элемент проекта       | Назначение                                                                                                                 |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `app.py`              | Основная точка входа в проект. Используется как общий шлюз для запуска сценариев и проверки работы системы.                |
| `services/`           | Основная папка с реализацией элементов переработанной архитектуры. Каждый элемент архитектуры находится в отдельной папке. |
| `tests/`              | Папка с тестами. В ней находятся e2e-тесты и тесты негативных сценариев.                                                   |
| `scenarios/`          | Папка со штатными сценариями работы системы.                                                                               |
| `docs/`               | Папка с дополнительными материалами: PlantUML-сценариями и описанием сервисной структуры.                                  |
| `docker-compose.yml`  | Файл для запуска проекта и отдельных сервисов через Docker Compose.                                                        |
| `Dockerfile`          | Dockerfile для основного приложения.                                                                                       |
| `requirements.txt`    | Общие зависимости проекта.                                                                                                 |
| `pytest.ini`          | Настройки запуска тестов через pytest.                                                                                     |
| `README.md`           | Основное описание проекта, запуска и логики тестирования.                                                                  |
| `ARCHITECTURE_MAP.md` | Карта соответствия элементов архитектуры и папок в коде.                                                                   |

### Структура отдельного сервиса

Каждый элемент архитектуры оформлен как отдельный сервис. Внутри каждой папки сервиса находится одинаковый набор файлов:

```text
service_name/
├── app.py
├── __init__.py
├── Dockerfile
├── requirements.txt
└── README.md
```

Назначение файлов:

| Файл               | Назначение                                          |
| ------------------ | --------------------------------------------------- |
| `app.py`           | Реализация логики конкретного элемента архитектуры. |
| `__init__.py`      | Файл инициализации Python-пакета.                   |
| `Dockerfile`       | Описание сборки Docker-образа конкретного сервиса.  |
| `requirements.txt` | Зависимости конкретного сервиса.                    |
| `README.md`        | Краткое описание назначения сервиса.                |

Благодаря этому каждый элемент архитектуры можно рассматривать отдельно: у него есть собственная папка, собственная реализация и собственная контейнеризация.

### Сервисы проекта

| Элемент архитектуры              | Папка в проекте                         |
| -------------------------------- | --------------------------------------- |
| Терминал                         | `services/terminal/`                    |
| Авторизация                      | `services/authorization/`               |
| Криптографический шифратор       | `services/crypto_encryptor/`            |
| Подсистема управления            | `services/management_subsystem/`        |
| Локальный операторский терминал  | `services/local_operator_terminal/`     |
| Пульт управления экипажа         | `services/crew_control_panel/`          |
| Интерфейс связи с Землёй         | `services/earth_interface/`             |
| Интерфейс связи с кораблями      | `services/ship_interface/`              |
| Криптографический дешифратор     | `services/crypto_decryptor/`            |
| Интерфейс фильтрации команд      | `services/command_filter/`              |
| Проверка авторизации             | `services/authorization_check/`         |
| Проверка политик безопасности    | `services/security_policy_check/`       |
| Валидатор команд                 | `services/command_validator/`           |
| Центральная система управления   | `services/central_control/`             |
| Политики безопасности            | `services/security_policies/`           |
| Монитор безопасности команд      | `services/command_security_monitor/`    |
| Сервис контроля отсеков          | `services/compartment_control_service/` |
| Контроль показателей             | `services/metrics_control/`             |
| Верификатор показателей датчиков | `services/sensor_verifier/`             |
| Подсистема контроля доступа      | `services/access_control_subsystem/`    |
| Система аутентификации           | `services/authentication_system/`       |
| Подсистема управления шлюзами    | `services/airlock_control_service/`     |
| Контроль шлюзов                  | `services/airlock_control/`             |
| Управление приводами             | `services/drive_control/`               |
| Приводы шлюзов                   | `services/airlock_drives/`              |
| Подсистема стыковки кораблей     | `services/docking_subsystem/`           |
| Проверка телеметрии корабля      | `services/ship_telemetry_check/`        |
| Контроль стыковочного узла       | `services/docking_node_control/`        |
| Подтверждение стыковки           | `services/docking_confirmation/`        |
| Монитор сервисов                 | `services/service_monitor/`             |
| Анализатор событий               | `services/event_analyzer/`              |
| Аварийный модуль                 | `services/emergency_module/`            |
| МАРЧС                            | `services/emergency_response_module/`   |
| Переход в безопасный режим       | `services/safe_mode_transition/`        |
| Исполнительные устройства        | `services/executive_devices/`           |
| База состояний                   | `services/state_db/`                    |
| База телеметрии                  | `services/telemetry_db/`                |
| Сервис журналирования            | `services/logging_service/`             |
| Контроль целостности журнала     | `services/log_integrity_control/`       |

### Логика взаимодействия сервисов

В переработанной архитектуре команда не передаётся сразу к центральной системе управления или к шлюзам. Сначала она проходит несколько уровней обработки и проверки:

```text
Терминал
→ Авторизация
→ Криптографический шифратор
→ Подсистема управления
→ Локальный операторский терминал
→ Интерфейс фильтрации команд
→ Проверка авторизации
→ Проверка политик безопасности
→ Валидатор команд
→ Монитор безопасности команд
→ Центральная система управления
```

После этого центральная система управления обращается к другим сервисам:

```text
Сервис контроля отсеков
→ Контроль показателей
→ Верификатор показателей датчиков
→ База телеметрии
```

Затем проверяется доступ:

```text
Подсистема контроля доступа
→ Система аутентификации
```

После этого проверяется состояние шлюза:

```text
Подсистема управления шлюзами
→ Контроль шлюзов
→ Управление приводами
→ Приводы шлюзов
```

Результат операции фиксируется в хранилище и журнале:

```text
База состояний
Сервис журналирования
Контроль целостности журнала
```

Таким образом, выполнение команды зависит не от одного компонента, а от цепочки проверок. Если один из элементов входного контура или управляющей логики скомпрометирован, команда должна быть остановлена защитным элементом до того, как она дойдёт до исполнительных устройств.

## API проекта

В проекте используется два уровня API.

Первый уровень — основное приложение `app.py`. Оно выполняет роль входной точки для демонстрации сценариев, запуска тестовых операций и проверки состояния системы.

Второй уровень — отдельные сервисы в папке `services/`. Каждый элемент переработанной архитектуры имеет собственную папку и минимальный API для проверки работы сервиса.

### API основного приложения

Основное приложение доступно по адресу:

```text
http://localhost:8000
```

| Метод             | Тип запроса | Назначение                                                                     |
| ----------------- | ----------- | ------------------------------------------------------------------------------ |
| `/`               | GET         | Главная страница проекта. Показывает краткое описание архитектуры и сценариев. |
| `/ready`          | GET         | Проверка готовности основного приложения.                                      |
| `/reset`          | POST        | Сброс состояния системы перед повторным запуском сценариев или тестов.         |
| `/components`     | GET         | Получение списка элементов архитектуры.                                        |
| `/protection_map` | GET         | Получение связи негативных сценариев с защитными элементами.                   |

### API штатных сценариев

Эти методы используются для проверки нормальной работы системы. Они нужны для e2e-тестов, которые показывают, что система выполняет свои функции в штатном режиме.

| Метод                     | Тип запроса | Назначение                                               |
| ------------------------- | ----------- | -------------------------------------------------------- |
| `/terminal/open_airlock`  | POST        | Штатная команда открытия шлюза через терминал оператора. |
| `/terminal/unlock_sector` | POST        | Штатная команда разблокировки сектора при наличии прав.  |
| `/ship/docking_request`   | POST        | Штатный запрос на стыковку корабля.                      |
| `/emergency/safe_mode`    | POST        | Запуск безопасного режима при аварийном событии.         |

### API негативных сценариев

Эти методы используются для проверки атак. В переработанной архитектуре атаки не должны проходить.

| Метод                              | Тип запроса |    НС | Назначение                                                   |
| ---------------------------------- | ----------- | ----: | ------------------------------------------------------------ |
| `/earth/send_command`              | POST        |  НС-1 | Проверка подмены команды через интерфейс связи с Землёй.     |
| `/ship/docking_request`            | POST        |  НС-2 | Проверка поддельного запроса на стыковку.                    |
| `/central/open_airlock`            | POST        |  НС-3 | Проверка попытки открыть шлюз без контроля давления.         |
| `/airlocks/operate`                | POST        |  НС-4 | Проверка попытки открыть обе створки шлюза одновременно.     |
| `/access/unlock`                   | POST        |  НС-5 | Проверка попытки дать доступ пользователю без прав.          |
| `/compartments/check`              | POST        |  НС-6 | Проверка ложных данных о давлении.                           |
| `/docking/approve`                 | POST        |  НС-7 | Проверка подтверждения стыковки при неисправном узле.        |
| `/central/tamper_command`          | POST        |  НС-8 | Проверка подмены команды закрытия на открытие.               |
| `/storage/tamper_airlock`          | POST        |  НС-9 | Проверка ложного статуса шлюза в базе состояний.             |
| `/log`                             | POST        | НС-10 | Проверка попытки не записать критическое событие.            |
| `/monitoring/power`                | POST        | НС-11 | Проверка скрытия отказа мониторингом.                        |
| `/terminal/unlock_sector`          | POST        | НС-12 | Проверка несанкционированной команды с терминала.            |
| `/emergency/safe_mode`             | POST        | НС-13 | Проверка отказа аварийного модуля включить безопасный режим. |
| `/earth/replay_command`            | POST        | НС-14 | Проверка повторной отправки старой команды.                  |
| `/ship/docking_request`            | POST        | НС-15 | Проверка искажения телеметрии корабля.                       |
| `/central/conflicting_commands`    | POST        | НС-16 | Проверка конфликтующих команд.                               |
| `/central/delayed_emergency_close` | POST        | НС-17 | Проверка задержки аварийной команды закрытия шлюза.          |

### API отдельных сервисов

Каждый сервис внутри `services/` имеет одинаковую базовую структуру API:

| Метод      | Тип запроса | Назначение                               |
| ---------- | ----------- | ---------------------------------------- |
| `/ready`   | GET         | Проверка готовности сервиса.             |
| `/info`    | GET         | Получение информации о сервисе.          |
| `/process` | POST        | Обработка входящего события или команды. |

Пример структуры отдельного сервиса:

```text
services/command_validator/
├── app.py
├── __init__.py
├── Dockerfile
├── requirements.txt
└── README.md
```

Пример типового API сервиса:

```python
@app.get("/ready")
def ready():
    return {"status": "ready"}

@app.get("/info")
def info():
    return {
        "service": "command-validator",
        "description": "Проверяет корректность команды перед передачей в ЦСУ"
    }

@app.post("/process")
def process(payload: dict):
    return {
        "service": "command-validator",
        "accepted": True,
        "payload": payload
    }
```

## Политики безопасности

Политики безопасности задают допустимые маршруты взаимодействия между элементами архитектуры. Это нужно для того, чтобы скомпрометированный или недоверенный компонент не мог напрямую обратиться к критическому сервису.

Например, интерфейс связи с Землёй не должен напрямую передавать команду в сервис управления шлюзами. Его команда сначала должна пройти через интерфейс фильтрации команд, проверку авторизации, проверку политик, валидатор команд и монитор безопасности команд.

В коде политики безопасности задаются как список разрешённых пар:

```text
источник → получатель
```

Если такой пары нет в списке, обращение блокируется.

### Список политик безопасности

```python
policies = (
    {"src": "terminal", "dst": "authorization"},
    {"src": "authorization", "dst": "crypto-encryptor"},
    {"src": "crypto-encryptor", "dst": "management-subsystem"},
    {"src": "management-subsystem", "dst": "local-operator-terminal"},

    {"src": "local-operator-terminal", "dst": "command-filter"},
    {"src": "earth-interface", "dst": "command-filter"},
    {"src": "ship-interface", "dst": "command-filter"},

    {"src": "command-filter", "dst": "authorization-check"},
    {"src": "authorization-check", "dst": "command-filter"},

    {"src": "command-filter", "dst": "security-policy-check"},
    {"src": "security-policy-check", "dst": "command-filter"},

    {"src": "command-filter", "dst": "command-validator"},
    {"src": "command-validator", "dst": "command-filter"},

    {"src": "command-filter", "dst": "command-security-monitor"},
    {"src": "command-security-monitor", "dst": "command-filter"},

    {"src": "command-filter", "dst": "central-control"},

    {"src": "central-control", "dst": "security-policies"},
    {"src": "security-policies", "dst": "central-control"},

    {"src": "central-control", "dst": "command-security-monitor"},
    {"src": "command-security-monitor", "dst": "central-control"},

    {"src": "central-control", "dst": "compartment-control-service"},
    {"src": "compartment-control-service", "dst": "metrics-control"},
    {"src": "metrics-control", "dst": "sensor-verifier"},
    {"src": "sensor-verifier", "dst": "telemetry-db"},
    {"src": "telemetry-db", "dst": "sensor-verifier"},
    {"src": "sensor-verifier", "dst": "metrics-control"},
    {"src": "metrics-control", "dst": "central-control"},

    {"src": "central-control", "dst": "access-control-subsystem"},
    {"src": "access-control-subsystem", "dst": "authentication-system"},
    {"src": "authentication-system", "dst": "access-control-subsystem"},
    {"src": "access-control-subsystem", "dst": "central-control"},

    {"src": "central-control", "dst": "airlock-control-service"},
    {"src": "airlock-control-service", "dst": "airlock-control"},
    {"src": "airlock-control", "dst": "drive-control"},
    {"src": "drive-control", "dst": "airlock-drives"},
    {"src": "airlock-drives", "dst": "drive-control"},
    {"src": "drive-control", "dst": "airlock-control"},
    {"src": "airlock-control", "dst": "airlock-control-service"},
    {"src": "airlock-control-service", "dst": "central-control"},

    {"src": "central-control", "dst": "docking-subsystem"},
    {"src": "docking-subsystem", "dst": "ship-telemetry-check"},
    {"src": "ship-telemetry-check", "dst": "docking-node-control"},
    {"src": "docking-node-control", "dst": "docking-confirmation"},
    {"src": "docking-confirmation", "dst": "docking-subsystem"},
    {"src": "docking-subsystem", "dst": "central-control"},

    {"src": "central-control", "dst": "state-db"},
    {"src": "state-db", "dst": "central-control"},

    {"src": "airlock-control-service", "dst": "state-db"},
    {"src": "docking-subsystem", "dst": "state-db"},

    {"src": "central-control", "dst": "logging-service"},
    {"src": "command-validator", "dst": "logging-service"},
    {"src": "security-policies", "dst": "logging-service"},
    {"src": "command-security-monitor", "dst": "logging-service"},
    {"src": "metrics-control", "dst": "logging-service"},
    {"src": "airlock-control", "dst": "logging-service"},
    {"src": "authentication-system", "dst": "logging-service"},
    {"src": "docking-confirmation", "dst": "logging-service"},
    {"src": "ship-telemetry-check", "dst": "logging-service"},

    {"src": "logging-service", "dst": "log-integrity-control"},
    {"src": "log-integrity-control", "dst": "state-db"},

    {"src": "service-monitor", "dst": "event-analyzer"},
    {"src": "event-analyzer", "dst": "emergency-module"},
    {"src": "event-analyzer", "dst": "emergency-response-module"},

    {"src": "emergency-module", "dst": "emergency-response-module"},
    {"src": "emergency-response-module", "dst": "safe-mode-transition"},
    {"src": "safe-mode-transition", "dst": "executive-devices"},
    {"src": "executive-devices", "dst": "airlock-drives"},
    {"src": "emergency-response-module", "dst": "logging-service"},
)
```

### Проверка политики

Проверка выполняется через функцию `check_operation`. Она получает идентификатор события и данные обращения. Из данных берутся два поля:

* `source` — кто отправляет команду;
* `deliver_to` — кому команда должна быть передана.

Если пара `source → deliver_to` есть в списке `policies`, обращение разрешается. Если пары нет, команда блокируется.

```python
def check_operation(id, details) -> bool:
    """Проверка возможности совершения обращения."""
    src: str = details.get("source")
    dst: str = details.get("deliver_to")

    if not all((src, dst)):
        return False

    print(f"[info] checking policies for event {id}, {src}->{dst}")

    return {"src": src, "dst": dst} in policies
```

### Пример работы политик

Разрешённый маршрут:

```python
{"src": "earth-interface", "dst": "command-filter"}
```

Это значит, что интерфейс связи с Землёй может передать команду только в интерфейс фильтрации команд.

Запрещённый маршрут:

```python
{"src": "earth-interface", "dst": "airlock-control-service"}
```

Такого маршрута в политиках нет. Значит, интерфейс связи с Землёй не может напрямую обратиться к сервису управления шлюзами.

Благодаря этому даже если интерфейс связи с Землёй будет скомпрометирован, он не сможет напрямую открыть шлюз. Его команда сначала должна пройти через защитный контур.

### Роль политик безопасности в негативных сценариях

Политики безопасности помогают нейтрализовать несколько негативных сценариев.

| НС    | Как помогают политики                                                                       |
| ----- | ------------------------------------------------------------------------------------------- |
| НС-1  | Не дают интерфейсу связи с Землёй напрямую передать изменённую команду в ЦСУ или к шлюзам.  |
| НС-3  | Не дают выполнить открытие шлюза без проверки состояния отсека.                             |
| НС-4  | Запрещают опасную операцию одновременного открытия створок.                                 |
| НС-8  | Блокируют подмену команды закрытия на открытие.                                             |
| НС-14 | Вместе с монитором безопасности команд не дают повторно выполнить старую команду.           |
| НС-16 | Не дают выполнить конфликтующие команды одновременно.                                       |
| НС-17 | Аварийная реакция может идти через отдельный маршрут, минуя задержку в обычной цепочке ЦСУ. |

Иными словами, политики безопасности не позволяют компонентам взаимодействовать произвольно. Каждый маршрут должен быть заранее разрешён архитектурой. Это снижает риск того, что скомпрометированный элемент сможет напрямую повлиять на критические исполнительные устройства.

## Цвета и обозначения доверия

В переработанной архитектуре используются три уровня доверия:

| Цвет    | Значение                                                                  |
| ------- | ------------------------------------------------------------------------- |
| Красный | Недоверенный или потенциально скомпрометированный элемент.                |
| Жёлтый  | Элемент, который повышает доверие к данным или команде.                   |
| Зелёный | Доверенный элемент, который принимает критически важное защитное решение. |

Также рядом с элементами используются буквенные обозначения. Первая буква показывает сложность средства ИБ, вторая — его размерность.

| Обозначение | Смысл                                       |
| ----------- | ------------------------------------------- |
| `SS`        | Простое и малое средство ИБ.                |
| `MM`        | Среднее по сложности и размеру средство ИБ. |
| `CXL`       | Более крупный и сложный элемент.            |

Эти обозначения нужны для качественной оценки того, насколько сложным является элемент безопасности и насколько удобно его анализировать.

## Тесты

В проекте используются два типа тестов.

Первый тип — e2e-тесты. Они показывают, что система работает в штатном режиме. Например, команда открытия шлюза проходит все проверки и выполняется только после подтверждения авторизации, политик, показателей датчиков и состояния шлюза.

Файл:

```text
tests/test_e2e_scenarios.py
```

Второй тип — тесты негативных сценариев. Они проверяют, может ли атака пройти через переработанную архитектуру.

Файл:

```text
tests/test_negative_scenarios.py
```

Логика тестов следующая:

```text
e2e-тест зелёный — система работает в штатном режиме;
НС-тест зелёный — атака прошла, система уязвима;
НС-тест красный — атака не прошла, система защитилась.
```

Для переработанной архитектуры ожидается, что e2e-тесты проходят, а тесты негативных сценариев не проходят. Это означает, что штатные функции системы сохраняются, но атаки не приводят к нарушению целей безопасности.

### Запуск проекта

Локальный запуск:

```bash
pip install -r requirements.txt
uvicorn app:app --reload
```

После запуска приложение доступно по адресу:

```text
http://localhost:8000
```

Запуск через Docker Compose:

```bash
docker compose up --build
```

Запуск e2e-тестов:

```bash
pytest -m e2e
```

Запуск тестов негативных сценариев:

```bash
pytest -m attack
```
