**Установка Python**
Рекомендуется использовать статью на Real Python
**[Installing Python](https://realpython.com/installing-python/)** — подробное руководство для Windows, macOS и Linux.

# Windows
**Современная рекомендация:** устанавливать Python из Microsoft Store.

Если устанавливаете с python.org
Обязательно отметьте опцию **"Add python.exe to PATH"** при установке.
У этого подхода есть нюансы безопасности.

![[CheckBoxPy.png]]

**Дополнительный ресурс:**
- [DigitalOcean: Install Python on Windows 10](https://www.digitalocean.com/community/tutorials/install-python-windows-10) 


# macOS
**Способ:** официальный установщик с python.org

Частая проблема
В конце установки **нужно запустить (двойным кликом)** установку SSL-сертификатов — специальный файл должен находиться в папке установки Python.

![[macOSFileInstall.png]]

**Дополнительный ресурс:**
- [DataQuest: Installing Python on Mac](https://www.dataquest.io/blog/installing-python-on-mac/) — смотрите раздел "Install Python 3 with the Official Installer"


# Linux

**Скорее всего Python уже установлен:**
```bash
which python3
python3 --version
```

**Рекомендуемая версия:** Python 3.9 или новее

Если версия старая или Python отсутствует:
-  Используйте системный менеджер пакетов
- Debian/Ubuntu: `apt`
- RHEL/CentOS: `yum` или `dnf`

**Независимо от ОС проверка установки**:
```bash
# Проверка установки
python3 --version
pip --version
```

________________________________________________________________________
Paths: [[Python]]
Tags: #Python   