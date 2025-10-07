# 📘 Основные Linux команды - заметки 🗃️

[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?style=flat-square&logo=linux)](https://kernel.org)
![Tested on](https://img.shields.io/badge/tested%20on-Red%20OS%207.3%20%7C%208.0%20%7C%20Astra%20SE%201.7.5%20%7C%201.8-orange?style=flat-square)
![Docs](https://img.shields.io/badge/docs-markdown-blueviolet?style=flat-square)

Решил собрать полезные инструкции и шпаргалки для работы в Linux.  
(Astra Linux, РЕД ОС и другие дистрибутивы).  
Все материалы собрал в формате отдельных статей для быстрого использования 📝

## 📌 Как использовать

Можно открывать статьи прямо на сайте.  
Либо клонировать себе мой репозиторий с GitHub:  
👉 [Перейти к репозиторию](https://github.com/soulpastwk/linux-help/tree/main) 

А так же просто скачать статьи с гитхаб и использовать на своё усмотрение. <br>
Если скачать статьи - то их удобно просматривать в [VScode](https://code.visualstudio.com/) все статьи написаны на markdown

Как всегда делаю для себя - делюсь со всеми 💁

---

## 📑 Содержание 

<div class="two-columns">

### ⏰ Время и синхронизация
1. [Настройка NTP (chrony, ntpd, systemd-timesyncd)](01_ntp)
2. [Временные метки файлов в Linux](07_file_timestamps)

### 📂 Работа с файлами и резервным копированием
3. [Копирование файлов: cp, cron, rsync](02_cp_cron_rsync)
4. [Расширенные техники копирования](03_copy_advanced)
5. [Работа с архивами](04_archives)
6. [Работа с архивами расширено](18_archive)
7. [Операции с файлами](13_file_operation)
8. [Операции с директориями](14_dir_operation)
9. [Поиск и просмотр файлов](15_find_file)

### 📊 Мониторинг и информация о системе
10. [Мониторинг и отладка Linux (top, htop, dstat, journalctl)](05_monitoring)
11. [Инфо о системе](06_sysinfo)
12. [Инфо о системе - подробно](06_01_system-audit)

### 👤 Пользователи и права
13. [Управление пользователями и группами](08_users)
14. [Управление правами доступа (chmod, chown, umask, ACL)](12_permissions)

### 🌐 Сети
15. [Кратко про работу с сетями](10_network_basics)
16. [Подробнее про сети (ip, nmcli, netplan, firewall)](11_network_details)
17. [Настройка и виды сетевых бондов (bonding) в Linux](16_Bonding)
18. [Статичные интерфейсы](17_inet_static)
19. [Проверка сетевой доступности](20_network-port-check)

### ⚙️ История и логи
20. [История команд (прокачиваем history)](09_shell_history)
21. [Ротация логов встроенными средствами](23_log-rotation-native-only)
22. [Проверка аудита журнала и контроля состояния journald](23_1_journalctl-audit-check)

### 🔧 Управление службами основы
23. [Полный гайд по systemctl и unit-файлам (systemd)](19_systemctl-guide)
24. [Journalctl — исчерпывающий справочник](21_journalctl-guide)
25. [Journalctl — Часть 2: конфигурация systemd-journald](21_1_journalctl-guide)
26. [Journalctl — Часть 3: Централизованный сбор журналов](21_2_journalctl-remote)
27. [Руководство по rsyslog](22_rsyslog-guide)

</div>
