**Условные операторы (if / elif / else)**

Синтаксис
```python
ip_addr = "10.1.1.1"

if "10.1" in ip_addr:
    print("Found Address")      # выполнится, если выражение True
```

- **Выражение** (expression) — вычисляется как `True` или `False`
- **Блок с отступом** — выполняется, если выражение истинно

---

## Конструкция `if / elif / else`

```python
if expression:
    print("Something happened")
    print("here")
elif other_expression:
    print("Other expression")
    print("happened here")
else:
    print("Else")
    print("happened")
```

> [!IMPORTANT]
> Выполнится **только один блок** — первый, условие которого оказалось истинным.  
> `else` — срабатывает, если все предыдущие условия ложны.

---

## Операторы сравнения

| Оператор | Значение |
|----------|----------|
| `==` | равно |
| `!=` | не равно |
| `>` | больше |
| `>=` | больше или равно |
| `<` | меньше |
| `<=` | меньше или равно |

```python
ssh_timeout = 20

if ssh_timeout == 10:
    print("Таймаут 10 секунд")
elif ssh_timeout > 30:
    print("Таймаут больше 30 секунд")
else:
    print("Неожиданный таймаут")
```

---

## Логические операторы

| Оператор | Значение |
|----------|----------|
| `and` | И — оба условия должны быть истинны |
| `or` | ИЛИ — хотя бы одно истинно |
| `not` | НЕ — отрицание |

```python
ssh_timeout = 20
host_reachable = True
ip_addr = "10.1.1.1"

if host_reachable and ssh_timeout >= 10:
    print("Пробуем подключиться")
elif not host_reachable or ip_addr == "10.1.1.1":
    print("Неверный хост, не подключаемся")
else:
    print("Неожиданная ошибка")
```

---

## Вложенные условия

```python
if host_reachable:
    if ssh_timeout is not None:
        print("Пробуем подключиться")
    else:
        print("ssh_timeout не определён, ошибка")
```

> [!NOTE]
> Можно вкладывать условия на любую глубину, но лучше не злоупотреблять — снижается читаемость.

---

## Идиоматические выражения (предпочтительные для линтеров)

```python
ssh_timeout = 20
host_reachable = False
ip_addr = None

if ssh_timeout is None:              # ✅ лучше чем == None
    print("Ошибка: нет таймаута")

if host_reachable is False:          # ✅ лучше чем == False
    print("Ошибка: хост недоступен")

if ip_addr is not None:              # ✅ лучше чем != None
    print("IP есть, подключаемся")
```

| Лучше | Хуже |
|-------|------|
| `is None` / `is not None` | `== None` / `!= None` |
| `is False` / `is True` | `== False` / `== True` (но часто достаточно `if not var`) |

---

## Truish (правдоподобность)

В логическом контексте Python считает `False`:

| Тип | False-значение |
|-----|----------------|
| `int` | `0` |
| `str` | `""` (пустая строка) |
| `list` | `[]` |
| `bool` | `False` |
| `None` | всегда `False` |

```python
ssh_timeout = 0

if not ssh_timeout:          # сработает, так как 0 → False
    print("Ошибка: нет таймаута")
```

> [!CAUTION]
> Это удобно, но может быть неочевидно. Проверяйте, что `0`, `""`, `[]` действительно означают для вас "ошибку" или "отсутствие значения".

---

## 📌 Итог

```python
# Базовая конструкция
if условие1:
    # блок 1
elif условие2:
    # блок 2
else:
    # блок 3

# Логические операторы
if a and b: ...
if a or b: ...
if not a: ...

# Сравнение с None
if value is None: ...
if value is not None: ...

# Проверка на "пустоту" (truish)
if not my_list:        # сработает, если список пуст
    print("Список пуст")
```

---

## 🔗 Связанные темы
- [[Booleans и None]]
- [[Циклы for]]
- [[Циклы while]]
```