# 🪟 OpenWork на Windows: полное руководство по установке и настройке

> **От свежей Windows до полностью рабочего OpenWork агента за один вечер.**
>
> Это пошаговое руководство не имеет аналогов — оно собрано из реального опыта
> ежедневной работы OpenWork Agent на Windows 11 с XAMPP, Python и AI-агентами.

---

## 📋 Содержание

1. [Что понадобится](#1-что-понадобится)
2. [Подготовка Windows](#2-подготовка-windows)
3. [Установка Python 3](#3-установка-python-3)
4. [Установка Tesseract OCR](#4-установка-tesseract-ocr)
5. [Установка Git](#5-установка-git)
6. [Установка Node.js (опционально)](#6-установка-nodejs-опционально)
7. [Python-пакеты для работы с файлами](#7-python-пакеты-для-работы-с-файлами)
8. [Установка и настройка OpenWork](#8-установка-и-настройка-openwork)
9. [Настройка AI-провайдера](#9-настройка-ai-провайдера)
10. [Настройка разрешений (Permissions)](#10-настройка-разрешений-permissions)
11. [Проверка всего стека](#11-проверка-всего-стека)
12. [Особенности PowerShell 5.1 для OpenWork](#12-особенности-powershell-51-для-openwork)
13. [Типичные проблемы и решения](#13-типичные-проблемы-и-решения)
14. [Шпаргалка по командам](#14-шпаргалка-по-командам)

---

## 1. Что понадобится

| Компонент | Версия | Зачем |
|-----------|--------|-------|
| **Windows** | 10/11 | Домашняя или Pro |
| **OpenWork** | последняя | Десктопное приложение-агент |
| **Python** | 3.13+ | Скрипты, парсинг файлов, OCR |
| **Tesseract OCR** | 5.x | Распознавание текста с картинок |
| **Git** | 2.x | Версионирование и работа с репозиториями |
| **Node.js** (необязательно) | LTS | Если нужны MCP-сервера на JS |
| **API-ключ** | — | От Anthropic / OpenAI / OpenRouter |

### Пути по умолчанию (запомнить)

```
Программа                 Путь
─────────────             ─────────────────────────────────────────────
Python 3.13               %USERPROFILE%\AppData\Local\Programs\Python\Python313\
Python Scripts            %USERPROFILE%\AppData\Local\Programs\Python\Python313\Scripts\
Tesseract OCR             C:\Program Files\Tesseract-OCR\
Git                       C:\Program Files\Git\
Node.js                   C:\Program Files\nodejs\
OpenWork (appdata)        %APPDATA%\com.differentai.openwork\
OpenWork (temp)           %TEMP%\opencode\
Рабочая директория        E:\ai\openwork\                          (или ваш путь)
```

---

## 2. Подготовка Windows

### 2.1. Включение UTF-8 (решит проблемы с русским текстом)

PowerShell от имени администратора:

```powershell
# Установить кодовую страницу 65001 (UTF-8) для текущей сессии
chcp 65001

# Для постоянного включения UTF-8 в PowerShell:
# Открыть "Параметры региона" → "Административные настройки языка"
# → "Изменить язык системы" → поставить галочку "Бета-версия: Использовать Unicode UTF-8"
```

### 2.2. Проверка PowerShell

OpenWork использует **PowerShell 5.1** (не 7). Проверьте:

```powershell
$PSVersionTable.PSVersion
# Major  Minor  Build  Revision
# -----  -----  -----  --------
# 5      1      22621  4249      ← норм
```

### 2.3. Разрешение выполнения скриптов

PowerShell от администратора:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

### 2.4. Структура папок проекта

Создайте рабочую директорию (отдельно от системного диска C:\):

```powershell
New-Item -ItemType Directory -Path "E:\ai\openwork" -Force
New-Item -ItemType Directory -Path "E:\ai\openwork\projects" -Force
New-Item -ItemType Directory -Path "E:\ai\openwork\temp" -Force
```

> **Почему не C:\?**
> OpenWork агент активно читает/пишет файлы. Отдельный диск E:\:
> — не забивает системный SSD и не давит на квоты OneDrive.

---

## 3. Установка Python 3

### 3.1. Скачивание

```
Сайт: https://www.python.org/downloads/
Версия: Python 3.13.x (или новее)
```

### 3.2. Установка

**Критически важно:**

1. В самом начале установки — **поставить галочку "Add Python to PATH"** ⚠️
2. Выбрать **"Install for all users"** (опционально, но удобно)
3. Путь по умолчанию:
   ```
   C:\Users\<ВАШ_ПОЛЬЗОВАТЕЛЬ>\AppData\Local\Programs\Python\Python313\
   ```

**НЕ СТАВИТЬ из Microsoft Store!** — `python.exe` из Store — это заглушка,
которая открывает магазин вместо запуска интерпретатора.

### 3.3. Проверка

```powershell
# Через полный путь (всегда сработает)
C:\Users\<USER>\AppData\Local\Programs\Python\Python313\python.exe --version

# Если PATH настроен — можно просто:
python --version
```

Ожидаемый вывод:
```
Python 3.13.2
```

### 3.4. Настройка PATH

Проверьте, что в PATH есть оба пути:

```powershell
# Текущие пути из PATH
$env:PATH -split ';' | Select-String "Python"
```

Должны быть:
```
%USERPROFILE%\AppData\Local\Programs\Python\Python313\
%USERPROFILE%\AppData\Local\Programs\Python\Python313\Scripts\
```

Если нет — добавить вручную:
1. `Win + R` → `SystemPropertiesAdvanced`
2. Кнопка **"Переменные среды"**
3. В **"Переменные среды пользователя"** → `Path` → Изменить
4. Добавить два пути (без `%USERPROFILE%` — подставить ваш логин)

### 3.5. Обновление pip

```powershell
python -m pip install --upgrade pip
```

---

## 4. Установка Tesseract OCR

> Нужен для распознавания текста с изображений (OCR).
> OpenWork использует его через `pytesseract`.

### 4.1. Установка через winget

```powershell
winget install "UB-Mannheim.TesseractOCR" --source winget
```

Если `winget` не установлен — скачать вручную:
```
https://github.com/UB-Mannheim/tesseract/wiki
```
Файл: `tesseract-ocr-w64-setup-5.x.x.exe`

### 4.2. Где устанавливается

```
C:\Program Files\Tesseract-OCR\tesseract.exe
```

**Не меняйте путь** — всё завязано на него.

### 4.3. Проверка

```powershell
& "C:\Program Files\Tesseract-OCR\tesseract.exe" --version
```

Ожидаемый вывод:
```
tesseract v5.4.0.20240606
 leptonica-1.84.1
 Found AVX2
 Found AVX
 Found FMA
 Found SSE4.1
```

### 4.4. Добавление в PATH (обязательно)

```powershell
# Проверить, есть ли в PATH
$env:PATH -split ';' | Select-String "Tesseract"

# Если нет — добавить вручную (через переменные среды):
# C:\Program Files\Tesseract-OCR\
```

**Если PATH не настроен** — в Python-скриптах придётся указывать явно:
```python
import pytesseract
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

### 4.5. Языки

По умолчанию — только английский. Чтобы проверить:

```powershell
& "C:\Program Files\Tesseract-OCR\tesseract.exe" --list-langs
```

Если нужен русский — скачать `rus.traineddata`:
```
https://github.com/tesseract-ocr/tessdata/raw/main/rus.traineddata
```
Положить в: `C:\Program Files\Tesseract-OCR\tessdata\`

---

## 5. Установка Git

```powershell
winget install Git.Git
```

Или скачать с: https://git-scm.com/

Проверка:
```powershell
git --version
```

---

## 6. Установка Node.js (опционально)

Нужен только если вы используете MCP-сервера на JavaScript:

```powershell
winget install OpenJS.NodeJS.LTS
```

Или скачать с: https://nodejs.org/ (LTS-версия)

Проверка:
```powershell
node --version
npm --version
```

---

## 7. Python-пакеты для работы с файлами

### 7.1. Массовая установка

```powershell
python -m pip install python-docx PyMuPDF pytesseract Pillow openpyxl pandas matplotlib python-pptx
```

### 7.2. Что и зачем

| Пакет | Модуль в коде | Зачем OpenWork |
|-------|---------------|----------------|
| `python-docx` | `docx` | Чтение/создание Word (.docx) |
| `PyMuPDF` | `fitz` | Чтение PDF, извлечение текста |
| `pytesseract` | `pytesseract` | OCR — распознавание текста с картинок |
| `Pillow` | `PIL` | Открыть, обрезать, конвертировать изображения |
| `openpyxl` | `openpyxl` | Чтение/запись Excel (.xlsx) |
| `pandas` | `pandas` | Анализ данных, таблицы, CSV |
| `matplotlib` | `matplotlib` | Построение графиков и диаграмм |
| `python-pptx` | `pptx` | Создание PowerPoint презентаций |

### 7.3. Проверка каждого пакета

```powershell
python -c "import docx; print('✅ python-docx', docx.__version__)"
python -c "import fitz; print('✅ PyMuPDF', fitz.version)"
python -c "import pytesseract; print('✅ pytesseract')"
python -c "from PIL import Image; print('✅ Pillow')"
python -c "import openpyxl; print('✅ openpyxl', openpyxl.__version__)"
python -c "import pandas; print('✅ pandas', pandas.__version__)"
python -c "import matplotlib; print('✅ matplotlib')"
python -c "import pptx; print('✅ python-pptx')"
```

---

## 8. Установка и настройка OpenWork

### 8.1. Скачивание

```
Сайт: https://openworklabs.com/download
```

### 8.2. Установка

Стандартный установщик Windows (`.exe`). Никаких особых настроек не требует.

### 8.3. Первый запуск

После установки:

1. Откройте OpenWork
2. Вас встретит экран приветствия
3. Нажмите **"Get Started"** или **"Sign In"**

### 8.4. Создание workspace (рабочей области)

Workspace — это корень проекта, к которому OpenWork имеет доступ.

1. Зайдите в **Settings** (шестерёнка в левом меню)
2. Раздел **Permissions**
3. Кнопка **"Add Folder"** → укажите `E:\ai\openwork` (или ваш путь)
4. При необходимости — добавьте дополнительные папки:
   - `E:\xampp\htdocs` (если работаете с XAMPP/веб-проектами)
   - `C:\Users\<USER>\AppData\Local\Temp\opencode` (временные файлы)

### 8.5. Проверка доступа

В окне OpenWork попросите агента выполнить:

```
Check if you can access E:\ai\openwork\
```

Если получаете **"permission denied"** — вернитесь в Settings → Permissions и добавьте нужную папку.

---

## 9. Настройка AI-провайдера

OpenWork не имеет своего AI — он подключается к вашему провайдеру.

### 9.1. Anthropic (Claude) — рекомендуется

1. **Settings** → **AI Providers**
2. Кнопка **"Add Provider"** → выберите **Anthropic**
3. API-ключ: https://console.anthropic.com/ → API Keys → Create Key
4. Модель: `claude-sonnet-4-20250514` (или новее)
5. Сохранить

### 9.2. Альтернативы

| Провайдер | Где взять ключ | Особенности |
|-----------|----------------|-------------|
| **OpenAI** | platform.openai.com/api-keys | Модели GPT-4o |
| **OpenRouter** | openrouter.ai/keys | Единый ключ для многих моделей |
| **OpenWork Cloud** | Через аккаунт в Den | Не требует своего ключа |
| **Ollama (локально)** | — | Бесплатно, но слабее |


---

## 10. Настройка разрешений (Permissions)

Это **самая частая проблема** при работе OpenWork на Windows.

### 10.1. Что нужно добавить

| Папка | Зачем |
|-------|-------|
| `E:\ai\openwork\` | Основной workspace |
| `E:\xampp\htdocs\` | Веб-проекты (если есть XAMPP) |
| `%TEMP%\opencode\` | Временные файлы агента |
| `C:\Users\<USER>\AppData\Local\Temp\` | Иногда нужен доступ к temp |

### 10.2. Как добавить

```
Settings → Permissions → "Add Folder" → выбрать папку → открыть
```

### 10.3. Что делать при ошибке

Ошибка агента: *"I don't have access to the file"* или *"permission denied"*

Решение:
```
→ Нажмите на сообщение об ошибке в чате
→ Будет ссылка "Open Settings" — нажмите
→ Добавьте нужную папку
→ Вернитесь в чат и повторите команду
```

---

## 11. Проверка всего стека

### 11.1. Скрипт-чекер

Создайте файл `check_setup.py`:

```python
"""
Проверка готовности Windows к работе OpenWork Agent.
Запускать: python check_setup.py
"""

import sys
import subprocess
import platform

print("=" * 52)
print("  OpenWork — проверка окружения Windows")
print("=" * 52)

# 1. Система
print(f"\n🪟  Система:       {platform.system()} {platform.release()}")
print(f"    Версия:        {platform.version()}")
print(f"    PowerShell:    {platform.win32_ver()[0]}")

# 2. Python
print(f"\n🐍  Python:        {sys.version.split()[0]}")
print(f"    Архитектура:   {platform.architecture()[0]}")
print(f"    Путь:          {sys.executable}")

# 3. Пакеты
packages = {
    "python-docx": "docx",
    "PyMuPDF": "fitz",
    "pytesseract": "pytesseract",
    "Pillow": "PIL",
    "openpyxl": "openpyxl",
    "pandas": "pandas",
    "matplotlib": "matplotlib",
    "python-pptx": "pptx",
}

print(f"\n📦  Python-пакеты:")
all_ok = True
for pkg_name, mod_name in packages.items():
    try:
        __import__(mod_name)
        print(f"      ✅ {pkg_name}")
    except ImportError:
        print(f"      ❌ {pkg_name} — не установлен")
        all_ok = False

# 4. Tesseract OCR
print(f"\n🔍  Tesseract OCR:")
tesseract_path = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
try:
    result = subprocess.run(
        [tesseract_path, "--version"],
        capture_output=True, text=True, timeout=5
    )
    first_line = result.stdout.split("\n")[0]
    print(f"      ✅ {first_line}")
    print(f"      Путь: {tesseract_path}")
except FileNotFoundError:
    print(f"      ❌ tesseract.exe не найден")
    print(f"      Ожидался путь: {tesseract_path}")
    all_ok = False

# 5. Git
print(f"\n🔧  Git:")
try:
    result = subprocess.run(["git", "--version"], capture_output=True, text=True)
    print(f"      ✅ {result.stdout.strip()}")
except FileNotFoundError:
    print(f"      ❌ Git не установлен")

# 6. Итог
print(f"\n{'=' * 52}")
if all_ok:
    print("  ✅  Среда полностью готова к работе OpenWork!")
else:
    print("  ❌  Есть проблемы (см. выше)")
print("=" * 52)
```

### 11.2. Запуск проверки

```powershell
python check_setup.py
```

### 11.3. Финальная проверка через OpenWork

В окне агента выполните:

```
Run check_setup.py from the workspace root and report all results
```

Если всё зелёное — вы готовы.

---

## 12. Особенности PowerShell 5.1 для OpenWork

OpenWork Agent работает **только с PowerShell 5.1** (не 7).
Вот ключевые отличия, которые нужно знать:

### 12.1. Нет `&&`

**НЕПРАВИЛЬНО:**
```powershell
cd project && npm install    # ❌ Не работает
```

**ПРАВИЛЬНО:**
```powershell
Set-Location -Path "project"; if ($?) { npm install }
```

### 12.2. Команды с пробелами — в кавычках с `&`

**НЕПРАВИЛЬНО:**
```powershell
C:\Program Files\Tesseract-OCR\tesseract.exe --version    # ❌
```

**ПРАВИЛЬНО:**
```powershell
& "C:\Program Files\Tesseract-OCR\tesseract.exe" --version
```

### 12.3. Не `cd`, а `workdir`

OpenWork-агент не должен менять директорию через `cd`.
Вместо этого используется параметр `workdir` инструмента bash.

**Как агенту:** агент вызывает bash с `workdir: "E:\ai\openwork"`,
а не пишет `cd E:\ai\openwork`.

### 12.4. Кавычки

| Тип | Пример | Назначение |
|-----|--------|------------|
| Двойные | `"Hello $name"` | Интерполяция переменных |
| Одинарные | `'Hello $name'` | Литерал (без подстановки) |

### 12.5. UTF-8 и русский текст

Если русский текст отображается как `????`:

```powershell
# Перед работой с русским:
chcp 65001
```

Или навсегда: **Параметры региона** → **Использовать Unicode UTF-8**.

### 12.6. Команды-цепочки (серия действий)

```powershell
cmd1; if ($?) { cmd2 }; if ($?) { cmd3 }
```

Эта конструкция выполнит `cmd2` только если `cmd1` успешен, и `cmd3` только если `cmd2` успешен.

---

## 13. Типичные проблемы и решения

### 13.1. OpenAI: это OpenWork, а не Open AI

```
Симптом: Агент пишет "I don't have access to OpenAI"
Причина: Путаница названий — OpenWork ≠ OpenAI
Решение: Уточните агенту: "OpenWork is the desktop app, 
         not OpenAI. Use the configured provider."
```

### 13.2. Permission Denied при работе с файлами

```
Симптом: Агент не может прочитать/записать файл
Причина: Папка не добавлена в разрешения OpenWork
Решение: Settings → Permissions → Add folder

Можно добавить папки, которые обычно нужны:
  • E:\ai\openwork
  • E:\xampp\htdocs
  • %TEMP%\opencode
```

### 13.3. Python из Microsoft Store

```
Симптом: python.exe открывает магазин вместо запуска
Причина: Установлен Python из Store (заглушка)
Решение: всегда использовать полный путь:
    C:\Users\<USER>\AppData\Local\Programs\Python\Python313\python.exe
    
    Проверка: where python покажет несколько путей
    — нужен тот, что ведёт в AppData\Local\Programs\Python\
```

### 13.4. TesseractNotFoundError

```
Симптом: pytesseract не может найти tesseract.exe
Причина: Tesseract не в PATH

Решение (два варианта):
1. Добавить C:\Program Files\Tesseract-OCR\ в PATH
2. Или в Python-коде указать явно:
   pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

### 13.5. PowerShell execution policy

```
Симптом: Ошибка "running scripts is disabled on this system"
Решение: от администратора:
    Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

### 13.6. Winget не найден

```
Симптом: winget: команда не найдена
Решение: Установить App Installer из Microsoft Store.
         Или скачать установщики вручную с сайтов.
```

### 13.7. Битые символы (кракозябры)

```
Симптом: Текст в консоли отображается как \234\567\890
Решение: Переключить кодировку:
    chcp 65001
```

### 13.8. OpenWork не видит установленные пакеты

```
Симптом: Агент пишет "module not found"
Причина: OpenWork может использовать другой Python или venv
Решение: Убедитесь, что у агента правильный путь к Python:
    C:\Users\<USER>\AppData\Local\Programs\Python\Python313\python.exe
    
    Или укажите агенту явно:
    "Use the Python at C:\Users\<USER>\AppData\Local\Programs\Python\Python313\python.exe"
```

---

## 14. Шпаргалка по командам

### Установка

```powershell
# Python
python -m pip install python-docx PyMuPDF pytesseract Pillow openpyxl pandas matplotlib python-pptx

# Tesseract OCR
winget install "UB-Mannheim.TesseractOCR" --source winget

# Git
winget install Git.Git

# Node.js
winget install OpenJS.NodeJS.LTS
```

### Обновление

```powershell
python -m pip install --upgrade pip
python -m pip install --upgrade python-docx PyMuPDF pytesseract Pillow openpyxl pandas matplotlib python-pptx
```

### Диагностика

```powershell
# Версия Python
python --version

# Версия Tesseract
& "C:\Program Files\Tesseract-OCR\tesseract.exe" --version

# Версия Git
git --version

# Список установленных пакетов
python -m pip list

# Экспорт в requirements.txt
python -m pip freeze > requirements.txt

# PowerShell версия
$PSVersionTable.PSVersion
```

---

## 🏁 Итоговый чеклист

После прохождения всех шагов:

- [ ] **Python 3.13+** установлен и в PATH
- [ ] **Tesseract OCR** установлен и в PATH
- [ ] **Git** установлен
- [ ] **Python-пакеты** установлены (8 шт.)
- [ ] **OpenWork** установлен
- [ ] **Workspace** создан (папка добавлена в Permissions)
- [ ] **AI-провайдер** настроен (ключ + модель)
- [ ] **UTF-8** включен или `chcp 65001` выполняется
- [ ] **ExecutionPolicy** = RemoteSigned
- [ ] `check_setup.py` прошёл без ошибок

---

*Гайд составлен на основе реального опыта эксплуатации OpenWork на Windows 11
с января по июнь 2026 года. Все пути и команды проверены в работе.*

*Последнее обновление: июнь 2026*
