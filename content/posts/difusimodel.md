+++
date = '2026-10-26T23:25:35+07:00'
draft = false
title = 'Difusi Model'
+++

---

## Penjelasan Tambahan

### Apa itu Model Difusi?

**Model difusi** (*diffusion model*) adalah jenis model AI generatif yang belajar **membuat data baru (gambar, audio, video, bahkan struktur protein) dengan cara menghilangkan *noise* secara bertahap**. Model inilah yang ada di balik generator gambar populer seperti Stable Diffusion, DALL·E 2/3, Imagen, dan Midjourney.

Ide dasarnya terinspirasi dari fisika, yaitu proses **difusi**: setetes tinta yang dijatuhkan ke air lama-lama menyebar hingga warnanya rata dan bentuk awalnya hilang. Model difusi mempelajari kebalikan dari proses itu, yaitu bagaimana "mengumpulkan kembali tinta" dari air yang sudah keruh rata.

### Dua Proses Utama

| Proses | Arah | Apa yang terjadi | Perlu dilatih? |
|---|---|---|---|
| **Forward process** (difusi maju) | Data → Noise | Gambar asli ditambah *noise* Gaussian sedikit demi sedikit sampai menjadi bintik acak murni | Tidak, ini rumus tetap |
| **Reverse process** (difusi balik) | Noise → Data | Jaringan saraf menebak dan membuang *noise* langkah demi langkah sampai muncul gambar yang jelas | **Ya**, inilah yang dipelajari model |

Secara matematis, pada versi DDPM satu langkah maju ditulis:

```
x_t = √(1 − β_t) · x_(t−1) + √β_t · ε
```

dengan β_t adalah besarnya *noise* pada langkah ke-t dan ε adalah *noise* acak Gaussian. Hebatnya, kita bisa langsung "melompat" ke langkah mana pun tanpa menghitung satu per satu:

```
x_t = √ᾱ_t · x_0 + √(1 − ᾱ_t) · ε
```

dengan ᾱ_t adalah hasil kali semua (1 − β) dari langkah 1 sampai t.

### Bagaimana Model Dilatih?

1. Ambil gambar asli x_0 dari dataset.
2. Pilih langkah acak t (misalnya antara 1 dan 1.000).
3. Tambahkan *noise* ε sesuai rumus di atas sehingga diperoleh x_t.
4. Minta jaringan saraf ε_θ **menebak noise apa yang ditambahkan**.
5. Hitung selisih tebakan dengan *noise* sebenarnya: `‖ε − ε_θ(x_t, t)‖²`.
6. Perbaiki bobot jaringan dengan *backpropagation* dan *gradient descent*, lalu ulangi jutaan kali.

Jadi model tidak belajar "menggambar kucing" secara langsung. Ia belajar **mengenali noise**, dan dengan membuang noise berulang kali, gambar kucing perlahan muncul.

Arsitektur jaringan yang dipakai awalnya adalah **U-Net** (Ronneberger dkk., 2015). Model-model terbaru banyak beralih ke **Diffusion Transformer / DiT** (Peebles & Xie, 2023), yang dipakai antara lain oleh Sora dan Stable Diffusion 3.

### Dari Teks ke Gambar

Supaya bisa mengikuti perintah seperti *"kucing astronaut di bulan, gaya lukisan cat minyak"*, model diberi **kondisi** berupa representasi teks (misalnya dari encoder CLIP atau T5). Teknik **classifier-free guidance** (Ho & Salimans, 2021) membuat hasil lebih patuh pada *prompt*: model menghitung tebakan dengan dan tanpa teks, lalu memperbesar selisih keduanya.

**Stable Diffusion** juga memakai trik **latent diffusion** (Rombach dkk., 2022): proses difusi tidak dijalankan langsung pada piksel, tetapi pada representasi terkompresi (*latent space*) hasil autoencoder. Ruangnya jauh lebih kecil, sehingga model ini bisa berjalan di GPU rumahan.

### Sejarah Singkat

| Tahun | Tonggak | Tokoh / Lembaga |
|---|---|---|
| 2015 | Makalah pertama model difusi: *Deep Unsupervised Learning using Nonequilibrium Thermodynamics* | Jascha Sohl-Dickstein dkk. (Stanford/Berkeley) |
| 2019 | *Score-based generative model* (NCSN) | Yang Song & Stefano Ermon (Stanford) |
| 2020 | **DDPM**, membuat difusi benar-benar bekerja untuk gambar berkualitas tinggi | Jonathan Ho, Ajay Jain, Pieter Abbeel (UC Berkeley) |
| 2020 | **DDIM**, sampling jauh lebih cepat | Jiaming Song dkk. |
| 2021 | Penyatuan difusi dan *score-based* lewat persamaan diferensial stokastik (SDE) | Yang Song dkk. |
| 2021 | *Diffusion Models Beat GANs on Image Synthesis* | Prafulla Dhariwal & Alex Nichol (OpenAI) |
| 2022 | DALL·E 2 (April), Imagen (Mei), **Stable Diffusion** dirilis terbuka (Agustus) | OpenAI, Google, Stability AI + CompVis + Runway |
| 2023 | RFdiffusion untuk desain protein; *consistency models* | Baker Lab (UW); OpenAI |
| 2024 | Sora (video), AlphaFold 3 memakai modul difusi, Stable Diffusion 3 memakai *rectified flow* | OpenAI, Google DeepMind & Isomorphic Labs, Stability AI |
| 2025 | Model bahasa berbasis difusi mulai dicoba (misalnya Gemini Diffusion, Mercury) | Google DeepMind, Inception Labs |

### Perbandingan dengan Model Generatif Lain

| Aspek | GAN | VAE | Model Difusi |
|---|---|---|---|
| Kualitas gambar | Tinggi | Cenderung buram | Sangat tinggi |
| Variasi hasil | Rawan *mode collapse* (hasil mirip-mirip) | Baik | Sangat baik |
| Stabilitas pelatihan | Sulit (dua jaringan saling "bertarung") | Stabil | Stabil |
| Kecepatan menghasilkan | Sangat cepat (1 langkah) | Cepat | Lambat (puluhan sampai ratusan langkah) |

Kelemahan utama difusi adalah **kecepatan**: DDPM aslinya butuh sekitar 1.000 langkah untuk satu gambar. Berbagai metode seperti DDIM, *distillation*, *consistency models*, dan *flow matching* berhasil memangkasnya menjadi puluhan, bahkan hanya 1–4 langkah.

### Kegunaan di Luar Gambar

- **Video**: Sora, Veo, Runway Gen.
- **Audio dan musik**: sintesis suara dan pembuatan musik.
- **Biologi**: desain protein (RFdiffusion) dan prediksi struktur molekul (AlphaFold 3).
- **Robotika**: *Diffusion Policy* untuk menghasilkan urutan gerakan robot.
- **Teks**: masih tahap eksperimen. Model bahasa besar seperti ChatGPT dan Claude pada umumnya masih *autoregressive* (menebak kata satu per satu), bukan difusi. Apakah difusi akan menyaingi pendekatan itu untuk teks **masih menjadi pertanyaan terbuka**.

### Catatan dan Isu

- **Hak cipta**: model dilatih dari miliaran gambar internet (misalnya dataset LAION-5B) dan sedang digugat oleh seniman serta Getty Images. Status hukumnya **masih diperdebatkan** dan berbeda antarnegara.
- **Deepfake**: kemampuan membuat gambar realistis menimbulkan risiko misinformasi, sehingga muncul upaya *watermark* seperti C2PA dan SynthID.
- Istilah "difusi" di sini adalah **analogi matematis** dari fisika statistik, bukan berarti model mensimulasikan difusi zat secara fisik.

### Ringkasnya

- Model difusi menghasilkan data dengan **membuang noise sedikit demi sedikit** dari bintik acak.
- Ada dua proses: **forward** (menambah noise, tidak dilatih) dan **reverse** (membuang noise, dilatih).
- Yang dipelajari jaringan saraf adalah **menebak noise**, dilatih dengan backpropagation.
- Tonggak penting: Sohl-Dickstein (2015), **DDPM** (2020), dan **Stable Diffusion** (2022).
- Dibanding GAN, difusi lebih stabil dan hasilnya lebih beragam, tetapi lebih lambat.
- Kini dipakai untuk gambar, video, audio, protein, robotika, dan mulai dicoba untuk teks.

**Rujukan:**
- Sohl-Dickstein dkk. (2015). *Deep Unsupervised Learning using Nonequilibrium Thermodynamics*. ICML.
- Ho, Jain & Abbeel (2020). *Denoising Diffusion Probabilistic Models*. NeurIPS.
- Song dkk. (2021). *Score-Based Generative Modeling through Stochastic Differential Equations*. ICLR.
- Dhariwal & Nichol (2021). *Diffusion Models Beat GANs on Image Synthesis*. NeurIPS.
- Rombach dkk. (2022). *High-Resolution Image Synthesis with Latent Diffusion Models*. CVPR.
