**Делетеры свойств (Property Deleters)**

Что такое делетер

Делетер — это метод, который вызывается при попытке **удалить** атрибут с помощью оператора `del`.

Декоратор: `@имя_свойства.deleter`


Синтаксис
```python
class TelnetConn:
    def __init__(self, host, username, password):
        self._host = host
        self.username = username
        self.password = password
        self._open()

    @property
    def host(self):
        return self._host

    @host.setter
    def host(self, value):
        if value != self._host:
            self._host = value
            del self.telnet_conn
            self._open()

    @host.deleter
    def host(self):
        print("Delete host attribute")
        del self._host
```


Пример использования

```python
(Pdb) tc = TelnetConn("rtr2.lasthop.io", ...)
(Pdb) tc.host
'rtr2.lasthop.io'

(Pdb) del tc.host
Delete host attribute
```

При вызове `del tc.host`:
1. Выполняется код в делетере
2. Выводится сообщение `"Delete host attribute"`
3. Удаляется приватный атрибут `self._host`


Полная структура свойства (геттер + сеттер + делетер)

```python
class MyClass:
    def __init__(self):
        self._value = 0

    @property
    def value(self):
        """Геттер — при чтении"""
        return self._value

    @value.setter
    def value(self, new_value):
        """Сеттер — при присваивании"""
        self._value = new_value

    @value.deleter
    def value(self):
        """Делетер — при удалении"""
        print("Deleting value")
        del self._value
```


📌 Итог

| Декоратор | Когда вызывается | Оператор |
|-----------|------------------|----------|
| `@property` | Чтение атрибута | `obj.attr` |
| `@attr.setter` | Присваивание | `obj.attr = value` |
| `@attr.deleter` | Удаление атрибута | `del obj.attr` |

> [!TIP]
> Делетеры полезны, когда при удалении атрибута нужно выполнить дополнительную очистку (закрыть ресурсы, удалить соединения и т.п.).



________________________________________________________________________
Paths: [[Python]]
Tags: #Python   