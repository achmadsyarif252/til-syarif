+++
date = '2026-11-12T23:48:12+07:00'
draft = false
title = 'Memory Alignment: Dari RAM sampai Padding'
tags = ["Teknologi","Pengetahuan","Programming"]
+++

Hari ini saya belajar **memory alignment**. Awalnya terasa susah, ternyata kuncinya adalah memahami fondasinya dulu, urut dari bawah:

1. Memori (RAM) itu apa
2. Variabel disimpan di memori
3. Memori punya alamat
4. CPU mengambil memori per blok
5. Memory alignment

Kalau nomor 1–4 sudah paham, nomor 5 tinggal menyambungkan.

---

## 1. Memori (RAM) itu apa

**RAM adalah "meja kerja" komputer atau HP: tempat menaruh data yang sedang dipakai *saat ini*.**

| Komponen | Analogi | Fungsi |
|---|---|---|
| CPU (prosesor) | Orang yang bekerja | Menghitung dan menjalankan perintah |
| **RAM** | **Meja kerja** | Tempat menaruh barang yang sedang dikerjakan |
| Storage (memori internal) | Lemari arsip | Tempat menyimpan barang jangka panjang |

Orang tidak bisa bekerja langsung di dalam lemari. Berkas harus diambil dari lemari, ditaruh di meja, baru dikerjakan. Begitu juga CPU: **ia hanya bisa mengolah data yang ada di RAM.**

Contoh di aplikasi Android:

1. Buka aplikasi → kode aplikasi dimuat dari storage ke RAM.
2. Reader RFID membaca tag → EPC-nya disimpan di RAM (di variabel).
3. Tekan "Simpan" → data dipindah dari RAM ke storage (database/file).
4. Aplikasi ditutup atau HP mati → **isi RAM hilang**, yang di storage tetap ada.

### Kenapa RAM mudah hilang datanya?

Semua data pada akhirnya disimpan sebagai **0 dan 1**, dan di dalam chip 0/1 itu berupa **ada atau tidaknya muatan listrik**.

- **RAM = ember bocor.** Tiap bit disimpan di kapasitor super kecil yang muatannya merembes keluar dalam hitungan milidetik. Chip RAM harus terus-menerus melakukan **refresh** (mengisi ulang) puluhan kali per detik, dan itu butuh listrik. **Listrik mati → tidak ada yang mengisi ulang → semua data hilang.**
- **Storage (flash) = botol yang tutupnya dilas.** Muatan dikurung di sel yang dikelilingi lapisan isolator, jadi data bisa bertahan **bertahun-tahun tanpa listrik**.

Kalau flash lebih awet, kenapa tidak pakai flash untuk semuanya?

| | RAM | Flash/storage |
|---|---|---|
| Kecepatan | **Sangat cepat** | Jauh lebih lambat |
| Tanpa listrik | Hilang | Tetap ada |
| Bisa ditulis berapa kali | Praktis tak terbatas | **Terbatas** (lama-lama aus) |

Variabel berubah jutaan kali per detik, jadi kalau ditaruh di flash, semuanya lambat dan flash cepat rusak. Pembagian kerjanya: **RAM untuk bekerja, storage untuk menyimpan.**

> Fakta nyambung: tag RFID tidak punya baterai, tapi EPC-nya tidak hilang, karena memori tag adalah jenis *non-volatile* (mirip flash). Dan seperti flash, ada batas berapa kali bisa ditulis. Chip NXP UCODE 9, misalnya, dijamin minimal 100.000 kali tulis. Membaca tidak mengurangi umurnya, hanya menulis.

---

## 2. Variabel disimpan di memori

**Variabel adalah "nama" atau label untuk sebuah tempat di RAM.**

```kotlin
val jumlahTag = 25
```

Artinya: *"sediakan satu tempat di RAM, isi dengan 25, dan tempat itu saya panggil `jumlahTag`."*

Bayangkan RAM sebagai deretan loker:

```
RAM:  [  25  ] [ "Gudang A" ] [ true ] [     ] ...
      jumlahTag  namaGudang   sudahDibaca
```

Saat variabel dipakai, misalnya `val total = jumlahTag + 5`, CPU sebenarnya bolak-balik ke RAM: ambil 25, hitung 30, simpan ke tempat baru berlabel `total`.

### Ukuran tempatnya berbeda-beda

| Tipe (Kotlin) | Contoh | Ukuran |
|---|---|---|
| `Byte` | `7` | 1 byte |
| `Short` | `300` | 2 byte |
| `Int` | `25` | 4 byte |
| `Long` | `9_000_000_000` | 8 byte |
| `String` | `"Gudang A"` | tergantung panjang teks |

Jadi satu variabel bisa menempati **beberapa kotak berurutan**. Dari sinilah nanti muncul pertanyaan inti alignment: *"kalau butuh 4 kotak, mulai di kotak nomor berapa?"*

Kalau membaca 1.000 tag dan menyimpannya di `List`, berarti ada 1.000 kelompok data di RAM. Itu sebabnya aplikasi yang menyimpan terlalu banyak data bisa lemot atau kena `OutOfMemoryError`: mejanya penuh.

---

## 3. Memori punya alamat

Satu kotak di RAM menampung **1 byte** (= 8 bit). Supaya bisa ditemukan, setiap kotak diberi **nomor urut**:

```
Nomor:   0      1      2      3      4      5    ...
       [    ] [    ] [    ] [    ] [    ] [    ]
```

**Nomor kotak inilah yang disebut alamat memori.** Ibarat alamat rumah di satu jalan yang sangat panjang: Jl. RAM No. 0, No. 1, No. 2, ...

### CPU tidak kenal nama variabel

> Nama variabel hanya untuk manusia. **CPU hanya mengenal alamat (angka).**

Compiler/runtime membuat "daftar catatan":

| Nama variabel | Alamat |
|---|---|
| `jumlahTag` | 1000 |
| `rssi` | 1004 |
| `antena` | 1008 |

Lalu `jumlahTag + 5` diterjemahkan menjadi: *"ambil isi alamat 1000, tambah 5, simpan ke alamat 1012."* Di level CPU, kata `jumlahTag` sudah tidak ada.

### Variabel besar = beberapa kotak, alamatnya = kotak pertama

`Int` butuh 4 byte. Kalau mulai di alamat 2000, ia memakai kotak **2000, 2001, 2002, 2003**, dan variabel berikutnya mulai di **2004**.

```
Alamat:  2000  2001  2002  2003   2004  2005  2006  2007
       [ -------- Int ke-1 ------] [ -------- Int ke-2 ------]
```

Catatan kecil:

- Alamat sering ditulis dalam **heksadesimal**, misalnya `0x7ffc3a10`. Itu hanya cara tulis yang ringkas, isinya tetap nomor kotak.
- **Kotlin/Java/Python menyembunyikan alamat** demi keamanan, karena salah alamat bisa menimpa data lain atau membuat crash. **C/C++ membukanya** lewat fitur *pointer*.

---

## 4. CPU mengambil memori per blok

### CPU dan RAM dihubungkan oleh bus

```
[ CPU ]  ═══════ bus ═══════  [ RAM ]
```

**Bus** adalah jalur kabel tempat data lewat, dan lebarnya **tetap**: 4 byte sekali angkut di CPU 32-bit, 8 byte di CPU 64-bit. Anggap saja truk dengan 4 kursi yang **selalu** mengangkut 4 kotak sekaligus.

Truk itu juga **hanya berhenti di "halte" tertentu**, yaitu alamat kelipatan 4:

```
Halte 0           Halte 4           Halte 8
[ 0 | 1 | 2 | 3 ] [ 4 | 5 | 6 | 7 ] [ 8 | 9 |10 |11 ]
  └── 1 angkut ─┘   └── 1 angkut ─┘   └── 1 angkut ─┘
```

Satu angkut = **satu blok**. RAM sendiri sebenarnya tidak punya sekat. Sekatnya muncul karena desain bus yang kaku: titik berhenti tetap membuat rangkaian listrik lebih sederhana, kecil, cepat, dan hemat daya.

### Data rapi vs data nyebrang

**Rapi (aligned):** `Int` di alamat 4 → utuh di satu blok → **1 kali angkut.**

```
[ 0 | 1 | 2 | 3 ] [ 4 | 5 | 6 | 7 ]
                  └─── Int utuh ──┘
```

**Nyebrang (unaligned):** `Int` di alamat 3 → terbelah di dua blok → **2 kali angkut + kerja menyambung.**

```
[ 0 | 1 | 2 | 3 ] [ 4 | 5 | 6 | 7 ]
            └──── Int terbelah ───┘
```

### Kenalan dengan register, PC, dan siklus CPU

Supaya lengkap, ini yang sebenarnya terjadi saat CPU memakai variabel.

- **Register** = "kantong" kecil **di dalam CPU**, sedikit jumlahnya tapi paling cepat. **CPU tidak bisa menghitung langsung di RAM.** RAM cuma tempat menyimpan; yang bisa menghitung adalah **ALU** di dalam CPU, dan ALU hanya mengambil bahan dari register.
- **PC (Program Counter)** = register khusus yang isinya **alamat instruksi berikutnya**, seperti jari penunjuk di buku resep. Register umum (r0, r1, ...) memegang **data**; PC memegang **alamat instruksi**.

Urutan kecepatan:

```
Register  >  Cache  >  RAM  >  Storage
(tangan)    (laci)    (meja)   (lemari)
```

### Simulasi: `c = a + b`

```c
int a = 10;   // compiler taruh di alamat 100
int b = 20;   // alamat 104
int c;        // alamat 108
c = a + b;
```

Satu baris itu menjadi **4 instruksi mesin**. Instruksinya sendiri **juga disimpan di RAM** sebagai angka (konsep *stored program*):

```
Alamat  Instruksi
  0     LDR r0, [100]    ; ambil a dari RAM → register r0
  4     LDR r1, [104]    ; ambil b → r1
  8     ADD r2, r0, r1   ; ALU: r2 = r0 + r1
 12     STR r2, [108]    ; simpan r2 → RAM alamat 108 (c)
```

CPU mengulang **Fetch → Decode → Execute** (ambil instruksi, pahami, kerjakan):

| PC | Instruksi | r0 | r1 | r2 | `c` di RAM |
|---|---|---|---|---|---|
| 0 | `LDR r0, [100]` | 10 | ? | ? | 0 |
| 4 | `LDR r1, [104]` | 10 | 20 | ? | 0 |
| 8 | `ADD r2, r0, r1` | 10 | 20 | 30 | **0** ← belum berubah! |
| 12 | `STR r2, [108]` | 10 | 20 | 30 | **30** ✅ |

Pelajaran yang sempat bikin saya salah jawab: **setelah `ADD`, hasil 30 masih di register (di dalam CPU), belum di RAM.** Baru setelah `STR`, nilai `c` di RAM berubah. "Sudah dihitung" belum tentu "sudah tersimpan".

Hitungannya: 1 baris kode = 4 instruksi = **7 perjalanan bus** (4 ambil instruksi + 2 ambil data + 1 simpan), tapi cuma **1 penjumlahan**. Ternyata sebagian besar kerja CPU adalah **memindahkan data**, bukan menghitung. Makanya hal yang membuat perpindahan data efisien, seperti alignment, sangat penting.

Di instruksi `LDR r0, [100]` itulah alignment bekerja: karena 100 kelipatan 4, cukup 1 kali angkut. Kalau `a` ada di alamat 101, butuh 2 kali angkut, atau di chip tertentu langsung crash.

---

## 5. Memory Alignment

### Definisi

> **Memory alignment = meletakkan data di alamat yang membuatnya mudah diambil CPU, yaitu tidak nyebrang blok.**

Aturan praktisnya:

> **Data berukuran N byte diletakkan di alamat kelipatan N.**

| Tipe (C / Kotlin) | Ukuran | Alamat yang benar |
|---|---|---|
| `char` / `Byte` | 1 | bebas |
| `short` / `Short` | 2 | 0, 2, 4, 6, ... |
| `int` / `Int`, `float` / `Float` | 4 | 0, 4, 8, 12, ... |
| `long` / `Long`, `double` / `Double` | 8 | 0, 8, 16, 24, ... |

⚠️ Koreksi untuk catatan saya sendiri: aturannya **"kelipatan ukuran datanya sendiri"**, bukan sekadar "jangan nyebrang blok". Contohnya di CPU 64-bit (blok 8 byte), `int` di alamat 1–4 **tidak nyebrang** blok 0–7, tapi tetap dianggap unaligned karena 1 bukan kelipatan 4. Blok bus adalah **alasan** aturan ini ada, tapi rumus yang dipakai compiler adalah kelipatan ukuran data.

### Kalau tidak aligned, apa yang terjadi?

| CPU | Reaksi |
|---|---|
| Laptop/PC (x86) | Tetap jalan, hampir tidak terasa |
| HP Android (ARM 64-bit) | Umumnya jalan, sedikit lebih lambat |
| Mikrokontroler menengah (Cortex-M3/M4) | Jalan, lebih lambat |
| Mikrokontroler kecil (Cortex-M0/M0+) | **Crash (HardFault)** |

Pelajaran penting: **"jalan di laptop" tidak membuktikan kode aman di semua chip.**

### Siapa yang mengatur? Compiler, dengan padding

Programmer tidak perlu mengatur sendiri. **Compiler** (C/C++) atau **ART** (runtime Android untuk Kotlin) otomatis menaruh variabel di alamat yang benar, dengan menyisipkan kotak kosong yang disebut **padding**.

```c
char a;   // 1 byte
int  b;   // 4 byte
```

Kalau ditaruh rapat ❌:

```
Blok 1               Blok 2
[ a | b | b | b ] ║ [ b |   |   |   ]
  0   1   2   3   ║   4
      └──── b nyebrang ────┘
```

Yang dilakukan compiler ✅:

```
Blok 1               Blok 2
[ a | ░ | ░ | ░ ] ║ [ b | b | b | b ]
  0   1   2   3   ║   4   5   6   7
      └ padding ┘
```

`b` dipindah ke alamat 4 (kelipatan 4). Harganya 3 byte terbuang, tapi CPU cepat dan aman.

Latihan yang saya kerjakan sendiri: `char x; short y;`

```
[ x | ░ | y | y ]
  0   1   2   3     → y di alamat 2, padding 1 byte, total 4 byte
```

### Dua aturan tambahan untuk struct

**(a) Alignment struct = alignment anggota terbesarnya.** Struct berisi `char` + `int` harus mulai di kelipatan 4.

**(b) Ukuran struct dibulatkan ke kelipatan alignment-nya (ada padding di ujung).**

```
int b; char a; char c;

[ b | b | b | b ][ a | c | ░ | ░ ]   → 8 byte, bukan 6
```

Padding ujung itu ada supaya kalau struct dipakai **berderet dalam array**, struct berikutnya tetap mulai di kelipatan 4:

```
Struct ke-1          Struct ke-2
[b b b b][a c ░ ░]   [b b b b][a c ░ ░]
 0                    8 ← b kedua tetap aligned ✅
```

### Urutan variabel memengaruhi pemborosan

```
Boros:  char a; int b; char c;
[ a ░ ░ ░ ][ b b b b ][ c ░ ░ ░ ]   → 12 byte (6 terbuang)

Hemat:  int b; char a; char c;
[ b b b b ][ a c ░ ░ ]               → 8 byte (2 terbuang)
```

Apakah artinya variabel **harus** selalu diurutkan dari yang terbesar? **Tidak wajib.**

| Situasi | Perlu diurutkan besar → kecil? |
|---|---|
| **Kotlin/Java** | ❌ Tidak. ART menyusun ulang field sendiri. |
| Variabel lokal di fungsi | ❌ Tidak. Sering malah langsung disimpan di register. |
| `struct` C untuk aplikasi biasa | 🤷 Jarang terasa |
| `struct` C di mikrokontroler, datanya ribuan | ✅ Layak |

Prioritas utama tetap **kode yang mudah dibaca**.

### Hubungan padding dengan jumlah RAM

Padding **benar-benar memakan RAM**. Kotak kosong itu dipesan dan tidak bisa dipakai variabel lain.

> **RAM terpakai = ukuran struct (termasuk padding) × jumlah data**

| Jumlah catatan tag | Versi boros (12 B) | Versi hemat (8 B) | Selisih |
|---|---|---|---|
| 1 | 12 B | 8 B | 4 B |
| 1.000 | 12 KB | 8 KB | 4 KB |
| 5.000 | 60 KB | 40 KB | 20 KB |

Besar atau kecil tergantung perangkatnya:

- **Mikrokontroler** (RAM 8–32 KB): selisih 4 KB bisa 12–50% RAM. Contoh, RAM 32 KB untuk 3.000 catatan: versi boros butuh 36 KB (**tidak muat**), versi hemat butuh 24 KB (**muat**). Hanya dengan mengubah urutan variabel!
- **HP Android** (RAM 4–12 GB): tidak terasa sama sekali. Yang lebih menentukan pemakaian RAM di Kotlin adalah **jumlah object**, **gambar/bitmap**, dan **memory leak**, bukan padding. Setiap object Kotlin juga punya "kepala" (header) tambahan, dan `String` EPC (24 karakter hex) jauh lebih boros daripada 12 byte mentahnya.

### Kapan alignment jadi masalah? Saat data datang dari luar

Selama compiler yang mengatur, semuanya aman. Masalah muncul ketika data **tidak ditata compiler**, yaitu **data mentah dari alat**, misalnya respons reader RFID lewat kabel serial/SPI:

```
Data dari reader:  [BB][02][22][00][11][C9] ...
Alamat:             0   1   2   3   4   5
                                └─ angka 2 byte mulai di alamat 3 (ganjil!)
```

Posisinya ditentukan **format protokol**, bukan compiler.

```c
uint16_t n = *(uint16_t*)(buf + 3);   // ❌ paksa baca → crash di Cortex-M0
uint16_t n = (buf[3] << 8) | buf[4];  // ✅ ambil per byte, sambung sendiri
```

Di Kotlin, cara yang salah itu **tidak mungkin ditulis**, karena kita tidak bisa memaksa alamat. Yang ada hanya akses aman per byte:

```kotlin
val n = ((buf[3].toInt() and 0xFF) shl 8) or (buf[4].toInt() and 0xFF)
```

Catatan Kotlin: `Byte` di Kotlin bertanda (-128..127), jadi **wajib `and 0xFF`**. Tanpa itu, byte `0xF7` terbaca -9, bukan 247.

### Topik-topik saudaranya

- **`packed`** (C): atribut untuk sengaja mematikan padding supaya struct persis sama dengan format protokol. Hemat ruang, tapi data di dalamnya bisa unaligned, jadi harus diakses hati-hati.
- **Endianness (urutan byte)**: alignment menjawab "mulai di alamat mana", endianness menjawab "urutan byte-nya bagaimana". Angka `0x12345678` disimpan `[12][34][56][78]` di big-endian (umum di protokol RFID dan jaringan) atau `[78][56][34][12]` di little-endian (cara CPU ARM dan x86 menyimpan). Salah urutan = angka terbaca terbalik.
- **Cache line**: CPU modern menyalin data dari RAM ke cache per 64 byte sekaligus. Prinsipnya sama: data yang nyebrang dua cache line lebih lambat.
- **Memori tag RFID**: bank EPC, TID, dan User dialamatkan per **word = 2 byte**, bukan per byte. Makanya EPC 96 bit selalu tepat 6 word, dan di SDK reader kita mengisi alamat awal dan **jumlah word**. Ini alignment versi "di udara".

### Relevansinya untuk saya (Kotlin + SDK RFID)

| Hal | Relevan? |
|---|---|
| Padding dan urutan field | ❌ Diurus ART |
| Crash karena data unaligned | ❌ Tidak bisa terjadi di Kotlin |
| Mengolah `ByteArray` dari SDK (`and 0xFF`) | ✅ Ya |
| Endianness saat menggabungkan byte jadi angka | ✅ Ya |
| Memori tag dihitung per word (2 byte) | ✅ Ya, saat baca/tulis EPC atau User memory |

Jadi alignment sendiri adalah wawasan "di balik layar", tapi saudara-saudaranya langsung terpakai.

---

## Ringkasnya

1. **RAM** = meja kerja CPU. Cepat, tapi isinya hilang tanpa listrik (ember bocor). Storage = lemari (botol dilas).
2. **Variabel** = label untuk tempat di RAM. Tiap tipe butuh ukuran berbeda.
3. **Alamat** = nomor tiap kotak (1 byte). CPU hanya kenal alamat, bukan nama variabel. Alamat variabel = kotak pertamanya.
4. **Bus** mengangkut data per **blok** (4/8 byte) yang mulai di kelipatan ukurannya. CPU harus `LDR` data ke register, `ADD` di ALU, lalu `STR` kembali ke RAM.
5. **Alignment**: data N byte → alamat kelipatan N. Compiler/ART mengaturnya otomatis dengan **padding**. Padding benar-benar memakan RAM (ukuran × jumlah data), penting di mikrokontroler, tidak terasa di HP.
6. Bahaya utamanya hanya saat **memaksa membaca data mentah dari alat** di C. Solusinya: baca per byte.

## Latihan

1. `char a; int b; short c;` Di alamat berapa `b` dan `c`, dan berapa total ukurannya?
2. Susun ulang soal 1 supaya paling hemat. Berapa ukurannya?
3. Data dari reader: angka 4 byte mulai di alamat 5. Apa risikonya di Cortex-M0?

**Jawaban:**

1. `a` di 0, padding 1–3, `b` di 4–7, `c` di 8–9, padding ujung 10–11 → **12 byte**.
2. `int b; short c; char a;` → b 0–3, c 4–5, a 6, padding 7 → **8 byte**.
3. Alamat 5 bukan kelipatan 4, jadi datanya nyebrang blok → **crash (HardFault)**. Solusinya baca `buf[5]` sampai `buf[8]` satu per satu, lalu gabungkan dengan geser bit sesuai urutan byte protokolnya.
