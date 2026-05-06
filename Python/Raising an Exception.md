Поднятие исключений (Raising Exceptions)

Зачем это нужно

Иногда в своём коде вы сталкиваетесь с ситуацией, которая **не должна происходить** — например, получены неверные данные. В этом случае можно **самостоятельно поднять исключение**, чтобы программа зафиксировала ошибку.

```python
ip_addr = "192.168.10.1"

if "10.88" not in ip_addr:
    raise ValueError(f"Invalid IP address used: {ip_addr}")
```

Вывод:
```
ValueError: Invalid IP address used: 192.168.10.1
```


Синтаксис

```python
raise Тип_исключения("Сообщение об ошибке")
```

- `Тип_исключения` — встроенный тип (`ValueError`, `TypeError`, `KeyError` и т.д.)
- Сообщение — строка, которая будет показана в traceback


Примеры

```python
# Проверка типа данных
if not isinstance(port, int):
    raise TypeError(f"Port must be integer, got {type(port)}")

# Проверка диапазона
if port < 1 or port > 65535:
    raise ValueError(f"Invalid port number: {port}")

# Проверка наличия ключа
if device_name not in devices:
    raise KeyError(f"Device {device_name} not found")
```


Создание своих типов исключений (опционально)

Можно создавать **собственные классы исключений**, но это более актуально при разработке библиотек и модулей.

```python
class NetworkError(Exception):
    pass

raise NetworkError("Connection timeout")
```



## Когда использовать

| Ситуация | Что делать |
|----------|------------|
| Функция получила неправильный аргумент | `raise TypeError` или `ValueError` |
| Не хватает прав для операции | `raise PermissionError` |
| Ресурс недоступен | `raise ConnectionError` |
| Данные не найдены | `raise KeyError` или `LookupError` |


## 📌 Итог

```python
# Поднятие встроенного исключения
if invalid_condition:
    raise ValueError("Something went wrong")

# Создание и поднятие своего исключения (редко)
class MyCustomError(Exception):
    pass

raise MyCustomError("Custom error message")
```

> [!IMPORTANT]
> Поднимайте исключения, когда дальнейшее выполнение кода **невозможно** или **небезопасно**. Не используйте `raise` для обычного потока управления.



________________________________________________________________________
Paths: [[Python]]
Tags: #Python   
