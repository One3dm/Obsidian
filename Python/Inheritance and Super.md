**Наследование и `super()`**

Проблема: как расширить, а не заменить метод родителя

Когда мы переопределяем `__init__` в дочернем классе, родительский `__init__` **не вызывается автоматически**. Если мы хотим **добавить** логику, а не **заменить** её, используем `super()`.


`super()` — вызов метода родительского класса

```python
class ConnectionClass(object):
    def __init__(self, device_type, host, username, password):
        self.device_type = device_type
        self.host = host
        self.username = username
        self.password = password

class SSHConnection(ConnectionClass):
    def __init__(self, device_type, host, username, password, key_file=None):
        if key_file is not None:
            self.key_file = key_file
        # Вызываем родительский __init__
        super().__init__(device_type, host, username, password)
```

**Как работает:**
1. Дочерний класс добавляет параметр `key_file`
2. Обрабатывает его
3. Вызывает родительский `__init__` через `super()`, чтобы остальные атрибуты (`device_type`, `host`, `username`, `password`) были привязаны


Использование `*args` и `**kwargs` для гибкости

Если родительский класс может измениться (добавятся новые параметры), лучше передавать аргументы через `*args` и `**kwargs`:

```python
class SSHConnection(ConnectionClass):
    def __init__(self, *args, **kwargs):
        # Проверяем наличие key_file среди именованных аргументов
        if kwargs.get("key_file"):
            self.key_file = kwargs.pop("key_file")
        # Передаём всё остальное родителю
        super().__init__(*args, **kwargs)
```

> [!TIP]
> Такой подход защищает код от изменений в родительском классе.


Пример: расширение `__repr__`

`super()` можно использовать и в других методах, не только в `__init__`:

```python
class SSHConnection(ConnectionClass):
    def __repr__(self):
        orig = super().__repr__()     # вызываем родительский __repr__
        return f"{orig} --> Hello"    # добавляем свои данные
```

```python
(Pdb) ssh_conn
<__main__.SSHConnection object at 0x7f2734939d10> --> Hello
```


📌 Итог

| Что делает `super()` | Пример |
|----------------------|--------|
| Вызывает метод родительского класса | `super().__init__(*args, **kwargs)` |
| Позволяет **дополнить**, а не заменить | Добавить `key_file`, сохранив родительскую логику |
| Используется в любом методе | `__init__`, `__repr__`, `__str__` и т.д. |
| Хорошо работает с `*args`, `**kwargs` | Устойчивость к изменениям в родителе |

```python
# Шаблон использования super() в __init__
class Child(Parent):
    def __init__(self, *args, **kwargs):
        # дополнительная логика
        super().__init__(*args, **kwargs)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   