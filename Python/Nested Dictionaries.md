**Вложенные словари**

Значением ключа в словаре может быть **другой словарь** (или список, или любая другая структура). Это позволяет создавать иерархические структуры данных.


Пример: словарь, содержащий словари
```python
my_devices = {
    "rtr1": {
        "host": "device1.domain.com",
        "device_type": "cisco_xe",
        "username": "cisco",
        "password": "cisco123"
    },
    "rtr2": {
        "host": "device2.domain.com",
        "device_type": "juniper_junos",
        "username": "junos",
        "password": "vmx123!"
    }
}
```


Доступ к вложенным данным

Используйте **цепочку ключей** в квадратных скобках:
```python
# Внешний ключ "rtr2" → внутренний словарь
my_devices["rtr2"]
# {'host': 'device2.domain.com', 'device_type': 'juniper_junos', ...}

# Затем ключ "device_type" внутри этого словаря
my_devices["rtr2"]["device_type"]   # 'juniper_junos'
```


Вложенность разных типов

Словарь, содержащий списки
```python
sf1 = {
    "routers": ["192.168.1.1", "192.168.1.2"],
    "switches": ["192.168.1.20", "192.168.1.21"]
}
```

Список словарей
```python
devices = [
    {
        "host": "device1.domain.com",
        "device_type": "cisco_xe",
        "username": "cisco",
        "password": "cisco123"
    },
    {
        "host": "device2.domain.com",
        "device_type": "juniper_junos",
        "username": "junos",
        "password": "vmx123!"
    }
]

devices[0]["host"]          # 'device1.domain.com'
devices[1]["device_type"]   # 'juniper_junos'
```

> [!NOTE]
> Вложенность может быть **сколь угодно глубокой**: словари в словарях, списки словарей, словари списков и т.д.


Как выбирать структуру данных

| Тип данных | Подходящая структура |
|------------|----------------------|
| Несколько объектов с одинаковыми полями | Список словарей |
| Группировка по именам (устройства, пользователи) | Словарь словарей |
| Простой список IP-адресов | Список |

Экспериментируйте и выбирайте то, что делает код понятнее.

---

## 📌 Итог

```python
# Словарь словарей
devices = {"rtr1": {"ip": "10.1.1.1", "model": "ISR"}}
devices["rtr1"]["ip"]   # '10.1.1.1'

# Словарь списков
networks = {"routers": ["10.1.1.1", "10.1.1.2"], "switches": ["10.1.1.101"]}
networks["routers"][0]  # '10.1.1.1'

# Список словарей
inventory = [{"name": "rtr1", "ip": "10.1.1.1"}, {"name": "sw1", "ip": "10.1.1.101"}]
inventory[0]["name"]    # 'rtr1'
```

---

## 🔗 Связанные темы
- [[Основы словарей (Dictionary Basics)]]
- [[Словари изменяемы (Mutable)]]
- [[Dictionary Comprehensions]]
```