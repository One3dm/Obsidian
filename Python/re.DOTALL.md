re.DOTALL

По умолчанию: точка `.` не включает символ новой строки

```python
m = re.search("^.*$", "simple test\nhello")
print(m)   # None
```

Точка `.` не захватывает `\n`, поэтому шаблон `^.*$` не находит совпадение во всей строке.


С флагом `re.DOTALL`: точка `.` включает символ новой строки
```python
m = re.search("^.*$", "simple test\nhello", flags=re.DOTALL)
print(m)                     # <re.Match object; span=(0, 17), match='simple test\nhello'>
print(m.group(0))            # 'simple test\nhello'
```

Флаг `re.DOTALL` заставляет точку `.` соответствовать **любому символу, включая `\n`**.

📌 Итог

| Флаг | Поведение точки `.` |
|------|---------------------|
| без флага | не включает `\n` |
| `re.DOTALL` | включает `\n` |

```python
# Краткая форма (re.S)
re.search("^.*$", text, flags=re.S)
```

