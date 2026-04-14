Булевы значения (bool)
```python
is_up = True
is_down = False

print(type(is_up))  # <class 'bool'>
```

Логические операторы
### `and` (И)
```python
# Оба должны быть True
interface_up = True
protocol_up = True

if interface_up and protocol_up:
    print("Интерфейс работает")
```

### `or` (ИЛИ)
```python
# Хотя бы один True
ssh_available = False
telnet_available = True

if ssh_available or telnet_available:
    print("Доступно управление")
```

### `not` (НЕ)
```python
# Отрицание
blocked = False

if not blocked:
    print("Трафик разрешён")
```

Truthy/Falsy значения
Python автоматически преобразует значения в bool в условиях:
```python
# Falsy (становятся False)
bool("")       # пустая строка
bool(0)        # ноль
bool([])       # пустой список
bool({})       # пустой словарь
bool(None)     # None

# Truthy (становятся True)
bool("text")   # непустая строка
bool(1)        # любое ненулевое число
bool([1])      # непустой список
bool({"a": 1}) # непустой словарь
```

**Практическое использование:**
```python
# Проверка наличия данных
config = load_config()
if config:  # True если config не пустой
    apply_config(config)

# Проверка списка устройств
devices = get_devices()
if not devices:  # True если список пустой
    print("Нет устройств для проверки")
```

None
`None` означает "ничего" или "отсутствие значения":
```python
result = None  # значение пока неизвестно

if result is None:
    print("Результат ещё не получен")
```

**Важно:** Используйте `is` для сравнения с None:
```python
# ✅ Правильно
if value is None:
    print("Нет значения")

# ❌ Избегайте
if value == None:
    print("Не идиоматично")
```


Практические примеры

Проверка состояния сети
```python
def check_device_status(device):
    """Проверка доступности устройства"""
    ping_ok = ping_device(device.ip)
    ssh_ok = check_ssh(device.ip)
    
    # Устройство доступно если пинг проходит И доступен SSH или Telnet
    return ping_ok and (ssh_ok or device.telnet_enabled)

# Использование
if check_device_status(router1):
    print("Можно выполнять конфигурацию")
```

Обработка результатов
```python
def get_device_config(device_name):
    """Получение конфигурации устройства"""
    config = query_device(device_name)
    
    if config is None:
        print(f"Не удалось получить конфигурацию {device_name}")
        return ""
    
    if not config:  # пустая строка
        print(f"Конфигурация {device_name} пуста")
        return ""
    
    return config

# Использование
config = get_device_config("Router1")
if config:
    save_config(config)
```

Инициализация переменных
```python
# Инициализация None
backup_result = None
last_check_time = None

# Позже обновляем
backup_result = perform_backup()

if backup_result is not None:
    print(f"Бэкап выполнен: {backup_result}")
```

📌 Итог

**Основные правила:**
1. `True`/`False` пишутся с заглавной буквы
2. Используйте `and`/`or`/`not` для логических операций
3. Непустые значения — Truthy, пустые — Falsy
4. Для проверки на None используйте `is None` или `is not None`

**Советы для сетевого инженера:**
- Используйте булевы флаги для состояний устройств
- Проверяйте `if data:` для наличия данных
- Инициализируйте переменные `None` при неизвестном значении
- Комбинируйте проверки с `and`/`or` для комплексных условий

```python
# ✅ Хороший шаблон
result = execute_command()

if result is None:
    print("Ошибка выполнения")
elif not result:  # пустой результат
    print("Команда выполнена, но вывод пуст")
else:
    process_result(result)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   