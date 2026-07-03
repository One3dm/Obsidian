**Сеттеры свойств и пример использования**

Создание сеттера

Чтобы сделать свойство **изменяемым**, добавляем сеттер с декоратором `@имя_свойства.setter`.

```python
class TelnetConn:
    def __init__(self, host, username, password):
        self._host = host
        self.username = username
        self.password = password
        self._open()                     # создаём соединение при создании объекта

    @property
    def host(self):                      # геттер (только чтение)
        return self._host

    @host.setter
    def host(self, value):               # сеттер (изменение)
        if value != self._host:
            self._host = value
            del self.telnet_conn         # удаляем старое соединение
            self._open()                 # создаём новое
```


Зачем это нужно

В примере с телнет-соединением: если мы меняем атрибут `host`, то:
- Старое телнет-соединение становится недействительным
- Сеттер автоматически **пересоздаёт соединение** с новым хостом


Пример работы

```python
# Начальное соединение с rt2.lasthop.io
(Pdb) tc.host
'rt2.lasthop.io'
(Pdb) tc.write("\n")
(Pdb) print(tc.read())
rt2#

# Меняем хост (и соединение пересоздаётся автоматически)
(Pdb) tc.host = "rt1.lasthop.io"
(Pdb) tc.write("\n")
(Pdb) print(tc.read())
rt1#
```

**Что произошло:** при присвоении `tc.host = "rt1.lasthop.io"`:
1. Сработал сеттер `@host.setter`
2. Удалилось старое соединение (`del self.telnet_conn`)
3. Создалось новое соединение с `rt1.lasthop.io`


Полная структура свойства с геттером и сеттером

```python
class MyClass:
    def __init__(self):
        self._value = 0

    @property
    def value(self):
        """Геттер — вызывается при чтении"""
        return self._value

    @value.setter
    def value(self, new_value):
        """Сеттер — вызывается при присваивании"""
        if new_value != self._value:
            # дополнительная логика
            self._value = new_value
```


📌 Итог

| Декоратор | Когда вызывается |
|-----------|------------------|
| `@property` | При чтении атрибута (`obj.attr`) |
| `@attr.setter` | При присваивании атрибута (`obj.attr = value`) |

> [!TIP]
> Свойства позволяют **инкапсулировать логику** (например, пересоздание соединения) за простым интерфейсом доступа к атрибуту.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   