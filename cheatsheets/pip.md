## pip cheatsheet

### basis

```bash
pip3 install colorama            # Команда для установки colorama
pip3 install --upgrade pip       # Команда для обновлкния PIP
pip3 --version                   # посмотреть версию PIP
pip3 freeze                      # посмотреть установленные пакеты
pip3 list -o                     # посмотреть версии пакетов
pip3 install [имя_пакета] -U     # обновить конкретный пакет
pip3 install -U $(pip3 freeze | cut -d '=' -f 1)  # обновляет все пакеты
pip3 install pyinstaller         # Команда установки мода для компеляции
pyinstaller -F [имя_файла.py]    # Команда чтобы компилировать Питон файл
pip3 install [имя_пакета]        # для установки [имя_пакета]
pip3 install pygame==2.0.0.dev6  # так установил пайгейм после 100500 попыток
```

---

THE END
