**Импорт библиотек и имена**

Способы импорта

1. `import re` — доступ через префикс

Все имена из библиотеки доступны с префиксом `re.`

```python
import re

re.search        # <function re.search(...)>
re.findall       # <function re.findall(...)>
re.MULTILINE     # re.MULTILINE
```

**Преимущество:** понятно, откуда взялась функция.


2. `from re import search, findall, MULTILINE` — прямой импорт

Имена импортируются напрямую в вашу программу.

```python
from re import search, findall, MULTILINE

search        # <function re.search(...)>
findall       # <function re.findall(...)>
MULTILINE     # re.MULTILINE
```

> [!NOTE]
> Python всё равно **полностью загружает** весь модуль `re` (построчно), это меняет только **имена**.


3. Изменение имени при импорте (`as`)

```python
# Импорт библиотеки с новым именем
import re as my_re

my_re.search    # работает как re.search

# Импорт функции с новым именем
from re import search as re_search

re_search       # работает как re.search
```


❌ Что почти никогда не нужно делать: `from re import *`

```python
from re import *
```

**Почему плохо:**
1. Непонятно, откуда взялись функции (`search`, `findall` — из `re` или из вашего кода?)
2. Можно случайно переопределить имена
3. Затрудняет поиск в коде (где определена функция?)

> [!WARNING]
> `from x import *` — антипаттерн. Избегайте его, особенно в больших проектах.


## 📌 Итог

| Способ | Пример | Когда использовать |
|--------|--------|---------------------|
| `import lib` | `import re` | Всегда, если нужно несколько функций |
| `from lib import name` | `from re import search` | Если нужна одна-две функции |
| `import lib as name` | `import re as my_re` | Если имя библиотеки длинное или конфликтует |
| `from lib import *` | `from re import *` | ❌ Почти никогда |


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   