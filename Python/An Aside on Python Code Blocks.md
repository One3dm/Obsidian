Блоки кода в Python (Indented Blocks)

Что такое блок кода

Блок кода — это группа операторов, которые выполняются вместе. В Python блоки определяются **отступами** (пробелами), а не фигурными скобками `{}`.

```python
# Python (отступы)
if condition:
    statement1
    statement2

# Другие языки (фигурные скобки)
if (condition) {
    statement1;
    statement2;
}
```

Как работают блоки

1. **Двоеточие** `:` — начинает блок
2. **Отступ** — показывает, что внутри блока
3. **Конец отступа** — конец блока

```python
with open("file.txt") as f:    # ← двоеточие
    data = f.read()            # ← отступ (внутри блока)
    print(data)                # ← отступ (внутри блока)
print("Готово")                # ← нет отступа (вне блока)
```

Правила отступов

| Правило | Пример | Результат |
|---------|--------|-----------|
| **4 пробела** (стандарт) | `[4 пробела]print()` | ✅ Работает |
| **Табуляция** | `[\t]print()` | ❌ Избегайте |
| **Разные отступы** | `[4 пробела]line1`<br>`[3 пробела]line2` | ❌ Ошибка |
| **Пустая строка с пробелами** | `[4 пробела]` | ❌ Ошибка |

```python
# ✅ Правильно
if True:
    print("Hello")
    print("World")

# ❌ Ошибки
if True:
print("Hello")  # Нет отступа

if True:
    print("Hello")
   print("World")  # Разные отступы
```

Где используются блоки

```python
# 1. Условия
if device == "router":
    configure_routing()
elif device == "switch":
    configure_vlans()
else:
    print("Неизвестное устройство")

# 2. Циклы
for interface in interfaces:
    print(f"Интерфейс: {interface}")

# 3. Функции
def ping_device(ip):
    result = os.system(f"ping {ip}")
    return result == 0

# 4. Контекстные менеджеры
with open("config.txt") as f:
    config = f.read()

# 5. Обработка ошибок
try:
    connect_to_device()
except ConnectionError:
    print("Не удалось подключиться")
```

Особые случаи

Пустые блоки (используйте `pass`)

```python
# ❌ Не работает
if device_is_router:
    # Ничего не делать

# ✅ Правильно
if device_is_router:
    pass  # Заглушка для пустого блока
```

Однострочные блоки
```python
# ❌ Не рекомендуется
if x > 0: print(x)

# ✅ Лучше
if x > 0:
    print(x)
```

Многострочные условия
```python
if (device_type == "router" and 
    device_status == "up" and 
    interface_count > 2):
    configure_device()
```

Практические примеры
```python
# Проверка доступности устройства
def check_device(ip):
    with open("ping_results.txt", "a") as f:
        result = os.system(f"ping -c 2 {ip}")
        
        if result == 0:
            f.write(f"{ip}: UP\n")
            print(f"{ip} доступен")
        else:
            f.write(f"{ip}: DOWN\n")
            print(f"{ip} недоступен")
```

📌 Итог
**Основные правила:**
1. После `:` всегда идет блок с отступом
2. Используйте 4 пробела для отступа
3. Все строки блока должны иметь одинаковый отступ
4. Не смешивайте пробелы и табуляции

**Совет:** Настройте редактор так, чтобы Tab вставлял 4 пробела.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   