Методы (Methods)

Что такое методы

Методы — это функции, которые принадлежат классу и работают с данными объекта. Первый параметр — всегда `self`.

Синтаксис метода

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

    def read(self, sleep=1.5):
        time.sleep(sleep)
        data = self.telnet_conn.read_very_eager().decode()
        return data
```

| Элемент | Описание |
|---------|----------|
| `def` | Ключевое слово |
| `write` / `read` | Имя метода |
| `self` | Первый параметр (ссылка на объект) |
| `data`, `sleep` | Дополнительные параметры |
| `return` | Возвращаемое значение (по умолчанию `None`) |

Вызов методов

```python
tc1 = TelnetConn(host="host1", username="admin", password="cisco123")

# Вызов метода
data = tc1.read()
tc1.write(f"{username}\n")
tc1.login()
```

Синтаксис: `объект.имя_метода(аргументы)`

Последовательный вызов методов

Методы можно вызывать последовательно — это позволяет разбить сложную операцию на шаги.

```python
tc1 = TelnetConn(host=host1, username=username, password=password)

data = tc1.read()                      # читаем приглашение
tc1.write(f"{username}\n")             # отправляем имя
data = tc1.read()                      # читаем запрос пароля
tc1.write(f"{password}\n")             # отправляем пароль
data = tc1.read()                      # читаем приглашение маршрутизатора
```


Метод `login()` — инкапсуляция логики

Метод может скрывать сложность взаимодействия и использовать атрибуты объекта (`self.username`, `self.password`):

```python
def login(self):
    debug = False
    prompt_terminator = r"#"

    data = self.read()
    output = data
    if re.search(r"username", data):
        self.write(self.username + "\n")
        data = self.read()
        output += data
    if re.search(r"ssword", data):
        self.write(self.password + "\n")
        data = self.read()
        output += data

    if debug:
        print(output)

    return bool(re.search(prompt_terminator, data))
```

**Использование:**

```python
tc1 = TelnetConn(host=host1, username=username, password=password)
tc1.login()
```

> [!TIP]
> Пользователю не нужно знать, как именно отправляются имя и пароль — он просто вызывает `.login()`.


📌 Итог

| Концепция | Описание |
|-----------|----------|
| Метод | Функция внутри класса |
| `self` | Ссылка на текущий объект (неявно передаётся при вызове) |
| `return` | Возвращает значение (без `return` — `None`) |
| Вызов | `объект.метод(аргументы)` |
| Инкапсуляция | Методы скрывают детали работы с данными внутри объекта |

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   