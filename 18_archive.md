---
layout: default
title: "Файловое сжатие и упаковка"
permalink: /18_archive/
---

# 🗂 Гайд по сжатию и упаковке файлов в Linux

[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?style=flat-square\&logo=linux)]()
[![Category](https://img.shields.io/badge/category-Files%20%26%20Compression-blue?style=flat-square)]()
[![Tools](https://img.shields.io/badge/tools-tar%20|%20gzip%20|%20bzip2%20|%20zip-yellow?style=flat-square)]()

---

## 📋 Быстрая таблица команд

| Команда | Назначение | Основные ключи / параметры |
|---------|------------|---------------------------|
| `tar` | Упаковка и распаковка архивов | `-c` создать, `-x` извлечь, `-v` подробный вывод, `-f` файл архива |
| `gzip` | Сжатие файлов (GNU Zip) | `-d` распаковать, `-k` сохранить оригинал |
| `gunzip` | Распаковка gzip | `-c` вывод на stdout |
| `bzip2` | Сжатие BZip2 | `-d` распаковать, `-k` сохранить оригинал |
| `bunzip2` | Распаковка BZip2 | `-c` вывод на stdout |
| `bzcat` | Просмотр содержимого BZip2 | — (вывод на stdout) |
| `compress` | Традиционное Unix-сжатие | `-v` подробный вывод |
| `uncompress` | Распаковка compress | — |
| `zcat` | Просмотр сжатых файлов gzip/compress | — |
| `zip` | Создание ZIP-архивов | `-r` рекурсивно включить папки |
| `unzip` | Распаковка ZIP | `-l` просмотр содержимого |
| `metamail` | Извлечение вложений из MIME | `-w` записать вложения в файлы |

---

## ⚡ 1. `tar` — упаковка нескольких файлов в один архив

```bash
tar -cvf archive.tar file1 file2 dir1/
````

* `-c` — создать архив
* `-v` — подробный вывод
* `-f` — указать имя файла архива

Распаковка:

```bash
tar -xvf archive.tar
```

* `-x` — извлечь файлы

---

## ⚡ 2. `gzip` / `gunzip` — сжатие файлов с GNU Zip

### Сжатие:

```bash
gzip file.txt
```

* Результат — `file.txt.gz`

### Распаковка:

```bash
gunzip file.txt.gz
```

* `-c` — вывод содержимого на экран без распаковки

---

## ⚡ 3. `bzip2` / `bunzip2` — сжатие BZip2

### Сжатие:

```bash
bzip2 file.txt
```

* Результат — `file.txt.bz2`

### Распаковка:

```bash
bunzip2 file.txt.bz2
```

### 4. `bzcat` — работа с BZip2 через стандартный ввод/вывод

```bash
bzcat file.txt.bz2
```

* Выводит содержимое файла на экран без распаковки на диск.

---

## ⚡ 5. `compress` / `uncompress` — традиционное Unix-сжатие

### Сжатие:

```bash
compress file.txt
```

* Результат — `file.txt.Z`

### Распаковка:

```bash
uncompress file.txt.Z
```

---

## ⚡ 6. `zcat` — просмотр сжатых файлов gzip или compress

```bash
zcat file.txt.gz
zcat file.txt.Z
```

* Выводит содержимое файла на стандартный вывод без распаковки.

---

## ⚡ 7. `zip` / `unzip` — сжатие в Windows Zip формат

### Сжатие:

```bash
zip archive.zip file1 file2
```

* `-r` — рекурсивно добавить директории

### Распаковка:

```bash
unzip archive.zip
```

* `-l` — показать содержимое архива

---

## ⚡ 8. `metamail` — извлечение MIME-данных в файлы

```bash
metamail -w message.eml
```

* Используется для извлечения вложений из email-сообщений в формате MIME.

---

## 💡 Советы

* Для больших файлов лучше использовать `bzip2` — более высокое сжатие, чем `gzip`.
* Для кроссплатформенных архивов используйте `zip`.
* Для пакетирования нескольких файлов перед сжатием — используйте `tar`.
* `bzcat` и `zcat` полезны для просмотра содержимого архива без распаковки.

