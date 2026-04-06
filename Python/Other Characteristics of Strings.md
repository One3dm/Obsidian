Другие характеристики строк

1. Проверка вхождения (Membership)

Оператор `in` проверяет, содержится ли подстрока в строке:
```python
some_str = "It was the best of times, it was the worst of times"

"the best" in some_str   # True
"the worst" in some_str  # True
"python" in some_str     # False
```

Возвращает **булево значение** (`True` / `False`).

Практическое использование для сетевых данных:
```python
# Проверка наличия ключевых слов в выводе команды
show_interface_output = """
GigabitEthernet0/1 is up, line protocol is up
  Hardware is Gigabit Ethernet, address is aabb.ccdd.eeff
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec
"""

# Проверка состояния интерфейса
if "is up" in show_interface_output and "line protocol is up" in show_interface_output:
    print("Интерфейс полностью работоспособен")
else:
    print("Проблемы с интерфейсом")

# Поиск конкретных данных
if "MTU" in show_interface_output:
    print("Информация о MTU найдена")
```

### Оператор `not in` (отрицание):

```python
config = "interface GigabitEthernet0/1\n switchport mode access"

if "trunk" not in config:
    print("Интерфейс не в режиме trunk")
```

2. Raw-строки (сырые строки)

Некоторые символы имеют специальное значение:  
- `\n` — новая строка  
- `\t` — табуляция  
- `\r` — возврат каретки
- `\b` — backspace
- `\\` — обратный слэш

Чтобы отключить обработку escape-последовательностей, используйте **raw-строку** — префикс `r`:
```python
# Обычная строка — символы интерпретируются
win_path = "c:\windows\new_dir\test\applications"
print(win_path)
# c:\windows
# ew_dir    estpplications  (не то, что ожидалось)

# Raw-строка — всё как есть
win_path = r"c:\windows\new_dir\test\applications"
print(win_path)
# c:\windows\new_dir\test\applications
```

Когда использовать raw-строки:
```python
# 1. Пути в Windows
windows_path = r"C:\Users\Admin\Documents\network_configs"

# 2. Регулярные выражения
import re
pattern = r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"  # Паттерн для IP-адреса

# 3. Команды с обратными слэшами
cisco_command = r"show running-config | include ^interface"

# 4. Сетевые маски и адреса
network = r"192.168.1.0\24"  # Обратный слэш остаётся как символ

# 5. Сравнение с обычной строкой
regular = "Line1\nLine2\tTab"      # Будет: Line1 (новая строка) Line2    Tab
raw = r"Line1\nLine2\tTab"         # Будет: Line1\nLine2\tTab
```

Важно: raw-строки не экранируют кавычки:
```python
# ❌ Не работает
# text = r"Это raw-строка с \"кавычками\""  # SyntaxError

# ✅ Работает
text = r"Это raw-строка без проблем"  # ОК
text2 = r'Можно и с "кавычками" внутри'  # ОК
```

3. Конкатенация строк (объединение)

Оператор `+`

```python
city = "San Francisco"
state = "CA"
location = city + ", " + state
print(location)   # 'San Francisco, CA'
```

Оператор `+=` (добавление к существующей строке)
```python
data = "line1 of some output\n"
data += "line2 of some output\n"
print(data)
# line1 of some output
# line2 of some output
```

Практический пример построения конфигурации:
```python
# Построение конфигурации коммутатора по частям
config = "!\n"

# Базовые настройки
config += "hostname SW-01\n"
config += "!\n"

# Настройки VLAN
config += "vlan 10\n"
config += " name Management\n"
config += "!\n"
config += "vlan 20\n"
config += " name Users\n"
config += "!\n"

# Настройки интерфейса
config += "interface GigabitEthernet0/1\n"
config += " description Uplink to Core\n"
config += " switchport mode trunk\n"
config += " switchport trunk allowed vlan 10,20\n"
config += "!\n"

print(config)
```

### Эффективность конкатенации:

```python
# ❌ Медленно для большого количества операций
result = ""
for i in range(1000):
    result += f"line {i}\n"  # Создаёт новую строку каждый раз

# ✅ Быстрее использовать список
lines = []
for i in range(1000):
    lines.append(f"line {i}\n")
result = "".join(lines)  # Однократная конкатенация
```

---

## 4. Строки как последовательности (sequences)

Строки обладают свойствами последовательностей:

1. **Имеют порядок** — первый символ, второй и т.д.
2. **Доступ по индексу** — `my_var[0]` (нумерация с 0)
3. **Имеют длину** — `len(my_var)`
4. **Можно перебирать в цикле**

### Доступ по индексу:

```python
my_var = "some string"
my_var[0]   # 's'
my_var[1]   # 'o'
my_var[2]   # 'm'
```

### Отрицательные индексы (с конца):

```python
ip = "192.168.1.1"
ip[-1]   # '1' (последний символ)
ip[-2]   # '.' (предпоследний символ)
ip[-3]   # '1' (третий с конца)
```

### Срезы (slicing):

```python
text = "GigabitEthernet0/1"

# text[start:end:step]
text[0:7]      # 'Gigabit' (символы с 0 по 6)
text[7:16]     # 'Ethernet' (символы с 7 по 15)
text[:7]       # 'Gigabit' (с начала до 6)
text[7:]       # 'Ethernet0/1' (с 7 до конца)
text[::2]      # 'GgbiEhnr0/' (каждый второй символ)
text[::-1]     # '1/0tenrehtEtabigiG' (обратный порядок)
```

### Практические примеры для сетевых данных:

```python
# Извлечение частей IP-адреса
ip = "192.168.1.1"
first_octet = ip[0:3]        # '192'
last_octet = ip[-3:]          # '.1' (осторожно!)
better_last = ip.split('.')[-1]  # '1' (лучший способ)

# Анализ имени интерфейса
interface = "GigabitEthernet0/1"
if_type = interface[:7]       # 'Gigabit'
if_number = interface[7:]     # 'Ethernet0/1'

# Проверка формата MAC-адреса
mac = "aa:bb:cc:dd:ee:ff"
if len(mac) == 17 and mac[2] == ":" and mac[5] == ":":
    print("MAC-адрес в правильном формате")

# Перебор символов в строке
for char in "Router":
    print(f"Символ: {char}, Код: {ord(char)}")
```

### Длина строки:

```python
commands = ["show version", "show running-config", "show interfaces"]
for cmd in commands:
    print(f"Команда '{cmd}' имеет длину {len(cmd)} символов")
# Команда 'show version' имеет длину 12 символов
# Команда 'show running-config' имеет длину 19 символов
# Команда 'show interfaces' имеет длину 16 символов
```

---

## 5. Неизменяемость строк (Immutability)

Строки в Python **неизменяемы** (immutable). Это означает, что после создания строку нельзя изменить.

```python
text = "router"
text[0] = "R"  # ❌ TypeError: 'str' object does not support item assignment

# Вместо этого создаём новую строку
text = "R" + text[1:]  # ✅ "Router"
```

**Почему это важно:**
- Безопасность: строки можно свободно передавать между функциями
- Эффективность: Python может кэшировать и оптимизировать строки
- Простота: не нужно беспокоиться о неожиданных изменениях

---

## 📌 Итог

| Характеристика | Пример | Когда использовать |
|----------------|--------|-------------------|
| Проверка вхождения | `"cat" in "catalog"` → `True` | Поиск ключевых слов в выводе команд |
| `not in` | `"error" not in output` | Проверка отсутствия проблем |
| Raw-строка | `r"C:\path\to\file"` | Пути, regex, команды с обратными слэшами |
| Конкатенация (`+`) | `"Hello" + " " + "World"` | Объединение небольших строк |
| Добавление (`+=`) | `config += "hostname R1\n"` | Построение конфигурации по частям |
| Доступ по индексу | `my_str[0]`, `my_str[-1]` | Извлечение конкретных символов |
| Срезы | `text[0:5]`, `text[::-1]` | Извлечение подстрок, реверс |
| Длина строки | `len(my_str)` | Проверка формата, валидация |
| Перебор в цикле | `for char in text:` | Обработка каждого символа |

### Практические советы для сетевого инженера:

1. **Используйте `in` для анализа вывода** с оборудования
2. **Всегда используйте raw-строки** для путей и регулярных выражений
3. **Для больших конфигураций** собирайте строки в список и используйте `join()`
4. **Используйте срезы** для извлечения частей IP-адресов, имен интерфейсов
5. **Помните про неизменяемость** — методы строк возвращают новые строки

### Быстрые проверки:
```python
# Проверка пустой строки
if not hostname:  # или if hostname == ""
    print("Имя устройства не задано")

# Проверка минимальной длины
if len(password) < 8:
    print("Пароль слишком короткий")

# Проверка формата (пример для версий ПО)
version = "15.2(4)M7"
if version.startswith("15.") and "M" in version:
    print("Это Mainline версия IOS 15")
```

---

## 🔗 Связанные темы
- [[Основы строк]]
- [[Методы строк]]
- [[f-строки]]
- [[Chaining String Methods]]
```