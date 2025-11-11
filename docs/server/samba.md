---
title: 🗂️ Setup Samba File Sharing
icon: material/folder-network
tags:
  - samba
  - ubuntu
  - windows
  - file-sharing
---

# 🗂️ Setup Samba File Sharing on Ubuntu

> Panduan ini menjelaskan cara membuat folder sharing di Ubuntu agar dapat diakses dari Windows menggunakan **Samba**.

## 1️⃣ Install Samba
```bash
sudo apt update
sudo apt install samba -y
```

## 2️⃣ Buat Folder Sharing
```bash
sudo mkdir -p /srv/samba/shared
sudo chmod -R 777 /srv/samba/shared
sudo chown -R nobody:nogroup /srv/samba/shared
```

## 3️⃣ Backup Konfigurasi
```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

## 4️⃣ Konfigurasi Samba
```bash
sudo nano /etc/samba/smb.conf
```

Isi dengan:
```ini
[global]
   workgroup = WORKGROUP
   server string = Samba Server
   netbios name = ubuntu-share
   security = user
   map to guest = Bad User
   dns proxy = no
   usershare allow guests = yes
   guest account = nobody
   smb encrypt = disabled
   ntlm auth = yes

[SharedFolder]
   path = /srv/samba/shared
   browseable = yes
   writable = yes
   guest ok = yes
   read only = no
   force user = nobody
   create mask = 0777
   directory mask = 0777
   comment = Folder Sharing Ubuntu
```

## 5️⃣ Restart dan Aktifkan Layanan
```bash
sudo systemctl restart smbd nmbd
sudo systemctl enable smbd nmbd
```

## 6️⃣ Tes Koneksi
```bash
smbclient -L localhost -N
smbclient //localhost/SharedFolder -U guest -N
```

## 7️⃣ Akses dari Windows
Buka **File Explorer** lalu ketik:
```
\\<IP_UBUNTU>\SharedFolder
```
Contoh:
```
\\192.168.1.171\SharedFolder
```

---
📘 **Lisensi:** CC BY-SA 4.0  
📅 **Terakhir diperbarui:** November 2025  
✍️ **Dibuat oleh:** Tim Pengembang IT Internal
