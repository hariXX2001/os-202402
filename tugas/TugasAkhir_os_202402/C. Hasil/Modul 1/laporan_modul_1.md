# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `<HARI CAHYONO>`
**NIM**: `<240202900>`
**Modul yang Dikerjakan**:
`(Contoh: Modul 1 – System Call dan Instrumentasi Kernel)`

---

## 📌 Deskripsi Singkat Tugas

Tuliskan deskripsi singkat dari modul yang Anda kerjakan. Misalnya:

* **Modul 1 – System Call dan Instrumentasi Kernel**:
  Menambahkan dua system call baru, yaitu `getpinfo()` untuk melihat proses yang aktif dan `getReadCount()` untuk menghitung jumlah pemanggilan `read()` sejak boot.
---

## 🛠️ Rincian Implementasi

Modul ini bertujuan untuk memperkenalkan teknik dasar modifikasi kernel sistem operasi melalui penambahan dua buah system call pada XV6. System call tersebut adalah:
1.	getpinfo() — untuk memperoleh informasi proses yang sedang aktif di sistem.
2.	getreadcount() — untuk memantau jumlah total pemanggilan read() sejak sistem di-boot.

File yang Dimodifikasi
Berikut adalah file yang dimodifikasi dan alasannya:
1. sysproc.c
•	Menambahkan implementasi sys_getpinfo() dan sys_getreadcount().
•	Di sinilah logika system call baru diletakkan.
2. proc.h
•	Menambahkan struktur baru struct pinfo untuk menyimpan data proses.
•	Tambahan global integer readcount untuk melacak jumlah pemanggilan read().
3. usys.S
•	Menambahkan syscall wrapper untuk getpinfo dan getreadcount agar dapat diakses dari user space.
4. user.h
•	Mendeklarasikan fungsi int getpinfo(struct pinfo *); dan int getreadcount(void); untuk digunakan dalam program user.
5. syscall.c dan syscall.h
•	Menambahkan entri syscall baru ke tabel syscall, menghubungkan nomor syscall dengan fungsi handler.
6. sysfile.c
•	Menambahkan readcount++ ke dalam sys_read() untuk menginstrumentasi pemanggilan fungsi baca.
•	Menangani error readcount undeclared dengan mendeklarasikannya secara global.
7. Makefile
•	Memastikan program ptest dan rtest dikompilasi sebagai bagian dari build system XV6.
8. ptest.c dan rtest.c (program user)
•	ptest.c: menampilkan informasi semua proses dengan getpinfo().
•	rtest.c: menampilkan nilai getreadcount() sebelum dan sesudah pemanggilan read().

### Contoh untuk Modul 1:

* Menambahkan dua system call baru di file `sysproc.c` dan `syscall.c`
* Mengedit `user.h`, `usys.S`, dan `syscall.h` untuk mendaftarkan syscall
* Menambahkan struktur `struct pinfo` di `proc.h`
* Menambahkan counter `readcount` di kernel
* Membuat dua program uji: `ptest.c` dan `rtest.c`
---

## ✅ Uji Fungsionalitas

Tuliskan program uji apa saja yang Anda gunakan, misalnya:

* `ptest`: untuk menguji `getpinfo()`
* 1. ptest
Program ini dibuat dan dikompilasi sebagai binary user di XV6. Saat dijalankan, ia memanggil getpinfo() dan mencetak semua proses aktif.
Output:
$ ptest
PID     MEM     NAME
1       12288   init
2       16384   sh
3       12288   ptest

* `rtest`: untuk menguji `getReadCount()`
* Program ini mencetak getreadcount() sebelum dan sesudah menjalankan read().
Output:
$ rtest
Read Count Sebelum: 12
Read Count Setelah: 13

* `cowtest`: untuk menguji fork dengan Copy-on-Write
* `shmtest`: untuk menguji `shmget()` dan `shmrelease()`
* `chmodtest`: untuk memastikan file `read-only` tidak bisa ditulis
* `audit`: untuk melihat isi log system call (jika dijalankan oleh PID 1)

---

## 📷 Hasil Uji



### 📍 Contoh Output `cowtest`:

```
Child sees: Y
Parent sees: X
```

### 📍 Contoh Output `shmtest`:

```
Child reads: A
Parent reads: B
```

### 📍 Contoh Output `chmodtest`:

```
Write blocked as expected
```

Jika ada screenshot:

```
![hasil cowtest](./screenshots/cowtest_output.png)
```

---

## ⚠️ Kendala yang Dihadapi

Tuliskan kendala (jika ada), misalnya:

* Salah implementasi `page fault` menyebabkan panic
* Salah memetakan alamat shared memory ke USERTOP
* Proses biasa bisa akses audit log (belum ada validasi PID)

---

## 📚 Referensi

Tuliskan sumber referensi yang Anda gunakan, misalnya:

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

