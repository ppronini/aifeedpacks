# Checklist — финальная проверка

## Система
- [ ] Windows 10 или 11
- [ ] PowerShell 5.1 (`$PSVersionTable.PSVersion`)
- [ ] UTF-8 включён (`chcp 65001` — работает)
- [ ] ExecutionPolicy = RemoteSigned

## Python
- [ ] Установлен с python.org (не из Store)
- [ ] Версия 3.13+ (`python --version`)
- [ ] В PATH (корень + Scripts)
- [ ] pip обновлён (`python -m pip install --upgrade pip`)

## Tesseract OCR
- [ ] Установлен (`& "C:\Program Files\Tesseract-OCR\tesseract.exe" --version`)
- [ ] В PATH
- [ ] Русский язык (если нужен)

## Git
- [ ] Установлен (`git --version`)

## Node.js (опционально)
- [ ] Установлен (`node --version`)
- [ ] npm работает (`npm --version`)

## Python-пакеты
- [ ] python-docx
- [ ] PyMuPDF (fitz)
- [ ] pytesseract
- [ ] Pillow (PIL)
- [ ] openpyxl
- [ ] pandas
- [ ] matplotlib
- [ ] python-pptx

## OpenWork
- [ ] Установлен
- [ ] AI-провайдер настроен (ключ + модель)
- [ ] Workspace добавлен в Permissions
- [ ] Дополнительные папки добавлены (XAMPP, temp)

## Проверка стека
- [ ] `check_setup.py` прошёл без ошибок
- [ ] Агент OpenWork видит файлы в workspace
- [ ] Агент может выполнять команды PowerShell

---

**Если все пункты отмечены — среда полностью готова ✅**
