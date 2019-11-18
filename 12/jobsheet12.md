# PEMROGRAMAN USER DATAGRAM PROTOCOL (UDP)

## Tujuan

- Memahami User Datagram Protocol (UDP)
- Menerapkan komunikasi UDP pada aplikasi

## Petunjuk

- Awali setiap sebelum membuat projek dengan berdoa.
- Baca dan pahami tujuan, dasar teori, dan latihan-latihan modul dengan baik.
- Kerjakan tugas-tugas projek dengan baik, sabar dan jujur

### Ulasan Teori

#### UDP Socket
User Datagram Protocol (UDP), adalah salah satu protokol lapisan transpor TCP/IP yang mendukung komunikasi yang tidak 
andal (unreliable), tanpa koneksi (connectionless) antara host-host dalam jaringan yang menggunakan TCP/IP. UDP 
memiliki karakteristik-karakteristik berikut:
1. _Connectionless_ (tanpa koneksi): Pesan-pesan UDP akan dikirimkan tanpa harus dilakukan proses negosiasi koneksi antara
 dua host yang hendak berukar informasi.
2. _Unreliable_ (tidak andal): Pesan-pesan UDP akan dikirimkan sebagai datagram tanpa adanya nomor urut atau pesan 
acknowledgment. Protokol lapisan aplikasi yang berjalan di atas UDP harus melakukan pemulihan terhadap pesan-pesan yang 
hilang selama transmisi. Umumnya, protokol lapisan aplikasi yang berjalan di atas UDP mengimplementasikan layanan 
keandalan mereka masing-masing, atau mengirim pesan secara periodik atau dengan menggunakan waktu yang telah didefinisikan.
3. UDP menyediakan mekanisme untuk mengirim pesan-pesan ke sebuah protokol lapisan aplikasi atau proses tertentu di 
dalam sebuah host dalam jaringan yang menggunakan TCP/IP. Header UDP berisi field Source Process Identification dan 
Destination Process Identification.
4. UDP menyediakan penghitungan checksum berukuran 16-bit terhadap keseluruhan pesan UDP.

Aplikasi-aplikasi yang menggunakan protokol UDP antara lain adalah:
- DNS
- Video Streaming
- Voice over IP (VoIP)
Soket UDP bisa menerima data dari lebih dari satu mesin atau host.
 

#### UDP dalam Java
Java mendukung pemrograman komunikasi protokol UDP dengan menyediakan 2 kelas: 
- `java.net.DatagramSocket` 
- `java.net.DatagramPacket` 

##### DatagramSocket
Kelas DatagramPacket mewakili paket data yang ingin ditransmisikan menggunakan protokol UDP. Kelas ini terdiri dari 
array bertipe `byte` yang menjadi tempat data yang ingin dikirim, atau tempat data yang akan diterima. Kelas ini juga 
memiliki alamat IP dan alamat port.

<figure style="text-align: center">
                  <img src="images/12-01.png" alt="Tugas Socket"/>
                  <figcaption style="text-align: center">Datagram Packet</figcaption>
              </figure>
              
Fungsi dari objek DatagramPacket adalah :
1. Untuk mengirim data ke komputer lain menggunakan UDP
2. Untuk menerima data dari komputer lain menggunakan UDP

Konstruktor yang bisa kita gunakan bergantung dari alasan untuk apa objek DatagramPacket tersebut kita buat tadi. Untuk 
mengirim datagram, maka konstruktornya adalah :
1. `public DatagramPacket(byte[] data, int length, InetAddress remote, int port)`
2. `public DatagramPacket(byte[] data, int offset, int length, InetAddress remote, int port)`
Contoh :
```java
InetAddress addr = InetAddress.getByName("192.168.0.1"); 
DatagramPacket packet = new DatagramPacket ( new byte[128], 128, addr, 2000);
```
Sedangkan konstruktor untuk menerima datagram adalah :
1. `public DatagramPacket(byte[] buffer, int length)`
2. `public DatagramPacket(byte[] buffer, int offset, int length)`
Contoh :
```java
DatagramPacket packet = new DatagramPacket(new byte[256], 256);
```

Sedangkan method-method yang dimiliki class DatagramPacket antara lain :
- `InetAddress getAddress()` — mengembalikan alamat IP asal datagram, atau IP tujuan datagam 
- `byte[] getData()` — mengembalikan isi dari DatagramPacket, yaitu array byte. 
- `int getLength()` — mengembalikan nilai panjangnya isi data yang ada pada DatagramPacket.
- `int getPort()` — mengembalikan nomor port asal datagram, atau port tujuan datagam, tergantung jenis datagramnya
- `void setAddress(InetAddress addr)` — mengeset alamat baru untuk DatagamPacket
- `void setData(byte[] buffer)` — memberikan buffer data baru dalam bentuk array byte
- `void setLength(int length)` — mengeset panjang dari DatagramPacket dengan nilai yang dimasukkan. Perlu diingat bahwa 
panjangnya harus lebih kecil dari ukuran maksimum data buffer atau IllegalArgumentException akan terjadi.
- `void setPort(int port)` — memberikan nilai port tujuan baru bagi DatagramPacket.

##### DatagramPaket
Kelas ini digunakan untuk mengirim dan menerima DatagramPacket dari atau ke jaringan. 
Konstuktor :
- `DatagramSocket(int port)` 
digunakan untuk menyatakan penggunaan suatu nomor port sebagai "pintu" untuk menerima koneksi dari client.  untuk 
membuat server datagram socket

- `DatagramSocket(int port, InetAddress laddr)`  
membentuk koneksi dengan protokol UDP pada alamat IP lokal tertentu dan pada nomor port tertentu.

- `DatagramSocket()` 
Kelas ini membentuk koneksi dengan protokol UDP pada alamat IP lokal host dengan penentuan nomor portnya secara random 
berdasar tersedianya nomor port yang dapat digunakan.  untuk membuat client datagram socket

Method-method :
- `void close()` — menutup socket dan melepaskannya dari port lokal. 
- `void connect(InetAddress remote_addr int remote_port)` — membatasi akses menuju alamat tujuan dan port komputer tujuan
- `void disconnect()` — memutuskan DatagramSocket. 
- `InetAddress getInetAddress()` — mengembalikan alamat komputer tujuan dimana socket terhubung, atau null apabila tidak
 ada koneksi ke tujuan. 
- `int getPort()` — mengembalikan alamat port komputer tujuan jika socket terhubung, atau -1 apabila tidak ada koneksi 
- `InetAddress getLocalAddress()` — mengembalikan alamat lokal dimana soket terkoneksi 
- `int getLocalPort()` — mengembalikan nilai alamat port di mana soket terkoneksi 
- `int getReceiveBufferSize() throws java.net.SocketException` — mengembalikan nilai ukuran maksimum buffer untuk paket 
UDP yang akan diterima 
- `int getSendBufferSize() throws java.net.SocketException` — mengembalikan nilai ukuran maksimum buffer untuk paket UDP
 yang akan dikirim 
- `int getSoTimeout() throws java.net.SocketException` — mengembalikan nilai optional timeout soket. Nilai ini digunakan
 untuk menentukan berapa milidetik suatu operasi pembacaan akan memblok sebelum mengeluarkan pesan error 
 `java.io.InterruptedIOException.` Secara default, nilai ini akan berisi 0, menandakan bahwa I/O yang terblok akan dapat digunakan.. 
- `void receive(DatagramPacket packet) throws java.io.IOException` — membaca paket UDP dan menyimpan isi paket tersebut. 
Atribut alamat dan port dari paket akan ditulis ulang dengan alamat pengirim dan port pengirim, dan atribut panjang 
paket akan berisi panjang paket asli yang mana bisa lebih kecil dari ukuran array byte paket. Jika nilai atribut timeout 
tidak ditentukan menggunakan metode DatagramSocket.setSoTimeout(int duration), method ini akan memblok seterusnya. Jika 
sudah ditentukan, maka error java.io.InterruptedIOException akan dikeluarkan apabila waktu melebihi waktu timeout yang sudah ditentukan. 
- `void send(DatagramPacket packet) throws java.io.IOException` — mengirim paket UDP, yang dimasukkan sebagai parameter. 
- `void setReceiveBufferSize(int length) throws java.net. SocketException` — mengeset ukuran maksimum buffer yang 
digunakan untuk UDP packet yang diterima. 
- `void setSendBufferSize(int length) throws java.net.SocketException` — mengeset ukuran maksimum buffer yang digunakan 
untuk UDP packet yang dikirim. 
- `void setSoTimeout(int duration) throws java.net.SocketException` — mengeset nilai option timeout soket. Berupa jumlah 
milidetik yang diperbolehkan untuk melakukan blok pada operasi pembacaan sebelum dinyatakan error `java.io.InterruptedIOException`.

#### Mengirim Data UDP
Langkah-langkah untuk mengirim datagram UDP ke sebuah server UDP :
1. Buatlah Project dengan nama `JavaSenderUDP`.
2. Mengkonversi data yang mau dikirim ke dalam bentuk `array byte`
3. Memasukkan `array byte` tadi, panjang data dalam array, dan `InetAddress` serta nomor port tujuan ke dalam 
DatagramPacket lewat konstruktornya
4. Buat `DatagramSocket` menggunakan konstruktornya (client `DatagramSocket`) , dan memanggil method `send()` dengan 
memasukkan `DatagramPacket` yang telah dibuat.
5. Contoh script sebagai berikut:
    ```java
    InetAddress ia = InetAddress.getByName(“localhost”);
    int Port = 2000;
    String s = “Pesan ini dari UDP“;
    byte[] b = s.getBytes();
    DatagramPacket dp = new DatagramPacket(b, b.length, ia, Port);
    DatagramSocket sender = new DatagramSocket();
    sender.send(dp);
    ```
6. Jalankan program tersebut!

#### Menerima Data UDP
Langkah untuk menerima datagram UDP dari klien adalah :
1. Buatlah sebuah project dengan nama `JavaRecieveUDP`.
2. Buatlah sebuah DatagramPacket kosong (`DatagramPacket` untuk menerima data)
3. Buatlah objek `DatagramSocket` melalui konstruktornya (`DatagramSocket` untuk server)
4. Lalu panggil method `receive()` dari datagramSocket dengan memasukkan `DatagramPacket` kosong yang telah dibuat tadi 
untuk menampung datagram yang datang.
5. Ambil datanya dalam bentuk `array byte` pada DatagramPacket yang telah terisi. Lalu konversikan ke bentuk data yang 
diinginkan.
6.	Contoh  :
```java
try {
      byte buffer = new byte[65536]; 
      DatagramPacket incoming = new DatagramPacket(buffer,buffer.length);
      DatagramSocket ds = new DatagramSocket(2134);
      ds.receive(incoming);
      byte[] data = incoming.getData();
      String s = new String(data, 0, data.getLength());
      System.out.println("Port " + incoming.getPort() + " on " + incoming.getAddress() + " sent this message:");
      System.out.println(s);
}catch (IOException e) {
    System.err.println(e);
}
```

##### Pertanyaan

1. Apakah output dari hasil potongan program di atas ketika dijalankan? Bandingkan dengan hasil di bawah ini

    <figure style="text-align: center">
                      <img src="images/12-02.png" alt="Tugas Socket"/>
                      <figcaption style="text-align: center">Output program</figcaption>
                  </figure>
              
2. Mengapa bisa demikian, silakan hilangkan karakter aneh tersebut, sehingga ketika tampil hanya data yang dikirimkan oleh 
server.
3. Silakan ubah baris perintah `byte buffer = new byte[65536]` sehingga ukuran arraynya lebih kecil, jalankan program 
Anda dan sebenarnya fungsi perintah tersebut.

#### Program Server
1.	Buatlah Project dengan nama Class `ServerDatagram.java.`
2.	Tulislah perintah berikut
```java
import java.net.*;
import java.io.*;
class ServerDatagram
{
    public static DatagramSocket ds;
    public static int clientport=800,serverport=900;
    public static void main (String args[]) throws Exception
    {
        byte buffer[]= new byte[1024];
        ds= new DatagramSocket (serverport);
        BufferedReader dis= new BufferedReader ( new InputStreamReader (System.in));
        System.out.println ("Server menunggu input");
        InetAddress i=InetAddress.getByName ("Localhost");
        while (true)
        {
            System.out.print("Inputan Server: ");
            String str=dis.readLine();
            if ((str==null || str.equals ("end")) )
            break;
            buffer=str.getBytes();
            ds.send ( new DatagramPacket (buffer,str.length(), i, clientport));
        }
    }
}
```
3. Jalankan program tersebut!

#### Program Client
1. Buatlah Project dengan nama Class `ClientDatagram`.
2. Tuliskan perintah berikut:
```java
class ClientDatagram
{
    public static DatagramSocket d;
    public static byte buffer[] = new byte [1024];
    public static int clientport=800,serverport =900;
    public static void main (String args[]) throws Exception
    {
        d= new DatagramSocket (clientport);
        System.out.println ("Client sedang menunggu server mengirimkan data ");
        System.out.println ("tekan Ctrl + C untuk mengakhiri ");
        while (true)
        {
            DatagramPacket p = new DatagramPacket (buffer, buffer.length);
            d.receive (p);
            String ps= new String (p.getData(),0,p.getLength());
            System.out.println("From Server: "+ ps);
        }
    }
}
```
3. Jalankan program tersebut!

## Tugas
1. Modifikasi program di atas sehingga tidak hanya mengirimkan data text, tetapi juga bisa mengirimkan sebuah file.
2. Modifikasilah program diatas agar menjadi program yang dapat digunakan untuk saling mengirim pesan menggunakan 2 komputer.
3. Buatlah Program untuk meremote `Cursor Mouse` komputer lain menggunakan UDP!

