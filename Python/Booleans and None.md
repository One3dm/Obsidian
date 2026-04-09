Booleans и None

Тип bool
`bool` — это тип данных, который может принимать только два значения:
- `True` — истина
- `False` — ложь
```python
my_var = True
type(my_var)    # <class 'bool'>

my_var2 = False
type(my_var2)   # <class 'bool'>
```

> [!NOTE]
> `True` и `False` — это булевы значения, **не строки**. Пишутся с заглавной буквы.

Преобразование в bool:
```python
# Явное преобразование
bool(1)        # True
bool(0)        # False
bool("hello")  # True
bool("")       # False
bool([])       # False
bool([1, 2])   # True

# Неявное преобразование (в условиях)
if "some text":  # Автоматически bool("some text") → True
    print("Выполнится")
```

Логические операторы

| Оператор | Значение | Пример | Результат |
|----------|----------|--------|-----------|
| `and` | И (оба истинны) | `True and False` | `False` |
| `or` | ИЛИ (хотя бы один истинен) | `True or False` | `True` |
| `not` | НЕ (отрицание) | `not False` | `True` |

Таблицы истинности:

**`and` (И):**

| A | B | A and B |
|---|---|---------|
| True | True | True |
| True | False | False |
| False | True | False |
| False | False | False |

**`or` (ИЛИ):**

| A | B | A or B |
|---|---|--------|
| True | True | True |
| True | False | True |
| False | True | True |
| False | False | False |

**`not` (НЕ):**

| A | not A |
|---|-------|
| True | False |
| False | True |

Практические примеры для сетевых проверок:
```python
# Проверка состояния интерфейса
interface_up = True
line_protocol_up = False

# Интерфейс работает только если оба условия True
if interface_up and line_protocol_up:
    print("Интерфейс полностью работоспособен")
else:
    print("Проблемы с интерфейсом")

# Проверка доступности устройств
ping_successful = True
ssh_available = False
telnet_available = True

# Устройство доступно если хотя бы один протокол работает
if ping_successful and (ssh_available or telnet_available):
    print("Устройство доступно для управления")
else:
    print("Устройство недоступно")

# Проверка безопасности
port_open = True
firewall_blocked = False

if port_open and not firewall_blocked:
    print("Порт открыт и не заблокирован фаерволом")
```

### Приоритет операторов:

```python
# Порядок выполнения: not → and → or
result = True or False and not True
# 1. not True → False
# 2. False and False → False  
# 3. True or False → True

# Используйте скобки для ясности
result = (True or False) and (not True)  # False
```

---

## Truish (правдоподобность)

Python позволяет использовать **небулевы значения** в логическом контексте.

**Правило:** каждому типу данных соответствует одно "нулевое" значение, которое считается `False`, всё остальное — `True`.

| Тип | False-значение | True-значения |
|-----|----------------|---------------|
| `str` | `""` (пустая строка) | `"hello"`, `"0"`, `"False"` |
| `int` | `0` | `42`, `-5`, `1` |
| `list` | `[]` (пустой список) | `[1]`, `[None]` |
| `float` | `0.0` | `3.14`, `-0.5` |
| `dict` | `{}` | `{"key": "value"}` |
| `tuple` | `()` | `(1,)`, `(None,)` |
| `set` | `set()` | `{1}`, `{"a"}` |
| `None` | всегда `False` | — |
| `bytes` | `b""` | `b"data"` |

### Примеры:

```python
bool("some string")   # True
bool("")              # False

bool(22)              # True
bool(0)               # False

bool(["element"])     # True
bool([])              # False

bool(0.0)             # False
bool(0.1)             # True

bool({"ip": "8.8.8.8"})  # True
bool({})              # False
```

### Практическое использование в сетях:

```python
# Проверка конфигурации
config = "hostname Router1\ninterface GigabitEthernet0/1"

if config:  # True если конфигурация не пустая
    print("Конфигурация загружена")
else:
    print("Конфигурация пуста")

# Проверка списка устройств
devices = ["R1", "R2", "R3"]

if devices:  # True если список не пустой
    print(f"Есть {len(devices)} устройств для проверки")
else:
    print("Нет устройств для проверки")

# Проверка наличия IP-адреса
ip_address = ""

if not ip_address:  # True если строка пустая
    print("IP-адрес не задан")

# Проверка счётчиков
error_count = 0

if error_count:  # False если 0
    print(f"Найдено {error_count} ошибок")
else:
    print("Ошибок не найдено")
```

### Важные нюансы:

```python
# Строка "False" — это True!
bool("False")  # True (не пустая строка)
bool("0")      # True (не пустая строка)
bool(" ")      # True (пробел — это символ)

# Числа кроме 0 — True
bool(-1)       # True
bool(0.000001) # True

# Списки и словари с элементами — True
bool([None])   # True (None внутри, но список не пустой)
bool({None})   # True
```

---

## None

`None` — специальное значение, означающее **"ничего"**, **"пусто"**, **"отсутствие значения"**. Это синглтон (единственный экземпляр) типа `NoneType`.

```python
my_var = None
type(my_var)          # <class 'NoneType'>

bool(my_var)          # False (None всегда False)
```

### Где используется:

1. **Значение по умолчанию** для аргументов функций
2. **Возвращаемое значение** функций, которые ничего не возвращают
3. **Инициализация переменной**, когда значение пока неизвестно
4. **Отметка отсутствия данных**

### Примеры использования:

```python
# 1. Значение по умолчанию
def connect_to_device(hostname, port=None):
    if port is None:
        port = 22  # Значение по умолчанию
    print(f"Подключение к {hostname}:{port}")

# 2. Возвращаемое значение
def find_device_by_ip(ip_address, device_list):
    for device in device_list:
        if device["ip"] == ip_address:
            return device
    return None  # Устройство не найдено

# 3. Инициализация переменной
config_data = None  # Пока не загружена

# Позже...
config_data = load_configuration()

# 4. Отметка отсутствия
last_backup_time = None  # Бэкап ещё не делался
```

### Проверка на None:

```python
my_var = None

# ✅ ПРАВИЛЬНО - используйте 'is' или 'is not'
if my_var is None:
    print("Переменная пуста")

if my_var is not None:
    print("Переменная содержит значение")

# ❌ НЕ РЕКОМЕНДУЕТСЯ - хотя работает
if my_var == None:      # Избегайте
    print("Работает, но не идиоматично")

# ❌ АБСОЛЮТНО НЕВЕРНО
if not my_var:          # Это проверит на False, а не только на None!
    print("Сработает для None, но также для 0, '', [], и т.д.")
```

### Почему `is None`, а не `== None`?

```python
# is проверяет идентичность объектов (один и тот же объект в памяти)
# == проверяет равенство значений

a = None
b = None

a is b     # True (один и тот же синглтон)
a == b     # True (значения равны)

# Для пользовательских объектов разница важна
class CustomNone:
    def __eq__(self, other):
        return True  # Всегда равен чему угодно

custom = CustomNone()
custom == None  # True (по нашей реализации)
custom is None  # False (это не настоящий None)
```

### Практические примеры для сетей:

```python
# Обработка результата команды
command_output = execute_command("show version")

if command_output is None:
    print("Не удалось выполнить команду")
elif not command_output:  # Пустая строка
    print("Команда выполнена, но вывод пуст")
else:
    print(f"Вывод команды: {command_output[:100]}...")

# Проверка конфигурации
interface_config = get_interface_config("GigabitEthernet0/1")

if interface_config is None:
    print("Интерфейс не найден")
else:
    print(f"Конфигурация интерфейса: {interface_config}")

# Инициализация данных мониторинга
device_stats = {
    "cpu_usage": None,  # Ещё не измерено
    "memory_usage": None,
    "uptime": None
}

# Позже обновляем
device_stats["cpu_usage"] = get_cpu_usage()

if device_stats["cpu_usage"] is not None:
    print(f"Загрузка CPU: {device_stats['cpu_usage']}%")
```

---

## 📌 Итог

### Основные концепции:

| Концепция | Описание | Пример |
|-----------|----------|--------|
| `True` / `False` | Булевы значения | `status = True` |
| `and` | Логическое И | `if up and running:` |
| `or` | Логическое ИЛИ | `if http or https:` |
| `not` | Логическое НЕ | `if not blocked:` |
| Truish | Небулевы значения в условиях | `if device_list:` |
| `None` | Отсутствие значения | `result = None` |

### Правила Truish:

- **Пустая строка** `""` → `False`
- **Ноль** `0`, `0.0` → `False`
- **Пустые коллекции** `[]`, `{}`, `()`, `set()` → `False`
- **None** → `False`
- **Всё остальное** → `True`

### Важные правила для None:

1. **Используйте `is None` и `is not None`** для проверки
2. **Не используйте `not variable`** для проверки на None
3. **None — это синглтон**, всегда один и тот же объект
4. **None не то же самое, что False**, хотя bool(None) == False

### Практические советы для сетевого инженера:

1. **Используйте булевы флаги** для состояний устройств
2. **Проверяйте пустые коллекции** через `if collection:` 
3. **Инициализируйте переменные None**, когда значение неизвестно
4. **Всегда проверяйте на None** при работе с внешними данными
5. **Используйте `and`/`or`** для комплексных проверок доступности

### Быстрые проверки:

```python
# Проверка наличия данных
if output:          # True если не пустая строка
    process_output(output)

# Проверка ошибок
if error_count:     # True если > 0
    send_alert()

# Проверка на None
if result is None:  # Только для None
    log_error("Нет результата")

# Комплексная проверка доступности
if (ping_successful and 
    (ssh_available or telnet_available) and 
    not maintenance_mode):
    perform_backup()
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   