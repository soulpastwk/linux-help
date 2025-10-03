# 🖥 Гайд по мониторингу и отладке Linux

[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?style=flat-square&logo=linux)]()
[![Category](https://img.shields.io/badge/category-Monitoring%20%26%20Debug-blue?style=flat-square)]()
[![Processes](https://img.shields.io/badge/processes-top%20|%20ps%20|%20pstree-yellow?style=flat-square)]()
[![Memory](https://img.shields.io/badge/memory-free-brightgreen?style=flat-square)]()
[![Disk](https://img.shields.io/badge/disk-smartctl-red?style=flat-square)]()
[![Logs](https://img.shields.io/badge/logs-journalctl%20|%20tail-blue?style=flat-square)]()

В этой инструкции вы найдёте подробное описание основных команд мониторинга процессов, ресурсов системы, журналов и отладки в Linux.

---

## ⚡ 1. Мониторинг процессов

### `top` — процессы в реальном времени
```bash
top
````

* Отображает процессы с автоматическим обновлением.
* Полезные клавиши:

  * `M` — сортировка по памяти
  * `P` — сортировка по CPU
  * `T` — сортировка по времени
  * `k` — отправка сигнала процессу
  * `q` — выход

> [!TIP]
> Для более удобного интерфейса можно использовать `htop`.

---

### `ps` — снимок процессов

```bash
ps -eafw
ps -e -o pid,args --forest
```

* `-eafw` — все процессы, полная информация, широкий вывод
* `--forest` — отображение в виде дерева

### `pstree` — дерево процессов

```bash
pstree -p
```

* Показывает процессы в виде дерева с PID.

---

## 🖥 2. Управление процессами

### `kill` — отправка сигналов

```bash
kill -9 98989        # SIGKILL, насильственное завершение
kill -TERM 98989     # SIGTERM, корректное завершение
kill -HUP 98989      # перечитать конфигурацию
```

> [!IMPORTANT]
> `SIGKILL` завершает процесс немедленно, данные могут быть потеряны. Используйте его только при необходимости.

---

## 💾 3. Файлы и ресурсы процессов

### `lsof` — открытые файлы

```bash
lsof -p 98989       # файлы процесса
lsof /home/user1    # файлы в директории
```

* Помогает находить зависшие файлы, сокеты и блокировки.

---

## ⚙️ 4. Отладка системных вызовов

### `strace`

```bash
strace -c ls > /dev/null
strace -f -e open ls > /dev/null
```

* `-c` — статистика вызовов
* `-f` — дочерние процессы
* `-e` — фильтр вызовов

---

## 📄 5. Мониторинг в реальном времени

### `watch`

```bash
watch -n1 'cat /proc/interrupts'
```

* Запуск команды каждые `n` секунд

### `last`

```bash
last reboot
last user1
```

* История перезагрузок и входов пользователей

---

## 💾 6. Память и модули ядра

### `free`

```bash
free -h
```

* Использование оперативной памяти и swap

### `lsmod`

```bash
lsmod
```

* Список загруженных модулей ядра

---

## 💽 7. Диагностика дисков

### `smartctl`

```bash
smartctl -A /dev/hda
smartctl -i /dev/hda
```

* Проверка состояния HDD/SSD через SMART

---

## 📄 8. Журналы системы

### `tail`

```bash
tail -f /var/log/messages
tail /var/log/dmesg
```

### `journalctl`

```bash
journalctl
journalctl -b
journalctl -u ssh
journalctl -f
journalctl -p err
journalctl --since "2025-10-01" --until "2025-10-02"
```

> [!TIP]
> Для поиска ошибок используйте:

```bash
journalctl -u ssh | grep "Failed"
```

---

## 🔗 Полезные ссылки

* [man top](https://linux.die.net/man/1/top)
* [man ps](https://linux.die.net/man/1/ps)
* [man kill](https://linux.die.net/man/1/kill)
* [man lsof](https://linux.die.net/man/8/lsof)
* [man strace](https://linux.die.net/man/1/strace)
* [man watch](https://linux.die.net/man/1/watch)
* [man last](https://linux.die.net/man/1/last)
* [man lsmod](https://linux.die.net/man/8/lsmod)
* [man free](https://linux.die.net/man/1/free)
* [man smartctl](https://linux.die.net/man/8/smartctl)
* [man journalctl](https://www.freedesktop.org/software/systemd/man/journalctl.html)

Хочешь, чтобы я сделал такую версию?
```
