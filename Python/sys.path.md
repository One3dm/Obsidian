`sys.path` — как Python находит библиотеки

`sys.path` — это **список директорий**, в которых Python ищет библиотеки при выполнении `import`.

```python
import sys
print(sys.path)
```

Пример вывода:
```python
[
    '',   # текущая директория (пустая строка)
    '/usr/local/lib/python311.zip',
    '/usr/local/lib/python3.11',
    '/usr/local/lib/python3.11/lib-dynload',
    '/home/ktbyers/VENV/lp/lib/python3.11/site-packages'
]
```

Порядок поиска

1. **Встроенные модули** (`built-in`) — например, `sys`, `time`
2. **Директории из `sys.path`** — по порядку

```python
import sys      # built-in
import time     # built-in
```

Где находятся библиотеки

Стандартная библиотека

```python
import math
math.__file__   # '/usr/local/lib/python3.11/lib-dynload/math.cpython-311-x86_64-linux-gnu.so'
```

Библиотеки из PyPI (установленные через pip)

```python
import rich
rich.__file__   # '/home/ktbyers/VENV/lp/lib/python3.11/site-packages/rich/__init__.py'
```

⚠️ Опасность: переопределение стандартных модулей

Если в текущей директории создать файл с именем стандартной библиотеки (например, `math.py`), Python загрузит **ваш** файл вместо стандартного.

```bash
$ ls math.py
math.py

$ python
>>> import math   # загрузится ваш math.py, а не стандартный
Hello              # ваш код в math.py
>>> math.__file__
'/home/ktbyers/EP/math.py'
```

> [!CAUTION]
> **Никогда не называйте свои файлы именами стандартных модулей** (`math.py`, `sys.py`, `re.py` и т.д.).


📌 Итог

| Что делает | Как |
|------------|-----|
| Посмотреть `sys.path` | `import sys; print(sys.path)` |
| Где находится библиотека | `module.__file__` |
| Библиотеки из PyPI обычно лежат | в `site-packages` |
| Первая директория в `sys.path` | текущая (`''`) — поэтому свои модули загружаются первыми |

```python
# sys.path — это список
sys.path.append("/my/custom/path")   # можно добавить свою директорию
```



________________________________________________________________________
Paths: [[Python]]
Tags: #Python   