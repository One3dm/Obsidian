Возвращаемые значения (Return Values)

Что делает `return`

Функция может **возвращать** результат своей работы с помощью ключевого слова `return`. Этот результат можно сохранить в переменную или использовать дальше.

```python
def test_func(x, y, z):
    return x + y + z

result = test_func(7, 9, 1)
print(result)   # 17
```


Если нет `return`

Если в функции нет `return`, она неявно возвращает `None`:

```python
def say_hello(name):
    print(f"Hello, {name}")

result = say_hello("Alice")
print(result)   # None
```

Несколько возвращаемых значений

Функция может возвращать несколько значений (как кортеж):

```python
def get_min_max(numbers):
    return min(numbers), max(numbers)

min_val, max_val = get_min_max([1, 5, 3, 9, 2])
print(min_val, max_val)   # 1 9
```



📌 Итог

| Случай | Что возвращает |
|--------|----------------|
| Есть `return выражение` | Значение выражения |
| Есть `return` без выражения | `None` |
| Нет `return` | `None` |

```python
def add(a, b):
    return a + b

def do_nothing():
    pass

print(add(2, 3))    # 5
print(do_nothing()) # None
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   