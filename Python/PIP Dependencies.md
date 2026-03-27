# PIP зависимости (PIP Dependencies)

## Что такое PIP

PIP — менеджер пакетов Python. Устанавливает и управляет сторонними библиотеками.

---

## Обновление самого PIP

Перед установкой зависимостей курса Kirk рекомендует обновить PIP до определённой версии.

### Команда:

```bash
pip install pip==22.3.1
```

Или (альтернативный синтаксис):

```bash
python -m pip install pip==22.3.1
```

### Пример выполнения:

```
[py311_venv] ktbyers@pydev2 ~/VENV
$ pip install pip==22.3.1
Collecting pip==22.3.1
  Using cached pip-22.3.1-py3-none-any.whl (2.1 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 22.3
    Uninstalling pip-22.3:
    Successfully uninstalled pip-22.3
Successfully installed pip-22.3.1
```

### Проверка версии:

```bash
pip list
```

Результат:
```
Package    Version
pip        22.3.1
setuptools 65.5.0
```

---

## Установка зависимостей курса

После обновления PIP устанавливаются необходимые для курса пакеты.

> [!NOTE]
> Команды для установки зависимостей будут в следующих материалах курса.

---

## 📌 Основные команды PIP

| Команда | Назначение |
|---------|------------|
| `pip install <package>` | Установить пакет |
| `pip install <package>==<version>` | Установить конкретную версию |
| `pip list` | Показать установленные пакеты |
| `pip uninstall <package>` | Удалить пакет |
| `pip freeze` | Вывести список пакетов (для requirements.txt) |


________________________________________________________________________
Paths: [[Python]]
Tags: #Python   