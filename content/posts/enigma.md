+++
date = '2026-08-28T22:32:44+07:00'
draft = false
title = 'Enigma'
+++

Mesin Enigma adalah mesin cipher elektro-mekanis yg dipakai militer jerman pada perang dunia II untuk mengengkripsi komunikasi taktis,diplomatik, dan kapal selam. memecahkan kode ini bukan hanya membalikan keadaan perang di Atlantik tapi juga jadi blueprint lahirnya ilmu komputer teoretis dan komputer digital modern

Cara kerja & kompleksitas enigma

Setiap kali operator menekan tombol huruf di keyboard

1. plugboard (steckerbrett) mengacak pasangan huruf sebelum sinyal listrik masuk ke silider internal
2. rotors (rotor berputar) sinyal melewati serangkaian rotor (umumnya 3-4 rotor) tiap kali tombol ditekan, rotor paling kanan berputar satu langkah seperti odometer, mengubah jalur sitrukut listrik untuk huruf berikutnya (enkripsi polialfabetik dinamis)
3. reflector (umkehrwazle) : membalikkan arus kembali melewati rotor melalu jalur berbeda dan menualakan bohlam huruf sandi di lamphboard

kombinasi plugboard, pilihan rotor,susunan ring, dan posisi awal menghasilkan sekitar 1,58 x 10^20 kemungikinan konfigurasi harian

Celah fatal dan serangan Alan Turing

Sebelum perang, matematikawan polandoa (marian rejewski) telah mengembangkan mesin peretas awal bernama Bomba. ketika jerman menambahkan kompleksitas enigma, inggris memusatkan operasi intelijen di bletchley park di bawah araham matematikawan alan turin dan gordon welchnan

turing mengksploitasi dua kelemahan utama

1. celah desain : karna adanya reflektor sebuah huruf tidak akan pernah terekripsi menjadi huruf itu sendiri (E(x) != x)

2. Disiplin operator & cribs : operator jerman kerap mengirim pesan dengan pola tetap misalnya laporan cuaca harian yang selalu diawali "WETTER" atau salah "HEIL HITLER"

Turing menrancan mesin elektromagnetis bernama The Bombe. mesin ini mensimulasikan puluhan enigma secara simultan, meolak ribuan permutasi mustahil dalam hitungan detik menggunakan logika eliminasi berbasis kontradiksi sirkuit listrik.


