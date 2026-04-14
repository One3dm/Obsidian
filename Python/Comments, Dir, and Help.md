Комментарии в коде и встроенные функции dir() и help()

Комментарии

Однострочные комментарии
```python
# Это комментарий - всё после решётки игнорируется
ip_addr = input("Enter IP address: ")

print(ip_addr)  # Комментарий после кода (через 2 пробела)
```

Многострочные комментарии
```python
# Если комментарий длинный,
# используйте несколько строк
# с символом решётки.
# Такой стиль рекомендуется PEP 8.
```

Специальные комментарии
```python
# TODO: добавить проверку валидности IP
# FIXME: временное решение, переписать
# NOTE: важно для совместимости
# HACK: костыль для обхода бага
```

> [!WARNING]
> Тройные кавычки `"""` — это **не комментарий**, а многострочная строка (docstring).

Функция dir()

Показывает все доступные методы и атрибуты объекта:
```python
# С аргументом - показывает методы объекта
my_str = "hello"
dir(my_str)  # ['__add__', '__class__', 'capitalize', 'split', 'upper', ...]

# Без аргументов - показывает имена в текущей области видимости
x = 10
y = "text"
dir()  # ['__builtins__', '__doc__', 'x', 'y', ...]
```

**Практическое использование:**
```python
def explore_string_methods():
    """Исследует методы строки"""
    ip = "192.168.1.1"
    
    print("Методы строки (IP-адреса):")
    for method in dir(ip):
        if not method.startswith('__'):  # пропускаем служебные методы
            print(f"  {method}()")
    
    # Ищем методы для работы с IP
    print("\nМетоды для работы с IP:")
    for method in dir(ip):
        if 'split' in method or 'start' in method or 'end' in method:
            print(f"  {method}()")

explore_string_methods()
```

Функция help()

Показывает документацию по методу, функции или объекту:
```python
# Документация метода
my_str = "test"
help(my_str.upper)

# Документация типа
help(str)
help(list)

# Интерактивная справка
help()  # вход в интерактивный режим
# help> str.split
# help> q  # выход
```

Практический workflow исследования

Типичная последовательность изучения объекта
```python
# 1. Создаём объект
ip = "192.168.1.1"

# 2. Узнаём тип
type(ip)  # <class 'str'>

# 3. Смотрим методы
dir(ip)   # ['split', 'startswith', 'strip', ...]

# 4. Читаем документацию
help(ip.split)

# 5. Пробуем использовать
ip.split('.')  # ['192', '168', '1', '1']

# 6. Ищем похожие методы
[m for m in dir(ip) if 'split' in m]  # ['rsplit', 'split', 'splitlines']
```

Пример для сетевых данных
```python
def explore_ip_methods():
    """Исследует методы для работы с IP-адресами"""
    ip = "192.168.1.1/24"
    
    # Смотрим доступные методы
    print("Доступные методы:")
    for method in dir(ip):
        if not method.startswith('__'):
            print(f"  {method}")
    
    # Изучаем конкретный метод
    print("\nДокументация метода split():")
    help(ip.split)
    
    # Пробуем использовать
    print("\nПример использования split():")
    parts = ip.split('/')
    print(f"IP: {parts[0]}, Mask: {parts[1]}")
    
    # Изучаем другой полезный метод
    print("\nДокументация метода partition():")
    help(ip.partition)
    
    print("\nПример использования partition():")
    network, separator, mask = ip.partition('/')
    print(f"Network: {network}, Separator: '{separator}', Mask: {mask}")

explore_ip_methods()
```

📌 Итог
**Основные правила:**
1. Используйте комментарии для объяснения "почему", а не "что"
2. Используйте `dir()` для исследования объектов
3. Используйте `help()` для чтения документации
4. Изучайте цепочкой: `type() → dir() → help() → пробуем`

**Советы для сетевого инженера:**
- Используйте `dir()` для изучения сетевых модулей (ipaddress, netmiko, paramiko)
- Используйте `help()` для понимания параметров функций
- Исследуйте объекты перед поиском в интернете
```python
# ✅ Хороший шаблон исследования
def explore_module(module_name):
    """Исследует модуль Python"""
    try:
        module = __import__(module_name)
        
        print(f"Исследование модуля: {module_name}")
        print("=" * 50)
        
        # Смотрим что есть в модуле
        print("\nСодержимое модуля:")
        for item in dir(module):
            if not item.startswith('_'):
                print(f"  {item}")
        
        # Пример использования help
        print("\nДокументация модуля:")
        help(module)
        
    except ImportError:
        print(f"Модуль {module_name} не найден")

# Использование
explore_module("ipaddress")
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   