Методы строк

Как вызывать методы

Методы вызываются через точку с **круглыми скобками**:
```python
text = "hello world"
text.upper()  # 'HELLO WORLD'
```

> [!WARNING]
> Если забыть скобки, получите не результат, а информацию о методе:
> ```python
> text.upper  # <function str.upper()>
> ```

Основные методы
### `.split()` — разделение строки
```python
# По умолчанию делит по пробелам
sentence = "This is a sentence"
words = sentence.split()  # ['This', 'is', 'a', 'sentence']

# С указанием разделителя
ip_addr = "192.168.1.1"
octets = ip_addr.split(".")  # ['192', '168', '1', '1']

# Ограничение количества разбиений
data = "value1,value2,value3,value4"
parts = data.split(",", 2)  # ['value1', 'value2', 'value3,value4']
```

### `.join()` — объединение списка в строку
```python
# Объединение с разделителем
octets = ['192', '168', '1', '1']
ip_addr = ".".join(octets)  # '192.168.1.1'

# Объединение без разделителя
chars = ['h', 'e', 'l', 'l', 'o']
word = "".join(chars)  # 'hello'
```

### `.strip()` — удаление пробелов по краям
```python
text = "   hello world   "
clean = text.strip()  # 'hello world'

# Удаление конкретных символов
text = "***hello***"
clean = text.strip("*")  # 'hello'
```

### `.upper()` / `.lower()` — изменение регистра
```python
"hello".upper()  # 'HELLO'
"WORLD".lower()  # 'world'
```

### `.replace()` — замена подстроки
```python
text = "hello world"
new_text = text.replace("world", "Python")  # 'hello Python'

# Замена нескольких вхождений
text = "a b c d"
new_text = text.replace(" ", "-")  # 'a-b-c-d'
```

Другие полезные методы

Проверка начала и конца строки
```python
filename = "config.txt"
if filename.endswith(".txt"):
    print("Это текстовый файл")

command = "show interface"
if command.startswith("show"):
    print("Это команда просмотра")
```

Поиск в строке
```python
text = "hello world"

# Поиск подстроки
position = text.find("world")  # 6
position = text.find("python")  # -1 (не найдено)

# Проверка вхождения
if "world" in text:
    print("Найдено 'world'")

# Количество вхождений
count = text.count("l")  # 3
```

Практические примеры

Обработка вывода с сетевого устройства
```python
def parse_interface_status(output):
    """Парсит статус интерфейса из вывода команды"""
    lines = output.strip().split("\n")
    
    for line in lines:
        if "line protocol" in line:
            parts = line.split()
            interface = parts[0]
            status = parts[1] + "/" + parts[4]
            return interface, status
    
    return None, None

# Использование
output = """
GigabitEthernet0/1 is up, line protocol is up
  Hardware is Gigabit Ethernet, address is aabb.ccdd.eeff
"""

interface, status = parse_interface_status(output)
print(f"Interface: {interface}, Status: {status}")
```

Нормализация IP-адреса
```python
def normalize_ip(ip_string):
    """Нормализует IP-адрес"""
    # Удаляем лишние пробелы
    ip_string = ip_string.strip()
    
    # Разбиваем на октеты
    octets = ip_string.split(".")
    
    # Проверяем, что ровно 4 октета
    if len(octets) != 4:
        raise ValueError(f"Invalid IP address: {ip_string}")
    
    # Убираем лишние пробелы в каждом октете
    octets = [octet.strip() for octet in octets]
    
    # Собираем обратно
    return ".".join(octets)

# Использование
ip = " 192.168. 1.1 "
normalized = normalize_ip(ip)  # '192.168.1.1'
```

Анализ конфигурации
```python
def find_vlans_in_config(config):
    """Находит все VLAN в конфигурации"""
    vlans = []
    
    for line in config.split("\n"):
        line = line.strip()
        if line.startswith("vlan "):
            # Извлекаем номер VLAN
            vlan_num = line.split()[1]
            vlans.append(vlan_num)
    
    return vlans

# Использование
config = """
vlan 10
 name Management
!
vlan 20
 name Users
!
"""

vlans = find_vlans_in_config(config)
print(f"Found VLANs: {', '.join(vlans)}")  # 'Found VLANs: 10, 20'
```

📌 Итог

**Основные методы:**
1. `.split()` — строка → список
2. `.join()` — список → строка  
3. `.strip()` — удаление пробелов по краям
4. `.upper()`/`.lower()` — изменение регистра
5. `.replace()` — замена подстроки

**Советы для сетевого инженера:**
- Используйте `.split()` для парсинга вывода команд
- Используйте `.join()` для сборки конфигураций
- Используйте `.strip()` для очистки пользовательского ввода
- Используйте `.startswith()`/`.endswith()` для проверки формата

```python
# ✅ Хороший шаблон для обработки вывода
def process_command_output(output):
    """Обрабатывает вывод команды"""
    results = []
    
    for line in output.strip().split("\n"):
        line = line.strip()
        if line and not line.startswith("!"):  # пропускаем пустые строки и комментарии
            results.append(line)
    
    return "\n".join(results)

# Использование
raw_output = """
!
interface GigabitEthernet0/1
 description Uplink
!
"""

clean_output = process_command_output(raw_output)
print(clean_output)
```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   