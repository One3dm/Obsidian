`$PYTHONPATH` — изменение пути поиска библиотек

`$PYTHONPATH` — это **переменная окружения**, которая позволяет **добавлять директории** в `sys.path`. Это удобно, когда нужно импортировать свои модули из нестандартных мест.


Как установить `$PYTHONPATH` (macOS / Linux)
```bash
export PYTHONPATH=/home/ktbyers/learning_python/bin
```

Проверка:
```bash
env | grep PYTHONPATH
# PYTHONPATH=/home/ktbyers/learning_python/bin
```

После этого директория появится в `sys.path`:
```python
import sys
print(sys.path)
# ... '/home/ktbyers/learning_python/bin', ...
```


Импорт библиотек из добавленной директории
```bash
$ ls ~/learning_python/bin
my_lib.py  __pycache__

$ python
>>> import my_lib
Hello world
>>> my_lib.__file__
'/home/ktbyers/learning_python/bin/my_lib.py'
```

Файл `my_lib.py` был найден благодаря `$PYTHONPATH`.

Как установить `$PYTHONPATH` (Windows)

В командной строке (cmd):
```cmd
set PYTHONPATH=C:\Users\Administrator\bin
```

После этого директория появится в `sys.path`:
```python
import sys
print(sys.path)
# ... 'C:\\Users\\Administrator\\bin', ...
```


Альтернатива: настройка через IDE

На macOS и Windows часто удобнее использовать **IDE** (VSCode, PyCharm) для управления `PYTHONPATH`.

Пример для VSCode (файл `.vscode/settings.json`):
```json
{
  "terminal.integrated.env.osx": {
    "PYTHONPATH": "${workspaceFolder}/src"
  },
  "terminal.integrated.env.windows": {
    "PYTHONPATH": "${workspaceFolder}/src"
  }
}
```


📌 Итог

| Действие | macOS / Linux | Windows |
|----------|---------------|---------|
| Установить переменную | `export PYTHONPATH=/path` | `set PYTHONPATH=C:\path` |
| Проверить | `env \| grep PYTHONPATH` | `echo %PYTHONPATH%` |
| Где используется | В текущей сессии терминала | В текущей сессии cmd |
| Альтернатива | IDE (VSCode, PyCharm) | IDE (VSCode, PyCharm) |

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   
