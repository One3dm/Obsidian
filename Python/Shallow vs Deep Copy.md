Мелкая и глубокая копия

Проблема с вложенными списками
```python
# Список с вложенными списками
configs = [
    ["hostname Router1", "interface GigabitEthernet0/1"],
    ["hostname Switch1", "interface Vlan1"]
]
```


Мелкая копия (shallow copy)
```python
backup = configs.copy()  # или backup = configs[:]
```

**Что происходит:**
- Внешний список — новый
- Внутренние списки — те же объекты

```python
# Изменение внутреннего списка
backup[0].append("ip address 192.168.1.1")

print(configs[0])  
# ["hostname Router1", "interface GigabitEthernet0/1", "ip address 192.168.1.1"]
# Оригинал тоже изменился!
```


Глубокая копия (deep copy)
```python
import copy

backup = copy.deepcopy(configs)
```

**Что происходит:**
- Внешний список — новый
- Внутренние списки — тоже новые
```python
# Теперь изменения независимы
backup[0].append("ip address 192.168.1.1")

print(configs[0])  
# ["hostname Router1", "interface GigabitEthernet0/1"] — не изменился
```


Когда что использовать

Мелкая копия (`.copy()` или `[:]`)
```python
# ✅ Подходит для простых списков
devices = ["router1", "switch1", "firewall"]
backup = devices.copy()
```

Глубокая копия (`copy.deepcopy()`)
```python
# ✅ Нужна для вложенных изменяемых объектов
import copy

network_config = [
    ["router1", ["192.168.1.1", "192.168.1.2"]],
    ["switch1", ["192.168.2.1", "192.168.2.2"]]
]

backup = copy.deepcopy(network_config)
```


Практические примеры

Конфигурация устройств
```python
import copy

# Базовая конфигурация
base_config = [
    ["hostname DEVICE", "interface GigabitEthernet0/1"],
    ["hostname DEVICE", "interface Vlan10"]
]

# Создаём копию для конкретного устройства
router_config = copy.deepcopy(base_config)

# Модифицируем для роутера
router_config[0][0] = "hostname Router1"
router_config[0].append("ip address 192.168.1.1 255.255.255.0")

# Базовая конфигурация осталась неизменной
print(base_config[0][0])      # "hostname DEVICE"
print(router_config[0][0])    # "hostname Router1"
```

Топология сети
```python
import copy

# Топология с детальной информацией
topology = [
    ["router1", {"interfaces": ["Gig0/1", "Gig0/2"], "neighbors": ["switch1"]}],
    ["switch1", {"interfaces": ["Gi1/0/1", "Gi1/0/2"], "neighbors": ["router1", "switch2"]}]
]

# Создаём копию для модификации
modified_topology = copy.deepcopy(topology)

# Добавляем новый интерфейс только в копию
modified_topology[0][1]["interfaces"].append("Gig0/3")

# Оригинал не изменился
print(topology[0][1]["interfaces"])       # ["Gig0/1", "Gig0/2"]
print(modified_topology[0][1]["interfaces"])  # ["Gig0/1", "Gig0/2", "Gig0/3"]
```


📌 Итог

| Тип копии | Метод | Когда использовать |
|-----------|-------|-------------------|
| **Мелкая** | `.copy()` или `[:]` | Простые списки (без вложенных изменяемых объектов) |
| **Глубокая** | `copy.deepcopy()` | Списки с вложенными списками/словарями |

**Правило:** Если есть вложенные изменяемые объекты — используйте `deepcopy`.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   