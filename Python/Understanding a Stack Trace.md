**Понимание трассировки стека (Stack Trace)**


**Трассировка стека (stack trace)** — это отчёт об ошибке, который показывает **последовательность вызовов** функций и методов, приведшую к исключению.

Умение читать stack trace — один из ключевых навыков отладки.



Как выглядит трассировка (пример)

```
File "/Users/user/test_netmiko/netmiko_ssh.py", line 10, in <module>
    with ConnectHandler(**device) as ssh:
File "./site-packages/netmiko/ssh_dispatcher.py", line 365, in ConnectHandler
    return ConnectionClass(*args, **kwargs)
File "./site-packages/netmiko/base_connection.py", line 904, in _try_session_preparation
    self.session_preparation()
File "./site-packages/netmiko/tplink/tplink_jetstream.py", line 30, in session_preparation
    self._test_channel_read(pattern=r"[>#]")
File "./site-packages/netmiko/base_connection.py", line 1144, in _test_channel_read
    return self.read_until_pattern(pattern=pattern, read_timeout=20)
File "./site-packages/netmiko/base_connection.py", line 672, in read_until_pattern
    raise ReadTimeout
ReadTimeout
```

 Структура одной строки:

`File "путь/к/файлу.py", line N, in имя_функции`

| Часть | Что означает |
|-------|--------------|
| `File "..."` | Файл, в котором произошёл вызов |
| `line N` | Номер строки |
| `in имя_функции` | Функция или модуль (`<module>` — глобальный уровень) |
| Сама строка кода (с отступом) | Конкретная инструкция, которая была выполнена |
| `raise ТипОшибки` | Исключение, которое было поднято (в самом конце) |

Порядок чтения:

Читайте **снизу вверх**:
- **Самая нижняя строка** — тип исключения (например, `ReadTimeout`)
- **Выше** — где именно произошла ошибка в последней вызванной функции
- **Ещё выше** — кто вызвал эту функцию, и т.д.
- **Самая верхняя строка** — точка входа (ваш код или библиотека)


Разбор примера

1. Самая нижняя строка:
```
ReadTimeout
```
Исключение: `ReadTimeout` — не удалось прочитать данные за отведённое время.

2. Поднимаемся выше:
```
File "./site-packages/netmiko/base_connection.py", line 672, in read_until_pattern
    raise ReadTimeout
```
Ошибка произошла в методе `read_until_pattern` на строке 672.

3. Ещё выше:
```
File "./site-packages/netmiko/base_connection.py", line 1144, in _test_channel_read
    return self.read_until_pattern(...)
```
`_test_channel_read` вызвал `read_until_pattern`.

4. Следующий уровень:
```
File "./site-packages/netmiko/tplink/tplink_jetstream.py", line 30, in session_preparation
    self._test_channel_read(pattern=r"[>#]")
```
Драйвер TP-Link пытается прочитать символы `>` или `#` (приглашение командной строки).

5. Ваш код:
```
File "/Users/user/test_netmiko/netmiko_ssh.py", line 10, in <module>
    with ConnectHandler(**device) as ssh:
```
Всё началось с вызова `ConnectHandler` в вашем коде.

**Вывод:** устройство не отдаёт ожидаемый промпт (`>` или `#`). Возможные причины: неверный пароль, проблема с SSH, устройство не в том режиме.


Почему это полезно

| Что можно узнать | Пример |
|------------------|--------|
| Где именно произошла ошибка | файл + номер строки |
| Какая функция/метод вызвал ошибку | `read_until_pattern` |
| Цепочка вызовов (кто кого вызывал) | от вашего кода до глубоко вложенной библиотеки |
| Тип исключения | `ReadTimeout` |
| Контекст ошибки | ожидались символы `>` или `#`, но их не получили |

Практические советы

1. **Не игнорируйте трассировку** — это главный источник информации об ошибке.
2. **Читайте снизу вверх** — начинайте с типа исключения и последнего вызова.
3. **Ищите строку с вашим кодом** — часто ошибка не в библиотеке, а в том, как вы её используете.
4. **Проверяйте данные** — например, `device` словарь может содержать неверный IP или пароль.
5. **Гуглите последние 2-3 строки** — тип исключения и название функции.


📌 Итог

```python
# Трассировка стека — это "карта" пути программы к ошибке.
# Читайте снизу вверх:
# 1. Тип исключения (самый низ)
# 2. Где произошла ошибка (файл, строка, функция)
# 3. Кто вызывал (выше по стеку)
# 4. Начальная точка (обычно ваш код)
```

> [!TIP]
> Когда видите длинную трассировку, не пугайтесь. Большая её часть — внутренности библиотек. Ищите строку, где упоминается **ваш файл** — там часто кроется причина.



________________________________________________________________________
Paths: [[Python]]
Tags: #Python   