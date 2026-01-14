Скрипт который подключается к оборудованию через терминальный сервер и выполняет команду sh run. После чего парсит вывод с помощью библиотеки CiscoConfParse. В пропаршеном конфиге ищет блоки маршрутизации router и в этих блоках ищет строку passive-interface default.

```python
import json
import os
import time
from netmiko import ConnectHandler
from ciscoconfparse import CiscoConfParse
import re

# Функция для чтения логина и пароля из файла credentials.json
def load_credentials(file_path):
    try:
        with open(file_path, 'r') as f:
            credentials = json.load(f)
            return credentials.get("username"), credentials.get("password")
    except FileNotFoundError:
        print("Файл с данными не найден.")
        return None, None

# Подключение к терминальному серверу
def connect_to_jump_host(jump_host_config):
    print("Подключение к терминальному серверу...")
    try:
        net_connect = ConnectHandler(**jump_host_config)
        print(f"Приглашение терминального сервера: {net_connect.find_prompt()}")
        return net_connect
    except Exception as e:
        print(f"Ошибка при подключении к терминальному серверу: {e}")
        return None

# Чтение конфигурации устройств
def load_device_config(config_file):
    try:
        with open(config_file, 'r') as f:
            return json.load(f)
    except (FileNotFoundError, json.JSONDecodeError) as e:
        print(f"Ошибка при чтении файла конфигурации: {e}")
        return None

# Главная функция
def main():
    # Путь к файлу с логином и паролем
    credentials_file = r"credentials.json"

    # Проверка файла credentials.json
    if not os.path.exists(credentials_file):
        print(f"Файл {credentials_file} не найден.")
        return

    # Чтение логина и пароля
    username, password = load_credentials(credentials_file)
    if not username or not password:
        print("Ошибка: Не удалось загрузить логин и пароль.")
        return

    # Конфигурация для подключения к терминальному серверу
    jump_host = {
        'device_type': 'terminal_server',
		'host': '192.0.2.1',          # RFC 5737 TEST-NET-1
	    'username': 'jump_admin',     
	    'password': 'dummy_jump_pass', 
        'port': 22,
        'session_log': 'session.log',
        'timeout': 10
    }

    # Подключение к терминальному серверу
    net_connect = connect_to_jump_host(jump_host)
    if not net_connect:
        return

    # Чтение конфигурации устройств
    devices_config_file = "devices_config.json"
    config = load_device_config(devices_config_file)
    if not config:
        print(f"Ошибка при загрузке конфигурации устройств из файла {devices_config_file}.")
        return

    common_settings = config[0]['common_settings']
    devices = config[0]['devices']
    common_commands = config[0]['common_commands']

    # Словарь для хранения результатов
    results = {}

    # Перебираем список целевых устройств
    for device in devices:
        try:
            # Объединяем общие настройки с настройками текущего устройства
            device.update(common_settings)

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
            try:
                net_connect.redispatch(device_type=device['device_type'])
            except Exception as e:
                print(f"Ошибка при перенастройке на устройство {device['host']}: {e}")
                continue

            # Проверка, что мы успешно подключились к целевому устройству
            if not net_connect.check_enable_mode():
                print("Ошибка: Не удалось переключиться в режим enable.")
                continue

            print("Название оборудования: {}".format(net_connect.find_prompt()))
            hostname = net_connect.find_prompt()  # Получаем хостнейм устройства

            # Выполняем команду sh run
            print(f"Выполнение команды: sh run")
            try:
                # Установить terminal length 0
                net_connect.send_command("terminal length 0")

                # Получаем вывод команды
                raw_output = net_connect.send_command("sh run", read_timeout=60)

                # Проверяем, есть ли строка passive-interface default в выводе
                if "passive-interface default" in raw_output:
                    print("Строка 'passive-interface default' найдена в выводе команды sh run.")

                # Попробуем парсинг с CiscoConfParse
                parse = CiscoConfParse(config=raw_output.splitlines())

                # Находим все блоки маршрутизации
                routing_blocks = parse.find_objects(r'^router\s+\S+')

                # Проверяем блоки маршрутизации
                print(f"Найдено {len(routing_blocks)} блоков маршрутизации:")
                for block in routing_blocks:
                    print(f"- {block.text}")
                    print(f"Содержимое блока: {block.all_children}")
                    print(f"Содержимое блока (исходный текст): {block.ioscfg}")

                    # Используем re_match_iter_typed для поиска passive-interface default
                    pidf = block.re_match_iter_typed(r'\s*(passive-interface default)', default=' NOTHING!!')
                    print(f"For hostname {hostname} for router {block.text} I found: {pidf}")

                    # Обработка результатов
                    if isinstance(pidf, list):
                        if pidf:
                            for match in pidf:
                                results[hostname] = results.get(hostname, [])
                                results[hostname].append(f"Команда passive-interface default найдена в блоке: {match.text}")
                            results[hostname] = results.get(hostname, [])
                            results[hostname].append("Команды passive-interface default НАЙДЕНЫ")
                        else:
                            results[hostname] = results.get(hostname, [])
                            results[hostname].append("Команды passive-interface default НЕ НАЙДЕНЫ")
                    elif isinstance(pidf, str):
                        results[hostname] = results.get(hostname, [])
                        results[hostname].append("Команды passive-interface default НЕ НАЙДЕНЫ")

                # Сохраняем результаты для текущего устройства
                if hostname not in results:
                    results[hostname] = []
                results[hostname].append("Строка 'passive-interface default' найдена в выводе команды sh run.")
                results[hostname].append(
                    "Строка 'passive-interface default' найдена с использованием регулярного выражения.")
                if pidf:
                    if isinstance(pidf, list) and pidf:
                        for match in pidf:
                            results[hostname].append(f"Команда passive-interface default найдена в блоке: {match.text}")
                    else:
                        results[hostname].append(
                            "Команды passive-interface default не были найдены в блоках маршрутизации.")

            except Exception as e:
                print(f"Ошибка при работе с устройством {device['host']}: {e}")

        finally:
            # Возврат на терминальный сервер
            net_connect.write_channel("logout\n")
            time.sleep(3)

    # Закрытие сессии
    if 'net_connect' in locals():
        net_connect.disconnect()

    # Сохранение результатов в файл
    if results:
        with open("results.txt", "w") as f:
            for hostname, result_list in results.items():
                f.write(f"Результаты для устройства {hostname}:\n")
                for result in result_list:
                    f.write(f"- {result}\n")
                f.write("\n")

if __name__ == "__main__":
    main()
```


File= devices_config.json for sript
```json
[
{
    "common_settings": {
        "username": "admin",
        "password": "cisco123",
        "global_delay_factor": 5,
        "fast_cli": false,
        "conn_timeout": 30,
        "auth_timeout": 20
    },
    "common_commands": [
        "sh run"
    ],
    "devices": [
        {
            "device_type": "cisco_ios",
            "host": "192.0.2.1",
            "session_log": "session_hostname1.log"
        },
        {
            "device_type": "cisco_ios",
            "host": "192.0.2.2",
            "session_log": "session_hostname2.log"
        }
    ]
}
]
```

File= credentials.json for sript
```json
{
    "username": "admin",
    "password": "cisco123"
}
```
________________________________________________________________________
Paths: [[Scripts]]
Tags: #Scripts   