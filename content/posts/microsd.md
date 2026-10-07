+++
date = '2026-10-07T23:41:33+07:00'
draft = false
title = 'Cara Kerja MicroSD: Kenapa Barang Sekecil Kuku Bisa Menyimpan Terabyte'
tags = ["Teknologi","Pengetahuan"]
+++

Kartu **microSD** ukurannya cuma **15 x 11 x 1 mm** dan beratnya sekitar **0,25 gram**, tapi sekarang ada yang berkapasitas **1 TB lebih**. Bandingkan dengan hard disk pertama, **IBM 350 (1956)**: kapasitasnya hanya **3,75 MB**, ukurannya sebesar dua lemari es, dan beratnya sekitar **1 ton**. Rahasianya ada di tiga hal: **sel memori yang sangat kecil**, **satu sel menyimpan banyak bit**, dan **semuanya ditumpuk ke atas**.

---

### Isi sebuah microSD

Kalau dibongkar, di dalamnya hanya ada:

1. **Chip memori NAND flash**: tempat data disimpan. Kartu berkapasitas besar berisi beberapa keping (*die*) yang ditumpuk.
2. **Controller**: "otak" kecil yang mengatur baca-tulis, memperbaiki error, dan menyembunyikan kerumitan dari HP atau laptop.
3. **Kontak logam (pin)** untuk koneksi, semuanya dibungkus plastik/resin.

Tidak ada bagian yang bergerak, tidak ada baterai. Karena itu microSD tahan guncangan dan datanya tetap ada walau listrik dicabut (**non-volatile**).

### Cara menyimpan 1 bit: menjebak elektron

Unit terkecilnya adalah **sel memori**, sejenis **transistor** khusus yang punya "kantong" penyimpan muatan (dulu disebut *floating gate*, sekarang umumnya *charge trap*). Kantong ini dikelilingi lapisan **isolator** sangat tipis.

- **Menulis (program):** diberi tegangan tinggi (sekitar 15-20 volt) sehingga elektron **"menembus" isolator** lewat efek kuantum (*tunneling*) dan terjebak di kantong.
- **Membaca:** elektron yang terjebak mengubah seberapa mudah transistor menyala (*threshold voltage*). Controller mengukur ini: banyak elektron = satu nilai, sedikit elektron = nilai lain.
- **Menghapus (erase):** tegangan dibalik sehingga elektron ditarik keluar.

Karena dikurung isolator, elektron bisa bertahan di sana **bertahun-tahun tanpa listrik**. Itulah kenapa disebut *flash* memory: menghapusnya dilakukan sekaligus per blok besar, "dalam sekejap" (*flash*).

### Trik 1: satu sel menyimpan banyak bit

Daripada hanya "ada elektron/tidak ada" (1 bit), jumlah elektronnya dibagi menjadi beberapa **tingkat**:

| Jenis | Bit per sel | Tingkat tegangan | Daya tahan (siklus tulis, kira-kira) |
|---|---|---|---|
| **SLC** (Single) | 1 | 2 | ~50.000-100.000 |
| **MLC** (Multi) | 2 | 4 | ~3.000-10.000 |
| **TLC** (Triple) | 3 | 8 | ~1.000-3.000 |
| **QLC** (Quad) | 4 | 16 | ~500-1.000 |

Rumusnya: **n bit butuh 2ⁿ tingkat**. Kebanyakan microSD murah berkapasitas besar memakai **TLC** atau **QLC**. Kapasitas jadi 3-4 kali lipat dari SLC dengan jumlah sel yang sama, tapi harganya: makin banyak tingkat, makin tipis jarak antar tingkat, makin mudah salah baca, lebih lambat, dan lebih cepat aus.

### Trik 2: sel super kecil

Sel memori berukuran **belasan sampai puluhan nanometer** (1 nm = sepersejuta milimeter; rambut manusia tebalnya sekitar 80.000 nm). Dibuat dengan teknik fotolitografi yang sama dengan [chip prosesor](/posts/chip/). Di sel modern, beda antara satu tingkat dan tingkat lain kadang hanya **puluhan sampai ratusan elektron**.

Masalahnya, sekitar tahun 2013-2015 sel datar (*planar*) sudah hampir mentok: kalau diperkecil lagi, elektronnya terlalu sedikit dan sel saling mengganggu (bocor).

### Trik 3: menumpuk ke atas (3D NAND)

Solusinya: **bangun gedung bertingkat, bukan memperluas tanah**.

- **3D NAND** (Samsung menyebutnya V-NAND, mulai produksi massal **2013**) menumpuk sel secara vertikal. Lubang-lubang sangat dalam dibor menembus tumpukan lapisan, dan setiap lapisan yang dilewati lubang menjadi satu sel.
- Awalnya 24 lapisan; sekarang chip terbaru sudah **lebih dari 200 sampai 300-an lapisan**.
- Karena tidak perlu terlalu kecil secara horizontal, sel 3D malah bisa sedikit lebih "longgar" sehingga lebih andal, tapi kepadatannya tetap naik berkali lipat berkat jumlah lantainya.

### Trik 4: menumpuk chipnya juga

Satu keping die NAND modern (seukuran kuku kelingking) bisa menyimpan sekitar **1 Tb (terabit) = 128 GB**. Untuk kartu 1 TB, sekitar **8 keping die ditumpuk** di dalam kartu setebal 1 mm. Supaya muat, setiap wafer silikon **diampelas sampai setipis puluhan mikrometer** (lebih tipis dari selembar kertas), lalu disambung dengan kabel emas/tembaga super halus (*wire bonding*).

Jadi kalau dihitung: 1 TB = **8 triliun bit**. Dengan QLC (4 bit/sel), itu sekitar **2 triliun sel** di dalam benda seberat 0,25 gram.

---

### Peran controller: si pengatur yang tak terlihat

Flash itu sebenarnya "rewel", dan controller menutupi semua kekurangannya:

- **Tidak bisa menimpa langsung:** data ditulis per *page* (beberapa KB), tapi dihapus per *block* (beberapa MB). Controller mengatur pemindahan data ini (**Flash Translation Layer**).
- **Wear leveling:** setiap sel punya batas jumlah tulis. Controller menyebar tulisan merata ke semua sel supaya tidak ada bagian yang cepat rusak.
- **ECC (Error Correction Code):** sel TLC/QLC sering salah baca sedikit-sedikit. Controller menyimpan data tambahan untuk mendeteksi dan memperbaikinya secara otomatis.
- **Bad block management:** blok yang rusak ditandai dan diganti dengan blok cadangan (*over-provisioning*).

### Kenapa kapasitasnya terlihat berkurang?

Kartu **128 GB** di Windows tampil sekitar **119 GB**. Bukan ditipu: pabrik menghitung 1 GB = 1.000.000.000 byte (desimal), sedangkan Windows menghitung 1 GiB = 1.073.741.824 byte (biner). Ditambah sedikit ruang untuk sistem file.

### Label di kartu

- **SDHC** (sampai 32 GB), **SDXC** (sampai 2 TB), **SDUC** (sampai 128 TB, standar masa depan).
- **Class 10 / U1 / U3 / V30:** kecepatan tulis minimum (10 / 10 / 30 / 30 MB/s), penting untuk merekam video.
- **A1 / A2:** performa untuk menjalankan aplikasi (baca-tulis acak), penting kalau dipakai di HP atau Nintendo Switch.

---

### Pelajaran

- microSD menyimpan data dengan **menjebak elektron** di dalam sel yang dikurung isolator, sehingga data bertahan tanpa listrik.
- Kapasitas raksasa datang dari empat trik yang dikalikan: **sel nanometer x banyak bit per sel (TLC/QLC) x ratusan lapisan 3D x beberapa die ditumpuk**.
- Ada harganya: makin padat, makin **lambat** dan makin **cepat aus**. Kartu murah berkapasitas besar cocok untuk menyimpan foto/video, bukan untuk ditulis terus-menerus (misalnya CCTV, kecuali kartu khusus *high endurance*).
- Flash tetap bisa rusak atau datanya memudar kalau didiamkan bertahun-tahun, jadi **jangan jadikan microSD satu-satunya tempat backup**.
- Waspada **kartu palsu**: kapasitas "1 TB" yang aslinya 32 GB dan datanya hilang begitu penuh. Cek dengan aplikasi seperti **H2testw** atau **F3**.
