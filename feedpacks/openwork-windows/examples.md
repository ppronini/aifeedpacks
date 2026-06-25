# Examples — примеры

## ✅ Хорошая настройка

### Python установлен правильно
```
Путь:  C:\Users\john\AppData\Local\Programs\Python\Python313\python.exe
PATH:  есть оба пути (корень + Scripts)
Store: нет в PATH
```

### Tesseract работает
```
Команда: & "C:\Program Files\Tesseract-OCR\tesseract.exe" --version
Вывод:   tesseract v5.4.0 ...
PATH:    C:\Program Files\Tesseract-OCR\ — есть
```

### PowerShell — правильная цепочка
```
# Вместо && (не работает в PS 5.1):
cd project; if ($?) { npm install }
```

---

## ❌ Плохая настройка

### Python из Microsoft Store
```
Симптом:  python.exe → открывается магазин
Причина:  PATH ведёт на %LOCALAPPDATA%\Microsoft\WindowsApps\
Решение:  Использовать полный путь к настоящему Python
```

### Tesseract не в PATH
```
Симптом:  pytesseract.pytesseract.TesseractNotFoundError
Причина:  PATH не содержит C:\Program Files\Tesseract-OCR\
Решение:  Добавить в PATH или указать в коде:
          pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

### Использование `&&` в PowerShell 5.1
```
Симптом:  Ошибка "&& was unexpected at this time"
Причина:  PowerShell 5.1 не поддерживает &&
Решение:  Использовать: cmd1; if ($?) { cmd2 }
```

### Папка проекта на C:\
```
Симптом:  Диск C: переполнен, OneDrive конфликтует
Причина:  C:\ — системный, часто синхронизируется
Решение:  Проекты на отдельном диске E:\
```

---

## 🔄 Типовой диалог

**Пользователь:** Я установил Python из Microsoft Store, что делать?

**Агент:** Удалите Python из Store (через Параметры → Приложения).
Затем скачайте установщик с python.org и при установке поставьте галочку
"Add Python to PATH". После установки проверьте командой `where python` —
должен показывать путь в `AppData\Local\Programs\Python\`.
