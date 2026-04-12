**Чтение из файла**

Открытие файла
```python
f = open("show_version.txt")
```

По умолчанию `open()` открывает файл в **текстовом режиме** (`'r'` — read).
```python
f
# <_io.TextIOWrapper name='show_version.txt' mode='r' encoding='UTF-8'>
```

Режимы открытия файла:

| Режим | Описание | Когда использовать |
|-------|----------|-------------------|
| `'r'` | Чтение (по умолчанию) | Только чтение файла |
| `'w'` | Запись (перезапись) | Создание нового файла или перезапись существующего |
| `'a'` | Добавление (append) | Добавление данных в конец файла |
| `'x'` | Эксклюзивное создание | Создание файла, ошибка если уже существует |
| `'r+'` | Чтение и запись | Одновременное чтение и запись |
| `'b'` | Бинарный режим | Работа с бинарными данными (например, `'rb'`, `'wb'`) |

```python
# Примеры разных режимов
f_read = open("config.txt", "r")      # только чтение
f_write = open("output.txt", "w")     # запись (перезапись)
f_append = open("log.txt", "a")       # добавление в конец
f_binary = open("image.jpg", "rb")    # бинарное чтение
```

Кодировка файлов:
```python
# Указание кодировки (важно для Windows и файлов не в UTF-8)
f = open("show_version.txt", encoding="utf-8")
f = open("config.txt", encoding="cp1251")  # Windows-1251
f = open("output.txt", encoding="ascii")

# Автоматическое определение кодировки (требуется установка chardet)
import chardet
with open("unknown.txt", "rb") as f:
    raw_data = f.read()
    result = chardet.detect(raw_data)
    encoding = result["encoding"]
```

Основные методы чтения

| Метод | Что делает | Результат | Память | Когда использовать |
|-------|-----------|-----------|--------|-------------------|
| `.read()` | Читает весь файл целиком | Одна строка (str) | Высокая | Маленькие файлы (<10MB) |
| `.readline()` | Читает одну строку | Строка (str) | Низкая | Построчная обработка |
| `.readlines()` | Читает все строки в список | Список строк (list) | Высокая | Нужны все строки как список |
| Цикл `for line in f` | Итерация по строкам | По одной строке за раз | Низкая | **Самый эффективный** для больших файлов |

**Примеры использования**

`.read()` — весь файл как одна строка
```python
f = open("show_version.txt")
data = f.read()  # Вся содержимое файла как одна строка
f.close()

# Практический пример: чтение конфигурации устройства
config_file = open("router_config.txt")
config = config_file.read()
config_file.close()

print(f"Размер конфигурации: {len(config)} символов")
print(f"Первые 100 символов:\n{config[:100]}...")
```

`.readline()` — построчное чтение
```python
f = open("show_version.txt")
first_line = f.readline()   # первая строка
second_line = f.readline()  # вторая строка
f.close()

# Удаление символов новой строки
first_line = first_line.rstrip("\n")  # или .strip()

# Практический пример: чтение заголовка лог-файла
log_file = open("system.log")
header = log_file.readline().strip()
print(f"Формат лога: {header}")

# Чтение определённого количества строк
for i in range(10):  # первые 10 строк
    line = log_file.readline()
    if not line:  # конец файла
        break
    print(f"Строка {i+1}: {line.strip()}")
    
log_file.close()
```

`.readlines()` — все строки в список
```python
f = open("show_version.txt")
lines = f.readlines()  # список строк
f.close()

# Количество строк в файле
print(f"В файле {len(lines)} строк")

# Практический пример: анализ конфигурации интерфейсов
config_file = open("switch_config.txt")
config_lines = config_file.readlines()
config_file.close()

# Поиск всех интерфейсов
interfaces = []
for line in config_lines:
    if line.strip().startswith("interface"):
        interfaces.append(line.strip())
        
print(f"Найдено интерфейсов: {len(interfaces)}")
for interface in interfaces:
    print(f"  {interface}")
```

Цикл по файлу (самый эффективный)
```python
f = open("show_version.txt")
for line in f:
    print(line.strip())  # обрабатываем каждую строку
f.close()

# Практический пример: обработка больших лог-файлов
log_file = open("access.log", encoding="utf-8")
error_count = 0
warning_count = 0

for line_num, line in enumerate(log_file, start=1):
    line = line.strip()
    
    # Поиск ошибок и предупреждений
    if "ERROR" in line:
        error_count += 1
        print(f"Ошибка в строке {line_num}: {line[:50]}...")
    elif "WARNING" in line:
        warning_count += 1
        
    # Обработка больших файлов по частям
    if line_num % 10000 == 0:
        print(f"Обработано {line_num} строк...")

log_file.close()
print(f"Итого: {error_count} ошибок, {warning_count} предупреждений")
```

**Контекстный менеджер `with` (рекомендуемый способ)**

Вместо ручного закрытия файла используйте конструкцию `with` — файл закроется автоматически:

```python
# ✅ РЕКОМЕНДУЕТСЯ
with open("show_version.txt") as f:
    data = f.read()
# Файл автоматически закрывается здесь

# Эквивалентно:
f = open("show_version.txt")
try:
    data = f.read()
finally:
    f.close()  # гарантированное закрытие даже при ошибке
```

Преимущества `with`:
1. **Автоматическое закрытие** файла
2. **Безопасность** — файл закроется даже при исключении
3. **Читаемость** кода
4. **Идиоматично** для Python

Примеры с `with`:
```python
# Чтение всего файла
with open("config.txt") as f:
    config = f.read()

# Построчная обработка
with open("log.txt") as f:
    for line in f:
        process_line(line.strip())

# Чтение в список
with open("devices.txt") as f:
    devices = f.readlines()
    devices = [line.strip() for line in devices]  # очистка от \n

# Несколько файлов одновременно
with open("input.txt") as f_in, open("output.txt", "w") as f_out:
    for line in f_in:
        f_out.write(line.upper())
```

Метод `.seek(0)` и позиция в файле
```python
with open("show_version.txt") as f:
    # Читаем первые 100 символов
    first_part = f.read(100)
    print(f"Позиция после чтения: {f.tell()}")  # 100
    
    # Возвращаемся в начало
    f.seek(0)
    print(f"Позиция после seek(0): {f.tell()}")  # 0
    
    # Читаем снова
    full_content = f.read()
    
    # Переход на конкретную позицию
    f.seek(50)  # перейти к 50-му символу
    from_50 = f.read(20)  # прочитать 20 символов с позиции 50
```

Практическое использование `.seek()`:
```python
# Анализ заголовка и тела файла
with open("network_dump.pcap", "rb") as f:
    # Чтение заголовка (первые 24 байта)
    header = f.read(24)
    
    # Возврат к началу для полного чтения
    f.seek(0)
    full_data = f.read()
    
    # Пропуск первых 100 байт
    f.seek(100)
    data_from_100 = f.read()
```

Обработка больших файлов

Стратегии для больших файлов:

```python
# 1. Построчное чтение (самый эффективный)
with open("huge_log.txt") as f:
    for line in f:
        process_line(line.strip())

# 2. Чтение блоками (для бинарных файлов)
CHUNK_SIZE = 1024 * 1024  # 1MB
with open("large_file.bin", "rb") as f:
    while True:
        chunk = f.read(CHUNK_SIZE)
        if not chunk:  # конец файла
            break
        process_chunk(chunk)

# 3. Использование генератора для повторного использования
def read_large_file(file_path):
    with open(file_path) as f:
        for line in f:
            yield line.strip()

# Использование генератора
for line in read_large_file("big_config.txt"):
    if "interface" in line:
        print(line)

# 4. Параллельная обработка (для очень больших файлов)
import multiprocessing

def process_chunk(start, end, file_path):
    with open(file_path) as f:
        f.seek(start)
        chunk = f.read(end - start)
        # обработка чанка
        return len(chunk.splitlines())
```

Обработка ошибок при чтении файлов
```python
import os

file_path = "show_version.txt"

# Проверка существования файла
if not os.path.exists(file_path):
    print(f"Файл {file_path} не существует")
    # Можно создать или выйти
    with open(file_path, "w") as f:
        f.write("")  # создаём пустой файл

# Безопасное чтение с обработкой ошибок
try:
    with open(file_path, "r") as f:
        data = f.read()
except FileNotFoundError:
    print(f"Ошибка: файл {file_path} не найден")
except PermissionError:
    print(f"Ошибка: нет прав на чтение файла {file_path}")
except UnicodeDecodeError:
    print(f"Ошибка: неверная кодировка файла {file_path}")
    # Попробовать другую кодировку
    with open(file_path, "r", encoding="latin-1") as f:
        data = f.read()
except Exception as e:
    print(f"Неизвестная ошибка: {type(e).__name__}: {e}")
else:
    print(f"Файл успешно прочитан, размер: {len(data)} символов")
```


Практические примеры для сетевой автоматизации

Пример 1: Чтение конфигурации устройств
```python
def read_device_configs(config_dir):
    """Чтение всех конфигурационных файлов из директории"""
    import os
    
    configs = {}
    
    for filename in os.listdir(config_dir):
        if filename.endswith(".cfg") or filename.endswith(".txt"):
            filepath = os.path.join(config_dir, filename)
            
            try:
                with open(filepath, "r", encoding="utf-8") as f:
                    device_name = filename.replace(".cfg", "").replace(".txt", "")
                    configs[device_name] = f.read()
                    
            except Exception as e:
                print(f"Ошибка чтения {filename}: {e}")
    
    return configs

# Использование
device_configs = read_device_configs("./configs")
for device, config in device_configs.items():
    print(f"{device}: {len(config)} символов")
```

### Пример 2: Анализ логов доступа

```python
def analyze_access_log(log_file, ip_address=None):
    """Анализ лог-файла доступа"""
    results = {
        "total_requests": 0,
        "successful": 0,
        "failed": 0,
        "by_hour": {},
        "unique_ips": set()
    }
    
    with open(log_file) as f:
        for line in f:
            line = line.strip()
            if not line:
                continue
                
            results["total_requests"] += 1
            
            # Парсинг лога (упрощённый)
            parts = line.split()
            if len(parts) >= 7:
                ip = parts[0]
                status = parts[8]
                hour = parts[3].split(":")[1]
                
                results["unique_ips"].add(ip)
                
                if ip_address and ip == ip_address:
                    print(f"Найдено обращение от {ip}: {line}")
                
                # Подсчёт по часам
                results["by_hour"][hour] = results["by_hour"].get(hour, 0) + 1
                
                # Подсчёт статусов
                if status.startswith("2"):
                    results["successful"] += 1
                elif status.startswith("4") or status.startswith("5"):
                    results["failed"] += 1
    
    results["unique_ips"] = len(results["unique_ips"])
    return results

# Использование
stats = analyze_access_log("access.log", ip_address="192.168.1.100")
print(f"Всего запросов: {stats['total_requests']}")
print(f"Уникальных IP: {stats['unique_ips']}")
```

### Пример 3: Чтение вывода команд с оборудования

```python
def parse_show_version(output_file):
    """Парсинг вывода команды show version"""
    with open(output_file) as f:
        lines = f.readlines()
    
    device_info = {}
    
    for line in lines:
        line = line.strip()
        
        if "Cisco IOS Software" in line:
            device_info["software"] = line.split(",")[0]
        elif "Version" in line and "Software" not in line:
            device_info["version"] = line.split(",")[0].split()[-1]
        elif "uptime is" in line:
            device_info["uptime"] = line.split("uptime is")[-1].strip()
        elif "bytes of memory" in line:
            device_info["memory"] = line.split()[0]
    
    return device_info

# Использование
info = parse_show_version("show_version.txt")
print(f"Версия IOS: {info.get('version', 'Неизвестно')}")
print(f"Аптайм: {info.get('uptime', 'Неизвестно')}")
```

---

## 📌 Итог

### Сравнение методов чтения:

| Метод | Возвращает | Память | Скорость | Когда использовать |
|-------|-----------|--------|----------|-------------------|
| `.read()` | `str` | Высокая | Быстро | Файлы < 10MB, нужен весь текст |
| `.readline()` | `str` | Низкая | Средне | Построчная обработка, чтение заголовков |
| `.readlines()` | `list` | Высокая | Быстро | Нужны все строки как список, файлы < 50MB |
| `for line in f` | `str` | Низкая | **Оптимально** | **Большие файлы**, потоковая обработка |

### Лучшие практики:

1. **Всегда используйте `with open(...) as f:`** для автоматического закрытия
2. **Для больших файлов** используйте цикл `for line in f:`
3. **Указывайте кодировку** для кросс-платформенной совместимости
4. **Обрабатывайте исключения** при работе с файлами
5. **Используйте `.strip()` или `.rstrip('\n')`** для очистки строк

### Практические советы для сетевого инженера:

1. **Конфигурации устройств** — читайте `.read()` для небольших файлов
2. **Лог-файлы** — используйте цикл `for line in f:` для больших файлов
3. **Экспорт таблиц** (ARP, MAC) — используйте `.readlines()` для анализа
4. **Бинарные дампы** — используйте `'rb'` режим и чтение блоками
5. **Обработка ошибок** — всегда проверяйте существование файла

### Шаблон для безопасного чтения:

```python
import os

def safe_read_file(file_path, default=""):
    """Безопасное чтение файла с обработкой ошибок"""
    if not os.path.exists(file_path):
        print(f"Предупреждение: файл {file_path} не существует")
        return default
    
    try:
        with open(file_path, "r", encoding="utf-8") as f:
            return f.read()
    except UnicodeDecodeError:
        # Попробовать другие кодировки
        for encoding in ["cp1251", "latin-1", "ascii"]:
            try:
                with open(file_path, "r", encoding=encoding) as f:
                    return f.read()
            except:
                continue
        return default
    except Exception as e:
        print(f"Ошибка чтения {file_path}: {e}")
        return default

# Использование
config = safe_read_file("router.cfg", "! Конфигурация не найдена")
```

---

## 🔗 Связанные темы
- [[Запись в файл]]
- [[Работа с путями файлов (os.path)]]
- [[Обработка исключений (try/except)]]
- [[Регулярные выражения для анализа текста]]
```
________________________________________________________________________
Paths: [[Python]]
Tags: #Python   