+++
date = '2026-11-24T22:15:35+07:00'
draft = false
title = 'Rust 101'
tags = ['rust']
+++


Alur dasar : State -> Message -> Update -> View

iced memakai pola yg sama dengan bahasa Elm. ada empat bagian 

1. State -> data yg disimpan
2. Message -> Daftar kejadian mungkin terjadi
3. update -> Mengubah State saat ada mEssage
4. view -> Menggambar tampilan dari State

Struct tempat menyimpan data

```rust
pub struct Navbar {
    pub show_workspace: bool,
}
```

struct adalah kumpulan field. bool hanya bernilai true atau false
pub berarti field ini boleh dibaca dari file lain

```rust
impl Navbar{
    pub fn New() -> Self {
        Self { show_workspace: true}
    }
}
```

impl adalah blok tempat menulis fungsi milik sebuah tipe, dan self berarti tipe itu sendiri.
kalau ada field yg lupa diisi, compiler langsung menolak