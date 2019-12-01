# MULTICAST

## Tujuan

- Memahami dan mengerti komunikasi multicast 
- Menerapkan komunikasi multicast pada aplikasi

## Petunjuk

- Awali setiap sebelum membuat projek dengan berdoa.
- Baca dan pahami tujuan, dasar teori, dan latihan-latihan modul dengan baik.
- Kerjakan tugas-tugas projek dengan baik, sabar dan jujur

### Ulasan Teori

#### Konsep Multicast
Multicast communication adalah proses pengiriman pesan dari sebuah proses kepada member-member yang ada dalam proses tersebut.
Karakteristik multicast messages :
• Toleransi kesalahan berdasarkan replika servis
Sebuah replika servis terdiri dari sekumpulan server. Permintaan klien akan dikirim secara multicast ke seluruh member 
dalam sebuah proses. Sehingga ketika salah satu member mengalami kegagalan, maka permintaan klien akan tetap bisa 
diproses oleh member yang lain.
• Menemukan servis dalam jaringan spontan
Pesan multicast dapat digunakan oleh server dan klien untuk menemukan servis dan mendaftarkan interface nya sendiri 
ataupun mencari interface lain dalam suatu sistem terdistribusi.
• Performansi lebih baik melalui replikasi data
Replikasi adalah suatu teknik untuk melakukan copy dan pendistribusian data dan objek-objek database dari satu database 
ke database lain dan melaksanakan sinkronisasi antara database sehingga konsistensi data dapat terjamin.
• Data direplikasi untuk meningkatkan performansi servis.
• Penyebaran notifikasi event
IP multicast digunakan untuk menyampaikan satu paket kepada banyak penerima (sekumpulan komputer) yang tergabung dalam grup multicast.
Alokasi alamat multicast :
Alamat-alamat multicast IPv4 didefinisikan dalam ruang alamat kelas D, yakni 224.0.0.0/4, yang berkisar dari 224.0.0.0 
hingga 224.255.255.255. Prefiks alamat 224.0.0.0/24 (dari alamat 224.0.0.0 hingga 224.0.0.255) tidak dapat digunakan 
karena dicadangkan untuk digunakan oleh lalu lintas multicast dalam subnet local. Daftar alamat multicast yang ditetapkan oleh IANA.
Pada JAVA, komunikasi multicast dilakukan menggunakan kelas – kelas :
- DatagramPacket, dan
- MulticastSocket yang merupakan subclass dari DatagramSocket 

#### Multicast Socket
Paket MulticastSocket terletak pada java.net.MulticastSocket. Cara kerjanya mirip dengan DatagramSocket, karena memang 
turunan dari kelas DatagramSocket.
Konstruktor :
1. `public MulticastSocket( ) throws SocketException` : membuat objek multicastSocket yang terikat pada anonymous port 
(terserah alokasi dari sistem), cocok untuk klien dimana tidak membutuhkan port khusus/tertentu untuk menerima jawaban dari server yang dihubungi.
Contoh :
    ```java
    try {
      MulticastSocket ms = new MulticastSocket( );
      // send some datagrams...
    }
    catch (SocketException se) {
      System.err.println(se);
    }
    ```
2. `public MulticastSocket(int port) throws SocketException` membuat objek MulticastSocket untuk menerima datagram dari 
port yang ditentukan.
Contoh :
    ```java
    try {
      MulticastSocket ms = new MulticastSocket(40
      // receive incoming datagrams...
    }
    catch (SocketException ex) {
      System.err.println(ex);
    }
    ```
3. `public MulticastSocket(SocketAddress bindAddress) throws IOException` : membuat objek MulticastSocket menggunakan 
objek SocketAddress. Jika SocketAddress nya adalah objek yang mengikat alamat port tertentu, maka konstruktor ini akan 
sama persis dengan konstruktor sebelumnya. 
    ```java
    Contoh :
    try {
        SocketAddress address = new InetSocketAddress(4000);
        MulticastSocket ms = new MulticastSocket(address);
     // receive incoming datagrams...
    }
    catch (SocketException ex) {
     System.err.println(ex);
    }
    ```
Jika objek SocketAddress terikat pada alamat dan port dari sebuah interace, maka multicastSocket hanya akan menerima 
data dari port dan alamat interface yang ditentukan (contoh kasus server yang memiliki banyak interface network). 
Contoh :
    ```java
    try {
    SocketAddress address = new InetSocketAddress("192.168.254.32", 4000);
        MulticastSocket ms = new MulticastSocket(address);
      // receive incoming datagrams...
    }
    catch (SocketException ex) {
        System.err.println(ex);
    }
    ```
    
Begitu objek MulticastSocket terbentuk, ada 4 operasi yang bisa dilakukan oleh objek ini :
1. Join ke sebuah grup multicast.
2. Mengirim data ke grup multicast
3. Menerima data dari grup multicast
4. Meninggalkan grup multicast.

Untuk bekerja menggunakan MulticastSocket, kita bisa menggunakan method-method yang dimilikinya :
1. `public void joinGroup(InetAddress address) throws IOException`
2. `public void joinGroup(SocketAddress address, NetworkInterface interface) throws IOException // Java 1.4`
3. `public void leaveGroup(InetAddress address) throws IOException`
4. `public void leaveGroup(InetAddress address) throws IOException`
5. `public void leaveGroup(SocketAddress multicastAddress, NetworkInterface  interface)throws IOException // Java 1.4`
6. `public void send(DatagramPacket packet, byte ttl)throws IOException`
7. `public void setInterface(InetAddress address)throws SocketException`
8. `public InetAddress getInterface( ) throws SocketException`
9. `public void setNetworkInterface(NetworkInterface interface)throws SocketException // Java 1.4`
10. `public NetworkInterface getNetworkInterface( ) throws SocketException // Java 1.4`
11. `public void setTimeToLive(int ttl) throws IOException // Java 1.2`
12.	`public void setTimeToLive(int ttl) throws IOException // Java 1.2`
13.	`public void setLoopbackMode(boolean disable) throws SocketException`
14.	`public boolean getLoopbackMode( ) throws SocketException`
15.	`receive()`

### Langkah-langkah Praktikum
#### Program Server
1. Buatlah Project baru dengan class ChatServer.
2. Tuliskan perintah berikut:
    ```java
    public class ChatServer {
       public static void main(String args[]) throws Exception {
          MulticastSocket server = new MulticastSocket(1234);
          InetAddress group = InetAddress.getByName("234.5.6.7");
          //getByName – Mengembalikan alamat IP yang diberikan oleh Host
          server.joinGroup(group);
          boolean infinite = true;
          /* Server terus-menerus menerima data dan mencetak mereka */
          while(infinite) {
             byte buf[] = new byte[1024];
             DatagramPacket data = new DatagramPacket(buf,
                                                   buf.length);
             server.receive(data);
             String msg = new String(data.getData()).trim();
             System.out.println(msg);
          }
          server.close();
       }
    }
    ```
3. Jalankan program tersebut!

#### Program Chat Client
1. Buatlah project baru dengan nama class ChatClient.
2. Tuliskan program berikut:
    ```java
    public class ChatClient {
       public static void main(String args[]) throws Exception {
          MulticastSocket chat = new MulticastSocket(1234);
    
    }
          InetAddress group = InetAddress.getByName("234.5.6.7");
          chat.joinGroup(group);
          String msg = "";
          System.out.println("Type a message for the server:");
          BufferedReader br = new BufferedReader(new
                                  InputStreamReader(System.in));
          msg = br.readLine();
          DatagramPacket data = new DatagramPacket(msg.getBytes(),
           0, msg.length(), group, 1234);
          chat.send(data);
          chat.close();
      }
    ```
3. Jalankan program tersebut.

#### Program MulticastSniffer menggunakan argument
1. Buatlah project dengan class MulcastSniffer.
2. Tuliskan perintah berikut:
    ```java
    public class MulticastSniffer {
    public static void main(String[] args) {
    InetAddress group = null;
    int port = 0;
    // read the address from the command line
    try {
    group = InetAddress.getByName(args[0]);
    port = Integer.parseInt(args[1]);
    }
    // end try
    catch (Exception ex) {
    // ArrayIndexOutOfBoundsException, NumberFormatException,
    // or UnknownHostException
    System.err.println(
    "Usage: java MulticastSniffer multicast_address port");
    System.exit(1);
    }
    MulticastSocket ms = null;
    try {
    ms = new MulticastSocket(port);
    ms.joinGroup(group);
    byte[] buffer = new byte[8192];
    while (true) {
    DatagramPacket dp = new DatagramPacket(buffer,
    buffer.length);
    ms.receive(dp);
    String s = new String(dp.getData( ));
    System.out.println(s);
    }
    }
    catch (IOException ex) {
    System.err.println(ex);
    }
    finally {
    if (ms != null) {
    try {
    ms.leaveGroup(group);
    ms.close( );
    }
    catch (IOException ex) {}
    }
    }
    }
    }
    ```
3. Compile project tersebut.

#### Program MulticastSender menggunakan argument
1.	Buatlah project dengan class MulticastSender.
2.	Tulislah perintah berikut:
    ```java
    public class MulticastSender {
      public static void main(String[] args) {
        InetAddress ia = null;
        int port = 0;
        byte ttl = (byte) 1;
        // read the address from the command line
        try {
          ia = InetAddress.getByName(args[0]);
          port = Integer.parseInt(args[1]);
          if (args.length > 2) ttl = (byte) Integer.parseInt(args[2]);
        }
        catch (Exception ex)  {
          System.err.println(ex);
          System.err.println(
           "Usage: java MulticastSender multicast_address port ttl");
          System.exit(1);
        }
        byte[] data = "Here's some multicast data\r\n".getBytes( );
        DatagramPacket dp = new DatagramPacket(data, data.length, ia, port);
        try {
          MulticastSocket ms = new MulticastSocket( );
          ms.joinGroup(ia);
          for (int i = 1; i <= 10; i++) {
            ms.send(dp, ttl);
          }
          ms.leaveGroup(ia);
          ms.close( );
        }
        catch (SocketException ex) {
          System.err.println(ex);
        }
        catch (IOException ex) {
          System.err.println(ex);
        }
      }
    }
    ```
3. Compile project tersebut.

### Tugas
1. Buatlah aplikasi berbasis GUI untuk mengirimkan pesan dari satu komputer ke semua komputer yang ada di lab!
2. Buatlah program berbasis GUI untul menampilkan semua pesan yang dikirim dari client ke server dalam satu tampilan!
3. Buatlah aplikasi berbasis GUI untuk mengirimkan file txt dari satu computer ke semua komputer yang ada di Lab!



