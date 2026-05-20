# Install Arch linux

## Siapkan bahan dan alat yang dibutuhkan

- Laptop (Disini saya menggunakan Thinkpad X260)
  
- Flashdisk 2GB (Disarankan 4GB atau lebih)

- file ISO arch dari website resmi Arch Linux

- Aplikasi rufus

## Lamgkah-langkah

- Install fie iso arch linux dari website resmi dan rufus

   Link file iso arch linux : https://mirror.citrahost.com/archlinux/iso/2026.05.01/

   <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/c231ed9d-a8a7-4786-ae20-88f6eb4ebede" />

   Link rufus : https://rufus.ie/en/

   <img width="474" height="580" alt="rufus-arch-linux-screenshot-01" src="https://github.com/user-attachments/assets/1f944625-aa63-4850-8a0a-7d1b57b5fce3" />

- Jangan lupa sesuaikan partition scheme dari storage yang digunakan via disk management. Caranya liat di web https://www.bagitekno.net/windows/cek-hardisk-gpt-atau-mbr.html

  <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/323f0109-c284-42d7-aab9-259ffe4dc46a" />

- Masuk ke mode BIOS dan matikan secure boot, susun boot priority dan exit and save changes

  <img width="236" height="180" alt="download" src="https://github.com/user-attachments/assets/c2dcf7e9-1d2a-41b3-9eea-6bb9abf3b668" />

- Boot ke USB bootable arch linux
- Sambungkan ke Internet via

  ```bash
  iwctl
  ```

- setelah itu cek wifi card dan sambungkan ke jaringan wifi

  ```bash
  station list
  ```

   ```bash
   station wlan0 connect NAMA-WIFI
   ```

 - Masukkan password wifi, exit dan tes jaringan

  ```bash
  exit
  ```

  ```bash
  ping 8.8.8.8
  ```

  - Selelah itu ctrl+C

  <img width="3072" height="4080" alt="IMG_20260517_205155" src="https://github.com/user-attachments/assets/16fe5dec-2ff2-4561-95f1-4aef3a574cd2" />

## Sinkronisasi waktu

  ```bash
  timedatectl
  ```

 <img width="3072" height="4080" alt="IMG_20260517_205430" src="https://github.com/user-attachments/assets/66d2f0bf-e911-4e0f-a472-71c2e90a3057" />
