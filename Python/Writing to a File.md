
```markdown
---
topic: Запись в файл (Writing to a File)
source: Lesson 2 / Writing to a File
instructor: Kirk Byers
tags: [pynet, python, files, writing]
---

# Запись в файл

## Открытие файла для записи

```python
f = open("file.txt", "w")  # режим 'w' - write (запись)
```

---

## Режимы записи

| Режим | Что делает | Когда использовать |
|-------|------------|-------------------|
| `"w"` | **Перезаписывает** файл | Новые данные, конфигурации |
| `"a"` | **Добавляет** в конец | Логи, статистика |
| `"x"` | Создаёт новый файл (ошибка если существует) | Гарантия нового файла |
| `"w+"` | Чтение и запись (перезаписывает) | Редактирование файлов |

```python
# Примеры
with open("config.txt", "w") as f:   # перезапись
    f.write("новая конфигурация\n")

with open("log.txt", "a") as f:      # добавление
    f.write("новая запись\n")
```

---

## Метод `.write()`

```python
with open("output.txt", "w") as f:
    bytes_written = f.write("Hello\n")  # возвращает количество байт
    print(f"Записано {bytes_written} байт")  # 6
```

**Важно:**
- Не добавляет `\n` автоматически
- Принимает только строки
- Можно вызывать несколько раз

```python
with open("output.txt", "w") as f:
    f.write("Строка 1\n")
    f.write("Строка 2\n")
    f.write(f"Число: {42}\n")  # числа нужно преобразовать
```

---

## Метод `.writelines()`

```python
lines = ["Строка 1\n", "Строка 2\n", "Строка 3\n"]

with open("output.txt", "w") as f:
    f.writelines(lines)  # записывает все строки сразу
```

---

## Буферизация данных

Данные не записываются на диск сразу, а накапливаются в буфере.

```python
with open("log.txt", "w") as f:
    f.write("Сообщение 1\n")  # в буфере
    f.write("Сообщение 2\n")  # в буфере
    f.flush()  # записываем на диск сейчас
    f.write("Сообщение 3\n")  # снова в буфере
# При выходе из with буфер сбрасывается автоматически
```

**Когда использовать `flush()`:**
- Критически важные данные
- Отладка
- Режим реального времени

---

## Важное предупреждение

**Режим `"w"` перезаписывает файл!**

```python
# ❌ Потеря данных!
with open("file.txt", "w") as f:
    f.write("Новые данные\n")
# Старые данные удалены
```

**Как избежать:**
```python
import os

if os.path.exists("config.txt"):
    print("Внимание: файл уже существует")
    # Создайте бэкап перед перезаписью
```

---

## Практические примеры

### Запись конфигурации устройства

```python
def save_config(hostname, interfaces):
    with open(f"{hostname}_config.txt", "w") as f:
        f.write(f"hostname {hostname}\n!\n")
        
        for interface, config in interfaces.items():
            f.write(f"interface {interface}\n")
            for command in config:
                f.write(f"  {command}\n")
            f.write("!\n")
    
    print(f"Конфигурация сохранена")

# Использование
interfaces = {
    "GigabitEthernet0/0": ["ip address 192.168.1.1 255.255.255.0", "no shutdown"],
    "GigabitEthernet0/1": ["description Uplink", "no shutdown"]
}

save_config("Router1", interfaces)
```

### Экспорт в CSV

```python
def export_to_csv(devices, filename="devices.csv"):
    with open(filename, "w") as f:
        f.write("Hostname,IP,Model,Status\n")
        
        for device in devices:
            line = f"{device['name']},{device['ip']},{device['model']},{device['status']}\n"
            f.write(line)
    
    print(f"Экспортировано {len(devices)} устройств")

# Использование
devices = [
    {"name": "Router1", "ip": "192.168.1.1", "model": "Cisco 2911", "status": "up"},
    {"name": "Switch1", "ip": "192.168.1.2", "model": "Cisco 2960", "status": "up"}
]

export_to_csv(devices)
```

---

## 📌 Итог

**Основные правила:**
1. Используйте `with open(..., "w")` для записи
2. Добавляйте `\n` в конце строк
3. Для чисел используйте f-строки или `str()`
4. Используйте `flush()` для важных данных
5. Помните: `"w"` перезаписывает файл!

**Советы:**
- Для логов используйте `"a"` (append)
- Для конфигураций используйте `"w"` с бэкапами
- Используйте `.writelines()` для списков строк

```python
# ✅ Правильный шаблон
with open("output.txt", "w") as f:
    f.write("Заголовок\n")
    f.write("Данные 1\n")
    f.write("Данные 2\n")
    f.flush()  # если нужно сразу на диск
# Файл автоматически закрывается
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   