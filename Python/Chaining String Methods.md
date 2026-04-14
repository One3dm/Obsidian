Цепочки методов строк (Method Chaining)

Что такое chaining

Методы можно вызывать **последовательно**, один за другим:
```python
text = "  SOME STRING  "
result = text.lower().strip()
print(result)  # 'some string'
```

**Порядок выполнения:**
1. `text.lower()` → `"  some string  "`
2. `"  some string  ".strip()` → `"some string"`

Практические примеры

Нормализация имени интерфейса
```python
interface = "  gi0/1  "
normalized = interface.strip().lower().replace("gi", "GigabitEthernet")
print(normalized)  # 'GigabitEthernet0/1'
```

Обработка MAC-адреса
```python
mac = "  AA:BB:CC:DD:EE:FF  "
clean_mac = mac.strip().lower().replace(":", "")
print(clean_mac)  # 'aabbccddeeff'
```

Очистка вывода команды
```python
raw_output = "  Gi0/1 is UP, 100 Mbps, Full Duplex  "
clean_output = raw_output.strip().lower().replace("gi0/1", "GigabitEthernet0/1")
print(clean_output)  # 'GigabitEthernet0/1 is up, 100 mbps, full duplex'
```

Извлечение номера VLAN
```python
vlan_info = "VLAN0010 - Management"
vlan_num = vlan_info.strip().split()[0].replace("VLAN", "")
print(vlan_num)  # '0010'
```

Важные моменты

Порядок методов имеет значение
```python
text = "  HELLO WORLD  "

# Сначала strip, потом replace
print(text.strip().replace(" ", "_"))  # "HELLO_WORLD"

# Сначала replace, потом strip
print(text.replace(" ", "_").strip())  # "__HELLO_WORLD"
```

Исходная строка не изменяется
```python
text = "  SOME TEXT  "
result = text.lower().strip()

print(text)   # '  SOME TEXT  '  (оригинал не изменился)
print(result) # 'some text'       (новый объект)
```

Лучшие практики

Избегайте слишком длинных цепочек
```python
# ❌ Слишком длинно и сложно читать
result = input_data.strip().lower().replace(" ", "_").replace(".", "-").upper()

# ✅ Лучше разбить на логические шаги
cleaned = input_data.strip().lower()
normalized = cleaned.replace(" ", "_").replace(".", "-")
result = normalized.upper()

# ✅ Или использовать временные переменные
result = (
    input_data
    .strip()
    .lower()
    .replace(" ", "_")
    .replace(".", "-")
    .upper()
)
```

Используйте для простых преобразований
```python
def normalize_interface_name(interface):
    """Нормализует имя интерфейса"""
    return (
        interface
        .strip()
        .lower()
        .replace("gi", "GigabitEthernet")
        .replace("fa", "FastEthernet")
        .replace("te", "TenGigabitEthernet")
    )

# Использование
print(normalize_interface_name("  gi0/1  "))  # 'GigabitEthernet0/1'
```

📌 Итог
**Основные правила:**
1. Методы выполняются слева направо
2. Каждый метод применяется к результату предыдущего
3. Исходная строка не изменяется
4. Порядок методов важен
5. Избегайте слишком длинных цепочек

**Советы для сетевого инженера:**
- Используйте цепочки для быстрой очистки вывода
- Нормализуйте имена интерфейсов, MAC-адреса
- Разбивайте сложные преобразования на шаги
- Тестируйте порядок методов

```python
# ✅ Хороший шаблон
def process_device_output(raw_output):
    """Обрабатывает вывод с сетевого устройства"""
    # Первый этап: базовая очистка
    cleaned = raw_output.strip().lower()
    
    # Второй этап: нормализация
    normalized = (
        cleaned
        .replace("gi", "GigabitEthernet")
        .replace("up/up", "connected")
        .replace("down/down", "disconnected")
    )
    
    # Третий этап: форматирование
    formatted = normalized.capitalize()
    
    return formatted

# Использование
output = "  gi0/1 is up/up, 100 mbps  "
result = process_device_output(output)
print(result)  # 'GigabitEthernet0/1 is connected, 100 mbps'
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   