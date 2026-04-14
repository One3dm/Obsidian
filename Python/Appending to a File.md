Добавление в файл (Append)

Режим `"a"` (append)

Открывает файл для **добавления** данных в конец. Старое содержимое сохраняется.
```python
with open("log.txt", "a") as f:
    f.write("Новая запись\n")
```

Разница между режимами

| Режим | Что делает | Пример |
|-------|------------|--------|
| `"w"` | **Перезаписывает** файл | `with open("file.txt", "w")` |
| `"a"` | **Добавляет** в конец | `with open("file.txt", "a")` |
| `"r"` | **Только чтение** | `with open("file.txt", "r")` |

Пример:
```python
# Файл содержит: "Старая строка"

# Режим 'w' — перезаписывает
with open("file.txt", "w") as f:
    f.write("Новая строка\n")
# Результат: "Новая строка"

# Режим 'a' — добавляет
with open("file.txt", "a") as f:
    f.write("Ещё строка\n")
# Результат: "Новая строка\nЕщё строка"
```

Особенности режима `"a"`

1. Создаёт файл, если не существует
```python
# Если файла нет — он создаётся
with open("new_log.txt", "a") as f:
    f.write("Первая запись\n")
```

2. Нельзя читать (только запись)
```python
# ❌ Ошибка
with open("log.txt", "a") as f:
    data = f.read()  # io.UnsupportedOperation

# ✅ Правильно
with open("log.txt", "r") as f:
    data = f.read()
```

3. Режим `"a+"` — чтение и добавление
```python
with open("log.txt", "a+") as f:
    # Читаем (курсор в конце, нужно вернуться)
    f.seek(0)
    old_data = f.read()
    
    # Добавляем
    f.write("Новая запись\n")
```

Практические примеры

Логирование работы скрипта
```python
import datetime

def log_event(message, level="INFO"):
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    log_entry = f"[{timestamp}] [{level}] {message}\n"
    
    with open("network.log", "a") as f:
        f.write(log_entry)
    
    print(log_entry.strip())

# Использование
log_event("Скрипт запущен")
log_event("Подключение к Router1", "INFO")
log_event("Ошибка подключения", "ERROR")
```

Мониторинг устройств
```python
def check_device(ip, device_name):
    import subprocess
    
    result = subprocess.run(["ping", "-c", "2", ip], 
                          capture_output=True)
    
    status = "UP" if result.returncode == 0 else "DOWN"
    
    with open("availability.log", "a") as f:
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        f.write(f"{timestamp},{device_name},{ip},{status}\n")
    
    print(f"{device_name}: {status}")
```

📌 Итог
**Когда использовать `"a"`:**
- Логирование
- Сбор статистики
- Добавление записей
- Мониторинг

**Основные правила:**
1. Используйте `with open(..., "a")` для безопасности
2. Для чтения логов используйте отдельно `"r"`
3. Добавляйте timestamp к записям
4. Используйте `flush()` для важных данных

```python
# ✅ Правильный шаблон
import datetime

with open("app.log", "a") as f:
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    f.write(f"[{timestamp}] Событие\n")
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   