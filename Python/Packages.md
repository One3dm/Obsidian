Пакеты (Packages)


**Пакет** — это директория, которая содержит один или несколько модулей (Python-файлов) и может быть импортирована. Внутри пакета могут быть подпакеты (вложенные директории).


Структура пакета

```
pkg1/
    __init__.py
    mod1.py
    mod2.py
```


Файл `__init__.py`

- **Специальный файл**, который обрабатывается при импорте пакета
- Может быть **пустым** (но должен существовать)
- В Python 3.8+ `__init__.py` **не обязателен**, но рекомендуется для явного обозначения пакета

```python
# pkg1/__init__.py
print("Processing package...")

>>> import pkg1
Processing package...
```


Без `__init__.py` — доступ через dot-нотацию

Если `__init__.py` нет, пакет всё равно работает — модули доступны через `пакет.модуль`.

```python
>>> import pkg2.mod1
pkg2, mod1
```


Использование `__init__.py` для интеграции

Часто в `__init__.py` импортируют функции из модулей, чтобы пользователю не нужно было импортировать каждый модуль отдельно.

```python
# pkg3/__init__.py
from pkg3.mod1 import my_func
from pkg3.mod2 import alt_func

# Теперь пользователь может:
>>> import pkg3
>>> pkg3.my_func()
```


Переменная `__all__`

Контролирует, что будет импортировано при `from pkg import *`:

```python
# pkg4/__init__.py
from pkg4.mod1 import my_func, ALT_CONST
from pkg4.mod2 import alt_func

CONST = "Learning Python"

__all__ = ["my_func", "alt_func", "ALT_CONST", "CONST"]
```

**Использование:**

```python
>>> from pkg4 import *
>>> my_func()          # ✅ работает
>>> ALT_CONST          # ✅ работает
```

> [!WARNING]
> `from pkg import *` — не рекомендуется в коде, но может быть полезно для библиотек.


Подпакеты

Внутри пакета могут быть вложенные пакеты (подпакеты):

```
pkg5/
    __init__.py
    cisco/
        __init__.py
    juniper/
        __init__.py
```

```python
# pkg5/__init__.py
from pkg5.cisco import cisco_func1
from pkg5.juniper import juniper_func1
```

```python
>>> import pkg5
>>> pkg5.juniper_func1()
Juniper here
```


📌 Итог

| Компонент | Описание |
|-----------|----------|
| Пакет | Директория с модулями |
| `__init__.py` | Инициализирует пакет |
| `__all__` | Список имён для `from pkg import *` |
| Подпакет | Вложенная директория (пакет в пакете) |

```python
# Структура пакета
my_package/
    __init__.py
    module1.py
    module2.py
    subpackage/
        __init__.py
        module3.py
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   