Модули (Modules)


**Модуль** — это Python-файл (`.py`), который предназначен для импорта в другие программы.

```python
# my_lib1.py
def func1():
    print("Hello")

def func2():
    print("Something else")
```


Использование модуля

```python
import my_lib1          # импортируем файл (без расширения .py)

my_lib1.func1()         # Hello
my_lib1.func2()         # Something else
```


Поиск модуля

Модуль ищется через:
- `sys.path`
- `$PYTHONPATH`

```python
import my_lib1
my_lib1.__file__        # '/home/ktbyers/.../my_lib1.py'
```


Структура модуля

В модуле применима та же структура, что и в обычной программе:

```python
# my_lib2.py

# Константы
E = 2.718

# Функции
def func1():
    print("Hello")

def func2():
    print("Something else")

# Классы
class SomeClass:
    def __init__(self):
        self.e = E

    def print_e(self):
        print(f"{self.e}")

def main():
    obj1 = SomeClass()
    obj1.print_e()

if __name__ == "__main__":
    main()
```


Использование модуля

```python
import my_lib2

my_lib2.E               # 2.718
my_lib2.main()          # 2.718
my_lib2.func2()         # Something else
```


📌 Итог

| Что | Описание |
|-----|----------|
| Модуль | Python-файл для импорта |
| Импорт | `import имя_файла` (без `.py`) |
| Поиск | через `sys.path` и `$PYTHONPATH` |
| Структура | Константы, функции, классы, `main()`, `__name__` |

```python
# my_module.py
def my_func():
    return "Hello"

if __name__ == "__main__":
    print(my_func())
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   