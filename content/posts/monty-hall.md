+++
date = '2026-10-02T21:00:00+07:00'
draft = false
title = 'Masalah Monty Hall: Kenapa Sebaiknya Ganti Pintu'
tags = ["Matematika","Pengetahuan"]
+++

Bayangkan kamu ikut kuis. Ada **tiga pintu**. Di balik salah satunya ada mobil, di balik dua lainnya ada kambing. Kamu memilih satu pintu, misalnya pintu 1. Pembawa acara, yang **tahu** letak mobilnya, lalu membuka pintu lain, misalnya pintu 3, dan menunjukkan seekor kambing. Lalu ia bertanya: "Mau tetap di pintu 1, atau pindah ke pintu 2?"

Kebanyakan orang berpikir peluangnya sekarang 50:50, jadi pindah atau tidak sama saja. Ternyata salah. **Kalau pindah, peluang menangmu 2/3. Kalau bertahan, cuma 1/3.** Ini disebut **Masalah Monty Hall**.

---

### Asal namanya

Namanya diambil dari **Monty Hall**, pembawa acara kuis Amerika *Let's Make a Deal* (mulai 1963). Soal ini pertama kali dirumuskan oleh ahli statistik **Steve Selvin** tahun **1975**.

Soal ini terkenal tahun **1990**, saat **Marilyn vos Savant**, kolumnis majalah *Parade* yang pernah tercatat punya IQ tertinggi di dunia, menjawab: "Ya, sebaiknya pindah." Ia lalu menerima **sekitar 10.000 surat**, hampir 1.000 di antaranya dari orang bergelar doktor, dan sebagian besar bilang ia salah. Ada yang menulis, "Kamu yang keliru, dan kamu keliru dengan sangat yakin."

Bahkan **Paul Erdős**, salah satu matematikawan paling produktif dalam sejarah, konon baru yakin setelah melihat simulasi komputer.

Tapi vos Savant benar.

---

### Penjelasan paling sederhana

Kuncinya: **pilihan pertamamu dibuat saat belum ada informasi apa pun.**

- Peluang pintu pilihanmu berisi mobil: **1/3**.
- Peluang mobil ada di **salah satu dari dua pintu lain**: **2/3**.

Saat pembawa acara membuka satu pintu berisi kambing, ia tidak memindahkan mobilnya. Ia hanya **menyingkirkan satu pilihan yang salah** dari kelompok "dua pintu lain". Peluang 2/3 milik kelompok itu sekarang terkumpul di satu pintu yang tersisa.

Pintumu tetap 1/3. Pintu satunya jadi 2/3.

---

### Cek dengan semua kemungkinan

Misalkan kamu selalu memilih pintu 1.

| Mobil di | Pembawa acara membuka | Hasil kalau bertahan | Hasil kalau pindah |
|---|---|---|---|
| Pintu 1 | Pintu 2 atau 3 | **Menang** | Kalah |
| Pintu 2 | Pintu 3 | Kalah | **Menang** |
| Pintu 3 | Pintu 2 | Kalah | **Menang** |

Bertahan menang di 1 dari 3 kasus. Pindah menang di **2 dari 3 kasus**.

---

### Versi 100 pintu

Kalau masih terasa janggal, bayangkan ada **100 pintu**. Kamu memilih pintu 1. Pembawa acara, yang tahu letak mobil, lalu membuka **98 pintu** lain yang semuanya berisi kambing, dan hanya menyisakan pintu 57.

Apakah peluangnya 50:50? Tentu tidak. Pilihan awalmu cuma punya peluang 1/100. Pintu 57 yang "kebetulan" tidak dibuka hampir pasti berisi mobil, dengan peluang **99/100**.

---

### Syarat penting

Jawaban "pindah lebih baik" hanya berlaku kalau:

1. Pembawa acara **tahu** di mana mobilnya.
2. Ia **selalu** membuka pintu berisi kambing.
3. Ia **selalu** menawarkan kesempatan pindah.

Kalau pembawa acara membuka pintu secara acak dan kebetulan muncul kambing, peluangnya memang jadi 50:50. Informasi yang ia berikan bergantung pada **apa yang ia tahu**, dan inilah yang sering terlewat.

---

### Pelajaran

- Intuisi kita soal peluang sering meleset, sama seperti di [Paradoks Ulang Tahun](/posts/paradoks-ulang-tahun/).
- Informasi baru bisa mengubah peluang, tapi tergantung **dari mana** informasi itu datang. Ini inti dari cara berpikir Bayesian.
- Ribuan orang pintar bisa salah bersama-sama. Kalau ragu, cek dengan cara paling jujur: hitung semua kemungkinan, atau coba langsung dengan tiga gelas dan satu koin.
