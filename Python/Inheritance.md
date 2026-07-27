Наследование и иерархия классов (Inheritance)

Наследование — это механизм, позволяющий **одному классу** (дочернему, child) **унаследовать** атрибуты и методы другого класса (родительского, parent).

Базовая иерархия
Все классы в Python автоматически наследуют от базового класса `object`.
```python
# Оба варианта эквивалентны
class ConnectionClass:
    pass

class ConnectionClass(object):
    pass
```


Создание иерархии: родитель → дочерние классы
```python
class ConnectionClass(object):
    def __init__(self, device_type, host, username, password):
        self.device_type = device_type
        self.host = host
        self.username = username
        self.password = password

class TelnetConnection(ConnectionClass):
    pass

class SSHConnection(ConnectionClass):
    pass
```

- `TelnetConnection` и `SSHConnection` **наследуют** метод `__init__()` от `ConnectionClass`
- Не нужно переписывать код — он берётся из родительского класса


Переопределение методов

Дочерний класс может **переопределить** метод родителя:
```python
class TelnetConnection(ConnectionClass):
    def print_host(self):
        print(f"Telnet Connection to: {self.host}")

class SSHConnection(ConnectionClass):
    def print_host(self):
        print(f"SSH Connection to: {self.host}")
```

У каждого дочернего класса своя реализация `print_host()`.

Method Resolution Order (MRO) — порядок поиска методов
При вызове метода Python ищет его **снизу вверх**:

1. В **текущем** классе (дочернем)
2. В **родительском** классе
3. В следующем по иерархии (если есть)

```python
ssh_conn = SSHConnection(
    device_type="cisco_ios",
    host="rtr1.domain.com",
    username="admin",
    password="cisco123"
)

ssh_conn.print_host()   # 'SSH Connection to: rtr1.domain.com'
```

Если метод не найден в `SSHConnection` — Python поднимется в `ConnectionClass`.


Зачем нужно наследование

**DRY — Don't Repeat Yourself**

Наследование позволяет **избежать дублирования кода**:

- Общие атрибуты и методы выносятся в родительский класс
- Каждый дочерний класс добавляет свою специфику
- Изменения в родительском классе автоматически применяются ко всем дочерним


📌 Итог

```python
# Родительский класс
class Animal:
    def speak(self):
        pass

# Дочерние классы
class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"
```

| Термин | Описание |
|--------|----------|
| `object` | Базовый класс в Python |
| Родительский класс | Класс, от которого наследуются |
| Дочерний класс | Класс, который наследует |
| Переопределение | Замена метода родителя в дочернем классе |
| MRO | Порядок поиска методов (снизу вверх) |

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   