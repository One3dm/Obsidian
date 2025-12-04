Подключение к сетевому оборудованию через терминальный сервер. Для подключения использовался SecureCRT. Терминальный сервер x.x.x.x. 

```python
# Импорт библиотек
import json
import time

from netmiko import ConnectHandler
from netmiko import redispatch

# Определение параметров подключения к устройству
device = {
    'device_type': 'terminal_server',  # Тип устройства (jump server)
    'host': 'x.x.x.x',                # IP-адрес устройства
    'username': 'Логин',             # Имя пользователя
    'password': 'Пароль',            # Пароль
    'port': 22,                      # Порт для подключения (SSH)
    'session_log': 'session.log'     # Путь к файлу лога сессии
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
    net_connect.write_channel('Transiver8754\n')

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


________________________________________________________________________
Paths: [[Scripts]]
Tags: #Scripts   