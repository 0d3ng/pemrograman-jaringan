# Object Persistence dan Object Serialization

## Tujuan

- Memahami konsep `object persistence`
-	Menerapkan `object persistence` dengan menggunakan `object serialization` pada aplikasi

## Petunjuk

-   Awali setiap sebelum membuat projek dengan berdoa.
-	Baca dan pahami tujuan, dasar teori, dan latihan-latihan modul dengan baik.
-	Kerjakan tugas-tugas projek dengan baik, sabar dan jujur

### Ulasan Teori

#### Object Persistence
Object persistence adalah kemampuan sebuah object untuk tetap hidup dalam sebuah sistem, tidak terpengaruh waktu dan ruang (space), bahkan apabila aplikasi pembuat object tersebut telah dihentikan atau komputer dimatikan. Untuk itu objek yang diinginkan supaya persistence harus disimpan secara utuh. 
Penyimpanan objek secara utuh melibatkan penyimpanan struktur data yang dimiliki oleh objek tersebut, nilai-nilai atribut yang dimilikinya, dan pemetaan terhadap referensi ke objek lain apabila ada, sekaligus objek yang direferensikannya. Proses itu merupakan sebuah proses yang kompleks dan rumit. 

#### Object Serialization
Object Serialization adalah teknik yang bisa mewujudkan object persistence. Teknik ini mengontrol bagaimana data yang berisi informasi-informasi nilai dan status sebuah objek (termasuk nilai atribut-atributnya, hak akses tiap atributnya baik itu public, protected atau private, dan lain sebagainya) dituliskan dan disimpan dalam bentuk rangkaian byte-byte data.
Objek yang diserialisasi nantinya dapat dikirimkan lewat jaringan, atau disimpan dalam file, untuk kemudian dapat diakses di lain waktu sesuai kebutuhan. Hal tersebut memungkinkan pindahnya suatu objek dari satu JVM ke JVM lain, entah dalam satu mesin atau mesin yang berbeda.

#### Cara Kerja Object Serialization
Setiap objek yang mengimplementasikan interface `java.IO.Serializable` dapat diserialisasikan hanya dengan beberapa baris kode. Mengimplementasikannya sama dengan cara menerapkan interface-interface yang lain pada JAVA, yaitu dengan menambahkan keyword implement pada deklarasi class yang diinginkan dan menggunakan konstrukto tanpa argumen tambahan. Interface ini menunjukkan bahwa class yang kita buat dapat mendukung serialisasi objek. Tidak ada method tambahan yang perlu diimplementasikan pada class tersebut.
Contoh deklarasi class yang menerapkan interface tersebut :

   ```java
    public class SomeClass extends SomeOtherClass implements java.io.Serializable {
            public class SomeClass()
            }
            }
            ...
    }
    
   ```

####   Permasalahan pada Penggunaan Serialisasi
Terdapat beberapa masalah pada awal munculnya metode serialisasi. Contohnya apabila objek yang ingin kita serialisasi memiliki informasi rahasia / sensitif yang tidak patut kita ikutkan dalam serialisasi untuk disimpan dalam bentuk file ke media penyimpanan atau dikirimkan ke jaringan. 
Apabila ada kasus seperti ini : kelas yang ingin kita serialisasi memiliki atribut password dimana bisa secara mudah dibaca apabila diserialisasikan. Untuk mencegah hal-hal tersebut, maka diciptakanlah keyword trancient. Keyword tersebut bisa diberikan pada deklarasi anggota / atribut dari kelas yang tidak kita inginkan untuk diikutkan pada proses serialisasi.
Contoh penggunaan trancient pada deklarasi kelas :
```java
Public class UserAccount implements java.io.Serializable {
		protected String username;
		protected transient String password;
		public UserAccount( )
		{
			….
		}
}
```

#### Membaca dan Menuliskan Object pada Stream
Point utama dari serialisasi adalah bagaimana kita menuliskan objek ke stream dan bagaimana cara mendapatkannya kembali untuk bisa digunakan. Hal tersebut bisa dilakukan dengan menggunakan kelas `java.io.ObjectOutputStream` dan `java.io.ObjectInputStream`

#### Kelas Object Input Stream
Digunakan untuk mengambil objek yang terserialisasi dari byte stream untuk dapat direkonstruksi ulang menjadi bentuk asli objek tersebut. 
Kelas `ObjectInputStream` mengimplementasikan interface ObjectInput yang merupakan turunan dari interface DataInput. Artinya kelas ini menyediakan banyak method untuk beroperasi dengan banyak tipe data seperti halnya pada kelas `DataInputStream`.
- Konstruktor
    -	`protected ObjectInputStream()` – merupakan konstruktor default untuk turunan dari ObjectInputStream
    -	`ObjectInputStream(InputStream input)` – membuat object input stream yang terkoneksi dengan input stream yang ditunjuk, dapat digunakan untuk mengembalikan objek yang terserialisasi
-	Method - method
        - `public final Object readObject()` – membaca objek yang terserialisasi dari stream dan merekonstruksinya kembali

#### Kelas ObjectOutputStream
Kelas ini digunakan untuk menserialisasi objek dan dikirimkan melalui byte stream dalam rangka object persistence. Bisa dihubungkan ke semua output stream seperti file output dan networking stream.
- Konstuktor
    -	`protected ObjectOutputStream()` – konstruktor default
    -	`ObjectOutputStream(OutputStream output)` – membuat objek output stream yang mampu menserialisasi objek dan dikirim melalui stream output yang ditunjuk
-	Method-method
        - `void writeObject (Object object)` – menuliskan objek yang dimaksud ke output stream melalui proses serialisasi.

### Langkah-langkah Praktikum
#### Penggunaan Object Serialization
Contoh penggunaan object serialization dapat dilihat pada kode aplikasi di awah ini, di mana aplikasi membuat objek yang dapat disimpan dan dipanggil kembali lengkap dengan informasi dan status terakhir (nilai-nilai atribut yang dimiliki) objek tersebut.
