# Laporan Praktikum Big Data
Repository ini berisi rangkaian praktikum Big Data yang mencakup penyimpanan terdistribusi (HDFS, MongoDB, Cassandra), pemrosesan data skala besar menggunakan MapReduce dan Apache Spark, pembangunan pipeline ingestion dengan Sqoop, Flume, dan Kafka, serta proses pra-pemrosesan dan feature engineering menggunakan PySpark sebagai dasar pemahaman ekosistem Big Data secara menyeluruh.

<br> 

| Variable           |             Isi            |
| -------------------|----------------------------|
| **Nama**           |     Fadil Aditya Adzima    |
| **NIM**            |          312310617         |
| **Mata Kuliah**    |           Big Data          |
| **Dosen Pengampu** | Agung Nugroho S.Kom., M.Kom.|

<br>

## Praktikum 1
Pastikan perangkat lunak berikut sudah terpasang:

*   **Java 8+**: Diperlukan untuk komponen Hadoop/HDFS.
*   **Git**: Diperlukan untuk mengkloning dataset atau repositori kode.
*   **Docker** (Opsional): Mempermudah proses instalasi dan manajemen dependensi.

### Bagian 1 Praktikum HDFS 
1. **Membuat direktori praktikum di HDFS**
   ```bash
   hdfs dfs -mkdir /praktikum
   ```
   ![Picture for HDFS](assets/assetshdfs/hdfs1.png) <br> <br>

2. **Membuat sebuah file beserta dummy data pada file datasest.csv**
   ```bash
    cat > dataset.csv << EOF
    nim,nama,jurusan,ipk
    12345,Andi,Informatika,3.75
    12346,Budi,Sistem Informasi,3.50
    12347,Citra,Teknik Komputer,3.85
    12348,Doni,Informatika,3.60
    12349,Eka,Sistem Informasi,3.90
    12350,Fani,Teknik Komputer,3.45
    12351,Gita,Informatika,3.70
    12352,Hadi,Sistem Informasi,3.55
    EOF
   ```
   ![Picture for HDFS](assets/assetshdfs/hdfs2.png) <br> <br>

3. **Mengunggah dataset ke HDFS & Verfikasi isi direktori**
   ```bash
   ## Perintah untuk upload dataset ke hdfs
   hdfs dfs -put dataset.csv /praktikum/

   ## Perintah untuk memeriksa isi dari direktori praktikum
   hdfs dfs -ls /praktikum/
   ```
   ![Picture for HDFS](assets/assetshdfs/hdfs3.png) <br> <br>

4. **Membaca isi dataset langsung dari HDFS**
   ```bash
   hdfs dfs -cat /praktikum/dataset.csv
   ```
   ![Picture for HDFS](assets/assetshdfs/hdfs4.png) <br> <br>
   

#### Latihan: coba upload file besar (>100MB) dan periksa apakah file tersebut terpecah menjadi blok-blok kecil di HDFS.
1. **Membuat file dummy 150MB lalu upload file besar tersebut ke dalam direktori praktikum**
   ```bash
   # Buat file dummy 150MB
   dd if=/dev/zero of=bigfile.dat bs=1M count=150
   
   # Upload ke HDFS
   hdfs dfs -put bigfile.dat /praktikum/
   ```
   ![Picture for HDFS](assets/assetshdfs/hdfs5.png) <br> <br>

2. **Periksa detail file telah terpecah menjadi beberapa blok**
   ```bash
   hdfs fsck /praktikum/bigfile.dat -files -blocks -locations
   ```
   ![Picture for HDFS](assets/assetshdfs/hdfs6.png) <br> <br>

**Penjelasan Singkat**
File bigfile.dat berukuran 150MB (157,286,400 bytes) dipecah oleh HDFS menjadi 2 blok dengan ukuran rata-rata sekitar 78.6MB per blok, karena HDFS secara otomatis memecah file besar menjadi blok-blok dengan ukuran default (biasanya 128MB atau lebih kecil tergantung konfigurasi) untuk memudahkan penyimpanan terdistribusi, paralelisme pemrosesan, dan fault tolerance dalam sistem big data.

<br> <br>



### Bagian 2 Praktikum MongoDB
1. **Memilih database yang ingin digunakan**
   ```mongodb
   use praktikum
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo1.png) <br>
   Pastikan database otomatis dibuat saat perintah insert pertama dijalankan. <br> <br>

2. **Menambahkan data pada tabel mahasiswa**
   ```mongodb
   db.mahasiswa.insertOne({ nim: "312310617", nama: "Fadil", jurusan: "Informatika" })
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo2.png) <br> <br>

3. **Memeriksa data yang sudah inputkan sebelumnya**
   ```mongodb
   db.mahasiswa.find()
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo3.png) <br> <br>

4. **Menambahkan data sekaligus banyak & Periksa data**
   ```mongodb
   db.mahasiswa.insertMany([
    { nim: "31231022", nama: "Budi", jurusan: "Sistem Informasi" },
    { nim: "31231023", nama: "Andi", jurusan: "Sistem Informasi" },
    { nim: "31231024", nama: "Cihuy", jurusan: "Teknik Mesin" },
    { nim: "31231025", nama: "Tomas", jurusan: "Arsitektur" }
   ])
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo4.png) <br> <br>

5. **Mencari data menggunakan query filter**
   ```mongodb
   ## Query filter bisa disesuaikan masing masing
   db.mahasiswa.find({ jurusan: "Informatika" })
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo5.png) <br> <br>

6. **Membuat indeks pada kolom NIM agar query cepat**
   ```mongodb
   db.
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo6.png) <br> <br>

7. **Menampilkan data secara urut berdasarkan nama (A-Z)**
   ```mongodb
   db.mahasiswa.find().sort({ nama: 1 })
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo7.png) <br> <br>


#### Latihan: coba simpan data dalam bentuk nested JSON (misalnya biodata dengan alamat & kontak).
1. **Membuat data dalam bentuk nested JSON**
   ```mongodb
   db.mahasiswa.insertOne({
    nim: "312310617",
    nama: "Fadil",
    jurusan: "Informatika",
    alamat: {
        jalan: "Jl. Sudirman No. 45",
        kota: "Jakarta",
        kodePos: "12190"
    },
    kontak: {
        email: "fadil@email.com",
        telepon: "08123456789"
    },
    nilai: [
        { matkul: "Cybersecurity", skor: 95 },
        { matkul: "Big Data", skor: 90 },
        { matkul: "Web Programming", skor: 88 }
    ]
   })
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo8.png) <br> <br>

2. **Menampilkan data yang telah dibuat dalam bentuk nested json dengan menggunakan query nested field**
   ```mongodb
   db.mahasiswa.find({ "alamat.kota": "Jakarta" })
   ```
   ![Picture for MongoDB](assets/assetsmongodb/mongo9.png)

   <br> <br>



### Bagian 3 Praktikum Cassandra
1. **Membuat keyspace praktikum**
   ```cassandra
   CREATE KEYSPACE praktikum
   WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};

   ## Gunakan keyspace
   USE praktikum;
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra1.png) <br> <br>

2. **Membuat tabel mahasiswa didalam keyspace praktikum**
   ```cassandra
   CREATE TABLE mahasiswa (
    nim text PRIMARY KEY,
    nama text,
    jurusan text
   );
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra2.png) <br> <br>

3. **Memasukkan data pada tabel mahasiswa**
   ```cassandra
   INSERT INTO mahasiswa (nim, nama, jurusan)
   VALUES ('12345', 'Budi', 'Informatika');

   ## Tampilkan data
   SELECT * FROM mahasiswa;
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra3.png) <br> <br>

4. **Memasukkan banyak data dan tampilkan berdasarkan filter**
   ```cassandra
   INSERT INTO mahasiswa (nim, nama, jurusan) 
   VALUES ('12346', 'Citra', 'Sistem Informasi');
   
   INSERT INTO mahasiswa (nim, nama, jurusan) 
   VALUES ('12347', 'Dewi', 'Teknik Komputer');
   
   INSERT INTO mahasiswa (nim, nama, jurusan) 
   VALUES ('12348', 'Eko', 'Informatika');

   ## Filter Data
   SELECT * FROM mahasiswa WHERE jurusan='Informatika' ALLOW FILTERING;

   ## Lalu Ubah replication factor (untuk cluster)
   ALTER KEYSPACE praktikum WITH replication =
   {'class':'SimpleStrategy','replication_factor':3};
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra4.png) <br> <br>


   #### Latihan: coba jalankan Cassandra dalam 2 node cluster menggunakan Docker Compose dan amati distribusi data.
1. **Pertama tama kita membuat skrip nya terlbih dahulu didalam file docker-compose.yml (yang sudah terlampir). Setelah itu periksa hasil skrip tadi dengan perintah:**
   ```cassandra
   docker-compose ps
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra5.png) <br> <br>

2. **Lalu jalankan sesi shell CQL interaktif**
   ```cassandra
   docker exec -it cassandra-node1 cqlsh
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra6.png) <br> <br>

3. **Membuat keyspace latihan & membuat tabel users**
   ```cassandra
   CREATE KEYSPACE latihan
   WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 2};
   cqlsh> USE latihan;
   cqlsh:latihan> CREATE TABLE users (
   id int PRIMARY KEY,
   name text
   );
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra7.png) <br> <br>

4. **Masukkan data**
   ```cassandra
   cqlsh:latihan> INSERT INTO users (id, name) VALUES (1, 'Fadil');
   cqlsh:latihan> SELECT * FROM users;
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra8.png) <br> <br>

5. **Menampilkan status keseluruhan kluster Apache Cassandraa**
   ```cassandra
   docker exec -it cassandra-node1 nodetool status
   ```
   ![Picture for Cassandra](assets/assetscassandra/cassandra9.png) <br>
   Perintah nodetool status memastikan cluster udah stabil (UN) dan ada dua node yang terdaftar, berarti cluster 2-node udah up.


<br> <br> <br>


## Praktikum 2 Pemrosesan Data Besar
<b> Kasus Studi: Word Count (Menghitung Frekuensi Kata) <b>

*   **Java 8+**: Disdfsfd
*   **Git**: Dipsdfsd
*   **Docker** (Opsional): fdsfdsfs

### Sesi 1 MapReduce (Arsitektur Generasi Pertama)
1. **Mfgagfg**
   ```bash
   hdfgdfgds
   ```
   ![Picture for ](assets/assets/.png) <br> <br>

















































