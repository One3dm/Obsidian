**Добавление в файл (Append)**

Режим `"a"` (append)

Открывает файл для **добавления** данных в конец. Старое содержимое сохраняется.

```python
f = open("test_file.txt", mode="a")
f.write("Hello again\n")
f.flush()
```

Cинтаксис открытия файла для добавления:
```python
# Разные способы указания режима
f = open("log.txt", "a")           # короткая форма
f = open("log.txt", mode="a")      # явное указание параметра
f = open("log.txt", "a", encoding="utf-8")  # с кодировкой

# С контекстным менеджером (рекомендуется)
with open("log.txt", "a") as f:
    f.write("Новая запись\n")
```

Разница между режимами

| Режим | Поведение | Когда использовать | Курсор в начале |
|-------|-----------|-------------------|-----------------|
| `"w"` (write) | **Перезаписывает** файл — старое содержимое теряется | Создание новых файлов, перезапись конфигураций | Да |
| `"a"` (append) | **Добавляет** в конец — старое содержимое сохраняется | Логирование, добавление записей, сбор статистики | Нет (в конце) |
| `"r"` (read) | **Только чтение** — нельзя записывать | Чтение конфигураций, логов, данных | Да |

Наглядный пример:
```python
# Исходный файл test.txt содержит:
# Строка 1
# Строка 2

# Режим 'w' — ПЕРЕЗАПИСЬ
with open("test.txt", "w") as f:
    f.write("Новое содержимое\n")

# Теперь файл содержит ТОЛЬКО:
# Новое содержимое

# Режим 'a' — ДОБАВЛЕНИЕ
with open("test.txt", "a") as f:
    f.write("Добавленная строка\n")

# Теперь файл содержит:
# Новое содержимое
# Добавленная строка
```

Практическое сравнение:
```python
def demonstrate_modes():
    """Демонстрация разницы между режимами w и a"""
    
    # 1. Создаём исходный файл
    with open("demo.txt", "w") as f:
        f.write("Исходная строка 1\n")
        f.write("Исходная строка 2\n")
    
    print("1. Исходный файл создан")
    with open("demo.txt") as f:
        print(f.read())
    
    # 2. Режим 'w' — перезаписывает
    with open("demo.txt", "w") as f:
        f.write("Перезаписанная строка\n")
    
    print("\n2. После режима 'w':")
    with open("demo.txt") as f:
        print(f.read())
    
    # 3. Режим 'a' — добавляет
    with open("demo.txt", "a") as f:
        f.write("Добавленная строка 1\n")
        f.write("Добавленная строка 2\n")
    
    print("\n3. После режима 'a':")
    with open("demo.txt") as f:
        print(f.read())

# Запустите эту функцию, чтобы увидеть разницу
demonstrate_modes()
```

Особенности режима `"a"`

1. Если файл не существует — он будет создан
```python
import os

filename = "new_log.txt"

# Проверяем существование
if not os.path.exists(filename):
    print(f"Файл {filename} не существует")

# Открываем в режиме 'a' — файл будет создан
with open(filename, "a") as f:
    f.write("Первая запись в новом файле\n")

print(f"Файл {filename} создан и запись добавлена")
```

2. Нельзя читать файл в режиме `"a"` (только запись)
```python
# ❌ НЕ РАБОТАЕТ — режим 'a' только для записи
with open("log.txt", "a") as f:
    content = f.read()  # Ошибка: io.UnsupportedOperation

# ✅ ПРАВИЛЬНО — для чтения используйте 'r'
with open("log.txt", "r") as f:
    content = f.read()

# ✅ ИЛИ используйте режим 'a+' для чтения и записи
with open("log.txt", "a+") as f:
    # Сначала читаем (курсор в конце, нужно вернуться)
    f.seek(0)  # Переходим в начало
    content = f.read()
    
    # Затем пишем (курсор снова в конце)
    f.write("Новая запись\n")
```

3. Курсор всегда в конце файла
```python
with open("data.txt", "a") as f:
    # При открытии в режиме 'a' курсор уже в конце файла
    position = f.tell()  # Позиция курсора
    print(f"Курсор в позиции: {position}")  # Размер файла в байтах
    
    # Любая запись идёт в конец
    f.write("Запись 1\n")
    
    # Даже если попытаться перейти в начало
    f.seek(0)  # Переходим в начало
    f.write("Запись 2\n")  # ВСЁ РАВНО запишет в конец!
    
    # В режиме 'a' seek() игнорируется для записи
    # но работает для чтения (если режим 'a+')
```

4. Режим `"a+"` — чтение и добавление
```python
# Режим 'a+' позволяет и читать, и добавлять
with open("log.txt", "a+") as f:
    # Читаем существующее содержимое
    f.seek(0)  # Переходим в начало для чтения
    existing_content = f.read()
    print(f"Существующее содержимое:\n{existing_content}")
    
    # Добавляем новую запись
    f.write(f"Новая запись в {datetime.now()}\n")
    
    # Можно прочитать то, что только что записали
    f.seek(-50, 2)  # Переходим на 50 байтов от конца
    recent_entries = f.read()
    print(f"Последние записи:\n{recent_entries}")
```

Практические примеры для сетевой автоматизации

Пример 1: Логирование работы скрипта
```python
import datetime
import os

class NetworkLogger:
    def __init__(self, log_file="network_operations.log"):
        self.log_file = log_file
        self.setup_log_file()
    
    def setup_log_file(self):
        """Настройка файла лога (создаёт заголовок при первом запуске)"""
        if not os.path.exists(self.log_file) or os.path.getsize(self.log_file) == 0:
            with open(self.log_file, "a") as f:
                f.write("=" * 60 + "\n")
                f.write("Network Operations Log\n")
                f.write(f"Started: {datetime.datetime.now()}\n")
                f.write("=" * 60 + "\n\n")
    
    def log(self, message, level="INFO", device=None):
        """Добавление записи в лог"""
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        
        if device:
            log_entry = f"[{timestamp}] [{level}] [{device}] {message}\n"
        else:
            log_entry = f"[{timestamp}] [{level}] {message}\n"
        
        # Используем режим 'a' для добавления в конец
        with open(self.log_file, "a") as f:
            f.write(log_entry)
        
        # Для критических ошибок сразу сбрасываем на диск
        if level in ["ERROR", "CRITICAL"]:
            f.flush()
        
        # Также выводим в консоль
        print(log_entry.strip())
    
    def get_recent_logs(self, num_lines=10):
        """Получение последних записей из лога"""
        try:
            with open(self.log_file, "r") as f:
                lines = f.readlines()
                return lines[-num_lines:] if len(lines) >= num_lines else lines
        except FileNotFoundError:
            return ["Лог-файл не найден"]

# Использование
logger = NetworkLogger()
logger.log("Скрипт запущен", "INFO")
logger.log("Подключение к Router1", "INFO", "Router1")
logger.log("Конфигурация применена успешно", "SUCCESS", "Router1")
logger.log("Ошибка подключения к Switch1", "ERROR", "Switch1")

# Просмотр последних записей
print("\nПоследние 5 записей в логе:")
for line in logger.get_recent_logs(5):
    print(line.strip())
```

Пример 2: Сбор статистики сети
```python
def collect_network_stats(interval_minutes=5, duration_hours=24):
    """Сбор статистики сети с интервалами"""
    import time
    from datetime import datetime
    
    stats_file = "network_stats.csv"
    
    # Создаём заголовок CSV если файл не существует
    if not os.path.exists(stats_file):
        with open(stats_file, "a") as f:
            f.write("Timestamp,Device,CPU_Usage,Memory_Usage,Bandwidth_In,Bandwidth_Out\n")
    
    devices = ["Router1", "Switch1", "Firewall1"]
    end_time = time.time() + (duration_hours * 3600)
    
    print(f"Сбор статистики начат. Длительность: {duration_hours} часов")
    
    while time.time() < end_time:
        timestamp = datetime.now().isoformat()
        
        for device in devices:
            # Имитация сбора метрик (в реальности здесь будут API вызовы)
            cpu = random.randint(5, 80)
            memory = random.randint(30, 90)
            bw_in = random.randint(100, 1000)
            bw_out = random.randint(50, 800)
            
            # Добавляем запись в CSV
            with open(stats_file, "a") as f:
                f.write(f"{timestamp},{device},{cpu},{memory},{bw_in},{bw_out}\n")
            
            print(f"Собрана статистика для {device}: CPU={cpu}%, Memory={memory}%")
        
        # Ждём указанный интервал
        time.sleep(interval_minutes * 60)
    
    print(f"Сбор статистики завершён. Данные сохранены в {stats_file}")

# Запуск сбора статистики (закомментируйте для реального использования)
# collect_network_stats(interval_minutes=1, duration_hours=0.1)  # 6 минут для теста
```

Пример 3: Мониторинг доступности устройств
```python
def monitor_device_availability(devices, check_interval=60):
    """Мониторинг доступности сетевых устройств"""
    import time
    import subprocess
    
    log_file = "availability.log"
    
    # Заголовок лога
    if not os.path.exists(log_file):
        with open(log_file, "a") as f:
            f.write("Device Availability Monitor\n")
            f.write("=" * 50 + "\n")
            f.write("Timestamp,Device,IP,Status,Response_Time\n")
    
    print(f"Мониторинг {len(devices)} устройств...")
    
    try:
        while True:
            timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            
            for device_name, ip_address in devices.items():
                try:
                    # Пинг устройства
                    start_time = time.time()
                    
                    # Для Windows: '-n 2', для Linux: '-c 2'
                    param = '-n' if os.name == 'nt' else '-c'
                    result = subprocess.run(
                        ['ping', param, '2', '-w', '1', ip_address],
                        capture_output=True,
                        text=True
                    )
                    
                    response_time = round((time.time() - start_time) * 1000, 2)
                    
                    if result.returncode == 0:
                        status = "UP"
                        print(f"{device_name} ({ip_address}): UP, {response_time}ms")
                    else:
                        status = "DOWN"
                        print(f"{device_name} ({ip_address}): DOWN")
                    
                    # Записываем результат
                    with open(log_file, "a") as f:
                        f.write(f"{timestamp},{device_name},{ip_address},{status},{response_time}\n")
                    
                except Exception as e:
                    print(f"Ошибка при проверке {device_name}: {e}")
                    with open(log_file, "a") as f:
                        f.write(f"{timestamp},{device_name},{ip_address},ERROR,0\n")
            
            print(f"Проверка завершена. Следующая через {check_interval} секунд...")
            time.sleep(check_interval)
            
    except KeyboardInterrupt:
        print("\nМониторинг остановлен пользователем")
        with open(log_file, "a") as f:
            f.write(f"\nМониторинг остановлен: {datetime.now()}\n")

# Использование
devices_to_monitor = {
    "Gateway": "192.168.1.1",
    "Core Switch": "192.168.1.10",
    "Web Server": "192.168.1.100",
    "DNS Server": "8.8.8.8"
}

# Запуск мониторинга (закомментируйте для реального использования)
# monitor_device_availability(devices_to_monitor, check_interval=30)
```

Пример 4: Ротация лог-файлов
```python
def setup_logging_with_rotation(log_file="app.log", max_size_mb=10, backup_count=5):
    """Настройка логгирования с ротацией файлов"""
    import os
    
    def check_and_rotate():
        """Проверка размера и ротация файлов"""
        if not os.path.exists(log_file):
            return
        
        # Проверяем размер файла
        file_size_mb = os.path.getsize(log_file) / (1024 * 1024)
        
        if file_size_mb >= max_size_mb:
            print(f"Лог-файл достиг {file_size_mb:.2f} MB, выполняем ротацию...")
            
            # Удаляем самый старый бэкап если нужно
            oldest_backup = f"{log_file}.{backup_count}"
            if os.path.exists(oldest_backup):
                os.remove(oldest_backup)
                print(f"Удалён старый бэкап: {oldest_backup}")
            
            # Переименовываем существующие бэкапы
            for i in range(backup_count - 1, 0, -1):
                old_name = f"{log_file}.{i}"
                new_name = f"{log_file}.{i + 1}"
                
                if os.path.exists(old_name):
                    os.rename(old_name, new_name)
            
            # Переименовываем текущий лог
            os.rename(log_file, f"{log_file}.1")
            print(f"Текущий лог перемещён в: {log_file}.1")
    
    def log_message(message, level="INFO"):
        """Добавление сообщения в лог с проверкой ротации"""
        # Проверяем нужно ли делать ротацию
        check_and_rotate()
        
        # Добавляем сообщение
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        log_entry = f"[{timestamp}] [{level}] {message}\n"
        
        with open(log_file, "a") as f:
            f.write(log_entry)
        
        print(log_entry.strip())
    
    return log_message

# Использование
log = setup_logging_with_rotation("network.log", max_size_mb=1, backup_count=3)

# Имитация записи в лог
for i in range(1000):
    log(f"Тестовое сообщение {i}", "INFO")
    time.sleep(0.01)  # небольшая задержка
```

📌 Итог

Сравнение режимов работы с файлами:

| Режим | Чтение | Запись | Позиция курсора | Если файл не существует |
|-------|--------|--------|-----------------|-------------------------|
| `"r"` | ✅ Да | ❌ Нет | Начало | Ошибка (FileNotFoundError) |
| `"w"` | ❌ Нет | ✅ Да (перезапись) | Начало | Создаётся |
| `"a"` | ❌ Нет | ✅ Да (добавление) | **Конец** | Создаётся |
| `"r+"` | ✅ Да | ✅ Да (с начала) | Начало | Ошибка |
| `"w+"` | ✅ Да | ✅ Да (перезапись) | Начало | Создаётся |
| `"a+"` | ✅ Да | ✅ Да (добавление) | **Конец** | Создаётся |

Когда использовать режим `"a"`:

1. **Логирование** — добавление записей в конец лог-файла
2. **Сбор статистики** — накопление данных измерений
3. **Аудиторские журналы** — запись событий безопасности
4. **Мониторинг** — сохранение результатов проверок
5. **Длительные процессы** — промежуточное сохранение результатов

Лучшие практики для режима append:

1. **Используйте `with open(...)`** для автоматического закрытия
2. **Для чтения логов** открывайте файл отдельно в режиме `"r"`
3. **Реализуйте ротацию** для больших лог-файлов
4. **Добавляйте timestamp** к каждой записи
5. **Используйте `flush()`** для критически важных записей

Шаблон для логгирования:
```python
import datetime
import os

def log_event(filename, message, level="INFO"):
    """Добавление события в лог-файл"""
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    log_entry = f"[{timestamp}] [{level}] {message}\n"
    
    # Создаём директорию если не существует
    os.makedirs(os.path.dirname(filename), exist_ok=True)
    
    # Добавляем запись в конец файла
    with open(filename, "a", encoding="utf-8") as f:
        f.write(log_entry)
    
    # Для ошибок сразу сбрасываем на диск
    if level in ["ERROR", "CRITICAL"]:
        f.flush()
    
    return log_entry.strip()

# Использование
log_event("logs/network.log", "Скрипт запущен", "INFO")
log_event("logs/network.log", "Ошибка подключения к устройству", "ERROR")
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   