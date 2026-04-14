Числа в Python

Типы чисел
```python
# Целые числа (int)
port = 22
vlan_id = 100
counter = 0

# Числа с плавающей точкой (float)
packet_loss = 0.05  # 5%
bandwidth = 1000.0  # 1000 Mbps
temperature = 25.5
```

Основные операции

Арифметические операторы
```python
# Сложение
total = 10 + 5  # 15

# Вычитание  
remaining = 100 - 25  # 75

# Умножение
area = 10 * 5  # 50

# Деление (всегда возвращает float)
result = 7 / 2  # 3.5

# Целочисленное деление
result = 7 // 2  # 3

# Остаток от деления
remainder = 7 % 2  # 1

# Возведение в степень
squared = 3 ** 2  # 9
```

Составные операторы
```python
counter = 0
counter += 1  # увеличение на 1
counter -= 1  # уменьшение на 1
counter *= 2  # умножение на 2
counter /= 2  # деление на 2
```

Практические примеры для сетей

Расчёт количества хостов в подсети
```python
def calculate_hosts(subnet_mask):
    """Вычисляет количество хостов в подсети"""
    # Пример: /24 → 32 - 24 = 8 → 2^8 - 2 = 254
    host_bits = 32 - subnet_mask
    hosts = 2 ** host_bits - 2
    return hosts

# Использование
print(f"/24: {calculate_hosts(24)} хостов")  # 254
print(f"/30: {calculate_hosts(30)} хостов")  # 2
```

Проверка портов
```python
def is_valid_port(port):
    """Проверяет валидность номера порта"""
    return 1 <= port <= 65535

# Использование
port = 8080
if is_valid_port(port):
    print(f"Порт {port} валиден")
else:
    print(f"Порт {port} невалиден")
```

Подсчёт статистики
```python
# Подсчёт пакетов и байт
total_packets = 0
total_bytes = 0

# В цикле обработки
for packet in packets:
    total_packets += 1
    total_bytes += packet.size

print(f"Пакетов: {total_packets}, Байт: {total_bytes}")
```

Округление чисел
```python
import math

value = 3.75

# Округление до ближайшего
rounded = round(value)  # 4

# Округление вниз
floored = math.floor(value)  # 3

# Округление вверх
ceiled = math.ceil(value)  # 4

# Округление до N знаков
packet_loss = 0.034567
print(f"{packet_loss:.2%}")  # 3.46%
print(f"{round(packet_loss * 100, 2)}%")  # 3.46%
```

Преобразование типов
```python
# Из строки в число
port_str = "8080"
port = int(port_str)  # 8080

speed_str = "1000.0"
speed = float(speed_str)  # 1000.0

# Из числа в строку
port = 22
port_str = str(port)  # "22"

# Автоматическое преобразование
result = 3 + 4.5  # 7.5 (int + float → float)
```

Полезные функции
```python
# Абсолютное значение
abs(-10)  # 10

# Минимум и максимум
min(5, 10, 2)  # 2
max(5, 10, 2)  # 10

# Сумма
sum([1, 2, 3, 4])  # 10

# Двоичное представление
bin(42)  # '0b101010'
```

📌 Итог
**Основные правила:**
1. Используйте `int` для целых чисел (порты, VLAN, счётчики)
2. Используйте `float` для дробных чисел (проценты, метрики)
3. Деление `/` всегда возвращает `float`
4. Используйте `//` для целочисленного деления
5. Используйте `%` для проверки делимости

**Советы для сетевого инженера:**
- Проверяйте порты: `1 <= port <= 65535`
- Используйте `2 ** n` для расчёта количества хостов
- Округляйте метрики для читаемости
- Преобразуйте ввод пользователя в числа явно

```python
# ✅ Хороший шаблон
def calculate_bandwidth_usage(total, used):
    """Расчёт использования пропускной способности"""
    if total <= 0:
        return 0.0
    
    usage = (used / total) * 100
    return round(usage, 2)  # округляем до 2 знаков

# Использование
total_bandwidth = 1000  # Mbps
used_bandwidth = 350    # Mbps
usage = calculate_bandwidth_usage(total_bandwidth, used_bandwidth)
print(f"Использование: {usage}%")  # 35.0%
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   