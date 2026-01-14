Простой скрипт для подключения к сетевому оборудованию через терминальный сервер. Скрипт после подключения выполняет заданные команды и сохраняет их  в .json. 

```python
#DISCLAIMER: This is a sanitized example for portfolio purposes. All sensitive data (IPs, hostnames, credentials) have been replaced with placeholders. Real-world scripts must use secure credential management (e.g., environment variables, vaults).
# Импорт библиотек
import json
import time

from netmiko import ConnectHandler
from netmiko import redispatch

# Определение параметров подключения к устройству
device = {
    'device_type': 'terminal_server',
    'host': '192.0.2.1',               # RFC 5737 TEST-NET-1 address
    'username': 'dummy_username',      # Placeholder username
    'password': 'dummy_password_123',  # Placeholder password
    'port': 22,
    'session_log': '/tmp/netmiko_session.log'  # Упрощённый путь
}

# Создание сессии подключения
net_connect = ConnectHandler(**device)

# Вывод промта промежуточного сервера
print("Jump Server Prompt:{}".format(net_connect.find_prompt()))
print("\n\n")

# Выполнение команды SSH для перехода к следующему устройству
net_connect.write_channel("ssh localuser@x.x.x.x\n")
time.sleep(3)  # Ожидание 3 секунд для обработки команды
output = net_connect.read_channel()

# Проверка на запрос пароля и его ввод
if 'Password:' in output:
    net_connect.write_channel('dummy_ssh_password\n')  # Заглушка

# Вывод промта роутера
print("Router Prompt:{}".format(net_connect.find_prompt()))

# Перенастройка сессии для работы с Cisco IOS
redispatch(net_connect, device_type='cisco_ios')

# Выполнение команды и получение вывода в формате TextFSM
new_output = net_connect.send_command("sh ip int brief", use_textfsm=True)
print(new_output)

# Сохранение результата в файл JSON
with open('Вывод из Пайтона.json', 'w', encoding='utf-8') as f:
    json.dump(new_output, f, indent=4, ensure_ascii=False)
```

Также использовались статьи [Автоматизация сетевого оборудования на Python. Работа через jump-host / Хабр](https://habr.com/ru/companies/rostelecom/articles/823282/)

________________________________________________________________________
Paths: [[Scripts]]
Tags: #Scripts   