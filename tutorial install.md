# Install Arch linux

## Siapkan bahan dan alat yang dibutuhkan

- Laptop
  
- Flashdisk 2GB (Disarankan 4GB atau lebih)

- file ISO arch dari website resmi Arch Linux

- Aplikasi rufus

# Panduan install

## Langkah-langkah

- Install fie iso arch linux dari website resmi dan rufus

   Link file iso arch linux : https://mirror.citrahost.com/archlinux/iso/2026.05.01/

   <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/c231ed9d-a8a7-4786-ae20-88f6eb4ebede" />

   Link rufus : https://rufus.ie/en/

   <img width="474" height="580" alt="rufus-arch-linux-screenshot-01" src="https://github.com/user-attachments/assets/1f944625-aa63-4850-8a0a-7d1b57b5fce3" />

- Sesuaikan partition scheme dan ukuran storage yang akan dibutuhkan untuk install arch linux dari storage yang digunakan via disk management (Disarankan untuk kapasitas partisinya 50GB atau lebih). Caranya liat di web https://www.bagitekno.net/windows/cek-hardisk-gpt-atau-mbr.html

  https://www.asus.com/id/support/faq/1044688/

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

  - Setelah itu CTRL+C

  <img width="3072" height="4080" alt="IMG_20260517_205155" src="https://github.com/user-attachments/assets/16fe5dec-2ff2-4561-95f1-4aef3a574cd2" />

---

## Tampilan waktu

  ```bash
  timedatectl
  ```

 <img width="3072" height="4080" alt="IMG_20260517_205430" src="https://github.com/user-attachments/assets/66d2f0bf-e911-4e0f-a472-71c2e90a3057" />

 ---

 ## Membagi partisi **JANGAN SENTUH PARTISI SELAIN UKURAN YANG SUDAH DISIAPKAN!**

   ```bash
   cfdisk
   ```
    
  - Kurang lebih seperti ini tampilannya, setelah itu buat skema ukuran partisi
    - 512MB sebagai boot
    - 4GB sebagai SWAP
    - sisanya sebagai root
      
Gunakan key anak panah kanan & kiri untuk bernavigasi (bergerak ke kanan dan kiri pada tampilan terminal). Geser ke kiri untuk memilih opsi New dan type lalu masukkan ukuran partisi secara berurutan, yaitu 512M untuk partisi pertama sebagai boot  (EFI system), 4G untuk partisi kedua sebagai linux swap, dan sisa ruang yang tersedia untuk partisi ketiga sebagai root (Linux filesystem).

<img width="800" height="503" alt="image" src="https://github.com/user-attachments/assets/9df63007-f70b-4be5-8505-1e3c4ff590e5" />


  <img width="4080" height="3072" alt="IMG_20260521_124435" src="https://github.com/user-attachments/assets/d6e677a4-7e6e-42a7-9416-42d261fb8b44" />

  - Setelah itu, format partisi. Ketikan:
    
  ```bash
  lsblk
  ```

  Untuk mengetahui partisi yang sudah dibuat.

<img width="3072" height="4080" alt="IMG_20260521_124554" src="https://github.com/user-attachments/assets/87c1c588-539a-4b5f-8635-4c6dd1de0e6a" />

  ---

  ## Format partisi **JANGAN SALAH PARTISI SELAIN UKURAN YANG SUDAH DISIAPKAN!** menyesuaikan dengan nama partisi yang ada di lsblk. Contohnya: sda4 sebagai /boot.
  
  # Format Partisi

```bash
mkfs.ext4 /dev/partisi_root
```

### Penjelasan
Membuat filesystem ext4 pada partisi root.

---

## Membuat Swap

```bash
mkswap /dev/partisi_swap
```

Aktifkan swap:

```bash
swapon /dev/partisi_swap
```

### Penjelasan
Swap digunakan sebagai memori cadangan ketika RAM penuh.

---

## Format EFI Partition

```bash
mkfs.fat -F 32 /dev/partisi_boot
```

### Penjelasan
EFI partition harus menggunakan FAT32.

---

# Mount Filesystem

Mount root:

```bash
mount /dev/partisi_root /mnt
```

Mount EFI:

```bash
mount --mkdir /dev/partisi_boot /mnt/boot
```

### Penjelasan
Partisi harus dipasang (mount) sebelum sistem diinstal.


<img width="3072" height="4080" alt="IMG_20260521_125523" src="https://github.com/user-attachments/assets/7d85a702-db41-4dd4-9cb9-ec1263c50262" />

---

# Instalasi Sistem Dasar dan desktop environment

```bash
pacstrap -K /mnt base linux linux-firmware sudo nano networkmanager grub efibootmgr os-prober xorg plasma-desktop sddm konsole dolphin pipewire pipewire-alsa pipewire-pulse pipewire-jack
```

# Membuat fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Penjelasan
fstab menentukan partisi mana yang otomatis dimount saat boot.

---

# Masuk ke Sistem Baru (Chroot)

```bash
arch-chroot /mnt
```

### Penjelasan
Mengubah root shell ke sistem Arch yang baru dipasang.

---

# Timezone

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

Sinkronkan hardware clock:

```bash
hwclock --systohc
```

# Lokalisasi

- Buka file locale.gen
  
    ```bash
    nano /etc/locale.gen
    ```

- Cari baris #en_US.UTF-8 UTF-8 lalu hapus tanda pagar (#) di depannya
    
- Simpan (Ctrl+O, Enter) dan keluar (Ctrl+X)
    
- Jalankan perintah generate
  
```bash
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```
    
# Hostname

```bash
echo "myarchpc" > /etc/hostname
```

# Generate Initramfs

```bash
mkinitcpio -P
```

# Password Root

```bash
passwd
```

# Tambah user

  - Tambahkan user baru
    
    ```bash
    useradd -m -G wheel user
    passwd user
    ```
    
  - Pastikan paket sudo terinstal (jika belum ada)
    
    ```bash
    pacman -S sudo
    ```
    
  - Buka berkas konfigurasi sudoers

    ```bash
    EDITOR=nano visudo
    ```
    
  - Cari baris berikut:
    %wheel ALL=(ALL:ALL) ALL

  - Hapus tanda pagar (#) di depan %wheel sehingga menjadi:
    %wheel ALL=(ALL:ALL) ALL

Simpan (Ctrl+O, Enter) dan keluar (Ctrl+X).

# Buka dan edit konfigurasi OS Prober

```bash
nano /etc/default/grub
```
Cari baris

#GRUB_DISABLE_OS_PROBER=false

Hapus tanda "#"
GRUB_DISABLE_OS_PROBER=false

cara keluar dari nano nya 
1. CTRL + O  untuk save lalu enter
2. CTRL + X untuk keluar

Setelah itu update GRUB

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

Jika muncul tulisan seperti ini berarti udah bisa dual boot

Found Windows Boot Manager on /dev/sdaX

# Aktifkan networkmanager dan sddm

```bash
systemctl enable NetworkManager
systemctl enable sddm
```

# restart

```bash
exit
umount -R /mnt
reboot
```

# Selesai

# Sumber referensi

https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/module/module-1.md

https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/class-c/kelompok/Kelompok-5_4C/dokumentasi.md


