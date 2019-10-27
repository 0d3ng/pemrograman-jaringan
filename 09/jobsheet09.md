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

Jika untuk melakukan analisis data historis membutuhkan waktu 0.5 jam, selanjutnya seorang pengguna ingin download 
ketersediaan barang di gudang dan tentunya info harga barang akan telat disampaikan ke pembeli atau konsumen. Idealnya, 
proses - proses di atas dilakukan oleh thread - thread yang berbeda sehingga tidak mengakibatkan saling tunggu proses
yang satu dengan proses yang lain.

>Sebuah thread sebenarnya adalah sebuar proses yang ringan. Tidak seperti bahasa pemrograman yang lain Java menyediakan
>dukuangan untuk multihreaded, sebuah multithreaded terdiri dari satu atau lebih bagian yang dapat dijalankan secara
>bersamaan. Masing-masing bagian dari sebuah program disebut dengan thread dan masing-masing thread secara terpisah
>untuk menjalankannya.

#### Life Cycle Thread
<figure style="text-align: center">
                  <img src="images/Life_cycle_of_a_Thread_in_Java.jpg" alt="Life cycle thread"/>
                  <figcaption style="text-align: center">https://www.baeldung.com/java-thread-lifecycle</figcaption>
              </figure>

Pada class `java.lang.Thread` terdapat sebuah `static State enum`, yang mendefinisikan kemungkinan sebuah state atau
kondisi sebuah thread. Kondisi thread tersebut adalah sebagai berikut
1. `NEW` - kondisi ketika sebuah thread dibuat atau diciptakan
2. `RUNNABLE` - kondisi ketika sebuah thread berjalan
3. `BLOCKED` - menunggu untuk mendapatkan notifikasi sebuah thread berjalan kembali
4. `WAITING` - menunggu thread yang lain untuk menjalankan operasi tertentu tanpa batas waktu ditentukan
5. `TIMED_WAITING` - menunggu thread yang lain untuk menjalankan operasi tertentu dengan waktu ditentukan
6. `TERMINATED` - sebuah thread telah selesai menjalankan operasi

#### Konstuktor Thread
Thread memiliki 8 konstuktor, dan yang paling sederhana adalah sebagai berikut
1. `Thread()`, membuat sebuah thread dengan nama bawaan
2. `Thread(String nama)`, membuat sebuah thread dengan nama tertentu

#### Methode Thread
Berikut ini adalah methode yang terdapat pada `class Thread`
1. `public void start()`, menjalankan thread dan memanggil methode `run()` pada objek `Thread`
2. `public void run()`, methode yang harus dioverride ketika `extends class Thread` dan harus diimplementasi ketika
`implements interface Runnable` 
3. `public final void setName(String name)`, mengeset nama sebuah object thread
4. `public final void setPriority(int priority)`, mengeset prioritas dari sebuah thread. Nilai prioritas antara 1-10.

#### Membuat Thread
Untuk membuat Thread dapat dilakukan dengan 2 cara yaitu
1. Implementasi `interface Runnable`

   Cara yang paling mudah adalah dengan membuat class dengan implementasi `interface Runnable`, sebuah class hanya cukup
   mengimplemntasi satu mehode yaitu `run()`. 
   Contoh:
   ```java
    public class MyClass implements Runnable {
    public void run(){
       System.out.println("Hello Thread");
       } 
    }
    ```
   Untuk menjalankan method `run()` oleh sebuah thread, yang perlu dilakukan adalah melewatkan `instance MyClass` pada
   sebuah kontruktor `Thread`.
   ```java
    Thread t1 = new Thread(new MyClass()); 
    t1.start();
    ```
   Ketika thread mulai maka akan memanggil method `run()` dari `class MyClass` dan akan menampilkan `Hello Thread`
2. Menurunkan `class Thread`

   Cara yang kedua adalah dengan membuat class yang menurunkan `class Thread`, kemudian mengoverride methode `run()`
   dan menginstansiasi class tersebut. Method `run()` akan dijalankan oleh thread ketika memanggil method `start()`.
   Contohnya adalah di bawah ini
   ```java
       public class MyClass extends Thread {
       public void run(){
          System.out.println("Hello Thread");
          } 
       }
   ```
   Untuk membuat thread dan menjalankan dapat dilihat pada potongan kode di bawah ini
   ```java
   MyClass t1 = new MyClass(); 
   t1.start();
    ```
   Ketika method `run()` dijalankan maka akan menampilkan `Hello Thread`.
   >Selain cara di atas, sebenarnya ada cara lagi yaitu dengan instance class Thread dan melewatkan interface Runnable
   >pada konstruktor. Tetapi model ini disarankan ketika berfungsi hanya untuk menjalankan operasi yang sederhan dan
   >kita tidak bisa mengontrol thread tersebut.
   >```java
   >new Thread(new Runnable() {
   >             @Override
   >            public void run() {
   >                 System.out.println("Hello Thread");
   >             }
   >        }).start();
   >```
   >Jika dijalankan maka akan menampilkan `Hello Thread`