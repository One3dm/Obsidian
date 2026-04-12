**Запись в файл**

Открытие файла для записи
```python
f = open("test_file.txt", "w")
```

Параметр `"w"` означает **write** (запись).

Режимы записи:

| Режим | Описание | Поведение | Когда использовать |
|-------|----------|-----------|-------------------|
| `"w"` | Запись (write) | Создаёт новый файл или **перезаписывает** существующий | Сохранение новых данных, перезапись логов |
| `"a"` | Добавление (append) | Добавляет данные в **конец** существующего файла | Логирование, добавление записей |
| `"x"` | Эксклюзивное создание | Создаёт файл, **ошибка** если уже существует | Гарантия создания нового файла |
| `"w+"` | Чтение и запись | Перезаписывает файл, позволяет читать и писать | Редактирование файлов на месте |

```python
# Примеры разных режимов
f_write = open("output.txt", "w")      # перезапись
f_append = open("log.txt", "a")        # добавление в конец
f_exclusive = open("new_config.txt", "x")  # только если не существует
f_rw = open("data.txt", "w+")          # чтение и запись
```

Метод `.write()`
```python
f = open("test_file.txt", "w")
bytes_written = f.write("Testing...\n")
print(f"Записано {bytes_written} байт")  # 11
f.close()
```

Особенности `.write()`:

1. **Возвращает количество записанных байт** (символов)
2. **Не добавляет автоматически `\n`** — нужно указывать явно
3. **Принимает только строки** (str) — для других типов нужно преобразование
4. **Можно вызывать несколько раз** — данные дописываются

```python
with open("output.txt", "w") as f:
    # Несколько вызовов write
    f.write("Первая строка\n")
    f.write("Вторая строка\n")
    f.write("Третья строка\n")
    
    # Запись чисел требует преобразования
    counter = 42
    f.write(f"Счётчик: {counter}\n")  # f-строка
    f.write("Счётчик: " + str(counter) + "\n")  # конкатенация
    
# Файл содержит:
# Первая строка
# Вторая строка
# Третья строка
# Счётчик: 42
# Счётчик: 42
```

Практический пример: запись конфигурации
```python
def save_router_config(hostname, interfaces, filename="router_config.txt"):
    """Сохранение конфигурации маршрутизатора в файл"""
    with open(filename, "w") as f:
        # Заголовок конфигурации
        f.write(f"! Конфигурация устройства {hostname}\n")
        f.write("!\n")
        
        # Базовые настройки
        f.write(f"hostname {hostname}\n")
        f.write("!\n")
        
        # Настройки интерфейсов
        f.write("! Настройки интерфейсов\n")
        for interface, config in interfaces.items():
            f.write(f"interface {interface}\n")
            for command in config:
                f.write(f"  {command}\n")
            f.write("!\n")
        
        f.write("! Конец конфигурации\n")
    
    print(f"Конфигурация сохранена в {filename}")

# Использование
interfaces = {
    "GigabitEthernet0/0": ["ip address 192.168.1.1 255.255.255.0", "no shutdown"],
    "GigabitEthernet0/1": ["ip address 10.0.0.1 255.255.255.0", "description Uplink", "no shutdown"]
}

save_router_config("Router1", interfaces)
```

Данные не записываются сразу (буферизация)

После вызова `.write()` данные могут **не появиться на диске** немедленно. Python использует **буферизацию** для повышения производительности.

Как работает буферизация:
```python
with open("log.txt", "w") as f:
    f.write("Сообщение 1\n")  # Попадает в буфер в памяти
    f.write("Сообщение 2\n")  # Тоже в буфер
    # Данные ещё НЕ на диске!
    
    f.flush()  # Теперь данные записываются на диск
    f.write("Сообщение 3\n")  # Снова в буфер
    
# При выходе из with файл закрывается и буфер сбрасывается автоматически
```

Принудительная запись на диск:

| Метод | Описание | Когда использовать |
|-------|----------|-------------------|
| `f.flush()` | Немедленно записывает буфер на диск | Критически важные данные, отладка |
| `f.close()` | Закрывает файл и **автоматически** сбрасывает буфер | Завершение работы с файлом |
| Открытие с `buffering=0` | Отключает буферизацию | Режим реального времени |

```python
# Пример с flush для логов
log_file = open("critical.log", "w")

try:
    # Важная операция
    log_file.write(f"Начало операции в {datetime.now()}\n")
    log_file.flush()  # Немедленно на диск
    
    perform_critical_operation()
    
    log_file.write(f"Операция успешна в {datetime.now()}\n")
    log_file.flush()  # Снова немедленно
    
except Exception as e:
    log_file.write(f"Ошибка: {e}\n")
    log_file.flush()  # Важно записать ошибку сразу
finally:
    log_file.close()

# Отключение буферизации (не рекомендуется для больших файлов)
with open("realtime.log", "w", buffering=0) as f:
    f.write("Каждое сообщение сразу на диск\n")  # Нет буфера
```

Запись разрушает содержимое файла

**Важное предупреждение:** Режим `"w"` **перезаписывает** файл с нуля!
```python
# Демонстрация перезаписи
with open("example.txt", "w") as f:
    f.write("Первая версия файла\n")

# Файл содержит: "Первая версия файла"

with open("example.txt", "w") as f:
    f.write("Вторая версия файла\n")

# Теперь файл содержит ТОЛЬКО: "Вторая версия файла"
# Первая версия БЕЗВОЗВРАТНО УТЕРЯНА!
```

Как избежать случайной перезаписи:
```python
import os

def safe_write(filename, content, backup=True):
    """Безопасная запись с созданием бэкапа"""
    
    # Проверяем, существует ли файл
    if os.path.exists(filename):
        print(f"Внимание: файл {filename} уже существует")
        
        if backup:
            # Создаём бэкап
            backup_name = f"{filename}.backup"
            import shutil
            shutil.copy2(filename, backup_name)
            print(f"Создан бэкап: {backup_name}")
    
    # Записываем новые данные
    with open(filename, "w") as f:
        f.write(content)
    
    print(f"Файл {filename} успешно сохранён")

# Использование
config = "hostname Router1\ninterface GigabitEthernet0/1\n no shutdown"
safe_write("router.cfg", config, backup=True)
```

 Метод `.writelines()` для записи списка строк
```python
# .writelines() принимает список строк
lines = [
    "Первая строка\n",
    "Вторая строка\n", 
    "Третья строка\n"
]

with open("output.txt", "w") as f:
    f.writelines(lines)  # Записывает все строки сразу

# Эквивалентно:
with open("output.txt", "w") as f:
    for line in lines:
        f.write(line)
```

Практическое использование `.writelines()`:
```python
def save_device_list(devices, filename="devices.txt"):
    """Сохранение списка устройств в файл"""
    
    # Подготовка строк с добавлением \n
    lines = [f"{device['name']},{device['ip']},{device['type']}\n" 
             for device in devices]
    
    with open(filename, "w") as f:
        f.writelines(lines)
    
    print(f"Сохранено {len(devices)} устройств в {filename}")

# Пример с обработкой вывода команды
def save_arp_table(arp_output, filename="arp_table.txt"):
    """Сохранение ARP-таблицы"""
    with open(filename, "w") as f:
        # Заголовок
        f.write("ARP Table\n")
        f.write("=" * 50 + "\n")
        
        # Данные
        f.writelines(arp_output)  # arp_output должен быть списком строк
        
        # Итог
        f.write(f"\nTotal entries: {len(arp_output)}\n")
```

Запись в несколько файлов одновременно
```python
# Запись в несколько файлов
with open("input.txt") as src, \
     open("output.txt", "w") as dst1, \
     open("backup.txt", "w") as dst2:
    
    for line in src:
        # Записываем в оба выходных файла
        dst1.write(line.upper())  # Верхний регистр
        dst2.write(line)          # Оригинал

# Разделение вывода по условиям
with open("network_log.txt") as log, \
     open("errors.txt", "w") as errors, \
     open("warnings.txt", "w") as warnings, \
     open("info.txt", "w") as info:
    
    for line in log:
        if "ERROR" in line:
            errors.write(line)
        elif "WARNING" in line:
            warnings.write(line)
        else:
            info.write(line)
```

Практические примеры для сетевой автоматизации

Пример 1: Логирование работы скрипта
```python
import datetime

class NetworkLogger:
    def __init__(self, log_file="network_operations.log"):
        self.log_file = log_file
        self.setup_logging()
    
    def setup_logging(self):
        """Настройка логгирования"""
        # Добавляем заголовок при первом запуске
        if not os.path.exists(self.log_file):
            with open(self.log_file, "w") as f:
                f.write("=" * 60 + "\n")
                f.write("Network Operations Log\n")
                f.write("=" * 60 + "\n\n")
    
    def log(self, message, level="INFO"):
        """Запись сообщения в лог"""
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        log_entry = f"[{timestamp}] [{level}] {message}\n"
        
        # Используем режим 'a' для добавления в конец
        with open(self.log_file, "a") as f:
            f.write(log_entry)
        
        # Для критических сообщений сразу сбрасываем на диск
        if level in ["ERROR", "CRITICAL"]:
            f.flush()
        
        # Также выводим в консоль
        print(log_entry.strip())

# Использование
logger = NetworkLogger()
logger.log("Начало конфигурации устройства", "INFO")
logger.log("Успешно подключились к Router1", "SUCCESS")
logger.log("Ошибка подключения к Switch1", "ERROR")
```

Пример 2: Экспорт данных в CSV
```python
def export_devices_to_csv(devices, filename="devices.csv"):
    """Экспорт списка устройств в CSV файл"""
    
    with open(filename, "w") as f:
        # Заголовок CSV
        f.write("Hostname,IP Address,Model,Status,Uptime\n")
        
        # Данные
        for device in devices:
            # Экранирование запятых в данных
            hostname = device['hostname'].replace(',', ';')
            ip = device['ip']
            model = device.get('model', 'Unknown').replace(',', ';')
            status = device.get('status', 'unknown')
            uptime = device.get('uptime', 'N/A')
            
            f.write(f"{hostname},{ip},{model},{status},{uptime}\n")
    
    print(f"Экспортировано {len(devices)} устройств в {filename}")

# Использование
devices = [
    {"hostname": "Router1", "ip": "192.168.1.1", "model": "Cisco 2911", "status": "up", "uptime": "30 days"},
    {"hostname": "Switch1", "ip": "192.168.1.2", "model": "Cisco 2960", "status": "up", "uptime": "15 days"},
    {"hostname": "Firewall1", "ip": "192.168.1.3", "model": "ASA 5506", "status": "down", "uptime": "0 days"}
]

export_devices_to_csv(devices)
```

Пример 3: Создание конфигурационных файлов для оборудования
```python
def generate_switch_config(hostname, vlans, interfaces, filename=None):
    """Генерация конфигурации коммутатора"""
    
    if filename is None:
        filename = f"{hostname}_config.txt"
    
    config_lines = []
    
    # Базовые настройки
    config_lines.append(f"hostname {hostname}\n")
    config_lines.append("!\n")
    
    # VLAN
    config_lines.append("! VLAN Configuration\n")
    for vlan_id, vlan_name in vlans.items():
        config_lines.append(f"vlan {vlan_id}\n")
        config_lines.append(f" name {vlan_name}\n")
        config_lines.append("!\n")
    
    # Интерфейсы
    config_lines.append("! Interface Configuration\n")
    for interface, settings in interfaces.items():
        config_lines.append(f"interface {interface}\n")
        
        if settings.get('mode') == 'trunk':
            config_lines.append(" switchport mode trunk\n")
            allowed_vlans = settings.get('allowed_vlans', '1-4094')
            config_lines.append(f" switchport trunk allowed vlan {allowed_vlans}\n")
        else:
            config_lines.append(" switchport mode access\n")
            vlan = settings.get('vlan', 1)
            config_lines.append(f" switchport access vlan {vlan}\n")
        
        if 'description' in settings:
            config_lines.append(f" description {settings['description']}\n")
        
        config_lines.append(" no shutdown\n")
        config_lines.append("!\n")
    
    # Запись в файл
    with open(filename, "w") as f:
        f.writelines(config_lines)
    
    print(f"Конфигурация для {hostname} сохранена в {filename}")
    return filename

# Использование
vlans = {10: "Management", 20: "Users", 30: "Servers"}
interfaces = {
    "GigabitEthernet0/1": {"mode": "trunk", "description": "Uplink to Core"},
    "GigabitEthernet0/2": {"mode": "access", "vlan": 20, "description": "User PC"},
    "GigabitEthernet0/3": {"mode": "access", "vlan": 30, "description": "Server"}
}

generate_switch_config("SW-01", vlans, interfaces)
```

📌 Итог

Основные методы записи:

| Действие | Код | Когда использовать |
|----------|-----|-------------------|
| Открыть для записи | `open("file.txt", "w")` | Перезапись файла |
| Открыть для добавления | `open("file.txt", "a")` | Добавление в конец |
| Записать строку | `f.write("текст\n")` | Запись одной строки |
| Записать список строк | `f.writelines(list)` | Запись нескольких строк |
| Принудительная запись | `f.flush()` | Критически важные данные |
| Закрыть файл | `f.close()` | Завершение работы |

Важные предупреждения:

1. **Режим `"w"` перезаписывает файл** — старые данные теряются безвозвратно
2. **`.write()` не добавляет `\n`** — нужно указывать явно
3. **Данные буферизируются** — используйте `flush()` для немедленной записи
4. **Всегда закрывайте файлы** — используйте `with` для автоматического закрытия

Лучшие практики для сетевого инженера:

1. **Для логов** используйте режим `"a"` (append)
2. **Для конфигураций** используйте режим `"w"` и создавайте бэкапы
3. **Используйте `with`** для гарантированного закрытия файлов
4. **Проверяйте существование файлов** перед перезаписью
5. **Используйте `.writelines()`** для записи подготовленных списков строк

Шаблон безопасной записи:
```python
import os
from datetime import datetime

def safe_file_write(filename, content, mode="w", backup=True):
    """
    Безопасная запись в файл с поддержкой бэкапов
    
    Args:
        filename: Имя файла
        content: Содержимое для записи (str или list)
        mode: Режим записи ('w', 'a')
        backup: Создавать ли бэкап при перезаписи
    """
    
    # Проверка режима
    if mode not in ["w", "a"]:
        raise ValueError("Режим должен быть 'w' или 'a'")
    
    # Для режима 'w' проверяем существование файла
    if mode == "w" and os.path.exists(filename) and backup:
        # Создаём бэкап с timestamp
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        backup_name = f"{filename}.backup_{timestamp}"
        import shutil
        shutil.copy2(filename, backup_name)
        print(f"Создан бэкап: {backup_name}")
    
    # Запись в файл
    with open(filename, mode, encoding="utf-8") as f:
        if isinstance(content, list):
            f.writelines(content)
        else:
            f.write(content)
    
    print(f"Файл {filename} успешно обновлён")
    return True

# Использование
config = "hostname Router1\ninterface GigabitEthernet0/1\n no shutdown"
safe_file_write("router.cfg", config, mode="w", backup=True)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   