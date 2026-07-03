**Якоря и флаг `re.MULTILINE`**

Якоря: `^` и `$`

| Якорь | Значение |
|-------|----------|
| `^` | Начало строки **(или начало всего текста)** |
| `$` | Конец строки **(или конец всего текста)** |

**По умолчанию** (без флага `re.MULTILINE`) якоря `^` и `$` привязаны к **началу и концу ВСЕГО текста**, а не к каждой строке.

Проблема: поиск по всей строке не работает

```python
data = """
6 Gigabit Ethernet interfaces
32768K bytes of non-volatile configuration memory.
4194304K bytes of physical memory.
2863103K bytes of flash memory at bootflash:.
0K bytes of WebUI ODM Files at webui:

Configuration register is 0x2102
"""

# Ищем строку, которая начинается с "Config" и заканчивается на "2102"
m = re.search(r"^Config.*2102$", data)
print(m)   # None — не нашло
```

`^` ищет начало **всего текста**, а там `"6 Gigabit..."`, не `"Config"`.

Решение: флаг `re.MULTILINE` (или `re.M`)

Флаг заставляет якоря `^` и `$` работать для **каждой строки** в многострочном тексте.

```python
m = re.search(r"^Config.*2102$", data, flags=re.MULTILINE)
print(m)   # <re.Match object; span=(..., ...), match='Configuration register is 0x2102'>
```

С флагом `re.M`:
- `^` — начало **каждой строки** в тексте.
- `$` — конец **каждой строки**.


Пример с захватом регистра конфигурации
```python
# ^Config.*2102$  — строка начинается с "Config", заканчивается "2102"
# Группа (\S+) захватывает значение (непробельные символы после "is ")
pattern = r"^Config.*is (\S+)$"

m = re.search(pattern, data, flags=re.MULTILINE)
print(m.group(1))  # '0x2102'
```

- `\S+` — один или более **непробельных** символов (захватывает `0x2102`).


Сокращённая запись флага

- `re.MULTILINE` — полное имя.
- `re.M` — краткая форма (можно использовать любую).

```python
# Оба варианта эквивалентны
re.search(r"^Config.*2102$", data, flags=re.MULTILINE)
re.search(r"^Config.*2102$", data, flags=re.M)
```


📌 Итог

| Флаг | Назначение |
|------|------------|
| **по умолчанию** | `^` и `$` — весь текст целиком |
| `re.MULTILINE` / `re.M` | `^` и `$` — начало/конец **каждой строки** |

> [!TIP]
> При работе с **выводом команд** (несколько строк) почти всегда нужен флаг `re.MULTILINE`.


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   