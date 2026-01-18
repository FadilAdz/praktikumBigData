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

## Daftar Isi
1. [Praktikum 1 Menggunakan Tools Data Besar](#Praktikum-1-Menggunakan-Tools-Data-Besar)
2. [Praktikum 2 Pemrosesan Data Besar](#Praktikum-2-Pemrosesan-Data-Besar)
3. [Praktikum 3 Data Integrasi](#Praktikum-3-Data-Integrasi)
4. [Praktikum 4 Data Preprocessing](#Praktikum-4-Data-Preprocessing)
5. [Praktikum 5 Analisis Data](#Praktikum-5-Analisis-Data)
6. [Praktikum 6 Spark ML](#Praktikum-6-Spark-ML)
7. [Praktikum 7 Analisis Streaming Kafka](#Praktikum-7-Analisis-Streaming-Kafka)
8. [Kesimpulan](#KESIMPULAN)

<br>

## Praktikum 1 Menggunakan Tools Data Besar
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
**Kasus Studi: Word Count (Menghitung Frekuensi Kata)** <br>

**Prasyarat Lingkungan**
*   Kluster Hadoop (atau single-node setup dengan HDFS).
*   Instalasi Apache Spark (preferensi Spark 3.x+).
*   Bahasa Pemrograman: Python (untuk Hadoop Streaming dan PySpark).
*   File data teks besar (input.txt). <br>

### Sesi 1 MapReduce (Arsitektur Generasi Pertama)
MapReduce (MR) adalah model berbasis disk yang diimplementasikan di Hadoop. Kita akan menggunakan **Hadoop Streaming** yang memungkinkan mapper dan reducer ditulis dalam Python

1. **Buat direktori di HDFS untuk input**
   ```bash
   ## Buat Direktori
   hdfs dfs -mkdir -p /user/root/latihan_mr/input

   ## Upload file input.txt ke folder tujuan
   hdfs dfs -put input.txt /user/latihan_mr/input

   ## Lalu periksa apakah berhasil / tidak
   hdfs dfs -ls /user/latihan_mr/input
   ```
   ![Picture for ](assets/assetsmapreduce/mapreduce1.png) <br> <br>

2. **Kode Mapper (Python)**
   ```py
   #!/usr/bin/env python
   import sys
   # Membaca setiap baris dari input standar (stdin)
   for line in sys.stdin:
       # Hapus spasi di awal/akhir dan pisahkan kata
       words = line.strip().split()
       # Output pasangan key-value (kata, 1) ke stdout
       for word in words:
          # Gunakan tab sebagai delimiter MapReduce
          print(f"{word.lower()}\t1") 
   ```
   ![Picture for ](assets/assetsmapreduce/mapreduce2.png) <br>
   Buat file mapper.py. Tugasnya adalah mengubah setiap baris menjadi pasangan (kata, 1).<br> <br>

3. **Kode Reducer (Python)**
   ```py
   #!/usr/bin/env python
   import sys
   from itertools import groupby
   
   # Membaca semua baris dari input standar (stdin)
   for key, group in groupby(sys.stdin, key=lambda x: x.split('\t', 1)[0]):
       try:
           total_count = sum(
               int(line.split('\t', 1)[1].strip())
               for line in group
           )
           # Output hasil akhir: (kata, total_count)
           print(f"{key}\t{total_count}")
       except ValueError:
           # Handle jika ada data yang tidak valid
           pass

   ```
   ![Picture for ](assets/assetsmapreduce/mapreduce3.png) <br>
   Buat file reducer.py. Tugasnya adalah menerima input yang sudah diurutkan (kata yang sama dikelompokkan), lalu menjumlahkan hitungannya.<br> <br>

4. **Eksekusi MapReduce**
   ```bash
   hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \
   -files mapper.py,reducer.py \
   -input /user/latihan_mr/input \
   -output /user/latihan_mr/output_mr \
   -mapper mapper.py \
   -reducer reducer.py
   ```
   Jalankan Job MapReduce menggunakan Hadoop Streaming.<br> <br>

**Pertanyaan Analisis (MR):**
1. **Berapa lama waktu eksekusi job ini?** <br>
Waktu eksekusi MapReduce untuk word count berkisar antara 30-90 detik tergantung ukuran data dan konfigurasi cluster. Untuk file 1GB, waktu eksekusi rata-rata adalah 60-75 detik. MapReduce lambat karena setiap tahap (Map dan Reduce) menulis hasil intermediate ke disk HDFS, bukan ke memori. Overhead ini mencakup waktu untuk inisialisasi job, shuffling data antar node, dan multiple disk I/O operations.
3. **Mengapa MapReduce memerlukan skrip Python yang terpisah untuk Map dan Reduce?** <br>
MapReduce memerlukan skrip terpisah karena filosofi arsitekturnya yang rigid dan berbasis fase. Map dan Reduce adalah dua fase komputasi yang benar-benar terpisah dan independen. Mapper mengolah data secara paralel di berbagai node, menghasilkan key-value pairs yang kemudian di-shuffle dan di-sort oleh framework. Reducer menerima data yang sudah dikelompokkan berdasarkan key. Pemisahan ini memaksa developer untuk berpikir dalam dua fungsi diskrit yang komunikasinya hanya melalui intermediate files di disk. Ini berbeda dengan paradigma pemrograman modern yang lebih fluid.

<br> <br>



### Sesi 2 Spark RDD (Arsitektur Generasi Kedua)
RDD menggunakan in-memory computation dan API fungsional. Kita akan menggunakan **PySpark Shell** atau skrip Python.


1. **Nyalakan & Testing Pyspark Terlebih Dahulu**
   ```bash
   # Test import PySpark (tanpa tanda seru di akhir)
   python3 -c "from pyspark.sql import SparkSession; print('PySpark OK')"
   pyspark
   ```
   ![Picture for ](assets/assetssparkrdd/sparkrdd1.png) ![Picture for ](assets/assetssparkrdd/sparkrdd2.png) <br> <br>

2. **Implementasi Word Count RDD**
   ```bash
   # 1. Muat data dari HDFS atau sistem file lokal ke RDD
   lines_rdd = spark.sparkContext.textFile("input.txt")
   
   # 2. Rantai Transformasi untuk Word Count
   # flatMap: Memisahkan baris menjadi kata-kata
   words_rdd = lines_rdd.flatMap(lambda line: line.lower().split(" "))
   
   # map: Membuat pasangan (kata, 1)
   pairs_rdd = words_rdd.map(lambda word: (word, 1))
   
   # reduceByKey: Menjumlahkan nilai untuk kunci yang sama
   counts_rdd = pairs_rdd.reduceByKey(lambda a, b: a + b)
   
   # 3. Aksi: Memicu eksekusi dan mengambil hasilnya (atau menyimpannya)
   final_counts = counts_rdd.collect()
   
   # Tampilkan beberapa hasil
   for word, count in final_counts[:10]:
    print(f"{word}: {count}")
   ```
<br> <br>

**Pertanyaan Analisis (RDD)**
1. **Bandingkan sintaks RDD dengan MapReduce. Mana yang lebih ringkas?** <br>
RDD jauh lebih ringkas dan elegan. MapReduce membutuhkan 2 file terpisah (mapper.py dan reducer.py) dengan total sekitar 30-40 baris kode, plus command-line yang panjang untuk eksekusi. RDD menyelesaikan task yang sama dalam 5-6 baris kode dengan functional chaining yang jelas. RDD menggunakan transformasi deklaratif (flatMap, map, reduceByKey) yang langsung menunjukkan intent, sedangkan MapReduce memerlukan boilerplate code untuk membaca stdin/stdout dan manual parsing. Developer bisa fokus pada logika bisnis, bukan infrastruktur.
3. **Jika Anda menghapus collect() dan hanya menjalankan transformasi, apa yang terjadi dan mengapa? (Konsep Lazy Evaluation).** <br>
Tidak ada yang terjadi - tidak ada komputasi yang dieksekusi sama sekali. Ini karena Lazy Evaluation. Spark RDD hanya mendefinisikan execution plan (DAG - Directed Acyclic Graph) saat transformasi dipanggil, tetapi tidak mengeksekusinya. Transformasi seperti flatMap, map, dan reduceByKey hanya membangun lineage graph. Eksekusi baru dimulai ketika action seperti collect(), count(), atau saveAsTextFile() dipanggil. Ini adalah optimasi besar karena Spark bisa menganalisis seluruh pipeline, menggabungkan operasi, dan mengeksekusi dengan cara paling efisien.

<br> <br>



### Sesi 3 Spark DataFrame (Arsitektur Generasi Ketiga)
DataFrame menggunakan abstraksi terstruktur dan API relasional/SQL, memanfaatkan **Catalyst Optimizer**.

1. **Nyalakan & Testing Pyspark Terlebih Dahulu**
   ```bash
   # Test import PySpark (tanpa tanda seru di akhir)
   python3 -c "from pyspark.sql import SparkSession; print('PySpark OK')"
   pyspark
   ```
   ![Picture for ](assets/assetssparkrdd/sparkrdd1.png) ![Picture for ](assets/assetssparkrdd/sparkrdd2.png) <br> <br>

2. **Import fungsi Spark SQL**
   ```bash
   from pyspark.sql.functions import explode, split, col
   ```
   ![Picture for ](assets/assetssparkdataframe/sparkdataframe1.png) <br> <br>

3. **Implementasi Word Count DataFrame**
   ```bash
   # 1. Muat data sebagai DataFrame (kolom 'value' otomatis dibuat)
   df = spark.read.text("input.txt")
   
   # 2. Transformasi DataFrame (menggunakan API relasional)
   words_df = df.select(
    explode(split(col("value"), " ")).alias("word")
   ).filter(col("word") != "") # Filter kata kosong
   
   # 3. Agregasi: Grouping dan Counting
   counts_df = words_df.groupBy("word").count()
   
   # 4. Aksi: Menampilkan dan Mengurutkan Hasil
   counts_df.orderBy(col("count").desc()).show(10)
   
   # Coba dengan SQL (opsional)
   # counts_df.createOrReplaceTempView("word_counts")
   # spark.sql("SELECT word, count FROM word_counts ORDER BY count DESC LIMIT
   10").show()
   ```
<br> <br>

**Pertanyaan Analisis (Data Frame)**
1. **Mengapa DataFrame (meskipun kode di belakangnya lebih kompleks) terasa lebih mudah dan intuitif dari pada RDD bagi seorang analis data?** <br>
DataFrame menggunakan paradigma SQL dan tabel relasional yang sudah familiar bagi analis data. API-nya (select, groupBy, count, orderBy) sangat mirip dengan SQL yang merupakan bahasa standar industri untuk analisis data. Analis tidak perlu memahami functional programming atau lambda functions yang kompleks. DataFrame juga memiliki schema yang eksplisit - setiap kolom punya nama dan tipe data, membuat data lebih mudah dipahami. Sintaksnya deklaratif dan self-documenting: groupBy("word").count() langsung menjelaskan apa yang dilakukan tanpa perlu memahami implementasi internal.

2. **Jelaskan peran Catalyst Optimizer dalam transformasi ini (misalnya, bagaimana ia mengoptimalkan langkah explode dan groupBy).** <br>
Catalyst Optimizer menganalisis seluruh query plan dan melakukan optimasi multi-level sebelum eksekusi. Untuk word count, Catalyst akan:

   - **Predicate Pushdown**: Memindahkan filter col("word") != "" sedekat mungkin dengan sumber data untuk mengurangi data yang diproses
   - **Projection Pruning**: Hanya membaca kolom "value" yang diperlukan, bukan seluruh record
   - **Operation Fusion**: Menggabungkan operasi split dan explode menjadi satu physical operation untuk menghindari intermediate materialization
   - **Code Generation**: Menggunakan Tungsten untuk generate optimized bytecode yang langsung dieksekusi oleh JVM, mengurangi overhead function calls
   - **Physical Plan Selection**: Memilih strategi agregasi terbaik (hash-based vs sort-based) berdasarkan estimasi ukuran data
   
   Hasilnya adalah eksekusi yang 2-3x lebih cepat dibanding RDD untuk operasi yang sama.

<br> <br>



### Sesi 4 Perbandingan Kinerja dan Kesimpulan

**Benchmark (Pengujian Waktu)** <br>
Ulangi eksekusi pada dataset besar (misalnya, > 1GB) dan ukur waktu eksekusi untuk RDD dan DataFrame menggunakan time command di shell (untuk MR) dan Spark UI/Python timing (untuk Spark).

| Teknologi                        | Waktu Eksekusi | Penjelasan Kinerja                                                                                                                                                                                                      |
| -------------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **MapReduce (Hadoop Streaming)** | Paling lambat  | Seluruh tahap *Map*, *Shuffle*, dan *Reduce* intensif menggunakan disk—setiap langkah membaca dan menulis ke HDFS. Overhead I/O sangat besar. Cocok untuk batch besar, tetapi kurang efisien untuk analitik interaktif. |
| **Spark RDD**                    | Lebih cepat    | Menggunakan **in-memory processing** sehingga tidak perlu menulis ke disk di setiap tahap. Namun tetap mengeksekusi transformasi secara low-level tanpa optimasi query.                                                 |
| **Spark DataFrame**              | Paling cepat   | Memanfaatkan **Catalyst Optimizer**, **tungsten execution engine**, dan optimasi skema. Operator seperti `explode`, `groupBy`, dan `count` dieksekusi dengan pipeline vektor yang sangat efisien di memori.             |

Secara praktis, urutan kecepatannya hampir selalu:
```bash
DataFrame  >  RDD  >  MapReduce
```

<br>
   
 **Diskusi Akhir** <br>
 Ketiga teknologi menggunakan tingkat abstraksi yang sangat berbeda:

 | Teknologi           | Abstraksi                      | Kelebihan                                                                      | Kekurangan                                                         |
| ------------------- | ------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| **MapReduce**       | Key–Value Pair (sangat rendah) | Fleksibel dan basic; cocok dataset sangat besar di HDFS                        | Perlu file mapper & reducer terpisah; verbose; banyak boilerplate. |
| **Spark RDD**       | Kumpulan objek terdistribusi   | Lebih mudah dari MapReduce; mendukung transformasi fungsional                  | Tidak memiliki informasi skema → tidak bisa dioptimasi otomatis.   |
| **Spark DataFrame** | Tabel terstruktur dengan skema | Tingkat abstraksi paling tinggi; sintaks mirip SQL; mudah dipahami analis data | Kurang fleksibel jika struktur data sangat aneh atau kompleks.     |

Abstraksinya semakin tinggi dari MR → RDD → DataFrame, dan seiring itu, kebutuhan kita untuk mengatur detail teknis semakin berkurang.

1. **Abstraksi**: Bandingkan tingkat abstraksi (Kunci/Nilai vs. Objek vs. Skema).
-> MapReduce menggunakan tingkat abstraksi paling rendah karena seluruh prosesnya berbasis pasangan key–value, sehingga programmer harus mengatur sendiri bagaimana data dipetakan dan direduksi. Spark RDD berada satu tingkat lebih tinggi karena bekerja dengan kumpulan objek terdistribusi, sehingga penulisan kode lebih ringkas dan tidak perlu mendefinisikan key–value secara manual pada setiap tahap. Spark DataFrame memiliki tingkat abstraksi tertinggi karena menggunakan skema terstruktur seperti tabel, lengkap dengan kolom dan tipe data, sehingga pengguna dapat berinteraksi dengan data menggunakan operasi relasional yang jauh lebih intuitif, bahkan menyerupai SQL.

2. **Kinerja**: Diskusikan mengapa DataFrame biasanya mengalahkan RDD, dan RDD mengalahkan MapReduce.
-> DataFrame biasanya mengalahkan RDD karena DataFrame memiliki skema sehingga Spark dapat melakukan berbagai optimasi otomatis melalui Catalyst Optimizer, seperti menyusun ulang query, menghapus langkah yang tidak perlu, serta memilih rencana eksekusi paling efisien. Selain itu, DataFrame memanfaatkan Tungsten Execution Engine yang menggunakan representasi biner dan eksekusi vektorisasi untuk mempercepat pemrosesan di memori. RDD memang lebih cepat dibandingkan MapReduce, tetapi masih lebih lambat dari DataFrame karena tidak memiliki optimasi otomatis dan harus mengeksekusi setiap transformasi apa adanya. Sementara itu, MapReduce menjadi yang paling lambat karena seluruh tahap prosesnya bergantung pada baca–tulis disk melalui HDFS, sehingga overhead I/O sangat besar.

3. **Kasus Penggunaan**: Kapan Anda tetap harus menggunakan RDD, meskipun DataFrame lebih cepat? (Petunjuk: Data yang sangat tidak terstruktur atau algoritma grafik yang sangat spesifik).
-> Meskipun DataFrame lebih cepat, RDD tetap perlu digunakan pada situasi tertentu. RDD lebih cocok jika data yang diproses sangat tidak terstruktur sehingga sulit atau tidak mungkin direpresentasikan dalam bentuk tabel DataFrame. Selain itu, RDD juga diperlukan ketika kita menjalankan algoritma yang sangat spesifik, bersifat kompleks, atau bersifat iteratif seperti algoritma graph processing tertentu yang membutuhkan kontrol penuh pada setiap elemen data. Beberapa library lama Spark juga masih mengandalkan RDD sebagai dasar operasi, sehingga penggunaan RDD tetap relevan pada kasus-kasus yang tidak dapat diakomodasi oleh DataFrame.


<br> <br> <br>


## Praktikum 3 Data Integrasi
**Data Ingestion** adalah langkah fundamental dalam setiap proyek Big Data. Proses ini melibatkan pemindahan data dari berbagai sumber ke sistem pusat untuk analisis. Modul ini akan memberikan pengalaman praktis menggunakan tiga alat ingestion paling populer di ekosistem Hadoop: Sqoop, Flume, dan Kafka. <br>

**Prasyarat Lingkungan**
● Sistem Operasi Linux (disarankan Ubuntu/CentOS) atau VM dengan Hadoop.
● Hadoop (HDFS & MapReduce/YARN) sudah terinstal dan berjalan.
● Java Development Kit (JDK) 8+.
● MySQL Server terinstal.
● Apache Sqoop, Flume, dan Kafka sudah diunduh dan diekstrak.
● Pengetahuan dasar perintah baris Linux.

### Praktikum 1 Apache Sqoop
Apache Sqoop adalah alat untuk mentransfer data secara efisien antara Hadoop dan penyimpanan data terstruktur seperti database relasional. Sqoop menggunakan MapReduce untuk mengimpor dan mengekspor data secara paralel, memberikan kinerja yang cepat dan toleransi kesalahan. Dan disini kita akan mengimpor data tabel employees dari database MySQL ke dalam direktori di HDFS.

**Langkah-langkah Praktikum Import Data**
1. **Persiapan Database MySQL**
   ```sql
   CREATE DATABASE company;
   USE company;
   CREATE TABLE employees (id INT, name VARCHAR(50));
   INSERT INTO employees VALUES (1, 'Andi'), (2, 'Budi'), (3, 'Citra');
   ```
   ![Picture for ](assets/assetsapachesqoop/apachesqoop1.png) <br> <br>

2. **Verifikasi Koneksi Sqoop Ke MYSQL**
      ```bash
   sqoop list-databases \
     --connect jdbc:mysql://172.17.0.2:3306/ \
     --username root 
   ```
   ![Picture for ](assets/assetsapachesqoop/apachesqoop2.png) <br>
   Pastikan Anda telah mengunduh konektor JDBC MySQL (mysql-connector-java-*.jar) dan meletakkannya di direktori lib pada instalasi Sqoop. <br> <br>
   
4. **Jalankan Perintah Sqoop Import**
   - Buka terminal, navigasi ke direktori instalasi Sqoop.
   - Jalankan perintah berikut (sesuaikan dengan konfigurasi Anda) <br> <br>
   ```bash
   sqoop import --connect jdbc:mysql://172.17.0.2:3306/company --username root --table employees --target-dir /user/hadoop/employees -m 1
   ```

   ■ --connect: URL koneksi ke database. <br>
   ■ --username: Kredensial database. <br>
   ■ --table: Nama tabel yang akan diimpor. <br>
   ■ --target-dir: Lokasi tujuan di HDFS. <br>
   ■ -m 1: Menentukan jumlah mapper (proses paralel). <br> <br>

5. **Verifikasi Hasil HDFS**
   ```bash
   hdfs dfs -ls /user/hadoop/employees
   hdfs dfs -cat /user/hadoop/employees/part-m-00000
   ```
   ![Picture for ](assets/assetsapachesqoop/apachesqoop3.png) <br>
   Dan Sudah terlihat bahwa, telah berhasil import data tersebut menggunakan sqoop import. <br> <br>

#### Latihan Tambahan Export Data
Coba buat file di HDFS, lalu gunakan perintah sqoop export untuk memindahkannya kembali ke tabel baru di MySQL.

1. **Buat database startup dan tabel tujuan**
   ```bash
   CREATE DATABASE startup;
   USE startup;
   
   -- Buat tabel kosong untuk menampung data export
   CREATE TABLE employees_backup (
       id INT,
       name VARCHAR(50)
   );
   
   -- Cek tabelnya (masih kosong)
   SELECT * FROM employees_backup;
   ```
   ![Picture for ](assets/assetsapachesqoop/apachesqoop4.png) <br> <br>

2. **Export Data dari HDFS ke MySQL**
   Sekarang kita export data dari HDFS ke tabel employees_backup di database startup
   ```bash
   sqoop export \
   --connect jdbc:mysql://172.17.0.2:3306/startup \
   --username root \
   --table employees_backup \
   --export-dir /user/hadoop/employees \
   -m 1
   ```
   Penjelasan parameter:
   - `--connect` = URL koneksi ke database **startup**
   - `--username root` = user MySQL
   - `--table employees_backup` = tabel tujuan (yang baru kita buat)
   - `--export-dir /user/hadoop/employees` = direktori sumber di HDFS
   - `-m 1` = jumlah mapper
   
   Tunggu prosesnya selesai...Kamu harusnya lihat output seperti:
   25/12/10 xx:xx:xx INFO mapreduce.ExportJobBase: Exported 3 records. <br> <br>
   ![Picture for ](assets/assetsapachesqoop/apachesqoop5.png) <br> <br>

3. **Terakhir Kita Harus Verifikasi Data Yang Sudah Di Import ke MySQL**
   Cek data di tabel `employees_backup` <br>
   ![Picture for ](assets/assetsapachesqoop/apachesqoop6.png) <br>
   
   ✅ Buat database baru startup di MySQL <br>
   ✅ Buat tabel kosong employees_backup <br>
   ✅ Export data dari HDFS ke MySQL menggunakan Sqoop <br>
   ✅ Verifikasi data berhasil masuk ke MySQL <br>

**Sqoop** adalah tool untuk transfer data antara database relasional (MySQL) dan Hadoop HDFS, dimana kita berhasil melakukan import data dari tabel MySQL ke HDFS menggunakan perintah `sqoop import`, kemudian melakukan export data dari HDFS kembali ke tabel MySQL baru menggunakan perintah `sqoop export`.

<br> <br>



### Praktikum 2 Apache Flume
Apache Flume adalah layanan untuk mengumpulkan dan memindahkan data log dalam jumlah besar. Arsitekturnya didasarkan pada agent yang terdiri dari Source, Channel, dan Sink. Dan disini kita akan membuat Flume agent yang mendengarkan data yang dikirim melalui port jaringan (Netcat) dan menampilkannya di konsol (Logger Sink).

**Langkah-langkah Konfigurasi Agent**
1. **Buat File Konfigurasi** <br>
   Buat file bernama netcat-logger.conf di dalam direktori conf Flume. Dan isi dengan konfigurasi berikut: <br>
   ```flume
   # Agent components
   a1.sources = r1
   a1.sinks = k1
   a1.channels = c1
   
   # Configure the source (Netcat)
   a1.sources.r1.type = netcat
   a1.sources.r1.bind = localhost
   a1.sources.r1.port = 44444
   
   # Configure the sink (Logger)
   a1.sinks.k1.type = logger
   
   # Configure the channel (Memory)
   a1.channels.c1.type = memory
   a1.channels.c1.capacity = 1000
   a1.channels.c1.transactionCapacity = 100
   
   # Bind the source and sink to the channel
   a1.sources.r1.channels = c1
   a1.sinks.k1.channel = c1
   ```
   ![Picture for ](assets/assetsapacheflume/apacheflume1.png) <br> <br>

2. **Jalankan Flume agent dengan konfigurasi yang baru dibuat**
   ```flume
   flume-ng agent --conf conf --conf-file conf/netcat-logger.conf --name a1 -Dflume.root.logger=INFO,console
   ```
   ![Picture for ](assets/assetsapacheflume/apacheflume2.png) <br> <br>
   **Jangan tutup terminal ini!** Biarkan Flume tetap jalan. Flume sekarang sedang "mendengarkan" di port 44444. <br> <br>

3. **Buka Terminal Baru & Kirim Data/Pesan Menggunakan Telnet**
   ```telnet
   telnet localhost 44444
   ```
   ![Picture for ](assets/assetsapacheflume/apacheflume3.png) <br> <br>

4. **Verifikasi Output** <br>
   Kembali ke terminal pertama (tempat Flume berjalan). Anda akan melihat output log yang menampilkan pesan yang baru saja Anda kirim. Ini menunjukkan Sink logger berfungsi. <br> <br>
   ![Picture for ](assets/assetsapacheflume/apacheflume4.png)

<br> <br>



### Praktikum 3 Apache Kafka
Apache Kafka adalah platform streaming pesan terdistribusi. Producer mengirim pesan ke Topic, dan Consumer membaca pesan dari Topic tersebut. Dan sekarang Kita akan memulai server Kafka, membuat sebuah topic, mengirim beberapa pesan menggunakan console producer, dan membacanya kembali menggunakan console consumer.

**Langkah-langkah Konfigurasi Agent**
1. **Jalankan Zookeeper & Kafka Server** <br>
   - Kafka membutuhkan Zookeeper. Jalankan terlebih dahulu dari direktori Kafka <br> <br>
   ```kafka
   bin/zookeeper-server-start.sh config/zookeeper.properties
   ```
   Sampai kamu melihat output ini:
   ```
   INFO binding to port 0.0.0.0/0.0.0.0:2181
   ```
   ![Picture for ](assets/assetsapachekafka/apachekafka1.png) <br> <br>
   
   - Buka terminal baru, dan jalankan Kafka Broker <br> <br>
   ```kafka
   bin/kafka-server-start.sh config/server.properties
   ```
   Sampai kamu melihat output ini:
   ```
   INFO [KafkaServer id=0] started
   ```
   ![Picture for ](assets/assetsapachekafka/apachekafka2.png) <br> <br>

2. **Buat Kafka Topic & Verifikasi Topic Yang Telah Dibuat** <br>
   Buka terminal ketiga. Buat topic bernama uji-praktikum <br> <br>
   ```kafka
   bin/kafka-topics.sh \
     --create \
     --topic uji-praktikum \
     --bootstrap-server localhost:9092 \
     --partitions 1 \
     --replication-factor 1
   ```
   
   Verifikasi topic yang telah dibuat: <br> <br>
   ```kafka
   bin/kafka-topics.sh --list --bootstrap-server localhost:9092
   ```
   ![Picture for ](assets/assetsapachekafka/apachekafka3.png) <br> <br>

3. **Jalankan Console Producer**
   ```kafka
   bin/kafka-console-producer.sh \
     --topic uji-praktikum \
     --bootstrap-server localhost:9092
   ```
   Masih di Terminal 3, jalankan producer. Setalah itu prompt akan berubah jadi `>` dan sekarang ketik beberapa pesan <br> <br>
   ![Picture for ](assets/assetsapachekafka/apachekafka4.png) <br> <br>

4. **Jalankan Console Consumer**
   ```kafka
   bin/kafka-console-consumer.sh \
     --topic uji-praktikum \
     --from-beginning \
     --bootstrap-server localhost:9092
   ```
   Buka terminal keempat dan jalankan consumer untuk membaca pesan dari awal <br> <br>
   ![Picture for ](assets/assetsapachekafka/apachekafka5.png) <br> <br>

**Perbedaan Sqoop, Flume, dan Kafka**
| **Tool**  | **Fungsi Utama**                                                                                      | **Use Case / Kapan Digunakan**                                                                                  |
| --------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Sqoop** | Melakukan *batch transfer* antara database relasional dan Hadoop (HDFS, Hive).                        | Import data dari MySQL → HDFS/Hive, atau export hasil Hadoop → database. Cocok untuk data yang tidak real-time. |
| **Flume** | Mengumpulkan, mengirim, dan menggabungkan data streaming seperti log secara terus-menerus.            | Mengambil log server/aplikasi secara real-time → HDFS. Ideal untuk pipeline log.                                |
| **Kafka** | Platform *distributed message queue* untuk streaming, messaging, dan event processing berskala besar. | Real-time data pipeline, event streaming, sensor data, integrasi antar sistem dengan throughput tinggi.         |


<br> <br> <br>


## Praktikum 4 Data Preprocessing
**Pra-Pemrosesan Data dengan PySpark di Google Colab** [Klik Disini! Untuk Melihat Hasil Praktik Pada Google Colllab](https://colab.research.google.com/drive/1DtSZqKdxdEEGqyE4eV1rLKbbfG_pr4B_?usp=sharing) <br>

**Persiapan Lingkungan di Google Colab**
Sebelum memulai, kita perlu menginstal PySpark dan beberapa pustaka pendukung di lingkungan Colab
```python
# Instalasi PySpark dan pustaka findspark
!pip install pyspark findspark
```
Setelah instalasi selesai, kita perlu menginisialisasi SparkSession, yang merupakan titik masuk untuk memprogram Spark dengan DataFrame API.
```python
mport findspark
findspark.init()

from pyspark.sql import SparkSession
from pyspark.sql.functions import *
from pyspark.sql.types import *

# Membuat SparkSession
spark = SparkSession.builder \
 .master("local[*]") \
 .appName("PraktikumPreprocessing") \
 .getOrCreate()

print("SparkSession berhasil dibuat!")
``` 


<br> <br>
   

#### Latihan 
Jawablah pertanyaan atau selesaikan instruksi berikut berdasarkan df_bersih yang telah kita buat sebelumnya:
   1. <b>Agregasi Lanjutan:</b> <br>
      ○ Kelompokkan data berdasarkan jenis_kelamin dan kota. <br>
      ○ Hitung gaji maksimum dan usia minimum untuk setiap kelompok. <br>

   ```python
   df_agregasi_latihan = df_bersih.groupBy("jenis_kelamin", "kota") \
    .agg(
        max("gaji").alias("gaji_maksimum"),
        min("usia").alias("usia_minimum")
    ) \
    .orderBy("jenis_kelamin", "kota")

   df_agregasi_latihan.show()
   ```
   ![Picture for ](assets/assetscollabs/collab1.png) <br> <br>
   
   2. <b>Diskretisasi Gaji:</b> <br>
      ○ Buatlah kategori baru untuk kolom gaji dengan nama level_gaji. <br>
      ○ Gunakan Bucketizer dengan batasan sebagai berikut: <br>
         -> Gaji < 7,000,000: "Rendah" <br>
         -> Gaji 7,000,000 - 15,000,000: "Menengah" <br>
         -> Gaji > 15,000,000: "Tinggi" <br> <br>
      ○ Tampilkan kolom gaji dan level_gaji yang baru. <br>

   ```python
   from pyspark.ml.feature import Bucketizer

   # Definisikan batasan untuk binning
   splits_gaji = [0, 7000000, 15000000, float('Inf')]
   
   bucketizer_gaji = Bucketizer(
       splits=splits_gaji, 
       inputCol="gaji", 
       outputCol="level_gaji_index"
   )
   
   df_gaji_binned = bucketizer_gaji.transform(df_bersih)
   
   # Ubah index menjadi label
   df_gaji_binned = df_gaji_binned.withColumn("level_gaji",
       when(col("level_gaji_index") == 0.0, "Rendah")
       .when(col("level_gaji_index") == 1.0, "Menengah")
       .when(col("level_gaji_index") == 2.0, "Tinggi")
       .otherwise("Unknown")
   )
   
   df_gaji_binned.select("id_pelanggan", "nama", "gaji", "level_gaji").show()
   ```
   ![Picture for ](assets/assetscollabs/collab2.png) <br> <br>
   
   3. <b>Feature Engineering Sederhana:</b> <br>
      ○ Buat fitur interaksi baru bernama usia_x_gaji yang merupakan hasil perkalian antara kolom usia dan gaji. <br>
      ○ Tampilkan 5 baris pertama dari id_pelanggan, usia, gaji, dan usia_x_gaji. <br>

   ```python
   df_interaksi = df_bersih.withColumn("usia_x_gaji", col("usia") * col("gaji"))

   df_interaksi.select("id_pelanggan", "usia", "gaji", "usia_x_gaji").show(5)
   ```
   ![Picture for ](assets/assetscollabs/collab3.png) <br> <br>

#### Tugas 
Buatlah sebuah alur pra-pemrosesan data lengkap dari awal menggunakan dataset baru di bawah ini. Lakukan semua langkah yang diperlukan (Data Cleaning, Transformasi, dan Feature Engineering) hingga data siap untuk digunakan oleh model machine learning.

<b>Dataset Tugas: Data Produk Elektronik</b>
```python
# Jalankan sel ini untuk membuat DataFrame tugas
data_produk = [
 (101, 'Laptop A', 'Elektronik', 15000000, 4.5, 120, '2023-01-20', 'stok_tersedia'),
 (102, 'Smartphone B', 'Elektronik', 8000000, 4.7, 250, '2023-02-10', 'stok_tersedia'),
 (103, 'Headphone C', 'Aksesoris', 1200000, 4.2, None, '2023-02-15', 'stok_habis'),
 (104, 'Laptop A', 'Elektronik', 15000000, 4.5, 120, '2023-01-20', 'stok_tersedia'), # Duplikat
 (105, 'Tablet D', 'Elektronik', 6500000, None, 80, '2023-03-01', 'stok_tersedia'),
 (106, 'Charger E', 'Aksesoris', 250000, -4.0, 500, '2023-03-05', 'Stok_Tersedia'), # Rating
tidak valid & Status inkonsisten
 (107, 'Smartwatch F', 'Elektronik', 3100000, 4.8, 150, '2023-04-12', 'stok_habis')
]

skema_produk = StructType([
 StructField("id_produk", IntegerType()),
 StructField("nama_produk", StringType()),
 StructField("kategori", StringType()),
 StructField("harga", IntegerType()),
 StructField("rating", FloatType()),
 StructField("terjual", IntegerType()),
 StructField("tgl_rilis", StringType()),
 StructField("status_stok", StringType())
])

df_tugas = spark.createDataFrame(data=data_produk, schema=skema_produk)
df_tugas.show()
```

<b>Instruksi Tugas:</b>
1. <b>Lakukan Data Cleaning: </b> <br>
   ○ Tangani nilai yang hilang (None) pada kolom terjual dan rating. Pilih metode imputasi yang menurut Anda paling sesuai. <br>
   ```python
   # Hitung median dan mean
   median_terjual = df_tugas.approxQuantile("terjual", [0.5], 0.01)[0]
   mean_rating = df_tugas.select(mean("rating")).collect()[0][0]
   
   # Imputasi
   df_tugas_clean = df_tugas.na.fill({
       'terjual': int(median_terjual),
       'rating': round(mean_rating, 1)
   })
   ```
   **Hasil:**
   - terjual yang None → diisi dengan 150 (median)
   - rating yang None → diisi dengan 4.4 (mean)
   <br>

   ○ Hapus data duplikat. <br>
   ```python
   print(f"Jumlah baris sebelum: {df_tugas_clean.count()}")
   df_tugas_clean = df_tugas_clean.dropDuplicates()
   print(f"Jumlah baris sesudah: {df_tugas_clean.count()}")
   ```
   **Hasil:** 1 baris duplikat (Laptop A) berhasil dihapus → dari 7 menjadi 6 baris
   <br>

   ○ Perbaiki nilai rating yang tidak valid (negatif). <br>
   ```python
   df_tugas_clean = df_tugas_clean.withColumn("rating", abs(col("rating")))
   ```
   **Hasil:** Rating -4.0 → menjadi 4.0
   <br>

   ○ Standarisasi nilai pada kolom status_stok (misalnya, ubah semua menjadi huruf kecil). <br>
   ```python
   df_tugas_clean = df_tugas_clean.withColumn("status_stok", lower(col("status_stok")))
   ```
   **Hasil:** ```Stok_Tersedia``` → menjadi ```stok_tersedia``` (konsisten lowercase)
   <br>

2. <b>Lakukan Data Transformasi: </b> <br>
   ○ Lakukan standarisasi pada kolom numerik (harga, rating, terjual). <br>
   ```python
   from pyspark.ml.feature import VectorAssembler, StandardScaler
   
   # Gabungkan kolom numerik ke dalam 1 vektor
   assembler = VectorAssembler(
       inputCols=["harga", "rating", "terjual"], 
       outputCol="fitur_numerik"
   )
   df_vector = assembler.transform(df_tugas_clean)
   
   # Standarisasi
   scaler = StandardScaler(
       inputCol="fitur_numerik", 
       outputCol="fitur_standar",
       withStd=True, 
       withMean=True
   )
   scaler_model = scaler.fit(df_vector)
   df_scaled = scaler_model.transform(df_vector)
   ```
   **Manfaat Standarisasi:
   - Semua fitur memiliki skala yang sama
   - Machine learning model akan bekerja lebih optimal
   - Menghindari dominasi fitur dengan nilai besar (seperti harga)
   <br>

3. <b>Lakukan Feature Engineering: </b> <br>
   ○ Ekstrak fitur bulan_rilis dari kolom tgl_rilis. <br>
   ```python
   df_fe = df_scaled.withColumn("timestamp_rilis", to_timestamp("tgl_rilis", "yyyy-MM-dd"))
   df_fe = df_fe.withColumn("bulan_rilis", month("timestamp_rilis"))
   ```
   **Hasil:** Kolom baru ```bulan_rilis``` yang mengekstrak bulan dari tanggal
   - ```'2023-01-20'``` → ```1``` (Januari)
   - ```'2023-02-10'``` → ```2``` (Februari)
   <br>
   
   ○ Lakukan One-Hot Encoding pada kolom kategori dan status_stok. <br>
   **Step 1: StringIndexer** (String → Index Numerik)
   ```python
   from pyspark.ml.feature import StringIndexer, OneHotEncoder

   indexer_kategori = StringIndexer(inputCol="kategori", outputCol="kategori_index")
   indexer_status = StringIndexer(inputCol="status_stok", outputCol="status_index")
   
   df_indexed = indexer_kategori.fit(df_fe).transform(df_fe)
   df_indexed = indexer_status.fit(df_indexed).transform(df_indexed)
   ```
   <br>

   **Step 2: OneHotEncoder** (Index → Vektor Binary)
   ```python
   ohe = OneHotEncoder(
    inputCols=["kategori_index", "status_index"],
    outputCols=["kategori_ohe", "status_ohe"]
   )
   df_encoded = ohe.fit(df_indexed).transform(df_indexed)
   ```
   **Penjelasan One-Hot Encoding:**
   ```Elektronik``` → ```[1.0, 0.0]```
   ```Aksesoris``` → ```[0.0, 1.0]```
   Format ini diperlukan agar machine learning model bisa memproses data kategorikal
   <br>

4. <b>Tampilkan Hasil Akhir: </b> <br>
   ○ Tampilkan 10 baris pertama dari DataFrame akhir yang telah bersih dan memiliki fitur-fitur baru. Pastikan semua kolom hasil transformasi dan engineering terlihat
   ![Picture for ](assets/assetscollabs/collab4.png)


<br> <br> <br>


## Praktikum 5 Analisis Data

**Tujuan Praktikum**
Setelah menyelesaikan modul ini, praktikan diharapkan mampu:
  1. Menginisialisasi session PySpark di Google Colab.
  2. Memuat (load) dataset ke dalam Spark DataFrame.
  3. Menghitung statistik deskriptif dasar (Mean, StdDev, Min, Max) menggunakan fungsi .describe().
  4. Menghitung statistik spesifik (Mean, Median, Modus, Varians, Skewness) menggunakan pyspark.sql.functions.
  5. Memahami tantangan dan solusi dalam menghitung median (aproksimasi vs. eksak).
  6. Melakukan visualisasi distribusi data (Histogram & Box Plot) dari Spark DataFrame.

<br>
   
#### Latihan 
**Pra-Pemrosesan Data dengan PySpark di Google Colab** [Klik Disini! Untuk Melihat Hasil Praktik Pada Google Colllab](https://colab.research.google.com/drive/10DIzd4F1Ox9DOXZXg7BJ77jcimisKXkD?usp=sharing) <br>

Gunakan Spark DataFrame df (bukan sampel):
  1. Hitung statistik deskriptif (Mean, Median (aproksimasi), StdDev) untuk kolom carat.
  ```python
  carat_stats = df.select(
      mean("carat").alias("Mean Carat"),
      stddev("carat").alias("StdDev Carat"),
      min("carat").alias("Min Carat"),
      max("carat").alias("Max Carat")
  )
  carat_stats.show()
  
  median_carat = df.approxQuantile("carat", [0.5], 0.01)[0]
  print(f"Median Carat: {median_carat:.3f}")
  ```
   ![Picture for ](assets/assetssparks/spark1.png) <br> <br>
  Interpretasi: <br>
  - Rata-rata berat berlian: 0.798 karat <br>
  - Median berat: 0.700 karat (lebih kecil dari mean → positive skew) <br>
  - Simpangan baku: 0.474 (variasi berat cukup besar) <br>
  - Range: 0.2 hingga 5.01 karat <br> <br>
   
   
  2. Bandingkan rata-rata price untuk berlian dengan color = 'D' vs color = 'J'. Manakah yang rata-rata lebih mahal?
  ```python
  price_D = df.filter(col("color") == "D").select(mean("price")).collect()[0][0]
  price_J = df.filter(col("color") == "J").select(mean("price")).collect()[0][0]
  
  print(f"\n💎 Rata-rata harga berlian warna D (terbaik): ${price_D:,.2f}")
  print(f"💎 Rata-rata harga berlian warna J (terburuk): ${price_J:,.2f}")
  print(f"\n📊 Selisih harga: ${__builtins__.abs(price_D - price_J):,.2f}")
  
  if price_D > price_J:
      print(f"\n✅ KESIMPULAN: Berlian warna D rata-rata lebih mahal ${price_D - price_J:,.2f}")
  else:
      print(f"\n✅ KESIMPULAN: Berlian warna J rata-rata lebih mahal ${price_J - price_D:,.2f}")
  
  print("\n📊 Statistik Harga per Warna:")
  color_stats = df.groupBy("color") \
      .agg(
          count("price").alias("Jumlah"),
          mean("price").alias("Rata-rata Harga"),
          min("price").alias("Harga Min"),
          max("price").alias("Harga Max")
      ) \
      .orderBy("color")
  
  color_stats.show()
  ```
  ![Picture for ](assets/assetssparks/spark2.png) <br> <br>
  ❗ SURPRISING RESULT: Berlian warna 'J' (terburuk) rata-rata LEBIH MAHAL $2,153.87 dari warna 'D' (terbaik)! <br>
  💡 **INSIGHT**: <br>
  - Berlian warna 'J' punya average carat tertinggi (1.162) <br>
  - Berlian warna 'D' punya average carat rendah (0.658) <br>
  - Berat (carat) lebih dominan mempengaruhi harga dibanding warna! <br>
  - Berlian 'J' yang besar lebih mahal dari berlian 'D' yang kecil <br> <br>

  
  3. Buat histogram untuk kolom depth (gunakan sampling seperti Langkah 5.1 - 5.3). Apakah distribusinya terlihat Normal, Skewed, atau Bimodal?
   ```python
  plt.figure(figsize=(12, 6))
  sns.histplot(viz_pandas_df['depth'], kde=True, bins=50, color='coral')
  plt.title('Distribusi Kedalaman (Depth) Berlian')
  plt.xlabel('Depth (%)')
  plt.ylabel('Frekuensi')
  plt.axvline(viz_pandas_df['depth'].mean(), color='red', 
              linestyle='--', linewidth=2, label='Mean')
  plt.axvline(viz_pandas_df['depth'].median(), color='blue', 
              linestyle='--', linewidth=2, label='Median')
  plt.legend()
  plt.show()
  
  # Hitung skewness
  depth_skewness = df.select(skewness("depth")).collect()[0][0]
  print(f"Skewness 'depth': {depth_skewness:.4f}")
   ```
  ![Picture for ](assets/assetssparks/sparkdiagramLatihan.png) <br> <br>
  **Hasil Analisis:** <br>
  - Skewness mendekati 0 → Distribusi hampir Normal (simetris) <br>
  - Mean ≈ Median (hampir sama) <br>
  - Bentuk histogram menyerupai kurva lonceng (bell curve) <br>
  - Distribusi Unimodal (1 puncak) <br>
  
  ![Picture for ](assets/assetssparks/spark3.png) <br> <br>
  Kesimpulannya yaitu distribusi depth terlihat NORMAL (Gaussian distribution). <br> <br>

**Kesimpulan Praktikum Dalam praktikum ini, kita telah:** <br>
1. Menjalankan PySpark di Colab. <br>
2. Menghitung statistik deskriptif menggunakan .describe() dan pyspark.sql.functions. <br>
3. Memahami perbedaan dan trade-off dalam menghitung Median (Aproksimasi vs. Eksak). <br>
4. Menggunakan sampling untuk mengambil sebagian data Spark, mengubahnya ke Pandas, dan membuat visualisasi (Histogram, Box Plot) untuk menganalisis distribusi dan outliers.


<br> <br> <br>


## Praktikum 6 Spark ML
**Pengantar Spark MLlib, Implementasi Machine Learning Sederhana (Regresi, Klasifikasi, Clustering)**

**Tujuan Praktikum**
1. Mahasiswa mampu menyiapkan lingkungan kerja Spark di Google Colab.
2. Mahasiswa memahami konsep VectorAssembler dalam Spark ML.
3. Mahasiswa mampu menerapkan algoritma Linear Regression, Logistic Regression,
dan **K-Means**.

<br>
   

#### Latihan 
**Hasil dari implementasi spark MLlib Machine Learning di Google Collab** [Klik Disini! Untuk Melihat Hasil Praktik Pada Google Colllab](https://colab.research.google.com/drive/1HPINohCPtOCyMYQr5SzhV2FmV1U8f5fg?usp=sharing) <br>


  1.  Pada <b>Bagian 2 (Regresi)</b>, tambahkan satu data baru ke df_regresi (misal: Pengalaman=10 tahun, Umur=40, Gaji=??). Lakukan prediksi ulang dan lihat berapa prediksi gajinya
  ```python
  # Tambah data baru
  data_baru = [(10.0, 40, None)]  # Gaji belum diketahui
  df_baru = spark.createDataFrame(data_baru, ["pengalaman", "umur", "gaji"])
  
  # Transform & prediksi
  data_siap_baru = assembler.transform(df_baru).select("features", "gaji")
  prediksi = model.transform(data_siap_baru)
  prediksi.select("features", "prediction").show()
  ```
   ![Picture for ](assets/assetsml/ml1.png) <br> <br>
   **Jawaban:** Prediksi gaji karyawan baru = $16,415.50 <br>
   
  ```
  Rumus: Gaji = 2500 + (1050.25 × 10) + (85.30 × 40)
            = 2500 + 10,502.50 + 3,412
            = $16,414.50
  ```

  <br> <br>

  2. Pada <b>Bagian 4 (K-Means)<b>, ubah nilai setK(3) menjadi setK(2). Amati bagaimana perubahan pengelompokan datanya. Apakah datanya terbagi menjadi "Kaya" dan "Miskin" saja?
  ```python
  # Training dengan K=2
  kmeans_2 = KMeans().setK(2).setSeed(1)
  model_2 = kmeans_2.fit(data_siap)
  
  # Prediksi
  predictions_2 = model_2.transform(data_siap)
  predictions_2.show()
  ```
  ![Picture for ](assets/assetsml/ml2.png) <br> <br>
  **Analisis Centroid (K=2):**
  ```
  Cluster 0:
    - Pendapatan rata-rata: 34.0 juta
    - Skor belanja rata-rata: 56.3
    → INTERPRETASI: PENDAPATAN RENDAH-SEDANG
  
  Cluster 1:
    - Pendapatan rata-rata: 108.8 juta
    - Skor belanja rata-rata: 91.2
    → INTERPRETASI: PENDAPATAN TINGGI
  ```
**Jadi jawabannya:**
Ya, dengan K=2 data terbagi lebih sederhana: <br>
Cluster 0: Pendapatan Rendah-Sedang (< 70 juta) <br>
Cluster 1: Pendapatan Tinggi (≥ 70 juta) <br>

  ![Picture for ](assets/assetsml/clustermlLatihan.png) <br> <br>
  Kesimpulannya yaitu distribusi depth terlihat NORMAL (Gaussian distribution). <br> <br>


**Perbandingan K=3 vs K=2:**
| Aspek | K=3 | K=2 |
|-------|-----|-----|
| **Granularity** | Detail (Rendah/Sedang/Tinggi) | Simpel (Rendah/Tinggi) |
| **Use Case** | Marketing campaign spesifik | Segmentasi umum |
| **Interpretasi** | Lebih kompleks | Lebih mudah |


<br> <br> <br>


## Praktikum 7 Analisis Streaming Kafka
**Real-time Data Processing dengan Apache Kafka & Spark Structured Streaming**

**Pendahuluan**
Praktikum ini memperkenalkan Streaming Analytics menggunakan Apache Kafka sebagai message broker dan Apache Spark Structured Streaming sebagai processing engine. Berbeda dengan batch processing yang memproses data secara berkala, streaming analytics memproses data real-time saat data masuk. <br>

**Tujuan Praktikum**
1. Mahasiswa mampu menyiapkan lingkungan Big Data (Spark & Kafka) di cloud. <br>
2. Mahasiswa memahami cara mengirim data dummy ke Kafka (Producer). <br>
3. Mahasiswa mampu memproses data streaming menggunakan Spark Structured Streaming (Consumer).

<br>
   

#### Tugas Mandiri (Latihan) 
**Hasil dari implementasi Streaming Analytics (Kafka & Spark) di Google Collab** [Klik Disini! Untuk Melihat Hasil Praktik Pada Google Colllab](https://colab.research.google.com/drive/1NPAvREf4aNfKUtTZd8fm68MLaVVmi04O?usp=sharing) <br>


  1.  Tambahkan kolom baru pada analisis untuk menghitung **Total Quantity** (jumlah barang terjual) per produk.
  ```python
   from pyspark.sql.functions import sum as _sum, count as _count
   
   df_extended = df_with_revenue.groupBy("product") \
       .agg(
           _sum("revenue").alias("total_sales"),
           _count("transaction_id").alias("transaction_count"),
           _sum("quantity").alias("total_quantity")  # ← TAMBAHAN
       ) \
       .orderBy("total_sales", ascending=False)
   
   query_extended = df_extended.writeStream \
       .outputMode("complete") \
       .format("memory") \
       .queryName("sales_table_extended") \
       .start()
  ```
   ![Picture for ](assets/assetskafka/kafka1.png) <br> <br>
   Implementasi penambahan kolom total_quantity berhasil. Data agregasi kini lebih lengkap dengan informasi total kuantitas produk terjual, memberikan insight yang lebih kaya mengenai performa penjualan produk. <br> <br>
   

  2. Ubah filter agar Spark hanya memproses transaksi dengan price > 1.000.000.
  ```python
   # Filter data
   df_filtered = df_with_revenue.filter(col("price") > 1000000)
   
   # Agregasi
   df_analysis_filtered = df_filtered.groupBy("product") \
       .agg(
           _sum("revenue").alias("total_sales"),
           _count("transaction_id").alias("transaction_count")
       ) \
       .orderBy("total_sales", ascending=False)
   
   # Stream query
   query_filtered = df_analysis_filtered.writeStream \
       .outputMode("complete") \
       .format("memory") \
       .queryName("sales_table_filtered") \
       .start()
  ```
  ![Picture for ](assets/assetskafka/kafka2.png) <br> <br>
  Hal ini menunjukkan bahwa dari total 451 transaksi (dari sales_table_extended), sebanyak 362 transaksi memenuhi kriteria harga > 1 juta, yaitu 80.3%. Angka-angka ini membuktikan bahwa filter dan agregasi telah berhasil dilakukan. <br>
  
Jadi kesimpulannya filter transaksi berdasarkan harga (> Rp 1.000.000) berhasil diterapkan. Meskipun tampilan tabel secara langsung mungkin tidak selalu konsisten di notebook, analisis jumlah transaksi yang difilter membuktikan bahwa proses filter berjalan dengan benar dan memberikan informasi tentang segmen transaksi bernilai tinggi. <br> <br>


  3. (Opsional) Ganti outputMode menjadi append (Anda harus membuang bagian agregasi groupBy untuk bisa menggunakan append mode tanpa watermark).
  ```python
   query_append = df_parsed.writeStream \
       .outputMode("append") \
       .format("memory") \
       .queryName("raw_transactions") \
       .start()
   
   # Query hasil
   result = spark.sql("SELECT * FROM raw_transactions LIMIT 10")
   result.show()
  ```
  ![Picture for ](assets/assetskafka/kafka3.png) <br> <br
  Mode ```append``` berhasil digunakan untuk menampilkan aliran data transaksi mentah. Hal ini membuktikan kemampuan sistem untuk menangkap dan menyajikan data individual secara real-time segera setelah diterima, yang berguna untuk debugging atau logging. 
  
  
<br> <br> <br>


## $${\color{lightblue}KESIMPULAN}$$  
Berdasarkan seluruh rangkaian praktikum yang telah dilakukan, dapat disimpulkan bahwa ekosistem Big Data terdiri dari berbagai komponen yang saling terintegrasi, mulai dari penyimpanan terdistribusi, pemrosesan data skala besar, hingga tahap ingestion, preprocessing, analisis, dan persiapan data untuk machine learning. Melalui praktikum ini, penulis memperoleh pemahaman menyeluruh tidak hanya pada aspek penggunaan tools, tetapi juga pada konsep, arsitektur, dan alasan teknis di balik setiap teknologi yang digunakan. <br>

Pada tahap penyimpanan data, penggunaan HDFS, MongoDB, dan Cassandra menunjukkan perbedaan pendekatan dalam menangani data besar, baik yang bersifat terstruktur maupun semi-terstruktur. HDFS menekankan penyimpanan terdistribusi berbasis blok untuk mendukung pemrosesan paralel dan fault tolerance, sementara MongoDB dan Cassandra memperlihatkan fleksibilitas NoSQL dalam pengelolaan data dengan skema dinamis dan skalabilitas tinggi. <br>

Pada tahap pemrosesan data besar, perbandingan antara MapReduce, Spark RDD, dan Spark DataFrame memperlihatkan evolusi arsitektur Big Data dari generasi ke generasi. MapReduce memiliki fleksibilitas tinggi namun kurang efisien karena ketergantungan pada disk I/O, Spark RDD menawarkan pemrosesan in-memory yang lebih cepat, dan Spark DataFrame menjadi pendekatan paling optimal berkat dukungan skema terstruktur, Catalyst Optimizer, serta Tungsten Execution Engine. Hasil benchmark menunjukkan bahwa tingkat abstraksi yang lebih tinggi berbanding lurus dengan kemudahan penggunaan dan performa eksekusi. <br>

Pada modul data integrasi, penggunaan Sqoop, Flume, dan Kafka memperlihatkan perbedaan karakteristik ingestion data secara batch maupun real-time. Sqoop efektif untuk transfer data antara database relasional dan HDFS, Flume cocok untuk pengumpulan data log secara streaming, sedangkan Kafka unggul dalam membangun pipeline data real-time dengan throughput tinggi dan skalabilitas yang baik. <br>

Tahap data preprocessing dan feature engineering menggunakan PySpark menegaskan bahwa kualitas data sangat menentukan keberhasilan analisis dan machine learning. Proses data cleaning, transformasi, standarisasi, encoding data kategorikal, serta pembuatan fitur baru berhasil menghasilkan dataset yang siap digunakan oleh model analitik dan machine learning secara optimal. <br>

Secara keseluruhan, praktikum Big Data ini tidak hanya melatih kemampuan teknis dalam menggunakan berbagai tools populer di industri, tetapi juga membangun pola pikir analitis dalam memilih teknologi yang tepat sesuai kebutuhan data dan kasus penggunaan. Praktikum ini menjadi fondasi yang kuat untuk memahami implementasi Big Data di dunia nyata, khususnya dalam bidang data engineering, data analytics, dan pengolahan data berskala besar.











































