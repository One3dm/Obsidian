Перевод импортированного excel файла из IMS в JSON формат для чтения скриптом по подключению к оборудованию

```python
import pandas as pd  
import json  
  
def excel_to_json(excel_file, json_file):  
    """  
    Функция читает данные из Excel файла и сохраняет их в формате JSON.  
    :param excel_file: Путь к файлу Excel (.xlsx)    
    :param json_file: Путь к файлу, в который будут сохранены данные в формате JSON    
    """    
    try:  
        # Читаем данные из Excel файла  
        df = pd.read_excel(excel_file)  
  
        # Проверяем, что необходимые столбцы существуют  
        required_columns = ["hostname", "device_ip"]  
        for column in required_columns:  
            if column not in df.columns:  
                raise ValueError(f"Столбец '{column}' отсутствует в Excel файле.")  
  
        # Создаем список словарей для JSON  
        data = []  
        for index, row in df.iterrows():  
            device = {  
                "device_type": "cisco_ios",  
                "host": row["hostname"],  
                "ip": row["device_ip"],  
                "session_log": "session_1.log",  
            }  
            data.append(device)  
  
        # Сохраняем данные в JSON файл  
        with open(json_file, 'w') as f:  
            json.dump(data, f, indent=4)  
  
        print(f"Данные успешно сохранены в {json_file}")  
    except Exception as e:  
        print(f"Произошла ошибка: {e}")  
  
# Пример использования  
if __name__ == "__main__":  
    # Путь к файлу Excel  
    excel_file = "unloading_SZ.xlsx"  
    # Путь для сохранения JSON файла  
    json_file = "devices_config.json"  
  
    # Вызываем функцию  
    excel_to_json(excel_file, json_file)
```

________________________________________________________________________
Paths: [[Scripts]]
Tags: #Scripts   