**Основы строк в Python**

Создание строк
Строки можно создавать с помощью **одинарных** или **двойных** кавычек:

```python
my_var = "Some String"      # двойные кавычки
my_var2 = 'another string'  # одинарные кавычки
```

```python
my_var3 = "Cannot do this'   # ❌ SyntaxError - несовпадающие кавычки
```

Проверка типа
```python
my_var = "some string"
type(my_var)    # <class 'str'>
isinstance(my_var, str)  # True - альтернативная проверка
```

Экранирование символов
Используйте обратный слэш `\` для специальных символов:

```python
# Примеры экранирования
path = "C:\\Users\\Admin\\Documents"  # Экранирование обратного слэша
quote = "He said, \"Hello!\""         # Кавычки внутри строки
new_line = "First line\nSecond line"  # Перевод строки
tab = "Column1\tColumn2"              # Табуляция

# Сырые строки (raw strings) - отключают экранирование
raw_path = r"C:\Users\Admin\Documents"  # Без двойных слэшей
regex_pattern = r"\d{3}-\d{2}-\d{4}"    # Регулярные выражения
```

Многострочные строки
Используйте **тройные кавычки** — одинарные `'''` или двойные `"""`:
```python
# Двойные тройные кавычки
my_var3 = """Это строка,
которая может
занимать несколько строк"""

# Одинарные тройные кавычки
my_var4 = '''Тоже работает
с тройными кавычками'''
```

Пример из REPL:
```python
m_string = """
this is a multiline

string

that says

something
"""
```

Конкатенация (сложение строк)
```python
# Оператор +
first = "Hello"
second = "World"
result = first + " " + second  # "Hello World"

# Оператор * (повторение)
separator = "-" * 20  # "--------------------"
```

## f-строки (форматированные строки) - Python 3.6+
```python
# Самый современный и удобный способ
name = "Router1"
ip = "192.168.1.1"
uptime = 365

# Базовое использование
info = f"Device: {name}, IP: {ip}"
# "Device: Router1, IP: 192.168.1.1"

# Выражения внутри f-строк
status = f"Uptime: {uptime} days ({uptime/365:.1f} years)"
# "Uptime: 365 days (1.0 years)"

# Для сетевых задач
network = f"{ip}/24"  # "192.168.1.1/24"
```

## Полезные методы строк (базовые)
```python
text = "  router-sw01  "

# Удаление пробелов
text.strip()      # "router-sw01"
text.lstrip()     # "router-sw01  "
text.rstrip()     # "  router-sw01"

# Регистр
"router".upper()      # "ROUTER"
"ROUTER".lower()      # "router"
"router name".title() # "Router Name"

# Проверки
"192.168.1.1".startswith("192")  # True
"config.txt".endswith(".txt")    # True
"R1".isupper()                   # True
```

## Практические примеры для сетевой автоматизации
```python
# Формирование команд для оборудования
hostname = "SW-01"
interface = "GigabitEthernet0/1"
vlan = "100"

command = f"interface {interface}\nswitchport access vlan {vlan}"
# "interface GigabitEthernet0/1\nswitchport access vlan 100"

# Обработка вывода с оборудования
output = "  GigabitEthernet0/1 is up, line protocol is up  "
clean_output = output.strip()  # Убираем лишние пробелы

# Создание конфигурационных блоков
banner = """
!
banner motd #
Unauthorized access prohibited
#
!
"""
```

## 📌 Итог

| Способ | Пример | Когда использовать |
|--------|--------|-------------------|
| Одинарные кавычки | `'hello'` | Короткие строки без апострофов |
| Двойные кавычки | `"hello"` | Если внутри есть одинарные (`"it's"`) |
| Тройные кавычки | `"""..."""` | Многострочный текст, docstring |
| f-строки | `f"{var}"` | Подстановка переменных (Python 3.6+) |
| Сырые строки | `r"path"` | Пути, регулярные выражения |

### Основные операции со строками:
1. **Создание**: `"текст"`, `'текст'`, `"""многострочный"""`
2. **Конкатенация**: `str1 + str2`, `"text" * 3`
3. **Форматирование**: f-строки (`f"{var}"`)
4. **Методы**: `.strip()`, `.upper()`, `.lower()`, `.startswith()`
5. **Проверка типа**: `type(text) == str` или `isinstance(text, str)`

### Для сетевого инженера:
- Используйте f-строки для формирования команд
- Применяйте `.strip()` для очистки вывода с оборудования
- Сырые строки (`r""`) полезны для путей и регулярных выражений
- Тройные кавычки удобны для многострочных конфигураций

> [!TIP]
> В Python строки **неизменяемы** (immutable). Любой метод строки возвращает **новую строку**, не изменяя оригинал.
> ```python
> text = "router"
> text.upper()  # Возвращает "ROUTER"
> print(text)   # Оригинал остался "router"
> ```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   