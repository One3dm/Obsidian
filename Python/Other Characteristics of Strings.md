Другие характеристики строк

## Проверка вхождения (Membership)

```python
# Оператор 'in'
output = "GigabitEthernet0/1 is up, line protocol is up"

if "is up" in output:
    print("Интерфейс активен")

# Оператор 'not in'
config = "switchport mode access"

if "trunk" not in config:
    print("Интерфейс не в режиме trunk")
```

---

## Raw-строки (сырые строки)

Используйте префикс `r` для отключения обработки специальных символов:

```python
# ❌ Проблема с обратными слэшами
path = "C:\network\new\config.txt"  # \n интерпретируется как новая строка

# ✅ Используйте raw-строку
path = r"C:\network\new\config.txt"  # всё как есть

# Практическое использование
windows_path = r"C:\Users\Admin\Documents"
regex_pattern = r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"  # для IP-адресов
cisco_command = r"show running-config | include ^interface"
```

---

## Конкатенация строк (объединение)

### Оператор `+`
```python
hostname = "Router"
location = "DC-1"
full_name = hostname + "-" + location  # "Router-DC-1"
```

### Оператор `+=`
```python
config = ""
config += "hostname R1\n"
config += "!\n"
config += "interface GigabitEthernet0/1\n"
config += " description Uplink\n"
```

### Метод `.join()` (эффективнее для многих строк)
```python
# ✅ Эффективно для большого количества строк
lines = []
for i in range(100):
    lines.append(f"interface GigabitEthernet0/{i}\n")
config = "".join(lines)
```

---

## Строки как последовательности

### Доступ по индексу
```python
interface = "GigabitEthernet0/1"
first_char = interface[0]    # 'G'
last_char = interface[-1]    # '1'
```

### Срезы (slicing)
```python
text = "GigabitEthernet0/1"

text[0:7]      # 'Gigabit'
text[7:]       # 'Ethernet0/1'
text[:7]       # 'Gigabit'
text[::2]      # 'GgbiEhnr0/' (каждый второй символ)
text[::-1]     # '1/0tenrehtEtabigiG' (реверс)
```

### Длина строки
```python
command = "show running-config"
length = len(command)  # 19
```

### Перебор символов
```python
for char in "Router":
    print(char)  # R, o, u, t, e, r
```

---

## Неизменяемость строк

Строки в Python **нельзя изменить** после создания:

```python
text = "router"
# text[0] = "R"  # ❌ Ошибка!

# ✅ Создаём новую строку
text = "R" + text[1:]  # "Router"
```

---

## Практические примеры

### Построение конфигурации
```python
def build_interface_config(interface, description, vlan):
    """Создаёт конфигурацию интерфейса"""
    config = f"interface {interface}\n"
    config += f" description {description}\n"
    config += f" switchport access vlan {vlan}\n"
    config += " switchport mode access\n"
    config += "!\n"
    return config

# Использование
config = build_interface_config("GigabitEthernet0/1", "User PC", 10)
print(config)
```

### Анализ вывода команд
```python
def analyze_interface_status(output):
    """Анализирует статус интерфейса из вывода команды"""
    if "is up" in output and "line protocol is up" in output:
        return "up/up"
    elif "is up" in output and "line protocol is down" in output:
        return "up/down"
    elif "is administratively down" in output:
        return "admin down"
    else:
        return "unknown"

# Использование
status = analyze_interface_status(show_interface_output)
print(f"Статус: {status}")
```

### Извлечение данных
```python
def extract_ip_address(text):
    """Извлекает IP-адрес из текста"""
    # Простой поиск паттерна
    words = text.split()
    for word in words:
        if "." in word and word.count(".") == 3:
            # Проверка, что это IP-адрес
            parts = word.split(".")
            if len(parts) == 4 and all(p.isdigit() for p in parts):
                return word
    return None

# Использование
text = "Device IP: 192.168.1.1, Gateway: 192.168.1.254"
ip = extract_ip_address(text)
print(f"Найден IP: {ip}")
```

---

## 📌 Итог

**Основные правила:**
1. Используйте `in`/`not in` для поиска подстрок
2. Используйте raw-строки (`r""`) для путей и регулярных выражений
3. Для объединения многих строк используйте `.join()`
4. Используйте срезы для извлечения частей строк
5. Помните: строки неизменяемы

**Советы для сетевого инженера:**
- Используйте `in` для анализа вывода команд
- Используйте raw-строки для команд Cisco с обратными слэшами
- Собирайте конфигурации с помощью `+=` или `.join()`
- Используйте срезы для парсинга имен интерфейсов, IP-адресов

```python
# ✅ Хороший шаблон для построения конфигурации
def build_config(hostname, interfaces):
    """Строит конфигурацию устройства"""
    config_lines = []
    
    config_lines.append(f"hostname {hostname}\n!\n")
    
    for interface, settings in interfaces.items():
        config_lines.append(f"interface {interface}\n")
        config_lines.append(f" description {settings['description']}\n")
        config_lines.append(f" ip address {settings['ip']} {settings['mask']}\n")
        config_lines.append("!\n")
    
    return "".join(config_lines)

# Использование
interfaces = {
    "GigabitEthernet0/0": {
        "description": "Uplink to Core",
        "ip": "10.0.0.1",
        "mask": "255.255.255.252"
    }
}

config = build_config("Router1", interfaces)
print(config)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   