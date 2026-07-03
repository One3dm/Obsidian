`__init__` и создание объектов

Синтаксис класса
```python
class TelnetConn:
    def __init__(self, host, username, password):
        self.host = host
        self.username = username
        self.password = password
        self.telnet_conn = Telnet(self.host)

    def write(self, data):
        byte_data = data.encode()
        self.telnet_conn.write(byte_data)
```

| Элемент | Описание |
|---------|----------|
| `class TelnetConn:` | Ключевое слово `class` и имя класса |
| `def __init__(self, ...):` | Метод инициализации (dunder-init) |
| `self.host = host` | Привязка аргументов к объекту (создание атрибутов) |
| `self.telnet_conn = ...` | Инициализация других атрибутов |

Что делает `__init__`

- **Принимает аргументы** (правила те же, что для функций, но с `self`)
- **Привязывает аргументы к объекту** через `self`
- **Создаёт атрибуты** (переменные, привязанные к объекту)


Создание объекта

```python
tc = TelnetConn(
    host="host1.domain.com",
    username="admin",
    password="cisco123",
)
```

**Процесс:**

1. Вызывается `__new__` — создаёт объект в памяти
2. Вызывается `__init__` — передаёт ссылку на объект (через `self`)
3. После завершения `__init__` переменная `tc` ссылается на готовый объект

> [!IMPORTANT]
> `self` передаётся **автоматически** — мы его не указываем при вызове.


Аргументы при создании объекта

```python
# Можно передавать как позиционные, так и именованные аргументы
tc = TelnetConn("host1.domain.com", "admin", "cisco123")
tc = TelnetConn(host="host1.domain.com", username="admin", password="cisco123")
```

- Работают **значения по умолчанию**, если они определены в `__init__`
- Не нужно передавать `self`


Несколько объектов одного класса

```python
tc1 = TelnetConn(host="host1.domain.com", username="admin", password="cisco123")
tc2 = TelnetConn(host="host2.domain.com", username="admin", password="cisco123")
```

Каждый объект — **независимый контейнер** со своими атрибутами.

Что такое `self`

- `self` — **ссылка на текущий объект в памяти**
- Внутри класса `self` используется для доступа к атрибутам и методам
- Снаружи объект доступен через имя переменной (например, `tc`)

```python
# tc и self указывают на один и тот же объект в памяти
tc = TelnetConn(...)   # tc → объект
# внутри __init__: self → тот же объект
```


📌 Итог
```python
# Создание класса
class MyClass:
    def __init__(self, value):
        self.value = value   # атрибут объекта

# Создание объекта
obj = MyClass(42)
print(obj.value)   # 42
```

| Концепция | Объяснение |
|-----------|------------|
| `class` | Чертёж для объектов |
| `__init__` | Конструктор (инициализация объекта) |
| `self` | Ссылка на текущий объект |
| Атрибут | Переменная, принадлежащая объекту |


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   