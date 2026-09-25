+++
date = '2026-09-25T23:51:40+07:00'
draft = false
title = 'Persamaan Roket Tsiolkovsky'
tags = ["Sains","Pengetahuan"]
+++

Kenapa roket harus sebesar gedung hanya untuk membawa kapsul kecil ke luar angkasa? Kenapa roket dibuat bertingkat? Semua jawabannya ada di satu rumus sederhana yang ditulis oleh seorang guru sekolah tuli dari Rusia lebih dari 120 tahun lalu.

---

### Siapa Tsiolkovsky?

Konstantin Tsiolkovsky (1857–1935) kehilangan sebagian besar pendengarannya karena demam scarlet saat berumur sekitar 10 tahun. Karena susah mengikuti sekolah, dia belajar sendiri di perpustakaan Moskow: fisika, kimia, astronomi, matematika.

Dia lalu menjadi guru matematika di kota kecil Kaluga, dan di waktu senggangnya memikirkan cara manusia keluar dari bumi. Tahun 1903 dia menerbitkan makalah *"Eksplorasi Ruang Angkasa dengan Alat Reaksi"* yang berisi rumus roket. Waktu itu pesawat Wright bersaudara bahkan baru terbang pertama kali.

Kutipannya yang terkenal:

> "Bumi adalah buaian umat manusia, tapi manusia tidak bisa tinggal di buaian selamanya."

---

### Rumusnya

    Δv = ve × ln(m0 / mf)

- **Δv** (delta-v): perubahan kecepatan yang bisa dicapai roket
- **ve**: kecepatan semburan gas buang dari mesin
- **m0**: massa awal roket (penuh bahan bakar)
- **mf**: massa akhir roket (setelah bahan bakar habis)
- **ln**: logaritma natural

Idenya: roket maju karena melempar massa (gas) ke belakang dengan sangat cepat, sesuai hukum Newton ketiga (aksi-reaksi). Semakin cepat gas dilempar dan semakin banyak bagian roket yang berupa bahan bakar, semakin besar kecepatan yang didapat.

---

### Masalahnya ada di "ln"

Karena pakai logaritma, menambah bahan bakar **tidak** menambah kecepatan secara sebanding. Contoh dengan ve = 3 km/s (kira-kira mesin kerosin + oksigen cair):

| Rasio massa (m0/mf) | Δv |
|---|---|
| 10 | 6,9 km/s |
| 20 | 9,0 km/s |
| 40 | 11,1 km/s |

Menggandakan bahan bakar hanya menambah sekitar 2 km/s. Kenapa? Karena bahan bakar tambahan itu juga harus ikut diangkat oleh... bahan bakar lainnya. Bahan bakar harus membawa dirinya sendiri.

Astronot NASA Don Pettit menyebut ini **"tirani persamaan roket"** (*the tyranny of the rocket equation*).

---

### Hitung-hitungan ke orbit

Untuk mencapai orbit rendah bumi, roket butuh sekitar **9,4 km/s** (kecepatan orbit ~7,8 km/s ditambah kerugian akibat gravitasi dan hambatan udara).

Dengan ve = 3 km/s:

    m0 / mf = e^(9,4 / 3) ≈ 23

Artinya massa awal roket harus 23 kali massa akhirnya. Dengan kata lain, **sekitar 96% roket adalah bahan bakar**. Tangki, mesin, badan roket, dan muatan harus muat di 4% sisanya. Sebagai perbandingan, kaleng minuman soda lebih "gemuk" wadahnya dibanding roket.

Kalau pakai hidrogen + oksigen cair (ve ≈ 4,4 km/s), rasionya turun jadi sekitar 8,5. Lebih ringan, tapi hidrogen susah disimpan karena harus didinginkan sampai −253°C.

---

### Solusinya: roket bertingkat

Tsiolkovsky sendiri yang mengusulkan jalan keluarnya (tahun 1929 dia menyebutnya "kereta roket"): **buang tangki kosong di tengah jalan.**

Begitu tingkat pertama habis bahan bakarnya, tangki dan mesinnya yang sudah tidak berguna dilepas. Tingkat kedua tidak perlu lagi mengangkut beban mati itu, sehingga rasio massanya kembali bagus. Inilah kenapa Saturn V, Soyuz, sampai Falcon 9 semuanya bertingkat.

---

### Pelajaran

- Rumus sederhana bisa menjelaskan kenapa eksplorasi luar angkasa mahal: hampir seluruh roket hanyalah bahan bakar untuk mengangkat bahan bakar.
- Ini juga alasan roket yang bisa dipakai ulang (seperti booster Falcon 9 yang mendarat kembali) jadi terobosan besar: yang mahal bukan bahan bakarnya, tapi roketnya yang dulu selalu dibuang.
- Seorang guru tuli di kota kecil, tanpa laboratorium, bisa meletakkan dasar teori penerbangan antariksa hanya dengan kertas, pensil, dan kalkulus.
