---
layout: default
title: "Статичные названия сетевых интерфейсов"
permalink: /17_inet_static/
---

# 🌐 Гайд по созданию статичных названий сетевых интерфейсов

[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?style=flat-square\&logo=linux)]()
[![Category](https://img.shields.io/badge/category-Networking-blue?style=flat-square)]()
[![Interfaces](https://img.shields.io/badge/interfaces-eth%20|%20ens%20|%20lan-green?style=flat-square)]()
[![Tools](https://img.shields.io/badge/tools-grub%20|%20udev%20|%20systemd-yellow?style=flat-square)]()

В этой инструкции вы узнаете, как задать статичные предсказуемые названия сетевых интерфейсов в разных Linux-дистрибутивах.

---

## ⚡ 1. Зачем нужны статичные имена

* Статичные имена обеспечивают стабильность и предсказуемость конфигурации сети.
* В системах с несколькими сетевыми адаптерами динамическое присвоение имен (`eth0 → eth1`) может привести к сбоям сервисов и скриптов.
* Например, интерфейс `eth0` может стать `ens1s0` после перезагрузки, что нарушает сетевые настройки.

---

## 🖥 2. Astra Linux / Debian / Ubuntu

### 1. Редактирование GRUB

```bash
sudo nano /etc/default/grub
```

Добавьте параметр `net.ifnames=1` в строку `GRUB_CMDLINE_LINUX_DEFAULT`:

```text
GRUB_CMDLINE_LINUX_DEFAULT="quiet net.ifnames=1 parsec.max_ilev=63"
```

### 2. Обновление конфигурации GRUB

```bash
sudo update-grub
```

### 3. Перезагрузка

```bash
sudo reboot
```

> После перезагрузки интерфейсы будут иметь предсказуемые имена вроде `ens1s0`.

---

## 🖥 3. Red Hat / AlmaLinux / Rocky Linux

### 1. Просмотр текущих интерфейсов

```bash
ip link
```

### 2. Создание правила udev

```bash
sudo nano /etc/udev/rules.d/70-persistent-net.rules
```

Пример содержимого:

```text
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="00:11:22:33:44:55", NAME="lan0"
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="66:77:88:99:AA:BB", NAME="lan1"
```

### 3. Перезагрузка

```bash
sudo reboot
```

> В RHEL 8 можно использовать systemd `.link` файлы:

```bash
sudo nano /etc/systemd/network/10-lan0.link
```

```text
[Match]
MACAddress=00:11:22:33:44:55

[Link]
Name=lan0
```

---

## 🖥 4. SUSE / openSUSE

### 1. Редактирование GRUB

```bash
sudo nano /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet net.ifnames=1"
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

### 2. Настройка через YaST

* Откройте **YaST → Network Devices → Global Options → Network Interface Names**
* Укажите желаемые имена интерфейсов.

### 3. Перезагрузка

```bash
sudo reboot
```

---

## ⚡ 5. Полезные советы

* После изменения имён интерфейсов обновите сетевые сервисы (`/etc/network/interfaces`, `nmcli`, `NetworkManager`).
* Виртуальные машины и контейнеры могут потребовать отдельной настройки MAC-адресов.
* Проверить новые имена можно командой:

```bash
ip link
```

> [!TIP]
> Статичные имена интерфейсов помогают избежать неожиданных сбоев сетевых сервисов при добавлении новых адаптеров или после обновлений ядра.
