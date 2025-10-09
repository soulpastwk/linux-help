---
layout: default
title: "Настройка LVM"
permalink: /25_LVM/
---

[![Linux](https://img.shields.io/badge/Platform-Linux-blue?style=flat-square&logo=linux)]()
[![Debian](https://img.shields.io/badge/Tested%20on-Astra%20Linux%20%7C%20RED%20OS%20%7C%20Debian-orange?style=flat-square&logo=debian)]()
[![Storage](https://img.shields.io/badge/Type-Logical%20Volume%20Management-lightgrey?style=flat-square&logo=database)]()
[![CLI](https://img.shields.io/badge/Interface-Console%20Only-success?style=flat-square&logo=gnu-bash)]()
[![Docs](https://img.shields.io/badge/Docs-LVM2%20%7C%20parted-important?style=flat-square&logo=markdown)]()
[![Filesystem](https://img.shields.io/badge/FS-ext4%20%7C%20xfs%20%7C%20btrfs-blue?style=flat-square&logo=files)]()
[![Reliability](https://img.shields.io/badge/Reliability-Flexible%20Storage-success?style=flat-square&logo=securityscorecard)]()
[![Performance](https://img.shields.io/badge/Performance-Scalable%20%26%20Dynamic-critical?style=flat-square&logo=performance)]()

# 🧱 Настройка LVM в Linux

---

## 📑 Содержание

- [Что такое LVM](#chto-takoe-lvm)
- [Основные преимущества LVM](#osnovnye-preimushchestva-lvm)
- [Структура LVM](#struktura-lvm)
- [Создание раздела под LVM с помощью parted](#sozdanie-razdela-s-parted)
- [Создание LVM: PV, VG, LV](#sozdanie-lvm)
- [Создание файловой системы и монтирование](#sozdanie-fs)
- [Автомонтирование через /etc/fstab](#fstab)
- [Проверка состояния LVM](#proverka-lvm)
- [Расширение LVM и ФС](#rasshirenie-lvm)
- [Удаление LVM](#udalenie-lvm)
- [Вывод](#vyvod)

---

<a id="chto-takoe-lvm"></a>
## 🔍 Что такое LVM

**LVM (Logical Volume Manager)** — это система управления логическими томами, позволяющая гибко управлять дисковым пространством:

- объединять несколько физических дисков в одно логическое хранилище,  
- создавать, расширять и уменьшать разделы без перезагрузки,  
- легко переносить тома между дисками.

Используется на серверах, в виртуальных машинах, системах хранения и контейнерах.

---

<a id="osnovnye-preimushchestva-lvm"></a>
## ⚙️ Основные преимущества LVM

| Преимущество | Описание |
| ------------- | -------- |
| ✅ Масштабируемость | Добавление новых дисков без форматирования |
| ✅ Гибкость | Изменение размеров разделов "на лету" |
| ✅ Снимки (snapshots) | Создание копий томов для резервного копирования |
| ✅ Объединение | Несколько дисков → один объём |
| ✅ Простое управление | Утилиты `lvm`, `vgdisplay`, `lvdisplay`, `pvs` и т.д. |

---

<a id="struktura-lvm"></a>
## 🧩 Структура LVM

| Уровень | Название | Описание |
| -------- | -------- | -------- |
| **PV** | *Physical Volume* | Физический диск или раздел (например, `/dev/sdb1`) |
| **VG** | *Volume Group* | Группа физических томов (например, `vg_data`) |
| **LV** | *Logical Volume* | Логический том внутри VG (например, `/dev/vg_data/lv_backup`) |

📊 **Схема:**

```

[ /dev/sdb1 + /dev/sdc1 ] → VG (vg_data)
↓
LV (lv_backup)
↓
Файловая система (ext4, xfs, …)

````

---

<a id="sozdanie-razdela-s-parted"></a>
## 💽 Создание раздела под LVM с помощью parted

Перед созданием LVM нужно подготовить диск.

```bash
sudo parted /dev/sdb
````

**Пример последовательности команд:**

```
(parted) mklabel gpt          ← создаём GPT
(parted) mkpart primary ext4 0% 100%   ← создаём раздел
(parted) name 1 data_disk     ← даём имя разделу
(parted) set 1 lvm on         ← включаем флаг LVM
(parted) print                ← проверяем
(parted) quit                 ← выходим
```

> 💡 **Аналог в fdisk:**
>
> ```
> Command (m for help): g       ← создаём GPT
> Command (m for help): n       ← создаём новый раздел
> Partition number (1-128, default 1): 1
> First sector (2048-...): <Enter>
> Last sector (...): <Enter>
> Command (m for help): w       ← сохранить изменения
> ```

После выхода появится раздел `/dev/sdb1`.

---

<a id="sozdanie-lvm"></a>

## 🏗️ Создание LVM (PV → VG → LV)

### 1️⃣ Инициализация физического тома

```bash
sudo pvcreate /dev/sdb1
```

Проверка:

```bash
sudo pvs
```

---

### 2️⃣ Создание группы томов (VG)

```bash
sudo vgcreate vg_data /dev/sdb1
```

Проверка:

```bash
sudo vgdisplay vg_data
```

---

### 3️⃣ Создание логического тома (LV)

```bash
sudo lvcreate -n lv_backup -L 50G vg_data
```

> Где:
>
> * `-n` — имя тома
> * `-L` — размер тома (можно `-l 100%FREE` — занять всё место VG)

Проверка:

```bash
sudo lvs
```

---

<a id="sozdanie-fs"></a>

## 📁 Создание файловой системы и монтирование

Создаём файловую систему:

```bash
sudo mkfs.ext4 /dev/vg_data/lv_backup
```

Создаём точку монтирования и монтируем:

```bash
sudo mkdir /mnt/data
sudo mount /dev/vg_data/lv_backup /mnt/data
```

Проверяем:

```bash
df -h /mnt/data
```

---

<a id="fstab"></a>

## ⚙️ Автомонтирование через `/etc/fstab`

Чтобы том монтировался автоматически при загрузке:

1. Узнаём UUID:

   ```bash
   blkid /dev/vg_data/lv_backup
   ```

2. Добавляем строку в `/etc/fstab`:

   ```
   UUID=<указать_свой_UUID> /mnt/data ext4 defaults 0 0
   ```

3. Проверяем корректность:

   ```bash
   sudo mount -a
   ```

---

<a id="proverka-lvm"></a>

## 🔎 Проверка состояния LVM

```bash
sudo pvs
sudo vgs
sudo lvs
```

Пример вывода:

```
  PV         VG       Fmt  Attr PSize   PFree
  /dev/sdb1  vg_data  lvm2 a--  100.00g 50.00g

  VG       #PV #LV #SN Attr   VSize   VFree
  vg_data   1   1   0 wz--n- 100.00g 50.00g

  LV        VG       Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_backup vg_data  -wi-ao---- 50.00g
```

---

<a id="rasshirenie-lvm"></a>

## 📈 Расширение LVM и файловой системы

### 1️⃣ Добавляем новый диск и создаём PV

```bash
sudo pvcreate /dev/sdc1
```

Добавляем в VG:

```bash
sudo vgextend vg_data /dev/sdc1
```

---

### 2️⃣ Расширяем LV

```bash
sudo lvextend -l +100%FREE /dev/vg_data/lv_backup
```

---

### 3️⃣ Увеличиваем файловую систему

Для **ext4**:

```bash
sudo resize2fs /dev/vg_data/lv_backup
```

Для **XFS**:

```bash
sudo xfs_growfs /mnt/data
```

---

<a id="udalenie-lvm"></a>

## 🧹 Удаление LVM

```bash
sudo umount /mnt/data
sudo lvremove /dev/vg_data/lv_backup
sudo vgremove vg_data
sudo pvremove /dev/sdb1
```

---

<a id="vyvod"></a>

## ✅ Вывод

**LVM** — гибкая и мощная система управления томами, идеально подходящая для серверов и виртуализированных сред.

**Основные преимущества:**

* легко масштабируется;
* не требует перезагрузки при изменении размеров;
* поддерживает снапшоты и резервные копии;
* позволяет объединять и разделять хранилища без потерь данных.

Использование LVM — стандарт для большинства Linux-систем в корпоративной инфраструктуре.
