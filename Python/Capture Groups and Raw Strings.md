**Группы захвата и сырые строки**

Группы захвата `()`

Круглые скобки в регулярном выражении **запоминают** часть найденного текста. К ним можно обратиться через `m.group(1)`, `m.group(2)` и т.д.

- `.group(0)` или `.group()` — весь найденный текст.
- `.group(1)` — содержимое первой группы.
- `.group(2)` — содержимое второй группы и т.д.


Пример: извлечение регистра конфигурации
```python
data = """
License Information for 'c880-data'
License Level: advpiservices    Type: Permanent
Next reboot license Level: advpiservices

Configuration register is 0x2102
"""

m = re.search(r"Configuration register is (.*)", data)

print(m.group(0))   # 'Configuration register is 0x2102'
print(m.group(1))   # '0x2102'
```

- Группа `(.*)` захватывает всё после `"Configuration register is "` до конца строки.

Пример: извлечение версии
```python
line = "Cisco IOS Software, C880 Software (C880DATA-UNIVERSALK9-M), Version 15.4(2)T1, RELEASE SOFTWARE (fc3)"

m = re.search(r"Version (.*)", line)
print(m.group(1))   # '15.4(2)T1, RELEASE SOFTWARE (fc3)'
```

Группа захватила **слишком много** — до конца строки, включая запятую и `RELEASE SOFTWARE`.


Решение 1: добавить запятую в шаблон

```python
m = re.search(r"Version (.*),", line)
print(m.group(1))   # '15.4(2)T1'
```

Шаблон теперь останавливается на первой запятой после версии.


Проблема: лишняя запятая в строке

```python
line = "Cisco IOS Software, C880 Software (C880DATA-UNIVERSALK9-M), Version 15.4(2)T1, RELEASE SOFTWARE (fc3),"

m = re.search(r"Version (.*),", line)
print(m.group(1))   # '15.4(2)T1, RELEASE SOFTWARE (fc3)'
```

Теперь запятая после `(fc3)` стала *первой* запятой после `Version`, и группа снова захватила лишнее.

Решение 2: ленивый квантификатор `.*?`
```python
m = re.search(r"Version (.*?),", line)
print(m.group(1))   # '15.4(2)T1'
```

- `.*?` — **ленивый** (non-greedy): захватывает минимально возможное количество символов до первой запятой.
- Вместо `.*?,` можно написать `(.+?),` (один или более символов до запятой).


Сырые строки (`r"..."`)

В Python **обратный слеш** `\` используется для экранирования (например, `\n` — новая строка).

Чтобы Python **не обрабатывал** `\s`, `\d`, `\w` как спецсимволы языка, а передал их движку regex, используют **сырые строки**:

```python
# Без r (неправильно)
pattern = "Cisco\sFuji"   # Python увидит "CiscosFuji" (проблема)

# С r (правильно)
pattern = r"Cisco\sFuji"  # Python передаст regex: Cisco\sFuji
```

**Правило:** всегда используйте **`r"..."`** для regex-шаблонов.



📌 Итог

| Конструкция | Что делает |
|-------------|------------|
| `( )` | Группа захвата |
| `.group(1)` | Доступ к содержимому группы |
| `.*?` | Ленивый квантификатор (не жадный) |
| `r"..."` | Сырая строка (отключает экранирование Python) |

```python
# Лучший способ извлечь версию до запятой
m = re.search(r"Version (.*?),", line)
if m:
    version = m.group(1)
```


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   