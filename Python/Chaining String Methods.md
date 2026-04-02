**Цепочки методов строк (Method Chaining)**

Что такое chaining

Методы можно вызывать **последовательно**, один за другим. Результат первого метода становится объектом для второго.

```python
my_var = "Some String"
my_var.lower().strip()
```

Читается **слева направо**:
1. `my_var.lower()` → `"some string"`
2. `"some string".strip()` → `"some string"`

Пример
```python
my_var = "  SOME STRING  "
my_var.lower().strip()
# 'some string'
```

По шагам:
```python
my_var.lower()      # '  some string  '
'  some string  '.strip()   # 'some string'
```

Пример длинной цепочки
```python
# Очистка и форматирование вывода с сетевого устройства
raw_output = "  Gi0/1 is UP, 100 Mbps, Full Duplex  "
clean = raw_output.strip().lower().replace("gi0/1", "GigabitEthernet0/1")
# 'GigabitEthernet0/1 is up, 100 mbps, full duplex'
```

**По шагам:**
1. `.strip()` → `"Gi0/1 is UP, 100 Mbps, Full Duplex"`
2. `.lower()` → `"gi0/1 is up, 100 mbps, full duplex"`
3. `.replace(...)` → `"GigabitEthernet0/1 is up, 100 mbps, full duplex"`

Практические примеры для сетевой автоматизации
```python
# 1. Нормализация имени интерфейса
interface = "  gi0/1  "
normalized = interface.strip().lower().replace("gi", "GigabitEthernet")
# 'GigabitEthernet0/1'

# 2. Обработка MAC-адреса
mac = "  aa:bb:cc:dd:ee:ff  "
clean_mac = mac.strip().lower().replace(":", "")
# 'aabbccddeeff'

# 3. Подготовка команды для отправки
command = "  show interface status  "
prepared = command.strip().upper()
# 'SHOW INTERFACE STATUS'

# 4. Извлечение номера VLAN из строки
vlan_info = "VLAN0010 - Management"
vlan_num = vlan_info.strip().split()[0].replace("VLAN", "")
# '0010'
```

Важно: исходная строка не меняется

Как и с одиночными методами, оригинал остаётся неизменным:
```python
my_var = "Some String"
my_var.lower().strip()   # 'some string'
my_var                   # 'Some String'
```

Чтобы сохранить результат — присвойте переменной:
```python
my_var = my_var.lower().strip()
```

Порядок методов имеет значение
```python
# Разный порядок → разный результат
text = "  HELLO WORLD  "

# Сначала strip, потом replace
text.strip().replace(" ", "_")   # "HELLO_WORLD"

# Сначала replace, потом strip  
text.replace(" ", "_").strip()   # "__HELLO_WORLD" (остались подчёркивания)

# Порядок влияет на результат!
```

Ограничения и лучшие практики

**Когда цепочки становятся слишком длинными:**
```python
# ❌ Слишком длинно и сложно читать
result = input_data.strip().lower().replace(" ", "_").replace(".", "-").upper()

# ✅ Лучше разбить на несколько строк
result = input_data.strip().lower()
result = result.replace(" ", "_")
result = result.replace(".", "-")
result = result.upper()

# ✅ Или использовать временные переменные с понятными именами
cleaned = input_data.strip().lower()
normalized = cleaned.replace(" ", "_").replace(".", "-")
final_result = normalized.upper()
```

**Правило:** Если цепочка требует прокрутки в редакторе кода — она слишком длинная.

📌 Итог

| Правило | Пример | Примечание |
|---------|--------|------------|
| Читаем слева направо | `"  TEXT  ".lower().strip()` | Порядок выполнения важен |
| Каждый метод применяется к результату предыдущего | `"  TEXT  "` → `"  text  "` → `"text"` | Результат промежуточных шагов |
| Исходная строка не изменяется | `my_var` остаётся прежним | Строки immutable |
| **Сохраняйте результат в переменную** | `result = text.strip().lower()` | Иначе изменения теряются |
| **Избегайте слишком длинных цепочек** | Разбивайте на логические шаги | Для читаемости кода |
| **Порядок методов важен** | `strip().replace()` ≠ `replace().strip()` | Тестируйте последовательность |

Ключевые принципы:
1. **Цепочки экономят переменные** — меньше промежуточных присваиваний
2. **Читаемость важнее краткости** — слишком длинные цепочки сложно понимать
3. **Тестируйте порядок** — разные последовательности дают разные результаты
4. **Используйте для простых преобразований** — сложную логику лучше разбивать

Для сетевого инженера:
- Используйте цепочки для быстрой очистки вывода с оборудования
- Нормализуйте имена интерфейсов, MAC-адреса, VLAN
- Но не переусердствуйте — сложные преобразования лучше делать пошагово

> [!TIP]
> Хорошее эмпирическое правило: если цепочка содержит **более 3-4 методов**, подумайте о разбиении на отдельные шаги. Читаемость кода важнее минимализма.

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   