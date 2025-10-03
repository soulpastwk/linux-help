---
layout: default
title: "Настройка NTP"
---

# ⏱ Настройка NTP и часового пояса на Astra Linux и РЕД ОС

[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?style=flat-square&logo=linux)]()
[![Tested on](https://img.shields.io/badge/tested%20on-R%C4%80D%20OS%207.3%20|%208.0%20|%20Astra%20SE%201.7.5%20|%201.8-orange?style=flat-square)]()
[![Service-NTP](https://img.shields.io/badge/service-NTP-blue?style=flat-square)]()
[![Sync-Status](https://img.shields.io/badge/sync-active-brightgreen?style=flat-square)]()
[![Timezone](https://img.shields.io/badge/timezone-Asia%2FYekaterinburg-yellow?style=flat-square)]()

Инструкция по настройке **NTP** и смены часового пояса для консолей **Astra Linux** и **РЕД ОС (7.3/8.0)**. Подходит для сетей с собственным сервером времени.

---

## 🔧 Общие сведения

- Собственный NTP-сервер: `192.168.10.100`
- Работаем **только в консоли**, без GUI
- NTP синхронизирует **UTC-время**, часовой пояс управляет локальным отображением времени

> [!TIP]  
> После смены часового пояса перезапуск NTP не требуется.

---

## 📌 1. Определение используемой службы NTP

```bash
systemctl status systemd-timesyncd ntp chronyd 2>/dev/null | grep -E "Active:|Loaded:"
# или по отдельности:
systemctl is-active systemd-timesyncd
systemctl is-active ntp
systemctl is-active chronyd
````

---

## 🐧 2. Настройка NTP

### Astra Linux (Debian-based)

#### ✅ `systemd-timesyncd` (по умолчанию)

```bash
sudo nano /etc/systemd/timesyncd.conf
```

```ini
[Time]
NTP=192.168.10.100
FallbackNTP=
```
FallbackNTP - заполняем если есть второй сервер времени - резервный.

```bash
sudo systemctl restart systemd-timesyncd
sudo systemctl enable systemd-timesyncd
timedatectl status
```

#### ✅ `chrony` (рекомендуется)

```bash
sudo apt update
sudo apt install chrony
sudo nano /etc/chrony/chrony.conf
```

```conf
server 192.168.10.100 iburst
# Закомментируйте другие server строки
```

```bash
sudo systemctl restart chrony
sudo systemctl enable chrony
chronyc sources -v
chronyc tracking
```

#### ✅ `ntpd`

```bash
sudo apt install ntp
sudo nano /etc/ntp.conf
```

```conf
server 192.168.10.100 iburst
```

```bash
sudo systemctl restart ntp
sudo systemctl enable ntp
ntpq -p
```

---

### РЕД ОС (ALT Linux-based)

#### ✅ `chrony`

```bash
sudo apt-get update
sudo apt-get install chrony
sudo nano /etc/chrony.conf   # или /etc/chrony/chrony.conf
```

```conf
server 192.168.10.100 iburst
```

```bash
sudo systemctl restart chronyd
sudo systemctl enable chronyd
chronyc sources -v
```

#### ✅ `ntpd`

```bash
sudo apt-get install ntp
sudo nano /etc/ntp.conf
```

```conf
server 192.168.10.100 iburst
```

```bash
sudo systemctl restart ntpd
sudo systemctl enable ntpd
ntpq -p
```

---

## 🔐 3. Настройка брандмауэра

```bash
# iptables
sudo iptables -A INPUT -p udp --dport 123 -j ACCEPT
```

> [!IMPORTANT]
> Для `nftables` или `firewalld` команды будут отличаться.

---

## 🕒 4. Смена часового пояса на Екатеринбург

Часовой пояс: `Asia/Yekaterinburg`

#### ✅ Через `timedatectl`

```bash
timedatectl          # проверить текущий
sudo timedatectl set-timezone Asia/Yekaterinburg
timedatectl          # проверить результат
date                 # локальное время
```

#### ✅ Вручную через символическую ссылку

```bash
sudo rm -f /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Yekaterinburg /etc/localtime
echo "Asia/Yekaterinburg" | sudo tee /etc/timezone  # Debian-based
```

---

## 🧪 5. Проверка

| Сервис            | Команда проверки     | Статус                                                                          |
| ----------------- | -------------------- | ------------------------------------------------------------------------------- |
| chrony            | `chronyc tracking`   | ![Sync](https://img.shields.io/badge/sync-active-brightgreen?style=flat-square) |
| ntpd              | `ntpq -p`            | ![Sync](https://img.shields.io/badge/sync-active-brightgreen?style=flat-square) |
| systemd-timesyncd | `timedatectl status` | ![Sync](https://img.shields.io/badge/sync-active-brightgreen?style=flat-square) |

---

## ⚡ 6. Пример полной настройки (Astra Linux + chrony + Екатеринбург)

```bash
# 1. Часовой пояс
sudo timedatectl set-timezone Asia/Yekaterinburg
echo "Asia/Yekaterinburg" | sudo tee /etc/timezone

# 2. Настройка chrony
echo "server 192.168.10.100 iburst" | sudo tee /etc/chrony/chrony.conf

# 3. Перезапуск сервиса
sudo systemctl restart chrony
sudo systemctl enable chrony

# 4. Проверка
timedatectl
chronyc tracking
date
```

---

## 🔖 Рекомендации

* **Chrony** — точная синхронизация, подходит для виртуальных машин.
* **NTPD** — классика, менее гибкий.
* **systemd-timesyncd** — простой клиент для базовой синхронизации.

> [!INFO]
> Применимо для:
>
> * Astra SE 1.7.5 / 1.8
> * РЕД ОС 7.3 / 8.0
> * Общие конфиги подходят для других Linux-дистрибутивов.
