Изменяемые объекты

Основные изменяемые типы
- **Списки** (`list`)
- **Словари** (`dict`)
- **Множества** (`set`)


Списки — изменяемые

Список можно изменять без создания нового объекта:
```python
devices = ["router1", "switch1", "firewall"]
old_id = id(devices)  # Запоминаем адрес

devices.append("loadbalancer")  # Изменяем список
new_id = id(devices)            # Адрес не изменился

print(old_id == new_id)  # True
```

Ключевое отличие от неизменяемых
```python
# Неизменяемый (int)
a = 5
b = a      # b ссылается на тот же объект
a = 10     # Создаётся новый объект для a
print(b)   # 5 (b не изменился)

# Изменяемый (list)
x = [1, 2, 3]
y = x      # y ссылается на тот же объект
x.append(4) # Изменяем объект
print(y)   # [1, 2, 3, 4] (y тоже изменился!)
```

Опасность простого присваивания
```python
# ❌ Опасный код
devices = ["router1", "switch1"]
backup = devices  # Это не копия!

devices.append("firewall")
print(backup)  # ["router1", "switch1", "firewall"] — тоже изменился!
```


Правильное копирование
```python
# ✅ Правильно — создаём копию
devices = ["router1", "switch1"]
backup = devices.copy()  # Или backup = devices[:]

devices.append("firewall")
print(devices)   # ["router1", "switch1", "firewall"]
print(backup)    # ["router1", "switch1"] — не изменился
```


Практические примеры

Работа с конфигурацией
```python
# Исходная конфигурация
base_config = ["hostname Router1", "interface GigabitEthernet0/1"]

# Создаём копию для модификации
device_config = base_config.copy()

# Добавляем специфичные команды
device_config.append("ip address 192.168.1.1 255.255.255.0")
device_config.append("no shutdown")

print(base_config)      # Оригинал не изменился
print(device_config)    # Модифицированная конфигурация
```

Управление списком устройств
```python
# Основной список
all_devices = ["router1", "switch1", "firewall"]

# Создаём подмножество
routers = all_devices.copy()
routers.remove("switch1")
routers.remove("firewall")

print(all_devices)  # ["router1", "switch1", "firewall"]
print(routers)      # ["router1"]
```


📌 Итог

| Действие | Результат |
|----------|-----------|
| `b = a` (для списка) | `b` и `a` ссылаются на один объект |
| `b = a.copy()` | `b` — независимая копия |
| `b = a[:]` | `b` — независимая копия |
| `a.append(x)` | Изменяет и `a`, и `b` (если `b = a`) |

**Важно:** Всегда используйте `.copy()` или `[:]` для создания независимых копий списков.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   