Вставка переменных в строки

3 способа форматирования строк

1. Старый стиль (`%`) — не рекомендуется
```python
# Одна переменная
"Device: %s" % "Router1"  # 'Device: Router1'

# Несколько переменных
"IP: %s, Mask: %s" % ("192.168.1.1", "255.255.255.0")
```

2. Метод `.format()` — устаревший
```python
"Device: {}, IP: {}".format("Router1", "192.168.1.1")
"Device: {name}, IP: {ip}".format(name="Router1", ip="192.168.1.1")
```

3. f-строки — **рекомендуемый способ** ✅
```python
name = "Router1"
ip = "192.168.1.1"
f"Device: {name}, IP: {ip}"  # 'Device: Router1, IP: 192.168.1.1'
```

Возможности f-строк

Выражения внутри фигурных скобок
```python
# Математические операции
bandwidth = 1000
f"Bandwidth: {bandwidth * 8} Mbps"  # 'Bandwidth: 8000 Mbps'

# Вызов методов
interface = "GigabitEthernet0/1"
f"Interface: {interface.upper()}"  # 'Interface: GIGABITETHERNET0/1'

# Условные выражения
status = True
f"Status: {'UP' if status else 'DOWN'}"  # 'Status: UP'

# Форматирование чисел
uptime = 365.256
f"Uptime: {uptime:.2f} days"  # 'Uptime: 365.26 days'
f"CPU Usage: {25.5:.1f}%"     # 'CPU Usage: 25.5%'
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

Отладка с f-строками (Python 3.8+)
```python
device = "Router1"
port = 22
print(f"{device=} {port=}")  # "device='Router1' port=22"
```

Практические примеры

Генерация конфигурации
```python
def generate_interface_config(interface, ip, mask, description):
    """Генерирует конфигурацию интерфейса"""
    return f"""
interface {interface}
 description {description}
 ip address {ip} {mask}
 no shutdown
!
"""

# Использование
config = generate_interface_config(
    "GigabitEthernet0/1",
    "10.0.0.1",
    "255.255.255.0",
    "Uplink to Core"
)
print(config)
```

Создание отчётов
```python
def create_availability_report(devices):
    """Создаёт отчёт о доступности устройств"""
    total = len(devices)
    online = sum(1 for d in devices if d["status"] == "up")
    
    return f"""
Network Availability Report
===========================
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

report = create_availability_report(devices)
print(report)
```

Формирование команд
```python
def create_show_command(device_type, command, filter_option=None):
    """Создаёт команду для оборудования"""
    if device_type == "cisco":
        if filter_option:
            return f"show {command} | include {filter_option}"
        else:
            return f"show {command}"
    elif device_type == "juniper":
        return f"show {command}"
    else:
        return f"display {command}"

# Использование
cmd = create_show_command("cisco", "running-config", "^interface")
print(f"Command: {cmd}")  # 'show running-config | include ^interface'
```

📌 Итог
**Основные правила:**
1. **Всегда используйте f-строки** для Python 3.6+
2. Помещайте выражения в `{}`: вычисления, вызовы методов, условия
3. Используйте форматирование чисел: `{value:.2f}`
4. Для отладки используйте `{variable=}` (Python 3.8+)

**Советы для сетевого инженера:**
- Используйте f-строки для генерации конфигураций
- Применяйте для формирования команд CLI
- Используйте для создания отчётов и логов
- Помните про многострочные f-строки для больших блоков

```python
# ✅ Хороший шаблон
def generate_device_config(hostname, interfaces):
    """Генерирует конфигурацию устройства"""
    config_lines = [f"hostname {hostname}\n!\n"]
    
    for intf_name, intf_config in interfaces.items():
        config_lines.append(f"interface {intf_name}\n")
        config_lines.append(f" description {intf_config['description']}\n")
        config_lines.append(f" ip address {intf_config['ip']} {intf_config['mask']}\n")
        config_lines.append("!\n")
    
    return "".join(config_lines)

# Использование
interfaces = {
    "GigabitEthernet0/0": {
        "description": "Uplink",
        "ip": "10.0.0.1",
        "mask": "255.255.255.252"
    }
}

config = generate_device_config("Router1", interfaces)
print(config)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   