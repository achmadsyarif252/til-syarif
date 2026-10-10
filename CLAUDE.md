# Panduan untuk Claude

Repo ini adalah blog TIL (Today I Learned) pribadi berbasis Hugo. Postingan ada di `content/posts/*.md`, gambar di `public/images/`.

## Perintah "perkaya" / "tambahkan"

Kalau saya memberi input seperti **"perkaya"**, **"tambahkan"**, atau sejenisnya untuk sebuah postingan, lakukan ini:

1. **Jangan ubah tulisan asli saya sama sekali.** Biarkan apa adanya, termasuk typo, gaya bahasa santai, dan gambar. Itu catatan murni saya.
2. Di bawah konten asli (setelah gambar, kalau ada), tambahkan pemisah `---` lalu heading `## Penjelasan Tambahan`.
3. Isi bagian itu dengan:
   - Bahasa Indonesia yang lebih rapi dan proper.
   - Fakta yang lebih kaya dan akurat: angka, tahun, tokoh, istilah ilmiah, sumber/rujukan bila relevan.
   - Subjudul (`###`) per topik, dan tabel atau daftar poin kalau membantu.
   - Bagian `### Ringkasnya` di akhir berisi beberapa poin inti.
4. Kalau ada hal di tulisan asli yang keliru atau kurang tepat, **jangan edit tulisan aslinya**. Luruskan dengan halus di bagian Penjelasan Tambahan, lalu sebutkan koreksinya di ringkasan balasan chat.
5. Untuk klaim yang masih diperdebatkan atau tidak didukung bukti, tandai dengan jelas.
6. Jangan commit kecuali saya minta.
