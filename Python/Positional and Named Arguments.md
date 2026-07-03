Позиционные и именованные аргументы

Терминология

| Термин | Значение |
|--------|----------|
| **Параметр** | Переменная в определении функции (существует только внутри функции) |
| **Аргумент** | Значение, которое передаётся в функцию при вызове |

Позиционные аргументы
Аргументы связываются с параметрами **по порядку**: первый с первым, второй со вторым и т.д.

```python
def display_output(msg1, msg2):
    print(f"msg1: {msg1}")
    print(f"msg2: {msg2}")

display_output("Hello", "Something")
```

Вывод:
```
msg1: Hello
msg2: Something
```


Именованные аргументы

Явно указываем, какому параметру какой аргумент передаётся. **Порядок не важен**.

```python
display_output(msg2="Hello", msg1="Something")
```

Вывод:
```
msg1: Something
msg2: Hello
```

---

## Смешивание позиционных и именованных аргументов

**Правило:** сначала все **позиционные**, потом **именованные**.

```python
display_output("This is a test", msg3="named args", msg2="of positional and")
```

Позиционный аргумент `"This is a test"` → `msg1`  
Именованные `msg2` и `msg3` получают свои значения.

---

## Ошибка: повторное назначение аргумента

```python
display_output("This is a test", "named args", msg2="of positional and")
# TypeError: display_output() got multiple values for argument 'msg2'
```

- Позиционные: `"This is a test"` → `msg1`, `"named args"` → `msg2`
- Именованный: `msg2="of positional and"` — конфликт

---

## 📌 Итог

| Тип | Пример | Правило |
|-----|--------|---------|
| Позиционные | `func(1, 2, 3)` | Порядок важен |
| Именованные | `func(a=1, b=2)` | Порядок не важен |
| Смешанные | `func(1, b=2, c=3)` | **Сначала позиционные** |

---

## 🔗 Связанные темы
- [[Основы функций (Function Basics)]]
- [[Default Values]]
- [[Using *args and **kwargs]]
```