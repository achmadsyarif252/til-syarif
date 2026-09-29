+++
date = '2026-09-29T23:05:00+07:00'
draft = false
title = 'Paradoks Ulang Tahun: 23 Orang Cukup untuk Peluang 50%'
tags = ["Matematika","Pengetahuan"]
+++

Berapa orang yang harus ada di satu ruangan supaya peluang dua di antaranya berulang tahun di tanggal yang sama mencapai 50%? Kebanyakan orang menebak sekitar 180, yaitu setengah dari 365. Jawaban sebenarnya hanya **23 orang**. Ini disebut **Paradoks Ulang Tahun** (*Birthday Paradox*).

---

### Kenapa disebut paradoks?

Bukan karena ada kontradiksi logika, tapi karena hasilnya **bertentangan dengan intuisi**. Kita otomatis membayangkan pertanyaan yang salah: "berapa peluang ada orang yang ulang tahunnya sama *dengan saya*?" Itu memang kecil. Tapi pertanyaan aslinya adalah: "berapa peluang *ada sepasang orang mana pun* yang tanggalnya sama?"

---

### Hitungannya

Lebih mudah menghitung kebalikannya: peluang **tidak ada** yang kembar tanggal.

- Orang ke-1: bebas, peluang 365/365
- Orang ke-2: harus beda dari orang pertama, peluang 364/365
- Orang ke-3: harus beda dari dua orang sebelumnya, peluang 363/365
- dan seterusnya

Kalikan semuanya, lalu kurangi dari 1:

**P(ada yang sama) = 1 − (365/365 × 364/365 × 363/365 × ... )**

Hasilnya untuk berbagai jumlah orang:

| Jumlah orang | Peluang ada yang sama |
|---|---|
| 10 | ~12% |
| 23 | ~51% |
| 30 | ~71% |
| 50 | ~97% |
| 70 | ~99,9% |

Dengan 70 orang saja, hampir pasti ada yang kembar tanggal.

---

### Kuncinya: jumlah pasangan

Dalam ruangan berisi 23 orang, bukan cuma ada 23 orang, tapi ada **253 pasangan** yang bisa dibandingkan (23 × 22 / 2). Tiap pasangan punya peluang 1/365 untuk cocok. Jumlah pasangan tumbuh **kuadratik**, jauh lebih cepat dari jumlah orangnya, sehingga peluangnya melonjak cepat.

---

### Kenapa ini penting?

Ide yang sama dipakai di dunia nyata:

- **Kriptografi.** *Birthday attack* memanfaatkan fakta bahwa mencari **dua** input yang menghasilkan hash sama (tabrakan/*collision*) jauh lebih mudah daripada mencari input yang cocok dengan satu hash tertentu. Karena itu hash yang aman harus punya panjang yang cukup besar.
- **Basis data dan ID acak.** Kalau kamu membuat ID acak, tabrakan bisa muncul lebih cepat dari perkiraan.
- **Statistik.** Contoh bagus bahwa intuisi manusia buruk soal peluang dan pertumbuhan non-linear.

---

### Catatan

Perhitungan ini mengasumsikan 365 hari dengan peluang lahir yang sama tiap hari. Di dunia nyata kelahiran tidak merata (misalnya lebih banyak di bulan tertentu), dan itu justru **sedikit menaikkan** peluangnya. Jadi 23 orang tetap patokan yang aman.

---

### Pelajaran

- Intuisi kita sering salah menilai hal yang tumbuh cepat, seperti pasangan, kombinasi, dan eksponensial.
- Sebelum menghitung, pastikan **pertanyaannya benar**: "ada yang cocok dengan saya" berbeda jauh dari "ada yang cocok dengan siapa pun".
- Kadang lebih mudah menyelesaikan masalah dengan menghitung **kebalikannya**.
