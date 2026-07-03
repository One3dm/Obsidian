`re.findall()` — поиск всех совпадений

Проблема: извлечь все IP-адреса из ARP-таблицы

```python
data = """
Protocol    Address         Age (min)   Hardware Addr    Type     Interface
Internet    10.220.88.1     15          0062.ec29.70fe    ARPA     FastEthernet4
Internet    10.220.88.20    -           c89c.1dea.0eb6    ARPA     FastEthernet4
Internet    10.220.88.21    142         1c6a.7aaf.576c    ARPA     FastEthernet4
...
"""
```


Шаг 1: Шаблон для IP-адреса

```python
pattern = r"Internet\s+(\d+\.\d+\.\d+\.\d+)\s+"
```

- `Internet` — буквально
- `\s+` — один и более пробельных символов
- `(\d+\.\d+\.\d+\.\d+)` — группа для IP-адреса
- `\s+` — пробелы после IP

---

## Шаг 2: `re.findall()` возвращает список

```python
ip_list = re.findall(pattern, data)
print(ip_list)
# ['10.220.88.1', '10.220.88.20', '10.220.88.21', ...]
```

- В отличие от `re.search()`, `re.findall()` находит **все** непересекающиеся совпадения.
- Если в шаблоне **одна группа** `()`, возвращает список строк.

---

## Шаг 3: Добавляем MAC-адрес (вторая группа)

```python
pattern = r"Internet\s+(\d+\.\d+\.\d+\.\d+)\s+[-\d]+\s+(\w+\.\w+\.\w+)\s+"
```

- `[-\d]+` — один или более символов: цифры или дефис (поле `Age`)
- `(\w+\.\w+\.\w+)` — группа для MAC-адреса (буквы, цифры, точки)

**Результат:** список **кортежей** (IP, MAC):

```python
found = re.findall(pattern, data)
print(found)
# [('10.220.88.1', '0062.ec29.70fe'), ('10.220.88.20', 'c89c.1dea.0eb6'), ...]
```

---

## Шаг 4: Делаем шаблон читаемым (f-строки)

Разбиваем сложный шаблон на части:

```python
ip_part = r"(\d+\.\d+\.\d+\.\d+)"
mac_part = r"(\w+\.\w+\.\w+)"
# Поле Age: один или более символов (цифры или дефис)
age_part = r"[-\d]+"

pattern = rf"Internet\s+{ip_part}\s+{age_part}\s+{mac_part}\s+"

matches = re.findall(pattern, data)
```

- `rf"..."` — сырая строка **и** f-строка одновременно (Python 3.6+).

---

## Другие полезные флаги и функции

### `re.DOTALL` / `re.S`

По умолчанию точка `.` не включает символ новой строки `\n`. Флаг `re.DOTALL` меняет это:

```python
# Ищем текст между "BEGIN" и "END" через любые символы (включая \n)
m = re.search(r"BEGIN(.*)END", data, flags=re.DOTALL)
```

### `re.escape()`

Экранирует все спецсимволы в строке, чтобы её можно было использовать как **буквальный** текст в regex.

```python
literal_text = "Cisco (NX-OS) [v 9.3]"   # содержит спецсимволы (, ), [, ]
safe_pattern = re.escape(literal_text)

# Теперь можно искать эту строку как есть
m = re.search(safe_pattern, data)
```

---

## 📌 Итог

| Функция/Флаг | Что делает |
|--------------|------------|
| `re.findall(pattern, data)` | Возвращает **список всех** совпадений |
| `re.DOTALL` (или `re.S`) | Точка `.` включает `\n` |
| `re.escape(text)` | Экранирует спецсимволы в строке |
| `rf"..."` | Сырая + f-строка для читаемых шаблонов |

---

## 🔗 Связанные темы
- [[re.search() и match object]]
- [[Специальные символы в регулярных выражениях]]
- [[Capture Groups and Raw Strings]]
```