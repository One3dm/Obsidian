**Блоки кода в Python (Indented Blocks)**

Что такое блок кода

Блок кода — это группа операторов, которые выполняются вместе. В Python блоки определяются **отступами** (пробелами), а не фигурными скобками `{}`, как в других языках.

Почему отступы важны в Python:
```python
# ❌ В других языках (C, Java, JavaScript)
if (condition) {
    statement1;
    statement2;
}

# ✅ В Python
if condition:
    statement1
    statement2
```

**Ключевая идея:** В Python отступы — это не просто стиль, а **синтаксическая необходимость**. Они определяют структуру программы.

Как обозначается блок

1. **Двоеточие** `:` в конце строки (после `if`, `for`, `with`, `def` и т.д.)
2. **Увеличенный отступ** для всех строк блока
3. **Конец отступа** — конец блока

```python
with open("show_version.txt", mode="r") as f:
    data = f.read()          # внутри блока (4 пробела)
    print(data)              # внутри блока
# здесь блок закончился (отступ сброшен)
print("Файл закрыт автоматически")
```

Визуализация блоков:
```python
# Уровень 0 (глобальная область видимости)
import os

def process_file(filename):          # ← двоеточие, начинается блок функции
    # Уровень 1 (внутри функции)     # ← 4 пробела отступа
    if os.path.exists(filename):     # ← двоеточие, начинается блок if
        # Уровень 2 (внутри if)      # ← 8 пробелов отступа
        with open(filename) as f:    # ← двоеточие, начинается блок with
            # Уровень 3 (внутри with) # ← 12 пробелов отступа
            content = f.read()
            print(f"Прочитано {len(content)} символов")
        # Уровень 2 (после with)     # ← 8 пробелов
        print("Файл обработан")
    else:                            # ← на том же уровне, что и if
        # Уровень 2 (внутри else)    # ← 8 пробелов
        print(f"Файл {filename} не найден")
    # Уровень 1 (после if/else)      # ← 4 пробела
    return True

# Уровень 0 (снова глобальная область)
result = process_file("config.txt")
```

Пример с `if`
```python
if True:
    print("Hello")           # блок if
    print("Something")       # всё ещё блок if
    for x in range(10):      # начало вложенного блока
        print(x)             # блок for
        print(x)             # всё ещё блок for
    print("Else")            # вернулись к блоку if
print("Конец")               # вне блоков
```

Разбор примера:
```python
# Исходный код с комментариями уровней
if True:                      # ← Уровень 0, начинается блок if
    print("Hello")            # ← Уровень 1 (4 пробела), внутри if
    print("Something")        # ← Уровень 1, всё ещё внутри if
    
    for x in range(10):       # ← Уровень 1, начинается вложенный блок for
        print(x)              # ← Уровень 2 (8 пробелов), внутри for
        print(x)              # ← Уровень 2, всё ещё внутри for
    
    print("Else")             # ← Уровень 1, снова внутри if (после for)

print("Конец")                # ← Уровень 0, вне всех блоков
```

Правила отступов

| Правило | Описание | Пример |
|---------|----------|--------|
| **4 пробела** | Стандарт PEP 8 (Python Enhancement Proposal 8) | `[4 пробела]print("Hello")` |
| **Не использовать табуляцию** | Табуляция может работать, но это плохая практика | ❌ `[\t]print("Hello")` |
| **Одинаковый отступ** | Все строки одного блока должны иметь одинаковый отступ | ✅ `[4 пробела]line1`<br>✅ `[4 пробела]line2` |
| **Смешивание** | Нельзя смешивать пробелы и табуляции в одном файле | ❌ `[4 пробела]line1`<br>❌ `[\t]line2` |
| **Пустые строки** | Могут быть внутри блока, но не должны содержать пробелов | ✅ Пустая строка без пробелов |

Настройка редактора:
```python
# .editorconfig (рекомендуемые настройки)
root = true

[*]
indent_style = space
indent_size = 4
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.py]
max_line_length = 88  # или 79 для строгого PEP 8
```

Настройки для популярных редакторов:**
- **VS Code:** Установите Python extension, включите "Editor: Insert Spaces" и "Editor: Tab Size = 4"
- **PyCharm:** По умолчанию настроено правильно (4 пробела)
- **Vim:** `:set expandtab tabstop=4 shiftwidth=4`
- **Sublime Text:** View → Indentation → Convert Indentation to Spaces, Tab Width: 4

Распространённые ошибки отступов:
```python
# ❌ ОШИБКА: Неправильный отступ
if True:
print("Hello")  # IndentationError: expected an indented block

# ❌ ОШИБКА: Смешанные отступы
if True:
    print("Hello")  # 4 пробела
        print("World")  # 8 пробелов (неожиданный отступ)

# ❌ ОШИБКА: Несовпадающие отступы
if True:
    print("Hello")  # 4 пробела
   print("World")   # 3 пробела (IndentationError)

# ❌ ОШИБКА: Лишние пробелы в пустой строке
if True:
    print("Hello")
    [4 пробела здесь]  # пустая строка с пробелами
    print("World")

# ✅ ПРАВИЛЬНО:
if True:
    print("Hello")
    print("World")
```

Где используются блоки

| Конструкция | Пример | Описание |
|-------------|--------|----------|
| `if` / `elif` / `else` | `if x > 0:` | Условное выполнение |
| `for` / `while` | `for i in range(10):` | Циклы |
| `with` (контекстный менеджер) | `with open(...) as f:` | Управление ресурсами |
| `def` (функции) | `def my_func():` | Определение функций |
| `class` | `class MyClass:` | Определение классов |
| `try` / `except` / `finally` | `try:` | Обработка исключений |
| `async` / `await` | `async def func():` | Асинхронные функции |

Примеры всех типов блоков:
```python
# 1. Условные операторы
if device_status == "up":
    print("Устройство доступно")
    if ping_successful:
        print("Ping успешен")
elif device_status == "down":
    print("Устройство недоступно")
else:
    print("Статус неизвестен")

# 2. Циклы
for interface in interfaces:
    print(f"Настройка {interface}")
    for command in interface_commands:
        print(f"  Применяем: {command}")

# 3. Контекстные менеджеры
with open("config.txt", "r") as config_file:
    config = config_file.read()
    # Файл автоматически закроется здесь

# 4. Функции
def configure_device(hostname, ip_address):
    """Настройка сетевого устройства"""
    print(f"Настройка {hostname} с IP {ip_address}")
    # ... код настройки ...
    return True

# 5. Классы
class NetworkDevice:
    """Класс для представления сетевого устройства"""
    def __init__(self, name, ip):
        self.name = name
        self.ip = ip
    
    def ping(self):
        """Проверка доступности устройства"""
        import subprocess
        result = subprocess.run(["ping", "-c", "2", self.ip], 
                               capture_output=True)
        return result.returncode == 0

# 6. Обработка исключений
try:
    with open("missing_file.txt") as f:
        content = f.read()
except FileNotFoundError:
    print("Файл не найден")
except PermissionError:
    print("Нет прав на чтение файла")
finally:
    print("Завершение обработки файла")
```

Особые случаи и нюансы

1. Пустые блоки (используйте `pass`)
```python
# ❌ ОШИБКА: Пустой блок
if device_is_router:
    # Ничего не делать, но нужно что-то написать

# ✅ ПРАВИЛЬНО: Используйте pass
if device_is_router:
    pass  # Заглушка для пустого блока

# Или лучше:
if device_is_router:
    # TODO: Добавить обработку маршрутизаторов
    pass
```

2. Однострочные блоки (не рекомендуется)
```python
# ❌ НЕ РЕКОМЕНДУЕТСЯ: Однострочный блок на той же строке
if True: print("Hello")

# ✅ ПРАВИЛЬНО: На отдельной строке с отступом
if True:
    print("Hello")

# ✅ Допустимо для очень простых случаев (PEP 8)
if x > 0: return x
```

3. Многострочные условия
```python
# Длинные условия можно разбивать
if (device_type == "router" and 
    device_status == "up" and 
    interface_count > 2):
    # Отступ здесь
    configure_routing_protocols()

# Или так
if (device_type == "router" 
        and device_status == "up" 
        and interface_count > 2):
    configure_routing_protocols()
```

4. Вложенные блоки и читаемость
```python
# ❌ СЛИШКОМ ГЛУБОКАЯ ВЛОЖЕННОСТЬ (трудно читать)
if condition1:
    if condition2:
        if condition3:
            if condition4:
                print("Слишком глубоко!")

# ✅ ЛУЧШЕ: Разбить на функции или использовать ранний возврат
def process_device(device):
    if not condition1:
        return
    if not condition2:
        return
    if not condition3:
        return
    if not condition4:
        return
    print("Все условия выполнены")
```

Практические примеры для сетевой автоматизации

Пример 1: Обработка конфигурации устройств
```python
def apply_configuration(device_config):
    """Применение конфигурации к устройству"""
    
    # Проверка входных данных
    if not device_config:
        print("Ошибка: конфигурация пуста")
        return False
    
    # Проверка типа устройства
    if device_config.get("type") == "router":
        # Блок для маршрутизаторов
        print("Настройка маршрутизатора...")
        
        # Вложенный блок для интерфейсов
        for interface in device_config.get("interfaces", []):
            print(f"  Настройка интерфейса {interface['name']}")
            
            # Ещё один уровень для команд
            for command in interface.get("commands", []):
                print(f"    Команда: {command}")
    
    elif device_config.get("type") == "switch":
        # Блок для коммутаторов
        print("Настройка коммутатора...")
        
        # Настройка VLAN
        for vlan in device_config.get("vlans", []):
            print(f"  Создание VLAN {vlan['id']}: {vlan['name']}")
    
    else:
        # Блок для неизвестных устройств
        print(f"Неизвестный тип устройства: {device_config.get('type')}")
        return False
    
    # Финальные действия
    print("Конфигурация применена успешно")
    return True
```

Пример 2: Обработка вывода команд
```python
def parse_show_interfaces(output_lines):
    """Парсинг вывода команды show interfaces"""
    
    interfaces = {}
    current_interface = None
    
    # Основной блок обработки строк
    for line in output_lines:
        line = line.strip()
        
        # Поиск строк с интерфейсами
        if line.startswith("GigabitEthernet") or line.startswith("FastEthernet"):
            # Нашли новый интерфейс
            interface_name = line.split()[0]
            current_interface = {
                "name": interface_name,
                "status": "unknown",
                "protocol": "unknown"
            }
            interfaces[interface_name] = current_interface
        
        # Проверка статуса (внутри блока текущего интерфейса)
        elif current_interface and "line protocol is" in line:
            # Вложенный блок для анализа статуса
            parts = line.split()
            if len(parts) >= 2:
                current_interface["status"] = parts[1]  # "up" или "down"
                current_interface["protocol"] = parts[4]  # "up" или "down"
        
        # Сбор статистики
        elif current_interface and "packets input" in line:
            # Ещё один уровень вложенности
            try:
                packets = int(line.split()[0])
                current_interface["input_packets"] = packets
            except (ValueError, IndexError):
                # Обработка ошибок внутри блока
                current_interface["input_packets"] = 0
    
    return interfaces
```

Пример 3: Создание конфигурационных шаблонов
```python
def generate_interface_config(interface_data):
    """Генерация конфигурации интерфейса"""
    
    config_lines = []
    
    # Блок для каждого интерфейса
    for interface in interface_data:
        # Добавляем заголовок интерфейса
        config_lines.append(f"interface {interface['name']}\n")
        
        # Блок описания (если есть)
        if "description" in interface:
            config_lines.append(f" description {interface['description']}\n")
        
        # Блок IP-адреса (для маршрутизаторов)
        if "ip_address" in interface and "subnet_mask" in interface:
            config_lines.append(
                f" ip address {interface['ip_address']} {interface['subnet_mask']}\n"
            )
        
        # Блок для коммутаторов
        if interface.get("switchport"):
            config_lines.append(" switchport mode access\n")
            
            # Вложенный блок для VLAN
            if "vlan" in interface:
                config_lines.append(f" switchport access vlan {interface['vlan']}\n")
        
        # Блок включения/выключения
        if interface.get("shutdown", False):
            config_lines.append(" shutdown\n")
        else:
            config_lines.append(" no shutdown\n")
        
        # Закрывающая строка
        config_lines.append("!\n")
    
    return config_lines
```

📌 Итог

Основные правила:
```python
# Структура блока
if условие:          # 1. Двоеточие в конце строки
    оператор1        # 2. Отступ 4 пробела
    оператор2        # 3. Тот же отступ для всего блока
    if другое:       # 4. Вложенный блок начинается с двоеточия
        оператор3    # 5. Ещё 4 пробела отступа
    оператор4        # 6. Вернулись к предыдущему уровню
# конец блока — отступ сброшен
оператор5
```

Ключевые моменты:
1. **Отступы обязательны** — без них код не работает
2. **4 пробела на уровень** — стандарт Python
3. **Двоеточие обязательно** — обозначает начало блока
4. **Одинаковые отступы** в пределах одного блока
5. **Не смешивать** пробелы и табуляции

Частые ошибки и как их избежать:

| Ошибка | Решение |
|--------|---------|
| `IndentationError` | Проверьте отступы, используйте 4 пробела |
| Смешанные отступы | Настройте редактор на замену табуляций пробелами |
| Неправильный уровень | Следите за вложенностью блоков |
| Пустые блоки | Используйте `pass` или комментарий |

Практические советы для сетевого инженера:
1. **Используйте IDE с подсветкой синтаксиса** — поможет видеть уровни вложенности
2. **Следуйте PEP 8** — стандарт оформления кода Python
3. **Избегайте глубокой вложенности** — если больше 3 уровней, пересмотрите логику
4. **Комментируйте сложные блоки** — особенно в скриптах автоматизации
5. **Используйте функции** для выделения логических блоков

Шпаргалка по отступам:
```python
# Уровень 0: Глобальная область
import module

# Уровень 1: Функции, классы, условия
def function():          # ← двоеточие
    # 4 пробела         # ← уровень 1
    
    if condition:        # ← двоеточие
        # 8 пробелов    # ← уровень 2
        
        for item in items:  # ← двоеточие
            # 12 пробелов   # ← уровень 3
            process(item)
        
        # 8 пробелов    # ← снова уровень 2
        continue_processing()
    
    # 4 пробела         # ← снова уровень 1
    return result

# Уровень 0: Снова глобальная область
function()
```

> [!TIP]
> Настройте редактор так, чтобы:
> 1. Tab вставлял 4 пробела
> 2. Показывал невидимые символы (пробелы и табуляции)
> 3. Автоматически выравнивал отступы

---

## 🔗 Связанные темы
- [[Контекстные менеджеры (with)]]
- [[Характеристики Python]]
- [[Функции (def)]]
- [[Классы (class)]]
- [[Условные операторы (if/elif/else)]]
- [[Циклы (for/while)]]
```