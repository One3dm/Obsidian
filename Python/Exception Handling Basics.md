Основы обработки исключений

Что такое исключение

**Исключение (exception)** — это ошибка, которая возникает во время выполнения программы.  
Если не обработать исключение, программа аварийно завершится и покажет **traceback** (стек вызовов).

```python
my_list = []
print("Try to access index that doesn't exist")
my_list[0]                     # IndexError: list index out of range
print("Это сообщение не будет напечатано")
```

Вывод:
```
Try to access index that doesn't exist
Traceback (most recent call last):
  File "except.py", line 3, in <module>
    my_list[0]
IndexError: list index out of range
```

---

## Конструкция `try` / `except`

Позволяет **перехватить** исключение и **грациозно** обработать его, не завершая программу.

```python
try:
    my_list[0]                 # строка, которая может вызвать ошибку
except IndexError:             # указываем тип исключения
    print("Gracefully handled error")
```

Если ошибка типа `IndexError` возникает в блоке `try`, выполнение немедленно переходит в блок `except`.

---

## Как работает выполнение

### Случай 1: ошибка есть

```python
try:
    print("Before")           # ✅ выполнится
    my_list[0]                # ❌ ошибка — переходим в except
    print("After")            # ⏩ пропускается
except IndexError:
    print("Gracefully handled error")   # ✅ выполнится

print("Outside block")        # ✅ выполнится всегда
```

Вывод:
```
Before
Gracefully handled error
Outside block
```

### Случай 2: ошибки нет

```python
my_list = ["10.1.1.1"]

try:
    print("Before")           # ✅
    my_list[0]                # ✅ без ошибки
    print("After")            # ✅ выполнится
except IndexError:
    print("Not printed")      # ⏩ пропускается

print("Outside block")        # ✅
```

Вывод:
```
Before
After
Outside block
```

---

## Блок `except` может содержать несколько строк

```python
try:
    my_list[0]
except IndexError:
    print("Gracefully handled error")
    print("More error handling")
    print("Outside block---happens regardless of error")
```

---

## Зачем обрабатывать исключения?

| Без обработки | С обработкой |
|---------------|--------------|
| Программа падает | Программа продолжает работу |
| Пользователь видит traceback | Пользователь видит понятное сообщение |
| Ресурсы могут не освободиться | Можно корректно закрыть файлы/соединения |

---

## 📌 Итог

```python
try:
    # Код, который может вызвать ошибку
    risky_operation()
except SomeException:
    # Код, который выполнится, если ошибка произошла
    handle_error()

# Код после try/except выполняется в любом случае
```

> [!TIP]
- Указывайте **конкретный тип исключения** (`IndexError`, `KeyError`, `ValueError` и т.д.)
- Не используйте `except:` без указания типа — это отлавливает **все** ошибки и может скрыть проблемы

---

## 🔗 Связанные темы
- [[Handling Multiple Exception Types]]
- [[Broad Exception Handling]]
- [[Finally]]
```