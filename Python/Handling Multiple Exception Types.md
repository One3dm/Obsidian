**Обработка нескольких типов исключений**

Ситуация

В блоке `try` может возникнуть **разные типы ошибок**: `IndexError`, `KeyError`, `ValueError` и т.д.  
Их можно обрабатывать либо **одинаково**, либо **по-разному**.

---

## Способ 1: один обработчик для нескольких типов

Если вы хотите реагировать на несколько типов исключений **одинаково**, перечислите их в кортеже после `except`.

```python
try:
    my_list[0]               # может быть IndexError
    my_dict["missing key"]   # может быть KeyError
except (IndexError, KeyError):
    print("Gracefully handled error")
```

> [!NOTE]
> Ошибка может возникнуть **на первом же проблемном месте**. Например, если `my_list[0]` вызовет `IndexError`, то `my_dict["missing key"]` уже не выполнится.

---

## Способ 2: разные обработчики для разных типов

Если нужно **разное поведение** для разных ошибок, используйте несколько блоков `except`.

```python
try:
    my_list[0]
    my_dict["missing key"]
except IndexError:
    print("Missing list index")
except KeyError:
    print("Key missing")

print("Outside block -- happens regardless of error")
```

**Пример вывода при `KeyError`:**
```
Key missing
Outside block -- happens regardless of error
```

---

## Порядок блоков `except` важен

Блоки проверяются **сверху вниз**. Как только подходящий тип найден — он выполняется, остальные игнорируются.

```python
try:
    # какой-то код
except IndexError:
    # обрабатываем IndexError
except KeyError:
    # обрабатываем KeyError
```

> [!WARNING]
> Если поставить `except Exception` (самый общий тип) первым, он перехватит **все** исключения, и следующие блоки никогда не сработают.

---

## Сравнение подходов

| Сценарий | Решение |
|----------|---------|
| Реакция на ошибки одинаковая | `except (Type1, Type2):` |
| Реакция разная | несколько `except` |
| Нужно отловить всё | `except Exception:` (но осторожно) |

---

## 📌 Итог

```python
# Один обработчик для нескольких типов
try:
    risky_code()
except (IndexError, KeyError, ValueError):
    print("Something went wrong with index/key/value")

# Разные обработчики для разных типов
try:
    risky_code()
except IndexError:
    print("Index problem")
except KeyError:
    print("Key problem")
```

---

## 🔗 Связанные темы
- [[Основы обработки исключений]]
- [[Broad Exception Handling]]
- [[Capturing and Re-raising Exceptions]]
```