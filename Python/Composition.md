Композиция (Composition)


**Композиция** — это способ организации объектов, когда один объект **содержит** другой объект в качестве атрибута (отношения **«has-a»**).

В отличие от наследования (отношения **«is-a»**), композиция не создаёт жёсткой иерархии.

Пример: NetworkDevice содержит объект connection

```python
class NetworkDevice:
    def __init__(self, device_type, host, username, password, conn_type="ssh"):
        self.device_type = device_type
        self.host = host

        if conn_type == "ssh":
            self.connection = SSHConn(device_type, host, username, password)
        elif conn_type == "telnet":
            self.connection = TelnetConn(device_type, host, username, password)
```

- `NetworkDevice` **имеет** (`has-a`) объект `connection`
- `SSHConn` и `TelnetConn` — отдельные классы, независимые от `NetworkDevice`


Почему композиция лучше, чем умножение классов

**Проблема:** если использовать только наследование, пришлось бы создавать классы для каждой комбинации:

```
CiscoXESSH, CiscoXETelnet, CiscoXESerial,
JunosSSH, JunosTelnet, JunosSerial,
AristaSSH, AristaTelnet, AristaSerial,
...
```

**Решение:** разделить две иерархии:
- **Иерархия устройств** (Cisco, Junos, Arista) — наследование
- **Иерархия подключений** (SSH, Telnet, Serial) — композиция

```python
class NetworkDevice:
    def __init__(self, connection):
        self.connection = connection

    def write(self, data):
        self.connection.write(data)

    def read(self):
        return self.connection.read()

class CiscoXE(NetworkDevice):
    """Cisco XE специфичные поведения"""
    pass

class Junos(NetworkDevice):
    """Junos специфичные поведения"""
    pass
```


Композиция в действии

```python
# Создаём SSH-соединение
ssh_conn = SSHConn(device_type="cisco_ios", host="rtr1.domain.com", username="admin", password="cisco123")

# Создаём устройство с этим соединением
device = CiscoXE(connection=ssh_conn)

# Вызов методов делегируется объекту connection
device.write("show version\n")
data = device.read()
```



Композиция vs Наследование

| Аспект | Наследование | Композиция |
|--------|--------------|------------|
| Отношение | «is-a» (является) | «has-a» (имеет) |
| Связь | Жёсткая (tight coupling) | Гибкая (loose coupling) |
| Код | Меньше кода, но неявный | Более явный |
| Изменения | Изменение в родителе влияет на всех | Объекты можно заменять независимо |
| Гибкость | Менее гибкая | Более гибкая |


Пример из Nornir (библиотека автоматизации)

Nornir использует **композицию** для основного объекта:

```python
class Nornir(object):
    def __init__(self, inventory, config=None, data=None, processors=None, runner=None):
        self.data = data if data is not None else GlobalState()
        self.inventory = inventory
        self.config = config or Config()
        self.processors = processors or Processors()
        self.runner = runner
```

Но при этом Nornir также использует **наследование** для `Host`:

```python
class Host(InventoryElement):
    pass
```

**Вывод:** можно использовать **и наследование, и композицию** вместе. Выбор зависит от задачи.


📌 Итог

```python
# Композиция — объект содержит другие объекты
class Car:
    def __init__(self):
        self.engine = Engine()
        self.wheels = [Wheel() for _ in range(4)]
```

| Когда использовать | Наследование | Композиция |
|--------------------|--------------|------------|
| Чёткая иерархия | ✅ | ❌ |
| Гибкая замена поведения | ❌ | ✅ |
| Повторное использование кода | ✅ | ✅ |
| Слабая связь между классами | ❌ | ✅ |

> [!TIP]
> Если сомневаетесь, начинайте с **композиции**. Она проще в поддержке и изменении.



________________________________________________________________________
Paths: [[Python]]
Tags: #Python   