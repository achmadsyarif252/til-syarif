+++
date = '2026-07-05T23:05:06+07:00'
draft = false
title = 'Rsa'
tags = ["Tech", "Math", "Security"]
+++

**RSA (Rivest-Shamir-Adleman)** adalah salah satu algoritma kriptografi asimetris yang paling populer. Asimetris artinya algoritma ini menggunakan dua kunci yang berbeda:
1. **Public Key**: Kunci yang disebarkan ke semua orang (digunakan untuk mengenkripsi pesan).
2. **Private Key**: Kunci rahasia yang disimpan rapat-rapat oleh pemiliknya (digunakan untuk mendekripsi pesan).

Keamanan RSA bertumpu pada matematika dasar: **sangat mudah untuk mengalikan dua bilangan prima yang besar, tetapi sangat sulit untuk mencari faktor prima dari hasil perkalian tersebut (faktorisasi prima).**

---

### Contoh Sederhana RSA (dengan angka kecil)

Mari kita buat simulasi sederhana untuk mengerti alur matematika di balik RSA.

#### 1. Pembuatan Kunci (Key Generation)

* **Pilih dua bilangan prima (p dan q):**
  Misal kita pilih angka prima kecil:
  * `p = 3`
  * `q = 11`

* **Hitung nilai n:**
  `n = p * q = 3 * 11 = 33`
  *(Nilai n ini akan dibagikan ke publik).*

* **Hitung Fungsi Totient Euler / phi(n):**
  `phi(n) = (p - 1) * (q - 1) = 2 * 10 = 20`

* **Pilih eksponen enkripsi (e):**
  Pilih `e` yang relatif prima dengan `phi(n)` (artinya FPB dari `e` dan `phi(n)` adalah 1), dan `1 < e < phi(n)`.
  Misal kita pilih `e = 3` (karena FPB dari 3 dan 20 adalah 1).

  > **Public Key** kita adalah: `(e, n) = (3, 33)`

* **Hitung eksponen dekripsi (d):**
  Nilai `d` harus memenuhi persamaan matematika: `(d * e) % phi(n) = 1`.
  Dengan `e = 3` dan `phi(n) = 20`, kita mencari `d` sedemikian rupa sehingga `(d * 3) % 20 = 1`.
  Kita bisa coba-coba:
  * Jika `d = 7`, maka `(7 * 3) = 21`.
  * `21 % 20 = 1` *(Cocok!)*
  
  Jadi `d = 7`.

  > **Private Key** kita adalah: `(d, n) = (7, 33)`

---

#### 2. Proses Enkripsi

Katakanlah kita ingin mengirim pesan berupa angka `M = 9` (nilai `M` harus lebih kecil dari `n`).

Rumus Enkripsi:
`C = (M^e) % n`

Masukkan angka kita:
`C = (9^3) % 33`
`C = 729 % 33`

Jika kita bagi 729 dengan 33, didapat `729 = (33 * 22) + 3`. Sisa pembagiannya adalah `3`.
Jadi, **pesan terenkripsi (Ciphertext / C) yang dikirim adalah 3**.

---

#### 3. Proses Dekripsi

Penerima mendapatkan pesan `C = 3`. Untuk membacanya kembali, penerima menggunakan Private Key `(d, n) = (7, 33)`.

Rumus Dekripsi:
`M = (C^d) % n`

Masukkan angka kita:
`M = (3^7) % 33`
`M = 2187 % 33`

Jika kita bagi 2187 dengan 33, didapat `2187 = (33 * 66) + 9`. Sisa pembagiannya adalah `9`.
Pesan berhasil dikembalikan menjadi pesan asli, yaitu **9**!

---

### Kenapa RSA Aman?

Di dunia nyata, bilangan prima `p` dan `q` yang digunakan tidak sekecil 3 dan 11, melainkan angka super raksasa dengan panjang ratusan hingga ribuan digit (seperti pada standar RSA 2048-bit). 

Meskipun semua orang tahu nilai `n` (karena disebar di Public Key), komputer super tercanggih saat ini pun butuh waktu ratusan hingga ribuan tahun hanya untuk memfaktorkan `n` menjadi `p` dan `q`. Tanpa mengetahui `p` dan `q`, orang lain tidak akan bisa mencari nilai `d` untuk mendekripsi pesan tersebut.
