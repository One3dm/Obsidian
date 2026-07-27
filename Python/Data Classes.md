Классы данных (Data Classes)

**Data classes** — это способ создания классов с минимальным шаблонным кодом. Вместо написания `__init__`, `__repr__`, `__eq__` вручную, Python генерирует их автоматически.


Синтаксис

```python
from dataclasses import dataclass

@dataclass
class NetDevice:
    device_type: str
    host: str
    username: str
    password: str
```

| Элемент | Описание |
|---------|----------|
| `@dataclass` | Декоратор, превращающий класс в data class |
| `device_type: str` | Поле с указанием типа (type hint) |
| Порядок полей | Определяет порядок аргументов в `__init__` |

> [!NOTE]
> Требуется Python 3.7 или новее.


Эквивалент обычного класса

Data class автоматически генерирует то же самое, что и ручное написание:
```python
class NetDevice:
    def __init__(self, device_type, host, username, password):
        self.device_type = device_type
        self.host = host
        self.username = username
        self.password = password
```


Использование
```python
rtr1 = NetDevice(
    device_type="juniper_junos",
    host="rt1.domain.com",
    username="admin",
    password="cisco123"
)

print(rtr1.device_type)   # 'juniper_junos'
print(rtr1.host)          # 'rt1.domain.com'
```


Значения по умолчанию
```python
@dataclass
class NetDevice:
    host: str
    username: str
    password: str
    device_type: str = "cisco_ios"   # значение по умолчанию
```

> [!IMPORTANT]
> Поля со значениями по умолчанию должны идти **после** полей без значений по умолчанию.



Автоматически генерируемый код

| Метод | Что делает |
|-------|------------|
| `__init__` | Привязывает аргументы к атрибутам |
| `__repr__` | Красивое представление объекта при `print()` |
| `__eq__` | Сравнение объектов по полям |

```python
(Pdb) print(rtr1)
NetDevice(device_type='cisco_ios', host='rt2.domain.com', username='admin', password='csco123')

(Pdb) rtr1 == rtr2   # __eq__ работает автоматически
True
```


Сложные значения по умолчанию
Для изменяемых значений (списки, словари) используется `field(default_factory=...)`:
```python
from dataclasses import dataclass, field
from typing import List

def default_list():
    return []

@dataclass
class NetDevice:
    device_type: str
    host: str
    username: str
    password: str
    interface_ips: List[str] = field(default_factory=default_list)
```


Когда использовать data classes

| За | Против |
|----|--------|
| Меньше кода | Меньше контроля над инициализацией |
| `__repr__` и `__eq__` бесплатно | Для сложной логики лучше обычный класс |
| Идеально для хранения данных | — |

> [!TIP]
> Data classes хороши для **контейнеров данных**. Для сложной логики используйте обычные классы.



📌 Итог

```python
# Data class — минимум кода
@dataclass
class Device:
    name: str
    ip: str
    port: int = 22

# Использование
d = Device("rtr1", "10.0.0.1")
print(d)   # Device(name='rtr1', ip='10.0.0.1', port=22)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python 