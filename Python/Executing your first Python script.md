
**Создание и запуск скриптов Python**

**Создание файла**

Создайте файл с расширением `.py`, например `my_code.py`:
```python
my_ip_addr = input("Enter an IP address: ")
print(my_ip_addr)
```

**Запуск скрипта**
### Базовый способ (все платформы)
```bash
python my_code.py
```

Пример:
```
$ python my_code.py
Enter an IP address: 10.17.88.10
10.17.88.10
```

**Shebang (macOS / Linux)**

Добавьте в **первую строку** файла:
```python
#!/usr/bin/env python

my_ip_addr = input("Enter an IP address: ")
print(my_ip_addr)
```

Сделайте файл исполняемым:
```bash
chmod 755 my_code.py
```

Теперь можно запускать напрямую:
```bash
./my_code.py
```

**Комментарии в коде**
```python
#!/usr/bin/env python

# Это однострочный комментарий
my_ip_addr = input("Enter an IP address: ")

# Если комментарий длинный,
# используйте несколько строк с #
print(my_ip_addr)
```

📌 Итог

| Способ запуска | Команда |
|----------------|---------|
| Базовый (Linux/macOS/Windows) | `python my_code.py` |
| Прямой (Linux/macOS, с shebang) | `./my_code.py` |

> [!NOTE]
> На Windows shebang не работает. Используйте `python my_code.py`.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   