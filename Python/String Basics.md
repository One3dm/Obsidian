Основы строк в Python

Создание строк

```python
# Одинарные кавычки
single = 'Hello World'

# Двойные кавычки
double = "Hello World"

# Тройные кавычки (многострочные)
multiline = """Это строка,
которая занимает
несколько строк"""
```

Экранирование символов
```python
# Обратный слэш для специальных символов
path = "C:\\Users\\Admin\\Documents"  # двойной слэш
quote = "He said, \"Hello!\""         # кавычки внутри
newline = "First line\nSecond line"   # перевод строки

# Сырые строки (raw strings) - отключают экранирование
raw_path = r"C:\Users\Admin\Documents"  # без двойных слэшей
```

Конкатенация строк
```python
# Оператор +
greeting = "Hello" + " " + "World"  # "Hello World"

# Оператор * (повторение)
separator = "-" * 20  # "--------------------"
```

f-строки (рекомендуемый способ)
```python
# Подстановка переменных
hostname = "Router1"
ip = "192.168.1.1"
info = f"Device: {hostname}, IP: {ip}"  # "Device: Router1, IP: 192.168.1.1"

# Выражения внутри f-строк
uptime = 365
status = f"Uptime: {uptime} days ({uptime/365:.1f} years)"  # "Uptime: 365 days (1.0 years)"

# Для сетевых задач
network = f"{ip}/24"  # "192.168.1.1/24"
```

Базовые методы строк
```python
text = "  router-sw01  "

# Удаление пробелов
clean = text.strip()    # "router-sw01"
left_clean = text.lstrip()  # "router-sw01  "
right_clean = text.rstrip() # "  router-sw01"

# Изменение регистра
"router".upper()      # "ROUTER"
"ROUTER".lower()      # "router"
"router name".title() # "Router Name"

# Проверки
"192.168.1.1".startswith("192")  # True
"config.txt".endswith(".txt")    # True
```

Практические примеры

Формирование команд
```python
def generate_interface_command(interface, vlan):
    """Генерирует команду для настройки интерфейса"""
    return f"interface {interface}\nswitchport access vlan {vlan}"

# Использование
command = generate_interface_command("GigabitEthernet0/1", 100)
print(command)
```

Обработка вывода
```python
def clean_device_output(output):
    """Очищает вывод с сетевого устройства"""
    # Убираем лишние пробелы по краям
    cleaned = output.strip()
    
    # Заменяем множественные пробелы на одинарные
    while "  " in cleaned:
        cleaned = cleaned.replace("  ", " ")
    
    return cleaned

# Использование
raw_output = "  GigabitEthernet0/1   is up,   line protocol is up  "
clean_output = clean_device_output(raw_output)
print(clean_output)  # "GigabitEthernet0/1 is up, line protocol is up"
```

Создание конфигурации
```python
def create_banner(message):
    """Создаёт баннер для сетевого устройства"""
    return f"""
!
banner motd #
{message}
#
!
"""

# Использование
banner = create_banner("Unauthorized access prohibited")
print(banner)
```

📌 Итог

**Основные правила:**
1. Используйте одинарные или двойные кавычки для простых строк
2. Используйте тройные кавычки для многострочного текста
3. Всегда используйте f-строки для подстановки переменных (Python 3.6+)
4. Используйте сырые строки (`r""`) для путей и регулярных выражений
5. Помните: строки неизменяемы — методы возвращают новые строки

**Советы для сетевого инженера:**
- Используйте f-строки для генерации команд и конфигураций
- Применяйте `.strip()` для очистки вывода с оборудования
- Используйте тройные кавычки для многострочных конфигураций
- Проверяйте форматы с помощью `.startswith()`/`.endswith()`

```python
# ✅ Хороший шаблон
def format_device_info(device):
    """Форматирует информацию об устройстве"""
    return f"""
Device Information
==================
Name: {device['name']}
IP: {device['ip']}
Model: {device['model']}
Uptime: {device['uptime']} days
"""

# Использование
device = {
    "name": "Router1",
    "ip": "192.168.1.1",
    "model": "Cisco ISR 4331",
    "uptime": 365
}

print(format_device_info(device))
```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   