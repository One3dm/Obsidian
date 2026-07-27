Атрибуты класса (Class Attributes)


**Атрибуты класса** — это переменные, которые являются **общими для всех экземпляров** класса. Они определяются на уровне класса, а не внутри `__init__`.

```python
class TelnetConn:
    # Атрибут класса — общий для всех объектов
    conns = 0

    def __init__(self, host, username, password):
        self._host = host
        self.username = username
        self.password = password
        self.open()

    def open(self):
        self.telnet_conn = Telnet(self._host)
        self.login()
        TelnetConn.conns += 1   # доступ через имя класса
        print(f"Количество подключений: {self.conns}")
```

Доступ к атрибутам класса

1. Через имя класса (рекомендуется)
```python
TelnetConn.conns += 1
```

2. Через `self` (только для чтения)
```python
print(self.conns)   # работает, но осторожно — если есть атрибут экземпляра с тем же именем, он перекроет
```

> [!WARNING]
> Если через `self` **присвоить** значение — создастся **атрибут экземпляра**, который перекроет атрибут класса.


Конфликт атрибутов: экземпляр перекрывает класс
```python
class A:
    my_attr = 100

    def __init__(self):
        self.my_attr = 10   # создаёт атрибут экземпляра

obj = A()
print(obj.my_attr)   # 10 (атрибут экземпляра перекрыл классовый)
```

**Правило:** если есть атрибут экземпляра, Python использует его. Если нет — ищет в классе.


Пример использования: адреса дата-центров
```python
class NetDevice:
    # Общий словарь для всех устройств
    addresses = {
        "sf1": "365 Main Street, San Francisco, CA 94105",
        "sf2": "200 Paul Avenue, San Francisco, CA 94124",
        "la1": "600 West 7th Street, Los Angeles, CA 90017",
    }

    def __init__(self, device_type, host, username, password, data_center):
        self.device_type = device_type
        self.host = host
        self.username = username
        self.password = password
        self.data_center = data_center
        self.address = NetDevice.addresses[data_center]   # доступ через имя класса

rt1 = NetDevice(
    device_type="juniper_junos",
    host="rt1.domain.com",
    username="admin",
    password="jnpr123",
    data_center="la1"
)

print(rt1.address)   # 600 West 7th Street, Los Angeles, CA 90017
```

**Результат:**

```
RTR1 DC: la1
RTR1 Address: 600 West 7th Street, Los Angeles, CA 90017
```


📌 Итог

| Свойство | Атрибут класса | Атрибут экземпляра |
|----------|----------------|-------------------|
| Где определяется | На уровне класса | Внутри `__init__` через `self` |
| Принадлежит | Всем объектам класса | Конкретному объекту |
| Доступ | `ClassName.attr` или `self.attr` | `self.attr` |
| При присваивании через `self` | Создаётся атрибут экземпляра, перекрывающий классовый | — |

```python
class Counter:
    count = 0

    def __init__(self):
        Counter.count += 1   # увеличиваем счётчик при создании каждого объекта
```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python 