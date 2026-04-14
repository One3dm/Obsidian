f-строки в Python

Основы f-строк
```python
# Без f - фигурные скобки как текст
name = "Router1"
text = "Device: {name}"
print(text)  # Device: {name}

# С f - подстановка значений
name = "Router1"
text = f"Device: {name}"
print(text)  # Device: Router1
```

**Золотое правило:** Если в строке есть `{}` и нужно подставить значения — **всегда начинайте с `f`**.

Выражения внутри f-строк
```python
ip_addr = "10.220.89.17"

# Простые переменные
print(f"IP: {ip_addr}")  # IP: 10.220.89.17

# Математические выражения
print(f"2 + 17 = {2 + 17}")  # 2 + 17 = 19

# Вызов методов
print(f"First octet: {ip_addr.split('.')[0]}")  # First octet: 10

# Условные выражения
status = True
print(f"Status: {'UP' if status else 'DOWN'}")  # Status: UP
```

Форматирование вывода

Выравнивание текста
```python
ip1 = "10.220.89.17"
ip2 = "192.168.10.1"

# Ширина 20 символов
print(f"{ip1:20}{ip2:20}")      # выравнивание влево
print(f"{ip1:>20}{ip2:>20}")    # выравнивание вправо
print(f"{ip1:^20}{ip2:^20}")    # выравнивание по центру
```

Форматирование чисел
```python
# Дробные числа
value = 1/3
print(f"Value: {value:.2f}")  # Value: 0.33

# Проценты
packet_loss = 0.034567
print(f"Packet loss: {packet_loss:.2%}")  # Packet loss: 3.46%

# Целые числа
count = 42
print(f"Count: {count:04d}")  # Count: 0042 (дополнение нулями)
```


Полезные трюки

Отладка (Python 3.8+)
```python
device = "Router1"
ip = "192.168.1.1"
print(f"{device = }, {ip = }")  # device = 'Router1', ip = '192.168.1.1'
```

Многострочные f-строки
```python
hostname = "SW-01"
ip = "192.168.1.1"
mask = "255.255.255.0"

config = f"""
hostname {hostname}
!
interface Vlan1
 ip address {ip} {mask}
!
"""
```

Кавычки в f-строках
```python
# ✅ Правильно
my_dict = {"ip": "8.8.8.8"}
print(f"IP: {my_dict['ip']}")    # одинарные внутри двойных
print(f'IP: {my_dict["ip"]}')    # двойные внутри одинарных
```

Практические примеры

Формирование таблицы
```python
def print_device_table(devices):
    """Выводит таблицу устройств"""
    print(f"{'Device':<15}{'IP':<20}{'Status':<10}")
    print(f"{'-'*15:<15}{'-'*20:<20}{'-'*10:<10}")
    
    for device in devices:
        print(f"{device['name']:<15}{device['ip']:<20}{device['status']:<10}")

# Использование
devices = [
    {"name": "Router1", "ip": "192.168.1.1", "status": "UP"},
    {"name": "Switch1", "ip": "10.0.0.1", "status": "DOWN"}
]

print_device_table(devices)
```

Генерация конфигурации
```python
def generate_vlan_config(vlan_id, vlan_name, interfaces):
    """Генерирует конфигурацию VLAN"""
    config = f"vlan {vlan_id}\n"
    config += f" name {vlan_name}\n"
    config += "!\n"
    
    for interface in interfaces:
        config += f"interface {interface}\n"
        config += f" switchport access vlan {vlan_id}\n"
        config += "!\n"
    
    return config

# Использование
config = generate_vlan_config(
    100,
    "Management",
    ["GigabitEthernet0/1", "GigabitEthernet0/2"]
)
print(config)
```

Создание отчётов
```python
def create_network_report(devices):
    """Создаёт отчёт о состоянии сети"""
    total = len(devices)
    online = sum(1 for d in devices if d["status"] == "up")
    
    return f"""
Network Status Report
=====================
Total devices: {total}
Online: {online}
Offline: {total - online}
Availability: {(online/total)*100:.1f}%
"""

# Использование
devices = [
    {"name": "R1", "status": "up"},
    {"name": "R2", "status": "down"},
    {"name": "R3", "status": "up"}
]

report = create_network_report(devices)
print(report)
```


📌 Итог
**Основные правила:**
1. Всегда используйте f-строки для Python 3.6+
2. Не забывайте букву `f` перед кавычками
3. Используйте выражения внутри `{}`
4. Применяйте форматирование для красивого вывода
5. Для отладки используйте `{var = }` (Python 3.8+)

**Советы для сетевого инженера:**
- Используйте f-строки для генерации конфигураций
- Применяйте форматирование для табличного вывода
- Используйте многострочные f-строки для больших блоков
- Помните про кавычки: двойные снаружи — одинарные внутри

```python
# ✅ Хороший шаблон
def format_device_info(device):
    """Форматирует информацию об устройстве"""
    return f"""
Device: {device['name']}
IP: {device['ip']}
Status: {device['status']}
Uptime: {device['uptime']:.1f} days
CPU: {device['cpu_usage']:.1f}%
Memory: {device['memory_usage']:.1f}%
"""

# Использование
device_info = {
    "name": "Router1",
    "ip": "192.168.1.1",
    "status": "UP",
    "uptime": 365.25,
    "cpu_usage": 25.5,
    "memory_usage": 60.3
}

print(format_device_info(device_info))
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   