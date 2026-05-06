Блок `finally`


`finally` — это блок, который выполняется **всегда**, независимо от того, произошло исключение в `try` или нет.

```python
try:
    my_list[0]                     # может быть IndexError
except IndexError:
    print("Wrong index...again")
finally:
    print("This always happens.")  # выполнится в любом случае
```



Пример 1: исключение произошло

```python
my_list = []

try:
    my_list[0]          # ❌ IndexError
except IndexError:
    print("Handler: index error")
finally:
    print("Finally: cleanup")

print("After try/except/finally")
```

Вывод:
```
Handler: index error
Finally: cleanup
After try/except/finally
```


Пример 2: исключения нет

```python
my_list = ["10.1.1.1"]

try:
    my_list[0]          # ✅ ok
except IndexError:
    print("Handler: index error")   # не выполнится
finally:
    print("Finally: cleanup")

print("After try/except/finally")
```

Вывод:
```
Finally: cleanup
After try/except/finally
```


Типичные сценарии использования `finally`

| Сценарий | Что делаем в `finally` |
|----------|------------------------|
| Работа с файлами | Закрыть файл |
| Сетевые соединения | Закрыть сокет |
| Подключение к БД | Закрыть соединение |
| Временные блокировки | Освободить ресурс |

```python
f = open("file.txt", "r")
try:
    data = f.read()
except IOError:
    print("Error reading file")
finally:
    f.close()          # закрыть файл в любом случае
```



`finally` vs код после `try/except`

**Разница:** код в `finally` выполнится **даже если** в `except` произойдёт новое исключение или будет `return`.

```python
def test():
    try:
        return 1
    finally:
        print("Finally runs before return")   # ✅ выполнится
    print("This never runs")                  # ❌ не выполнится
```


Полная конструкция `try/except/else/finally`

```python
try:
    # код, который может вызвать ошибку
except SomeError:
    # обработка ошибки
else:
    # выполнится, если ошибки не было
finally:
    # выполнится всегда
```


📌 Итог

```python
try:
    risky_operation()
except Exception:
    handle_error()
finally:
    cleanup()   # всегда: закрыть файл, освободить ресурс
```

> [!TIP]
> `finally` — идеальное место для **освобождения ресурсов** (файлы, соединения, блокировки), которые должны быть закрыты независимо от того, была ошибка или нет.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   