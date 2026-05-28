<h1 align=center>Tutorial Install Arch Linux Manual Indonesia</h1>

Panduan melakukan instalasi Arch Linux secara manual untuk pemula.

---

## 🛠️ Alat dan Bahan

Untuk melakukan Instalasi Arch Linux kita memerlukan alat-alat berikut:

- PC/Laptop sebagai media tempat instalasi Arch Linux
- USB Drive atau Flashdisk ukuran 2GB (_disarankan ukuran 4GB atau lebih_)
- ISO Arch Linux terbaru yang bisa di dapatkan dari website [Arch Linux](https://archlinux.org/download/) atau [Mirror Indonesia](https://mirror.citrahost.com/archlinux/iso/2026.05.01/)
- Aplikasi untuk Burn ISO
  - [Rufus](https://rufus.ie/)
  - [Balena Etcher](https://www.balena.io/etcher)
  - [Ventoy](https://www.ventoy.net/)

> [!NOTE]
> Pastikan untuk mendownload semua bahan dari website mereka masing-masing.

---

# 📜 Panduan Instalasi

## 💿 Burning ISO

- Lakukan burning ISO Arch Linux menggunakan aplikasi burning ISO. Untuk pemula sangat disarankan menggunakan Rufus dan pastikan partition scheme kalian antara `GPT` atau `MBR` jika kalian belum tahu cara mengetahui apakah disk kalian `GPT` atau `MBR` silahkan masuk ke silahkan ikuti langkah berikut:
  - Klik kanan pada tombol **_Start_** (Icon Windows)
  - Pilih menu **Disk Management**
  - Pada daftar di bagian bawah, **klik kanan** pada disk yang ingin kalian periksa.
  - Pilih **Properties**, buka tab **Volumes**
  - Lihat pada bagian **Partition Style**, akan tertulis **Master Boot Record** (`MBR `) atau **GUID Partition Table** (`GPT`)

> [!NOTE]
> Cara tersebut digunakan jika kalian menggunakan `Rufus`, apabila kalian menggunakan `Balena Etcher` kalian bisa langsung burn ISO kalian ke dalam USB Drive

> [!TIP]
> Jika kalian ingin menyimpan lebih banyak ISO ke dalam Flashdisk kalian penulis sangat menyarankan menggunakan `Ventoy`

---

## 💻 Boot ke dalam BIOS

- Untuk melakukan instalasi Arch kita perlu mengatur `BIOS` agar boot ke flashdisk. Untuk melakukan boot ke `BIOS` pertama kalian perlu melakukan reboot pada PC/Laptop kalian kemudian melakukan spam pada tombol `BIOS` beberapa diantaranya `F2` `Esc` `Del` atau lainnya.  
  Berikut adalah beberapa contoh panduan boot BIOS dari beberapa merk Laptop:

| Merk            | Tombol masuk BIOS     |
| --------------- | --------------------- |
| Asus            | `F2`                  |
| Acer            | `F2` atau `Del`       |
| HP              | `F10` atau `Esc`      |
| Lenovo (Laptop) | `F2` atau `Fn` + `F2` |
| Lenovo (PC)     | `F1`                  |
| Lenovo Thinkpad | `Enter` lalu `F1`     |
| Dell            | `F2` atau `F12`       |

- Di dalam mode BIOS silahkan cari menu boot menu dan atur Flashdisk kalian ke paling atas kemudian save dan exit.

> [!NOTE]
> Apabila laptop merk kalian tidak ada diatas penulis sarankan kalian browsing merk laptop beserta modelnya di browser kesayangan kalian.

> [!TIP]
> Apabila kalian melihat satu merk ada 2 tombol melakukan boot ke `BIOS` Penulis menyarankan kalian menekan kedua tombolnya.

---

## 🐧 Fase Instalasi Arch

Setelah berhasil boot ke dalam ISO Arch Linux silahkan pilih masuk menu Instalasi

### 🌐 Menghubungkan ke Internet

Untuk memastikan kalian terhubung ke Internet kalian bisa menggunakan command berikut:

Untuk memastikan bahwa perangkat _network interface_ kalian terdaftar dan aktif:

```bash
ip link
```

cek apakah kalian terhubung ke internet:

```bash
ping -c4 ping.archlinux.org
```

> [!NOTE]
> Jika kalian menggunakan kabel Ethernet kalian bisa melewatkan panduan instalasi Arch Linux menggunakan `iwctl` berikut

#### 🛜 Menghubungkan ke wifi

Untuk menghubungkan ke wifi ikuti langkah berikut:

Nyalakan service `iwctl`

```bash
iwctl
```

cek nama device wifi kalian

```bash
[iwd] device list
```

scan wifi disekitar

```bash
[iwd] station <Driver-Wifi> scan
```

> [!NOTE]
> Langkah tersebut tidak menampilkan output apapun.

cek wifi yang tersdia

```bash
[iwd] station <Driver-Wifi> get-networks
```

hubungkan dengan wifi yang tersedia

```bash
[iwd] station <Driver-Wifi> connect SSID
```

> [!NOTE]
> Jika kalian bingung apa driver wifi kalian bisa di cek menggunakan `ip link` atau `device list` di iwctl

---

### 💿 Partisi Disk

> [!IMPORTANT]
> Pada tahap ini penulis sangat menyarankan kalian berhati-hati dalam melakukan partisi terutama jika kalian yang ingin dual boot bersama Windows, salah langkah pada tahap ini bisa menyebabkan kalian kehilangan partisi Windows kalian jadi berhati-hati.

**1. Cek Nama Partisi**

Sebelum melakukan partisi silahkan cek dulu nama partisinya menggunakan command:

```bash
lsblk
```

disini kalian harus memperhatikan block mana partisi yang ingin kalian gunakan untuk instalasi Arch Linux umumnya antara `/dev/sda`, `/dev/nvme0n1` atau `/dev/mmcblk0`. Sebagai contoh kita akan menggunakan `/dev/sda`

**2. Buat Partisi**

untuk membuat partisi file system disarankan menggunakan `cfdisk` untuk pemula karena dia sifatnya TUI atau Terminal User Interface sehingga dia lebih intuitif.

```bash
cfdisk /dev/sda
```

> [!TIP]
> Jika laptop kalian support `UEFI` maka penulis sarankan kalian menggunakan `GPT` jika kalian hanya `Legacy/BIOS` maka disarankan menggunakan `MBR`

Contoh layout untuk partisi

**_UEFI_**
| Mount point Pada System | Partition | Partition type | Suggested size |
| ----------------------------------- | --------- | -------------- | -------------- |
| /boot | /dev/efi_system_partition | EFI system partition | 1 GiB |
| SWAP | /dev/swap_partition Linux | swap | Setidaknya 4 GiB |
| / | /dev/root_partition | Linux x86-64 root (/) | Setidaknya 23-32 GB Kosong untuk sistem |

**_MBR_**

| Mount point Pada System | Partition                 | Partition type        | Suggested size                          |
| ----------------------- | ------------------------- | --------------------- | --------------------------------------- |
| SWAP                    | /dev/swap_partition Linux | swap                  | Setidaknya 4 GiB                        |
| /                       | /dev/root_partition       | Linux x86-64 root (/) | Setidaknya 23-32 GB Kosong untuk sistem |

> [!NOTE]
> Layout diatas adalah konfigurasi dasar, kalian tetap bisa menggunakan beberapa partisi jika kalian terbiasa menggunakan banyak partisi sebagai contoh kalian terbiasa `/home` kalian bisa juga membuat partisi khusus untuk itu.

**3. Format Partisi**

Umumnya format filesystem pada partisi Linux adalah `ext4` untuk filesystem `swap` untuk swap dan `fat` untuk EFI

format paritisi root:

```bash
mkfs.ext4 /dev/partisi_root
```

format partisi EFI

```bash
mkfs.fat -F 32 /dev/partisi_boot
```

format partisi swap

```bash
mkswap /dev/partisi_swap
```

**4. Mount Partisi**

Mount partisi root ke dalam `/mnt` atau ke dalam disk

```bash
mount /dev/partisi_root /mnt
```

mount paritisi boot ke `/mnt/boot`

```bash
mount --mkdir /dev/partisi_boot /mnt/boot/efi
```

nyalakan swap

```bash
swapon /dev/partisi_swap
```

setelah selesai silahkan cek menggunakan perintah

```bash
lsblk
```

---

### 🐧 Proses Instalasi

> [!IMPORTANT]
> Akhirnya kita masuk ke fase instalasi Arch sesungguhnya, pada tahap ini kita akan membangun Arch Linux kita dari 0

#### Pacstrap

Pada tahap ini kita akan melakukan bootstrap pada mesin kita dan disini kita akan menginstall base system seperti tool-chain, kernel, dan lainnya.

```bash
pacstrap -K /mnt base linux linux-firmware base-devel sof-firmware grub efibootmgr networkmanager vim os-prober plasma-desktop sddm
```

> [!NOTE]
> Ada beberapa paket yang berbeda dari arch wiki yaitu:
>
> - `sof-firmware` jika kalian memiliki hardware yang relatif baru
> - `base-devel` untuk memudahkan kita jika inging menggunakan `AUR`
> - `grub` & `efibootmgr` sebagai bootloader
> - `vim` text editor (opsional)
> - `plasma-desktop` & `sddm` karena kita akan menggunakan KDE Plasma sebagai Desktop environment kita

> [!TIP]
> Proses ini akan memakan waktu cukup lama jadi kalian bisa menyeduh kopi untuk menunggu proses pacstrap

#### FSTAB

Proses ini untuk membuat filesystem table di system kalian menggunakan `genfstab`

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

#### Chroot

Baiklah saatnya kita melakukan `chroot` ke sistem Arch kita dengan menjalankan command berikut:

```bash
arch-chroot /mnt
```

harusnya output prompt kalian berubah menjadi seperti ini

```bash
[root@archiso /]#
```

Baiklah saatnya kita melakukan konfigurasi di `chroot`

**1. Date**

Hal pertama yang kita konfigurasikan adalah waktu dan tanggal, pertama-tama kita cek terlebih dahulu tanggal dan waktu

```bash
date
```

apabila tidak sesuai maka kita lakukan symlink dengan cara

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

untuk mengkonfirmasi zona waktu telah tepat silahkan masukan command

```bash
date
```

> [!TIP]
> Tutorial diatas adalah zona Waktu Indonesia bagian Barat. Jika kalian berada di WITA atau WIT kalian bisa menggunakan:
>
> - Asia/Makassar untuk WITA
> - Asia/Jayapura untuk WIT

kemudian sinkronisasi dengan hardware clock

```bash
hwclock --systohc
```

**2. Localization**

Ini biasanya step yang dihindari oleh banyak orang namun sebenarnya tidak sulit, cukup ikuti langkahnya dan ini akan sangat mudah

```bash
nano /etc/locale.gen
```

atau kalian jika menggunakan `vim`

```bash
vim /etc/locale.gen
```

cari baris yang berisi `en_US.UTF-8 UTF-8` dan uncomment baris tesebut, kemudian kita generate locale

```bash
locale-gen
```

> [!TIP]
> Jika kalian menggunakan nano untuk mencari sebuah tulisan kalian bisa menggunakan shortcut `Ctrl+w` untuk save `Ctrl+o` dan `Enter` dan keluar `Ctrl+x`

kemudian kita akan setup locale di `/etc/locale.conf` jadi kita akan menggunakan command berikut:

```bash
echo 'LANG=en_US.UTF-8' >> /etc/locale.conf
```

**3. Hostname**

Baiklah saatnya kita memberi nama pada PC/Laptop kita atau hostname:

```bash
echo 'arch-btw' >> /etc/hostname
```

> Kenapa namanya arch-btw? Karena _I Use Arch, BTW_

**4. Root Password**

Saatnya kita memberikan password untuk root kita dengan command:

```bash
passwd
```

> [!WARNING]
> Usahakan password root sulit dan berbeda dari password user untuk keamanan pada sistem kalian

**5. Nambah User Baru**

Saatnya kita menambah user baru untuk arch kita

```bash
useradd -m -G wheel -s /bin/bash user
passwd user
```

> [!NOTE]
> flag `-m` agar user kita memiliki home directory, `-G` agar kita bisa menambahkan user ke `wheel` dan `-s /bin/bash` agar kita menggunakan bash sebagai shell default

saatnya kita memberikan akses sudo ke user

nano

```bash
EDITOR=nano visudo
```

vim

```bash
EDITOR=vim visudo
```

cari bagian

```yaml
# %wheel ALL=(ALL:ALL) ALL
```

lalu uncomment baris tersebut

**6. Initramfs**

hal ini opsional bisa kalian lewati bisa juga tidak

```bash
mkinitcpio -P
```

**7. Grub**
saatnya kita setup Grub, untuk instalasi dual boot silahkan langkah berikut namun jika kalian tidak melakukan dual boot kalian bisa skip langkah berikut

```bash
nano /etc/default/grub
```

kemudian cari baris `#GRUB_DISABLE_OS_PROBER=false` uncomment baris tersebut lalu save

install grub (wajib!)

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

jika terdapat kalimat `found windows on /dev/partisi_windows` maka dia aman

**8. Enable core system**

kita nyalakan `networkmanager` dan juga `sddm`

```bash
systemctl enable NetworkManager
systemctl enable sddm
```

**9. Exit Chroot**

setelah semua langkah selesai silahkan keluar dari chroot

```bash
exit
```

### Unmount dan Reboot System

Setelah keluar dari chroot saatnya kita reboot sistem kita tapi kita perlu melakukan beberapa proses yaitu unmount filesystem dan juga mematikan swap

```bash
umount -R /mnt
swapoff /dev/partisi_swap
reboot
```

---

# 📜 Penutup

Baiklah teman-teman selamat kalian telah berhasil menginstall Arch Linux di PC/Laptop kalian dan akhirnya kalian bisa bilang

> **_I Use Arch, BTW_**

kepada teman-teman kalian, sekian tutorialnya apabila ada kesalahan dipersilahkan untuk memasukannya ke issue. sekian terima kasih.

# 📝 References

[Arch Wiki](https://wiki.archlinux.org/title/Installation_guide)
[worker-sentinel](https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/module/module-1.md)
[worker-sentinel2](https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/class-c/kelompok/Kelompok-5_4C/dokumentasi.md)
