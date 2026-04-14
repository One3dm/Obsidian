Изменяемые объекты

Основная идея

**Изменяемый объект можно изменить после создания.** Объект остаётся тем же в памяти.

Примеры:
- `list` (списки)
- `dict` (словари)
- `set` (множества)


Как это работает
```python
# Пример с list
devices = ["router1", "switch1"]
old_id = id(devices)  # Запоминаем адрес

devices.append("firewall")  # Изменяем существующий объект
new_id = id(devices)        # Адрес не изменился

print(old_id == new_id)  # True
```


Ключевые особенности
1. Опасность простого присваивания
```python
# ❌ Опасный код
devices = ["router1", "switch1"]
backup = devices  # Это не копия!

devices.append("firewall")
print(backup)  # ["router1", "switch1", "firewall"] — тоже изменился!
```

2. Нужно копировать явно
```python
# ✅ Правильно
devices = ["router1", "switch1"]
backup = devices.copy()  # Или backup = devices[:]

devices.append("firewall")
print(backup)  # ["router1", "switch1"] — не изменился
```

3. Методы изменяют объект
```python
config = ["hostname Router1"]
config.append("interface GigabitEthernet0/1")  # Изменяет существующий список
```

Практическое применение

Управление конфигурацией
```python
# Базовая конфигурация
base_config = ["hostname DEVICE"]

# Создаём копию для модификации
router_config = base_config.copy()
router_config[0] = "hostname Router1"
router_config.append("ip address 192.168.1.1")

switch_config = base_config.copy()
switch_config[0] = "hostname Switch1"
switch_config.append("interface Vlan10")

print(base_config)      # ["hostname DEVICE"]
print(router_config)    # ["hostname Router1", "ip address 192.168.1.1"]
print(switch_config)    # ["hostname Switch1", "interface Vlan10"]
```


📌 Итог

| Тип | Пример | Можно изменить? | При "изменении" |
|-----|--------|----------------|-----------------|
| **Изменяемые** | `list`, `dict`, `set` | ✅ Да | Изменяется существующий объект |

**Важно:** Всегда используйте `.copy()` или `[:]` для создания независимых копий.



________________________________________________________________________
Paths: [[Python]]
Tags: #Python   