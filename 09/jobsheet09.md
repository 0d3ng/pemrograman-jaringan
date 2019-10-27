# Thread

## Tujuan

- Memahami konsep `Thread`
- Dapat membuat `Thread` menggunakan bahasa pemrograman Java
- Menerapkan `Thread` untuk kasus-kasus dalam pemrograman jaringan

## Petunjuk

-   Awali setiap sebelum membuat projek dengan berdoa.
-	Baca dan pahami tujuan, dasar teori, dan latihan-latihan modul dengan baik.
-	Kerjakan tugas-tugas projek dengan baik, sabar dan jujur

### Ulasan Teori

#### Thread
Sebuah komputer umumnya di dalamnya terdapat processor multi core untuk mempercepat proses komputasi, selain dengan 
dukungan perangkat keras perlu dukungan juga perangkat lunak untuk memaksimalkan multi core tersebut. Java adalah salah 
satu bahasa pemrograman yang menyediakan fungsi multi threading.

Misalkan ada sebuah aplikasi jual beli yang memiliki fungsi kompleks sebagai berikut
- Melakukan download stock terakhir dari semua barang di semua cabang
- Melakukan pengecekan harga barang
- Menganalisis data historis untuk perusahaan xyz

Proses di atas butuh waktu lama untuk menjalankannya, jika digunakan sebuah single thread maka proses satu akan selesai
setelah proses yang lain selesai. Proses selanjutnya dapat dijalankan ketika proses yang sebelumnya sudah selesai.