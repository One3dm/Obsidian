**Циклы `while`**

Синтаксис
```python
while expression:
    print("A message")
    print("A second message")
```

- `while` — ключевое слово
- `expression` — условие (если `True` — входим в цикл, если `False` — выходим)
- **Блок с отступом** — выполняется, пока условие истинно


Важно: избегать бесконечного цикла

Нужно гарантировать, что условие когда-нибудь станет `False`.

Ошибка — бесконечный цикл
```python
i = 0
while i <= 5:
    if i == 3:
        continue   # ❌ i не увеличивается, застреваем на 3
    print(i)
    i += 1
```

Выход — только через `Ctrl+C`.

`break`, `continue`, `else` в `while`

Работают так же, как в `for`:
```python
i = 0
while i <= 5:
    if i == 3:
        i += 1
        continue          # прыгаем в начало цикла
    elif i == 5:
        break             # выходим из цикла полностью
    print(i)
    i += 1
else:
    print("No 'break' occurred")   # выполнится, если не было break
```


`while True` — бесконечный цикл с выходом по `break`
```python
while True:
    # делаем что-то
    if условие_выхода:
        break
```

> [!NOTE]
> `while True` — распространённый паттерн, когда условие выхода определяется внутри цикла.



Вложенные циклы
```python
base_addr = "172.31"
third_octet = 0

while third_octet < 10:
    last_octet = 2
    while last_octet < 255:
        ip_addr = f"{base_addr}.{third_octet}.{last_octet}"
        print(f"IP Address: {ip_addr}")
        last_octet += 1
    third_octet += 1
```

Можно вкладывать `while` в `while`, а также `for` внутрь `while` и наоборот.


`for` vs `while`

| `for` | `while` |
|-------|---------|
| Перебирает коллекции («для каждого») | Событийно-ориентированный («пока верно») |
| Количество итераций часто известно заранее | Количество итераций может быть неизвестно |
| `for ip in ip_list:` | `while timeout_not_expired:` |

```python
# for — для перебора коллекции
for ip in ip_list:
    print(ip)

# while — пока условие истинно
timeout = 5
start = time.time()
while time.time() - start < timeout:
    # проверяем что-то
    pass
```


📌 Итог

```python
# Базовая конструкция
while условие:
    # действия

# Бесконечный цикл с break
while True:
    if условие_выхода:
        break

# continue и else
while условие:
    if пропустить:
        continue
    if выход:
        break
else:
    print("Не было break")
```

| Ключевое слово | Что делает |
|----------------|------------|
| `break` | Немедленный выход из цикла |
| `continue` | Переход к следующей итерации |
| `else` | Выполняется, если не было `break` |

---

## 🔗 Связанные темы
- [[Циклы for (For Loops)]]
- [[Условные операторы (Conditionals)]]
```