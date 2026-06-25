# Workflow — пошаговый процесс установки

## Порядок выполнения

### Шаг 1: Подготовка Windows
- Включить UTF-8 (`chcp 65001`)
- Проверить версию PowerShell (нужна 5.1)
- Установить ExecutionPolicy = RemoteSigned
- Создать структуру папок проекта на отдельном диске

### Шаг 2: Python 3
- Скачать с python.org (не из Store!)
- Установить с галочкой "Add Python to PATH"
- Проверить: `python --version`
- Обновить pip

### Шаг 3: Tesseract OCR
- `winget install UB-Mannheim.TesseractOCR`
- Или скачать вручную с GitHub
- Добавить `C:\Program Files\Tesseract-OCR\` в PATH
- Проверить: `& "C:\Program Files\Tesseract-OCR\tesseract.exe" --version`

### Шаг 4: Git
- `winget install Git.Git`
- Проверить: `git --version`

### Шаг 5: Node.js (опционально)
- `winget install OpenJS.NodeJS.LTS`
- Проверить: `node --version`

### Шаг 6: Python-пакеты
- `python -m pip install python-docx PyMuPDF pytesseract Pillow openpyxl pandas matplotlib python-pptx`
- Проверить каждый: `python -c "import docx; print('OK')"`

### Шаг 7: OpenWork
- Скачать с openworklabs.com/download
- Установить
- Настроить AI-провайдера (Anthropic / OpenAI / OpenRouter / Ollama)
- Добавить папки в Settings → Permissions

### Шаг 8: Проверка стека
- Запустить `check_setup.py` (скрипт внутри WINDOWS_SETUP.md)
- Все проверки должны быть зелёными ✅
