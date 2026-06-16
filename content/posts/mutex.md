+++
date = '2026-06-16T23:55:43+07:00'
draft = false
title = 'Mutex'
tags = ["kotlin"]
+++

Mutex (mutual exclusion) adalah sebuah class yg digunakan buat mengantisipasi race condition dengan tidak mengizinkan process lain berjalan di sebelum proses yg dikerjakan selesai tapi tanpa mengaggu thread ui sehingga tidak membuat freeze ui

kalau synchronized(lock) itu dipakai buat mengunci sebuah data agar tidak bisa diakses secara bersamaan write dan read, jadi harus salah satu dulu baru bisa dilanjutkan, misal read baru write tidak bisa sekaligus read dan write karna akan membuat clash, selain itu ini blocking thread tetapi karna proses operasi yg dijalankan sangat2 cepat (mikrodetik) tidak akan menggangui ui

contoh kode
```kotlin
val mutex = Mutex()
var counter = 0
fun main(){
    mutex.lockWith{
        counter++
    }
}
```