---
layout: default
title: "Автоматизация задач с помощью cron"
permalink: /cron-guide/
---

[![Linux](https://img.shields.io/badge/Platform-Linux-blue?style=flat-square&logo=linux)]()
[![Debian](https://img.shields.io/badge/Tested%20on-Astra%20Linux%20%7C%20RED%20OS%20%7C%20Debian-orange?style=flat-square&logo=debian)]()
[![Type](https://img.shields.io/badge/Type-Task%20Scheduler-lightgrey?style=flat-square&logo=task)]()
[![CLI](https://img.shields.io/badge/Interface-Console%20Only-success?style=flat-square&logo=gnu-bash)]()
[![Docs](https://img.shields.io/badge/Docs-cron%20%7C%20crontab-important?style=flat-square&logo=markdown)]()

# ⏱ Автоматизация задач с помощью cron в Linux

---

## 📑 Содержание

- [Что такое cron](#chto-takoe-cron)
- [Основные преимущества cron](#osnovnye-preimushchestva-cron)
- [Структура cron](#struktura-cron)
- [Формат записи cron](#format-zapisi-cron)
- [Системные и пользовательские задачи](#sistemnye-i-polzovatelskie-zadachi)
- [Редактирование crontab](#redaktirovanie-crontab)
- [Примеры cron-заданий](#primer-cron-zadaniy)
  - [Ежедневное резервное копирование](#ezhednevnoe-rezervnoe-kopirovanie)
  - [Очистка временных файлов](#ochistka-vremennyh-faylov)
  - [Запуск скрипта каждые 5 минут](#zapusk-skripta-kazhdyie-5-minut)
- [Переменные среды в cron](#peremennye-sredy-v-cron)
- [Логирование и отладка](#logirovanie-i-otladka)
- [Рекомендации по использованию](#rekomendatsii-po-ispolzovaniyu)
- [Заключение](#zaklyuchenie)

---

<a id="chto-takoe-cron"></a>
## Что такое cron

**cron** — это встроенный планировщик задач в Linux, который позволяет запускать команды или скрипты автоматически в указанное время или с определённой периодичностью.

Используется для:

- автоматического резервного копирования  
- очистки временных файлов  
- мониторинга системы  
- выполнения повторяющихся задач  

---

<a id="osnovnye-preimushchestva-cron"></a>
## ⚙️ Основные преимущества cron

* ✅ Полная автоматизация рутинных задач  
* ✅ Гибкая настройка расписания  
* ✅ Поддержка системных и пользовательских задач  
* ✅ Лёгкая интеграция с Bash-скриптами и Python  

---

<a id="struktura-cron"></a>
## 📂 Структура cron

Cron состоит из двух основных компонентов:

1. **Демон `cron`** – работает в фоне и проверяет расписание задач.  
2. **Файлы `crontab`** – содержат список заданий для выполнения.  

Файлы crontab бывают двух типов:

| Тип             | Описание                                         | Пример пути                          |
| --------------- | ----------------------------------------------- | ------------------------------------ |
| Системный       | Задачи для всех пользователей                  | `/etc/crontab`, `/etc/cron.d/`       |
| Пользовательский | Задачи конкретного пользователя                 | `crontab -e` (хранится в `/var/spool/cron`) |

---

<a id="format-zapisi-cron"></a>
## 📝 Формат записи cron

Каждая строка crontab имеет 5 полей времени + команда:

```

* * * * * команда
| | | | |
| | | | └─ день недели (0-7, 0 и 7 = воскресенье)
| | | └── месяц (1-12)
| | └─── день месяца (1-31)
| └──── час (0-23)
└───── минута (0-59)

````

Примеры:

| Запись        | Описание                           |
| ------------- | --------------------------------- |
| `0 2 * * *`   | Каждый день в 02:00               |
| `*/5 * * * *` | Каждые 5 минут                     |
| `0 0 * * 0`   | Каждый воскресенье в 00:00         |

---

<a id="sistemnye-i-polzovatelskie-zadachi"></a>
## Системные и пользовательские задачи

- **Системные задачи** обычно выполняются от root и находятся в `/etc/crontab` или `/etc/cron.d/`.  
- **Пользовательские задачи** редактируются через `crontab -e` и выполняются с правами конкретного пользователя.  

---

<a id="redaktirovanie-crontab"></a>
## ✏️ Редактирование crontab

Команды для работы с crontab:

```bash
# Редактирование crontab текущего пользователя
crontab -e

# Просмотр crontab текущего пользователя
crontab -l

# Удаление всех заданий пользователя
crontab -r
````

---

<a id="primer-cron-zadaniy"></a>

## Примеры cron-заданий

<a id="ezhednevnoe-rezervnoe-kopirovanie"></a>

### 💾 Ежедневное резервное копирование

Запуск скрипта `/home/user/backup.sh` каждый день в 03:00:

```bash
0 3 * * * /home/user/backup.sh >> /var/log/backup.log 2>&1
```

* `>> /var/log/backup.log` – добавление логов в файл
* `2>&1` – вывод ошибок в тот же файл

---

<a id="ochistka-vremennyh-faylov"></a>

### 🧹 Очистка временных файлов

Удаление файлов старше 7 дней в `/tmp` каждое воскресенье в 01:30:

```bash
30 1 * * 0 find /tmp -type f -mtime +7 -delete
```

---

<a id="zapusk-skripta-kazhdyie-5-minut"></a>

### ⏱ Запуск скрипта каждые 5 минут

```bash
*/5 * * * * /home/user/script.sh
```

---

<a id="peremennye-sredy-v-cron"></a>

## 🌐 Переменные среды в cron

По умолчанию cron использует минимальные переменные среды. Рекомендуется указывать:

```bash
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```

Можно добавить в начале crontab.

---

<a id="logirovanie-i-otladka"></a>

## 📝 Логирование и отладка

* Системные логи cron находятся в `/var/log/syslog` или `/var/log/cron.log`
* Для проверки работы задания можно временно перенаправлять вывод в файл:

```bash
* * * * * /home/user/test.sh >> /tmp/test.log 2>&1
```

---

<a id="rekomendatsii-po-ispolzovaniyu"></a>

## ⚙️ Рекомендации по использованию

* Всегда указывайте полный путь к скриптам и командам
* Не используйте интерактивные команды в cron
* Используйте логирование для отладки
* Разделяйте системные и пользовательские задания

---

<a id="zaklyuchenie"></a>

## ✅ Заключение

`cron` — мощный инструмент автоматизации задач в Linux. С его помощью можно:

* Регулярно запускать скрипты и команды
* Автоматизировать резервное копирование и очистку
* Контролировать выполнение задач через логи

Правильная настройка crontab позволяет сэкономить время и повысить надёжность работы системы.
