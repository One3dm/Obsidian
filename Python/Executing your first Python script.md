Создание и запуск скриптов Python

Создание файла

Создайте файл с расширением `.py`:

```python
# my_script.py
device_ip = input("Enter device IP address: ")
print(f"Connecting to {device_ip}...")
```

**Правила именования:**
- Используйте `snake_case`: `backup_config.py`, `check_interfaces.py`
- Избегайте пробелов и спецсимволов
- Расширение должно быть `.py`

Запуск скрипта

Базовый способ
```bash
python my_script.py
```

Альтернативные команды
```bash
python3 my_script.py      # Явно Python 3 (Linux/macOS)
py my_script.py           # Windows Python launcher
python -m my_script       # Запуск как модуль (без .py)
```

Запуск из разных директорий
```bash
# Абсолютный путь
python /home/user/scripts/my_script.py

# Относительный путь
python ../network_scripts/my_script.py

# Сначала перейти в директорию
cd scripts && python my_script.py
```

Shebang (macOS / Linux)

Добавьте в первую строку файла:
```python
#!/usr/bin/env python3

device_ip = input("Enter device IP address: ")
print(f"Connecting to {device_ip}...")
```

Сделайте файл исполняемым:
```bash
chmod +x my_script.py    # Добавить права на выполнение
chmod 755 my_script.py   # Альтернативный вариант
```

Теперь можно запускать напрямую:
```bash
./my_script.py
```

Практические примеры

Пример 1: Простой скрипт для ввода IP
```python
#!/usr/bin/env python3
# get_device_info.py - запрос информации об устройстве

device_name = input("Введите имя устройства: ")
device_ip = input("Введите IP-адрес устройства: ")

print(f"\nИнформация об устройстве:")
print(f"Имя: {device_name}")
print(f"IP-адрес: {device_ip}")
print(f"Команда подключения: ssh admin@{device_ip}")
```

Пример 2: Скрипт с аргументами командной строки
```python
#!/usr/bin/env python3
# ping_device.py - проверка доступности устройства
import sys
import os

def main():
    if len(sys.argv) < 2:
        print("Использование: python ping_device.py <IP-адрес>")
        sys.exit(1)
    
    device_ip = sys.argv[1]
    print(f"Проверяю доступность {device_ip}...")
    
    # Простая проверка ping (для Linux/macOS)
    response = os.system(f"ping -c 1 {device_ip} > /dev/null 2>&1")
    
    if response == 0:
        print(f"Устройство {device_ip} доступно")
    else:
        print(f"Устройство {device_ip} недоступно")

if __name__ == "__main__":
    main()
```

Запуск:
```bash
python ping_device.py 192.168.1.1
```

Отладка и проверка
```bash
# Проверка синтаксиса без запуска
python -m py_compile my_script.py

# Запуск с подробным выводом
python -v my_script.py    # показывает импорты

# Интерактивный режим после выполнения
python -i my_script.py    # остаётесь в REPL с переменными

# Отладка с pdb
python -m pdb my_script.py
```

Распространённые ошибки

Файл не найден
```bash
# Ошибка: python: can't open file 'my_script.py'
# Решение: проверьте путь и имя файла
ls -la my_script.py    # Существует ли файл?
pwd                    # В какой директории вы находитесь?
```

Permission denied (Linux/macOS)
```bash
# Ошибка: bash: ./my_script.py: Permission denied
chmod +x my_script.py  # Добавьте права на выполнение
```

Shebang не работает
- Убедитесь, что shebang в первой строке
- Проверьте, что после shebang нет пробелов
- Убедитесь, что Python установлен

📌 Итог

**Основные правила:**
1. Используйте расширение `.py` для файлов
2. Используйте shebang (`#!/usr/bin/env python3`) для Linux/macOS
3. Используйте `chmod +x` для прямого запуска
4. На Windows используйте `python script.py`

**Советы для сетевого инженера:**
- Создавайте отдельные скрипты для разных задач
- Используйте аргументы командной строки для гибкости
- Добавляйте комментарии для объяснения сложной логики
- Тестируйте скрипты перед использованием в production

```python
#!/usr/bin/env python3
"""
network_backup.py - скрипт для резервного копирования конфигураций
Использование: python network_backup.py <device_ip> [<username>]
"""

import sys

def backup_device(device_ip, username="admin"):
    """Выполняет резервное копирование устройства"""
    print(f"Подключаюсь к {device_ip} как {username}...")
    # Здесь будет код подключения через Netmiko/Paramiko
    # и сохранения конфигурации
    print(f"Резервная копия {device_ip} завершена")

def main():
    if len(sys.argv) < 2:
        print("Использование: python network_backup.py <device_ip> [<username>]")
        sys.exit(1)
    
    device_ip = sys.argv[1]
    username = sys.argv[2] if len(sys.argv) > 2 else "admin"
    
    backup_device(device_ip, username)

if __name__ == "__main__":
    main()
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   