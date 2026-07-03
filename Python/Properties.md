**Свойства (Properties)**

Проблема

В классе есть атрибуты, к которым мы обращаемся напрямую:

```python
class TelnetConn:
    def __init__(self, host, username, password):
        self.host = host
        self.username = username
        self.password = password
        self.telnet_conn = Telnet(self.host)
```

Но что, если позже мы захотим **добавить логику** при доступе к атрибуту?  
Например, выводить `"Hello World"` при каждом чтении `host`.


Решение 1: геттеры и сеттеры (плохое)

```python
class TelnetConn:
    def __init__(self, host, ...):
        self._host = host

    def get_host(self):
        print("Hello world")
        return self._host

    def set_host(self, value):
        self._host = value
```

**Недостатки:**
1. Много шаблонного кода для каждого атрибута
2. **Ломает существующий код** — если раньше писали `tc.host`, теперь нужно `tc.get_host()`

```python
(Pdb) tc.host               # ❌ AttributeError
(Pdb) tc.get_host()         # ✅ 'Hello world'
(Pdb) tc.set_host("foo")
```


Решение 2: свойства (`@property`) — правильный путь

**Свойства** позволяют добавить логику при доступе к атрибуту, **не меняя интерфейс** (код продолжает работать как раньше).

```python
class TelnetConn:
    def __init__(self, host, username, password):
        self._host = host          # приватный атрибут

    @property
    def host(self):                # геттер
        print("Hello world")
        return self._host
```

**Использование:**

```python
(Pdb) tc = TelnetConn("rtr2.lasthop.io", ...)
(Pdb) tc.host                    # ✅ вызывается @property
Hello world
'rtr2.lasthop.io'

(Pdb) tc.host = "foo"            # ❌ нет сеттера
AttributeError: property 'host' of 'TelnetConn' object has no setter
```

- Код выглядит как обращение к обычному атрибуту
- Но на самом деле вызывается метод с логикой
- По умолчанию — **только для чтения** (read-only)


📌 Итог

| Подход | Интерфейс | Ломает код | Код при доступе |
|--------|-----------|------------|-----------------|
| Прямой атрибут | `obj.attr` | — | Нет логики |
| Геттеры/сеттеры | `obj.get_attr()` | ✅ Да | Можно добавить |
| `@property` | `obj.attr` | ❌ Нет | Можно добавить |

```python
# Шаблон свойства (только чтение)
class MyClass:
    def __init__(self, value):
        self._value = value

    @property
    def value(self):
        # логика при чтении
        return self._value
```

> [!TIP]
> Используйте `@property`, когда нужно добавить логику при доступе к атрибуту **без изменения API**.

---

## 🔗 Связанные темы
- [[Properties - Setters]]
- [[Property Example]]
- [[Deleters]]
```