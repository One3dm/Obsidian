
Чтение из файла

Открытие файла
```python
f = open("file.txt")  # по умолчанию 'r' (чтение)
```

4 способа чтения

1. `.read()` — весь файл

```python
with open("config.txt") as f:
    data = f.read()  # вся содержимое как одна строка

print(f"Размер: {len(data)} символов")
```

### 2. `.readline()` — одна строка

```python
with open("log.txt") as f:
    first_line = f.readline().strip()  # первая строка без \n
    second_line = f.readline().strip() # вторая строка
```

### 3. `.readlines()` — все строки в список

```python
with open("devices.txt") as f:
    lines = f.readlines()  # список строк

print(f"В файле {len(lines)} строк")
```

### 4. Цикл `for` — самый эффективный

```python
with open("big_log.txt") as f:
    for line in f:
        print(line.strip())  # обрабатываем построчно
```

---

## Сравнение методов

| Метод | Что возвращает | Память | Когда использовать |
|-------|---------------|--------|-------------------|
| `.read()` | Одна строка (str) | Много | Маленькие файлы (<10MB) |
| `.readline()` | Одна строка | Мало | Чтение заголовков |
| `.readlines()` | Список строк | Много | Нужны все строки как список |
| `for line in f` | По одной строке | **Мало** | **Большие файлы** |

---

## Практические примеры

### Чтение конфигурации устройства

```python
def read_config(filename):
    with open(filename) as f:
        config = f.read()
    
    # Поиск hostname
    if "hostname" in config:
        for line in config.split("\n"):
            if line.startswith("hostname"):
                hostname = line.split()[1]
                print(f"Устройство: {hostname}")
    
    return config

# Использование
config = read_config("router.cfg")
```

### Анализ лог-файла

```python
def analyze_log(log_file):
    error_count = 0
    
    with open(log_file) as f:
        for line_num, line in enumerate(f, start=1):
            line = line.strip()
            
            if "ERROR" in line:
                error_count += 1
                print(f"Ошибка в строке {line_num}: {line[:50]}...")
    
    print(f"Всего ошибок: {error_count}")

# Использование
analyze_log("system.log")
```

### Чтение списка устройств

```python
def read_device_list(filename):
    devices = []
    
    with open(filename) as f:
        for line in f:
            line = line.strip()
            if line and not line.startswith("#"):  # пропускаем пустые и комментарии
                devices.append(line)
    
    return devices

# Файл devices.txt:
# router1
# switch1
# # Комментарий
# firewall1

devices = read_device_list("devices.txt")
print(f"Найдено устройств: {len(devices)}")  # 3
```

---

## Обработка ошибок

```python
import os

filename = "config.txt"

# Проверка существования
if not os.path.exists(filename):
    print(f"Файл {filename} не найден")
    exit()

# Безопасное чтение
try:
    with open(filename) as f:
        data = f.read()
except FileNotFoundError:
    print("Файл не найден")
except PermissionError:
    print("Нет прав на чтение")
except Exception as e:
    print(f"Ошибка: {e}")
else:
    print("Файл успешно прочитан")
```

---

## 📌 Итог

**Основные правила:**
1. Используйте `with open(...) as f:` для автоматического закрытия
2. Для больших файлов используйте цикл `for line in f:`
3. Используйте `.strip()` для удаления `\n`
4. Всегда проверяйте существование файла

**Советы для сетевого инженера:**
- Конфигурации → `.read()` (обычно маленькие)
- Логи → цикл `for line in f:` (часто большие)
- Списки устройств → `.readlines()` или цикл

```python
# ✅ Правильный шаблон
import os

if os.path.exists("file.txt"):
    with open("file.txt") as f:
        for line in f:
            process_line(line.strip())
else:
    print("Файл не найден")
```

---

## 🔗 Связанные темы
- [[Запись в файл]]
- [[Добавление в файл (append)]]
- [[Контекстные менеджеры (with)]]
```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   