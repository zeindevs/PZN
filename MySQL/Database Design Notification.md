# Database Degign Notification

## Sebelum Belajar

- MySQL Database

## Agenda

- Studi Kasus Database Design dan Implementasinya untuk kasus Notification

## #1 Requirement Specification

### Feature

- Membuat fitur Notification yang bisa digunakan untuk melihat informasi untuk pengguna aplikasi

### UI Design

![UI Design](./images/database-design-notification-01.jpg)

### Feature : Inbox

- Inbox akan berisikan semua informasi notifikasi untuk pengguna

![Feature : Inbox](./images/database-design-notification-02.jpg)

### Feature : Kategori

- Terdapat dua jenis kategori; Promo dan Info
- Promo akan berisikan informasi promo yang akan di broadcast ke semua pengguna
- Info akan berisikan informasi yang ditujukan untuk pengguna tersebut, misal status pembayaran atau pembelian
- Jika di kategori nya di klik, secara otomatis isi Inbox hanya berisikan kategori tersebut

![Feature : Kategori](./images/database-design-notification-03.jpg)

### Feature : Read/Unread

- Inbox harus bisa membedakan mana notifikasi yang sudah di read atau belum di read
- Warna hijau menandakan notifikasi belum di read
- Warna putih menandakan notifikasi sudah di read

![Feature : Read/Unread](./images/database-design-notification-04.jpg)

### Feature : Jumlah Notifikasi

- Menu Notifikasi harus bisa menampilkan jumlah notifikasi yang belum dibaca
- Ini agar men-trigger pengguna untuk membuka halaman Inbox notifikasi

![Feature : Jumlah Notifikasi](./images/database-design-notification-05.jpg)

## #2 Persiapan

- Membuat database `belajar_mysql_notification`

## #3 User

![User](./images/database-design-notification-06.jpg)

## #4 Inbox

![Inbox](./images/database-design-notification-07.jpg)

## #5 Category

![Category](./images/database-design-notification-08.jpg)

## #6 Read dan Unread

![Read dan Unread](./images/database-design-notification-09.jpg)

## #7 Counter

![Counter](./images/database-design-notification-10.jpg)

## #8 Materi Selanjutnya

- Latihan Database Design lainnya, dari yang sederhana sampai yang kompleks
