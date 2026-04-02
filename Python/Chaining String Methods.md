**Цепочки методов строк**

Что такое chaining

Методы можно вызывать **последовательно**, один за другим. Результат первого метода становится объектом для второго.

```python
my_var = "Some String"
my_var.lower().strip()
```

Читается **слева направо**:
1. `my_var.lower()` → `"some string"`
2. `"some string".strip()` → `"some string"`

Пример

```python
my_var = "  SOME STRING  "
my_var.lower().strip()
# 'some string'
```

По шагам:
```python
my_var.lower()      # '  some string  '
'  some string  '.strip()   # 'some string'
```

Важно: исходная строка не меняется

Как и с одиночными методами, оригинал остаётся неизменным:

```python
my_var = "Some String"
my_var.lower().strip()   # 'some string'
my_var                   # 'Some String'
```

Чтобы сохранить результат — присвойте переменной:
```python
my_var = my_var.lower().strip()
```

📌 Итог

| Правило | Пример |
|---------|--------|
| Читаем слева направо | `"  TEXT  ".lower().strip()` |
| Каждый метод применяется к результату предыдущего | `"  TEXT  "` → `"  text  "` → `"text"` |
| Исходная строка не изменяется | `my_var` остаётся прежним |

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   