Виртуальные окружения (Virtual Environments)

Зачем нужны виртуальные окружения

1. **Системный Python** используется операционной системой — не хотим его трогать.
2. **Разные проекты** могут требовать разные версии библиотек.
3. **Изоляция** — изменения в одном окружении не влияют на другие.



Как создать виртуальное окружение

```bash
python3.11 -m venv test_venv
```

- `python3.11` — версия Python, которую вы хотите использовать
- `-m venv` — модуль для создания виртуального окружения
- `test_venv` — имя окружения (папка, которая будет создана)


Активация виртуального окружения

### macOS / Linux

```bash
source ~/VENV/test_venv/bin/activate
```

После активации в начале строки появляется имя окружения:

```bash
[test_venv] ktbyers@pydev2 ~$
```

Windows

```cmd
C:\Users\Administrator\CODE\pynet-ons-dec22>.\venv\Scripts\activate
```

VSCode

В VSCode можно выбрать интерпретатор через **Command Palette** → **Python: Select Interpreter** и выбрать нужное окружение.

---

## Деактивация

```bash
deactivate
```

---

## Что внутри свежего виртуального окружения

После создания в окружении установлены только самые базовые пакеты:

```bash
$ pip list
Package    Version
--------   -------
pip        22.3
setuptools 65.5.0
```

---

## Как установить разные версии Python

### macOS / Windows
- Скачать с [python.org](https://python.org) и установить

### Linux
- Использовать системные менеджеры пакетов (`apt`, `yum`)
- Или установить из исходников

### pyenv — универсальное решение

**pyenv** позволяет легко переключаться между разными версиями Python:

```bash
pyenv install 3.11.4
pyenv global 3.11.4
```

---

## 📌 Итог

| Действие | Команда |
|----------|---------|
| Создать окружение | `python3.11 -m venv my_venv` |
| Активировать (Linux/macOS) | `source my_venv/bin/activate` |
| Активировать (Windows) | `my_venv\Scripts\activate` |
| Деактивировать | `deactivate` |
| Посмотреть пакеты | `pip list` |

---

## 🔗 Связанные темы
- [[PIP]]
- [[sys.path — как Python находит библиотеки]]
- [[$PYTHONPATH]]
```