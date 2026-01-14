Модифицированный скрипт для подключения к сетевому оборудованию через терминальный сервер. Скрипт берет оборудование для подключения из отдельного devices_config.json файла после подключения выполняет заданные команды и сохраняет их  в .json. 

```python
#DISCLAIMER: This is a sanitized example for portfolio purposes. All sensitive data (IPs, hostnames, credentials) have been replaced with placeholders. Real-world scripts must use secure credential management (e.g., environment variables, vaults).
import json  
import time  
from netmiko import ConnectHandler  
  
# Конфигурация для подключения к терминальному серверу (jump host)  
jump_host = {  
    'device_type': 'terminal_server',  
    'host': '192.0.2.1',          # RFC 5737 TEST-NET-1
    'username': 'jump_admin',     
    'password': 'dummy_jump_pass', 
    'port': 22,  
    'session_log': 'session.log',  
    'timeout': 10  # Установите таймаут для всех операций  
}  
  
# Чтение конфигурации целевых устройств из JSON-файла  
with open('devices_config.json', 'r') as f:  
    devices_config = json.load(f)  
  
try:  
    # Подключение к терминальному серверу (jump host)  
    print("Подключение к терминальному серверу...")  
    net_connect = ConnectHandler(**jump_host)  
    print(f"Приглашение терминального сервера: {net_connect.find_prompt()}")  
  
    # Перебираем список целевых устройств  
    for device in devices_config:  
        try:  
            # Пересылка на новое устройство через SSH  
            print(f"\nПересылка на устройство {device['host']}")  
            net_connect.write_channel(f"ssh {device['username']}@{device['host']}\n")  
  
            # Ожидание запроса на ввод пароля  
            output = net_connect.read_until_pattern(pattern=r'Password:')  
            if 'Password:' in output:  
                net_connect.write_channel(f"{device['password']}\n")  
                # Ожидание приглашения устройства  
                net_connect.read_until_pattern(pattern=r'.+#')  
  
            # Перенастройка Netmiko на новый тип устройства  
            net_connect.redispatch(device_type=device['device_type'])  
  
            # Проверка, что мы успешно подключились к целевому устройству  
            if not net_connect.check_enable_mode():  
                print("Ошибка: Не удалось переключиться в режим enable.")  
                continue  
  
            print("Название оборудования: {}".format(net_connect.find_prompt()))  
  
            # Получаем список команд для текущего устройства  
            commands = device.get('commands', [])  
            if not commands:  
                print("Предупреждение: Команды не найдены для устройства {}".format(device['host']))  
                continue  
  
            # Выполняем каждую команду и сохраняем вывод  
            all_output = {}  
            for command in commands:  
                print(f"Выполнение команды: {command}")  
                try:  
                    # Используем TextFSM для структурированного вывода  
                    new_output = net_connect.send_command(command, use_textfsm=True)  
                    all_output[command] = new_output  
                except Exception as e:  
                    print(f"Ошибка при выполнении команды {command}: {e}")  
                    all_output[command] = str(e)  
  
            # Сохранение вывода в JSON-файл  
            filename = f"output_{device['host']}.json"  
            with open(filename, 'w', encoding='utf-8') as f:  
                json.dump(all_output, f, indent=4, ensure_ascii=False)  
            print(f"Вывод сохранен в файл {filename}")  
  
        except Exception as e:  
            print(f"Ошибка при работе с устройством {device['host']}: {e}")  
  
        finally:  
            # Возврат на терминальный сервер  
            net_connect.write_channel("logout\n")  
            # Ожидание возврата на терминальный сервер  
            time.sleep(3)  
except Exception as e:   
    print(f"Произошла ошибка: {e}")  
    
finally:  
    # Закрытие сессии  
    if 'net_connect' in locals():  
        net_connect.disconnect()
```

File= devices_config.json for sript
```json
[
    {
        "device_type": "cisco_ios",
	    "host": "192.0.2.101",        
	    "username": "admin",
	    "password": "cisco123",       
        "session_log": "session_1.log",
        "commands": [
            "sh ip int brief",
            "sh int description",
            "sh version"
        ]
    },
    {
        "device_type": "cisco_ios",
	    "host": "192.0.2.101",        
	    "username": "admin",
	    "password": "cisco123", 
        "session_log": "session_2.log",
        "commands": [
            "sh ip int brief",
            "sh int description",
            "sh version"
        ]
    },
    {
        "device_type": "cisco_ios",
	    "host": "192.0.2.101",        
	    "username": "admin",
	    "password": "cisco123", 
        "session_log": "session_3.log",
        "commands": [
            "sh ip int brief",
            "sh sh int description",
            "sh version"
        ]
    }
]
```

________________________________________________________________________
Paths: [[Scripts]]
Tags: #Scripts   

