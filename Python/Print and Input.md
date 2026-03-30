**print() и input()**

**print()** — вывод на экран
```python
print("Hello world")    # Hello world
print(22)               # 22
print(my_var)           # выводит значение переменной
```

**print() с несколькими аргументами:**
```python
ip = "10.0.0.1"
mask = "255.255.255.0"
print("IP:", ip, "Mask:", mask)  # автоматически добавляет пробелы

# Управление разделителем:
print(ip, mask, sep=" | ")  # 10.0.0.1 | 255.255.255.0
```

**input()** — ввод с клавиатуры
```python
my_ip_addr = input("Enter an IP Address: ")
```

Пользователь видит приглашение, вводит значение, оно сохраняется в переменную.

Пример из REPL:
```
In [14]: my_ip_addr = input("Enter an IP Address: ")
Enter an IP Address: 10.220.109.17

In [15]: my_ip_addr
Out[15]: '10.220.109.17'
```

> [!NOTE]
>**Важно:** `input()` всегда возвращает строку (тип `str`), даже если введено число:
>```python
>age = input("Enter your age: ")  # Пользователь вводит: 25
>print(type(age))  # <class 'str'> (строка, а не число!)
>```

**Преобразование ввода в число:**
```python
# Если нужно число, преобразуйте явно:
port = int(input("Enter port number: "))  # "22" → 22
retries = float(input("Enter retry count: "))  # "3.5" → 3.5
```

📌** Итог**

| Функция | Что делает | Важный нюанс |
|---------|-----------|--------------|
| `print()` | Выводит текст или переменную на экран | Можно выводить несколько значений через запятую |
| `input()` | Показывает приглашение и ждёт ввод от пользователя | **Всегда возвращает строку (str)** |

**Практический пример:**
```python
# Запрос данных у пользователя
device_name = input("Enter device name: ")
ip_address = input("Enter IP address: ")
port = int(input("Enter SSH port: "))

# Вывод собранной информации
print("Configuring device:", device_name)
print(f"IP: {ip_address}, Port: {port}")
```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   