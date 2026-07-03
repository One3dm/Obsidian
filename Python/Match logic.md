Логика поиска: объект Match и проверка на None

Что возвращает `re.search()`

- **Если шаблон найден** — возвращает объект `Match`
- **Если шаблон не найден** — возвращает `None`


Пример 1: успешный поиск

```python
match = re.search(r"^Configuration register is (\S+)$", data, flags=re.M)

if match:
    print(match.group())   # 'Configuration register is 0x2102'
    print(match.group(1))  # '0x2102'
```

- `match.group()` — весь найденный текст
- `match.group(1)` — содержимое первой группы `(\S+)`



Пример 2: неудачный поиск

```python
match = re.search(r"^Configuration FAIL register is (\S+)$", data, flags=re.M)

print(match)   # None
```

`match` — это `None`, поэтому вызывать `match.group()` нельзя — будет ошибка.


Важно: всегда проверяйте результат

```python
match = re.search(pattern, data)

if match:
    # безопасно работать с match.group()
    print(match.group(1))
else:
    print("Шаблон не найден")
```

Попытка вызвать `.group()` при `match = None` вызывает `AttributeError`.


Ключевые выводы

| Ситуация | Результат `re.search()` |
|----------|------------------------|
| Найдено совпадение | Объект `Match` |
| Не найдено совпадение | `None` |
| Проверка перед использованием | `if match:` |

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   
