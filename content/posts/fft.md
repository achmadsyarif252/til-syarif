+++
date = '2026-08-24T23:34:48+07:00'
draft = false
title = 'Fast Fourier Transformer'
+++

Suara manusia, musik, atau getaran mesin kalau direkam bentuknya hanya gelombang naik turun yang sangat rmit terhadap waktu (time domain). dari grafik itu, komputer ngga tahu mana naad bass,vokal atau bising

FFT bertindak seperti alat pemisah resep

1. input : gelombang suara rumit 
2. output : koponen frekuensi / nada penyusunnya 

Secara teknis, fft mengubah sinyal dari time domain (amplitudo vs waktu) menjadi frequency domain (amplitudo frekuensi)

Kenapa "Fast" ?

Rumus dasar pemecah frekuensi namanya Discrete Fourier Transformer, dengan kompleksitas o(n^2), sangat lambat buat ribuan sampel audio per deik

algoritma fft (cooley-tukey) pakai strategi divide & conquer untuk memecah kalkulasi indeks genap dan ganjil memeangkas kompleksitasnya menajdi o(n log2n) 

dipakai untuk apa aja ?

1. audio equalizer & visualizer : menampilkan grafik spektrum bar naik-turun di pemutar musik
2. kompmresi file (mp3/jpeg) membantu menyaring frekuensi yang tidak tertangkap indra manusia agar ukuran file lebih hemat
3. ai voice & speech recognition : mengubah suara ucapan jadi spektogram (gambar frekueni yang bisa dibaca dan dipelajari oleh model machine learning)