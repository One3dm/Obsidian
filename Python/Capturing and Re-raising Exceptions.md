Перехват и повторное поднятие исключений

Перехват исключения как переменной

Иногда нужно не просто обработать ошибку, но и **получить информацию** о ней — текст ошибки, тип, атрибуты.

Для этого после типа исключения пишут `as имя_переменной`:

```python
try:
    my_list[0]
except IndexError as e:
    print(f"Info about exception: {str(e)}")
```

Вывод:
```
Info about exception: list index out of range
```

> [!TIP]
> Переменная `e` содержит объект исключения. `str(e)` возвращает сообщение об ошибке.


Повторное поднятие исключения — `raise`

Иногда нужно:
- Сделать часть обработки (логирование, уборку ресурсов)
- **Затем** передать ошибку дальше, чтобы программа всё равно завершилась или вышестоящий код её обработал

Для повторного поднятия того же исключения используется `raise` без аргументов:

```python
try:
    my_list[0]
except IndexError as e:
    print(f"Info about exception: {str(e)}")
    raise                     # повторно поднимаем IndexError
```

Вывод:
```
Info about exception: list index out of range
IndexError: list index out of range    ← программа завершается
```


Поднятие другого исключения

Можно перехватить один тип ошибки и поднять **совсем другой**:

```python
try:
    my_list[0]
except IndexError:
    raise ValueError("Something went wrong with index")
```

> [!NOTE]
> Исходный `IndexError` будет скрыт. В реальном коде часто лучше сохранять исходное исключение как контекст (в Python 3 можно использовать `raise ... from ...`).



Типичные сценарии

| Задача | Решение |
|--------|---------|
| Записать в лог и продолжить работу | `except: log(e)` (без `raise`) |
| Записать в лог, но программа должна упасть | `except: log(e); raise` |
| Преобразовать ошибку в другой тип | `except OldError: raise NewError(...)` |


📌 Итог

```python
# Перехват исключения в переменную
try:
    risky_code()
except SomeError as e:
    print(f"Error message: {e}")

# Перехват, обработка и повторное поднятие
try:
    risky_code()
except SomeError as e:
    log_error(e)
    raise                     # программа всё равно упадёт

# Перехват одного исключения, поднятие другого
try:
    risky_code()
except ValueError:
    raise TypeError("Converted error")
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   