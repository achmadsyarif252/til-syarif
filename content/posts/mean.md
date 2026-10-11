+++
date = '2026-10-11T21:56:34+07:00'
draft = true
title = 'Mean'
+++

## Penjelasan Tambahan

*Catatan: postingan ini belum berisi tulisan asli, jadi "Mean" di sini diartikan sebagai **mean dalam statistika** (rata-rata). Arti lain kata* mean *dalam bahasa Inggris dibahas singkat di bagian akhir.*

### Apa itu mean?

**Mean** (rata-rata) adalah salah satu ukuran **pemusatan data** (*measure of central tendency*), yaitu satu angka yang mewakili "pusat" atau nilai khas dari sekumpulan data. Dua ukuran pemusatan lainnya adalah **median** dan **modus**.

Yang paling umum dipakai adalah **rata-rata aritmetika** (*arithmetic mean*):

```
mean = (x1 + x2 + ... + xn) / n
```

Artinya: jumlahkan semua nilai, lalu bagi dengan banyaknya data.

**Contoh:** nilai ujian 70, 80, 85, 90, 75

```
mean = (70 + 80 + 85 + 90 + 75) / 5 = 400 / 5 = 80
```

### Mean populasi vs mean sampel

Dalam statistika, simbolnya dibedakan tergantung datanya berasal dari mana:

| Jenis | Simbol | Dibaca | Keterangan |
|---|---|---|---|
| Mean populasi | **μ** | "mu" | Rata-rata dari *seluruh* anggota populasi |
| Mean sampel | **x̄** | "x bar" | Rata-rata dari *sebagian* data (sampel), dipakai untuk menaksir μ |

Karena populasi sering terlalu besar untuk diukur semuanya (misalnya tinggi badan seluruh orang Indonesia), kita mengambil sampel lalu memakai x̄ sebagai perkiraan μ.

### Jenis-jenis mean

Selain rata-rata aritmetika, ada beberapa jenis mean lain yang cocok untuk situasi berbeda:

| Jenis | Rumus singkat | Cocok untuk | Contoh |
|---|---|---|---|
| **Aritmetika** | jumlah ÷ n | Data umum yang dijumlahkan | Rata-rata nilai ujian |
| **Tertimbang** (*weighted*) | Σ(wᵢ·xᵢ) ÷ Σwᵢ | Data yang bobotnya berbeda | IPK (nilai × SKS) |
| **Geometrik** | akar pangkat n dari (x1 × x2 × ... × xn) | Laju pertumbuhan, persentase berantai | Rata-rata pertumbuhan investasi per tahun |
| **Harmonik** | n ÷ (1/x1 + 1/x2 + ... + 1/xn) | Rata-rata laju/kecepatan | Kecepatan rata-rata pergi-pulang |

**Contoh mean tertimbang (IPK):**

| Mata kuliah | Nilai | SKS |
|---|---|---|
| Kalkulus | 4,0 | 3 |
| Fisika | 3,0 | 2 |
| Bahasa | 3,5 | 1 |

```
IPK = (4,0×3 + 3,0×2 + 3,5×1) / (3 + 2 + 1) = 21,5 / 6 ≈ 3,58
```

Kalau dihitung dengan rata-rata biasa hasilnya 3,5, padahal Kalkulus yang SKS-nya paling besar mestinya lebih berpengaruh.

**Contoh mean geometrik:** investasi naik 50% di tahun pertama lalu turun 50% di tahun kedua. Rata-rata aritmetikanya 0%, seolah-olah impas. Padahal uang Rp100 menjadi Rp150 lalu Rp75, alias **rugi**. Mean geometrik dari faktor 1,5 dan 0,5 adalah √(1,5 × 0,5) ≈ 0,866, artinya rata-rata **turun sekitar 13,4% per tahun**. Inilah kenapa laju pertumbuhan harus dirata-rata secara geometrik.

**Contoh mean harmonik:** berangkat 60 km/jam, pulang 40 km/jam dengan jarak yang sama. Kecepatan rata-ratanya **bukan** 50 km/jam, melainkan

```
2 / (1/60 + 1/40) = 48 km/jam
```

karena perjalanan pulang yang lebih lambat memakan waktu lebih lama.

Untuk bilangan positif, selalu berlaku urutan **aritmetika ≥ geometrik ≥ harmonik** (dikenal sebagai ketaksamaan AM–GM–HM), dan ketiganya sama hanya jika semua datanya bernilai sama.

### Mean vs median vs modus

| Ukuran | Definisi | Kelebihan | Kelemahan |
|---|---|---|---|
| **Mean** | Jumlah ÷ banyak data | Memakai semua data, mudah diolah secara matematis | Sangat terpengaruh nilai ekstrem (*outlier*) |
| **Median** | Nilai tengah setelah data diurutkan | Tahan terhadap outlier | Tidak memakai informasi semua nilai |
| **Modus** | Nilai yang paling sering muncul | Bisa untuk data kategori (misal warna favorit) | Bisa ada lebih dari satu, atau tidak ada sama sekali |

### Kelemahan utama mean: outlier

Misalkan ada 5 karyawan dengan gaji (juta rupiah per bulan): 5, 5, 6, 6, 78 (yang terakhir gaji direktur).

- **Mean** = 100 / 5 = **20 juta**
- **Median** = **6 juta**

Mean 20 juta terdengar "rata-rata gajinya besar", padahal 4 dari 5 orang bergaji jauh di bawah itu. Karena itu, data yang distribusinya **miring** (*skewed*), seperti pendapatan, harga rumah, atau kekayaan, biasanya lebih jujur dilaporkan dengan **median**. Banyak lembaga statistik melaporkan pendapatan rumah tangga dalam bentuk median karena alasan ini.

Pada distribusi yang simetris, seperti **distribusi normal** (kurva lonceng), mean, median, dan modus bernilai sama. Pada distribusi miring ke kanan (ekor panjang di nilai besar), biasanya mean > median.

Salah satu cara mengurangi pengaruh outlier adalah **trimmed mean** (rata-rata terpangkas): buang sekian persen data terkecil dan terbesar, baru dirata-rata. Prinsip serupa dipakai pada penilaian beberapa cabang olahraga, misalnya nilai juri tertinggi dan terendah dibuang.

### Mean dan nilai harapan (expected value)

Dalam teori peluang, mean dari sebuah variabel acak disebut **nilai harapan** (*expected value*, ditulis E[X]). Contohnya, nilai harapan lemparan satu dadu adalah

```
(1 + 2 + 3 + 4 + 5 + 6) / 6 = 3,5
```

Angka 3,5 tidak mungkin muncul di dadu, tapi kalau dadu dilempar ribuan kali, rata-rata hasilnya akan makin mendekati 3,5. Fenomena ini dijelaskan oleh **Hukum Bilangan Besar** (*Law of Large Numbers*), yang pertama kali dibuktikan oleh **Jacob Bernoulli** dan terbit dalam *Ars Conjectandi* (1713).

Mean juga punya sifat khusus: ia adalah titik yang membuat **jumlah kuadrat selisih** semua data terhadapnya menjadi paling kecil. Sifat inilah yang menjadi dasar metode **kuadrat terkecil** (*least squares*) yang dikembangkan **Legendre** (1805) dan **Gauss** (1809), dan masih dipakai di regresi linear hingga *machine learning* modern (misalnya fungsi galat *Mean Squared Error*).

### Asal kata

Kata *mean* berasal dari bahasa Prancis Kuno *meien* ("tengah"), dari bahasa Latin *medianus*, yang juga menjadi akar kata *median*. Jadi secara bahasa, *mean* memang berarti "yang berada di tengah".

Sebagai catatan, kata *mean* dalam bahasa Inggris punya beberapa arti lain yang tidak berhubungan dengan statistika:

| Kelas kata | Arti | Contoh |
|---|---|---|
| Kata kerja | Bermaksud, berarti | *"What do you mean?"* |
| Kata sifat | Jahat, kejam, tidak ramah | *"Don't be mean to your sister."* |
| Kata sifat (Inggris Britania) | Pelit | *"He's too mean to buy a round of drinks."* |
| Kata sifat (slang) | Hebat, jago | *"She plays a mean guitar."* |
| Kata benda | Rata-rata; jalan tengah | *"the golden mean"* (jalan tengah ala Aristoteles) |

### Ringkasnya

- **Mean** adalah rata-rata, yaitu jumlah semua nilai dibagi banyaknya data; termasuk ukuran pemusatan bersama median dan modus.
- Simbol **μ** untuk populasi, **x̄** untuk sampel.
- Ada beberapa jenis mean: **aritmetika**, **tertimbang** (IPK), **geometrik** (laju pertumbuhan), dan **harmonik** (kecepatan rata-rata).
- Mean **sensitif terhadap outlier**; untuk data miring seperti gaji atau harga rumah, **median** sering lebih representatif.
- Dalam peluang, mean disebut **nilai harapan**, dan menurut Hukum Bilangan Besar rata-rata hasil percobaan akan mendekati nilai ini.
