Методы списков

Основные методы

| Метод | Что делает | Пример |
|-------|-----------|--------|
| `.append(x)` | Добавляет элемент в конец | `my_list.append("new")` |
| `.clear()` | Удаляет все элементы | `my_list.clear()` |
| `.copy()` | Создаёт копию списка | `new = my_list.copy()` |
| `.count(x)` | Считает вхождения | `my_list.count("hello")` |
| `.extend(iter)` | Добавляет элементы из итерируемого | `my_list.extend([1,2,3])` |
| `.index(x)` | Возвращает индекс первого вхождения | `my_list.index("hello")` |
| `.insert(i, x)` | Вставляет `x` на позицию `i` | `my_list.insert(0, "first")` |
| `.pop(i)` | Удаляет элемент на позиции `i` | `val = my_list.pop()` |
| `.remove(x)` | Удаляет первое вхождение `x` | `my_list.remove("foo")` |
| `.reverse()` | Переворачивает список | `my_list.reverse()` |
| `.sort()` | Сортирует список | `my_list.sort()` |

Ключевые методы
`.append()` — добавление в конец
```python
devices = ["router1", "switch1"]
devices.append("firewall")  # ["router1", "switch1", "firewall"]
```

`.extend()` — расширение списка
```python
devices.extend(["router2", "switch2"])  
# ["router1", "switch1", "firewall", "router2", "switch2"]
```

`.pop()` — удаление с возвратом
```python
last_device = devices.pop()  # "switch2"
first_device = devices.pop(0)  # "router1"
```

`.insert()` — вставка на позицию
```python
devices.insert(1, "core-switch")  # Вставить на вторую позицию
```

`.remove()` — удаление по значению
```python
devices.remove("switch1")  # Удалить "switch1"
```

`.index()` — поиск индекса
```python
pos = devices.index("firewall")  # Найти позицию firewall
```

Cравнение методов

| Метод | Изменяет список | Возвращает |
|-------|----------------|------------|
| `.append()` | ✅ | `None` |
| `.extend()` | ✅ | `None` |
| `.pop()` | ✅ | Удалённый элемент |
| `.insert()` | ✅ | `None` |
| `.remove()` | ✅ | `None` |
| `.index()` | ❌ | Индекс |
| `.count()` | ❌ | Количество |

Практические примеры

Управление списком устройств
```python
# Инициализация
devices = ["router1", "switch1"]

# Добавление
devices.append("firewall")
devices.extend(["router2", "switch2"])

# Удаление
devices.remove("switch1")
last = devices.pop()

# Поиск
if "router1" in devices:
    idx = devices.index("router1")
    print(f"router1 на позиции {idx}")

# Сортировка
devices.sort()  # По алфавиту
devices.reverse()  # Обратный порядок
```

Работа с IP-адресами
```python
# Разбить IP на октеты
ip = "192.168.1.1"
octets = ip.split(".")  # ["192", "168", "1", "1"]

# Изменить последний октет
octets[-1] = "100"
new_ip = ".".join(octets)  # "192.168.1.100"

# Добавить маску
octets.append("24")
```


📌 Итог

| Действие | Метод |
|----------|-------|
| Добавить элемент | `.append(x)` |
| Добавить несколько | `.extend(list)` |
| Удалить последний | `.pop()` |
| Удалить первый | `.pop(0)` |
| Удалить по значению | `.remove(x)` |
| Найти позицию | `.index(x)` |
| Отсортировать | `.sort()` |
| Перевернуть | `.reverse()` |


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   