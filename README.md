# Laporan Proyek Machine Learning - Indira Aline

## Project Overview

![Indonesia Tourism Destination]([https://blog.lancedb.com/content/images/2024/05/1_AatBvnpVpEPoQvZAMeqU-A.webp](https://www.balidiscovery.com/wp-content/uploads/2024/10/widiyanti3.jpg))

*Sumber: [Bali Discoverye](https://www.balidiscovery.com/ri-identifies-top-5-tourism-destinations/)*

Sistem rekomendasi telah menjadi bagian integral dari kehidupan digital kita, membantu pengguna menavigasi lautan informasi dan pilihan yang tersedia. Dari platform _e-commerce_ hingga layanan _streaming_, sistem ini berperan penting dalam meningkatkan pengalaman pengguna dengan menyajikan konten atau produk yang relevan dan personal.

Indonesia, dengan kekayaan budaya dan keindahan alamnya, menawarkan beragam destinasi wisata yang menarik. Namun, bagi wisatawan, baik domestik maupun mancanegara, menemukan destinasi yang sesuai dengan preferensi pribadi bisa menjadi tantangan. Informasi yang tersebar, pilihan yang melimpah, dan kurangnya rekomendasi yang dipersonalisasi seringkali menyulitkan proses perencanaan perjalanan. Tanpa panduan yang efektif, wisatawan mungkin melewatkan permata tersembunyi atau menghabiskan waktu pada destinasi yang tidak sesuai dengan minat mereka, yang pada akhirnya dapat mengurangi kepuasan pengalaman berwisata.[1](https://www.idpublications.org/wp-content/uploads/2017/01/Full-Paper-THE-DEVELOPMENT-OF-TOURISM-INDUSTRY-IN-INDONESIA.pdf)

Masalah ini dapat diselesaikan melalui pengembangan sistem rekomendasi destinasi wisata yang cerdas. Sistem ini akan menganalisis preferensi pengguna dan karakteristik destinasi untuk menyajikan rekomendasi yang paling relevan. Pendekatan ini tidak hanya akan mempermudah wisatawan dalam menemukan tempat-tempat menarik, tetapi juga akan berkontribusi pada peningkatan jumlah kunjungan ke berbagai destinasi wisata di Indonesia.[2](https://jbhost.org/jbhost/index.php/jbhost/article/view/170)


## Business Understanding

### Problem Statements

1. Bagaimana sistem dapat merekomendasikan destinasi yang belum pernah diketahui atau dipertimbangkan oleh wisatawan, namun memiliki potensi untuk disukai?
2. Bagaimana cara menyediakan rekomendasi destinasi wisata yang personal dan relevan bagi setiap individu berdasarkan preferensi unik mereka?

### Goals

1. Meningkatkan _Discovery_ Destinasi Baru: Membangun sistem yang tidak hanya merekomendasikan destinasi yang sudah populer, tetapi juga mampu mengidentifikasi dan menyajikan destinasi yang kurang dikenal namun memiliki potensi tinggi untuk disukai oleh pengguna berdasarkan pola minat yang serupa dari pengguna lain atau karakteristik destinasi itu sendiri.
2. Menyediakan Rekomendasi Destinasi Personal: Mengembangkan sistem yang mampu menganalisis data preferensi pengguna (misalnya, kategori wisata yang disukai, lokasi, _rating_ sebelumnya) dan karakteristik destinasi (misalnya, jenis wisata, fasilitas) untuk menyajikan rekomendasi yang sangat personal dan relevan bagi setiap individu.

    ### Solution statements
Sistem rekomendasi ini akan dibangun dengan menggabungkan kekuatan dari _Collaborative Filtering_ dan _Content-Based Filtering_ untuk memberikan rekomendasi yang komprehensif dan akurat.

**_Solution Approach 1: Collaborative Filtering_**
Pendekatan ini berfokus pada hubungan antar pengguna dan antar item. Sistem akan menganalisis preferensi dan perilaku pengguna lain yang serupa dengan pengguna saat ini untuk merekomendasikan item (destinasi wisata) yang disukai oleh pengguna-pengguna serupa tersebut. Ada dua sub-pendekatan utama dalam _Collaborative Filtering_:

- **_User-Based Collaborative Filtering_**: Mengidentifikasi pengguna dengan selera yang sama (misalnya, mereka memberi _rating_ serupa pada destinasi yang sama) dan merekomendasikan destinasi yang disukai oleh pengguna serupa tersebut kepada pengguna saat ini.
- **_Item-Based Collaborative Filtering_**: Mengidentifikasi item (destinasi) yang sering disukai bersama oleh banyak pengguna dan merekomendasikan item yang mirip dengan item yang sudah disukai pengguna saat ini

Keunggulan: Mampu merekomendasikan item yang belum pernah dilihat oleh pengguna (masalah _serendipity_) dan tidak memerlukan data deskriptif tentang item itu sendiri.

Keterbatasan: Memerlukan data _rating_ atau interaksi pengguna yang cukup banyak (_cold start problem_ untuk pengguna baru atau item baru) dan rentan terhadap sparsity data.

**_Solution Approach 2: Content-Based Filtering_**
Pendekatan ini merekomendasikan item (destinasi wisata) berdasarkan kemiripan antara fitur-fitur item tersebut dengan preferensi pengguna. Sistem akan membangun profil pengguna berdasarkan _rating_ atau interaksi sebelumnya dengan item dan kemudian merekomendasikan item baru yang memiliki fitur serupa dengan item yang disukai pengguna di masa lalu.

Keunggulan: Tidak mengalami masalah _cold start_ untuk item baru (jika ada deskripsi itemnya) dan mampu merekomendasikan item kepada pengguna baru yang belum memiliki riwayat interaksi yang banyak.

Keterbatasan: Cenderung merekomendasikan item yang serupa dengan apa yang sudah disukai pengguna (_lack of diversity_) dan memerlukan data deskriptif yang kaya tentang item.

## Data Understanding

**Informasi Datasets**

| Jenis | Keterangan |
| ------ | ------ |
| Title | _Indonesia Tourism Destination_ |
| Source | [Kaggle](https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination) |
| Maintainer | [A_Prabowo ⚡](https://www.kaggle.com/aprabowo) |
| License | Other (specified in description) |
| Visibility | Publik |
| Tags | _Beginner, Travel, Asia, Recommender Systems_ |
| Usability | 8.24 |

Dataset ini terdiri dari empat file CSV terpisah: `tourism_rating.csv`, `user.csv`, `tourism_with_id.csv`, dan `place.csv`. Secara keseluruhan, dataset ini mencakup informasi mengenai destinasi wisata di Indonesia, data pengguna, serta _rating_ yang diberikan oleh pengguna terhadap destinasi-destinasi tersebut. Kondisi data relatif bersih dan terstruktur, namun memerlukan beberapa langkah preprocessing seperti penanganan _missing values_ dan penggabungan tabel untuk mendapatkan data yang siap dianalisis.

Deskripsi Variabel (Fitur)
Berikut adalah uraian dari setiap variabel atau fitur yang terdapat dalam dataset:

`tourism_rating.csv`
File ini berisi informasi mengenai _rating_ yang diberikan oleh pengguna terhadap destinasi wisata.

**User_Id**: ID unik untuk setiap pengguna.
**Place_Id**: ID unik untuk setiap destinasi wisata.
**Place_Ratings**: _Rating_ yang diberikan oleh pengguna untuk destinasi wisata tertentu.

`user.csv`
File ini berisi informasi demografi pengguna.

**User_Id**: ID unik untuk setiap pengguna.
**Location**: Lokasi geografis asal pengguna (misalnya, provinsi atau kota).
**Age**: Usia pengguna.

## Exploratory Data Analysis (EDA)

![genres](https://github.com/user-attachments/assets/048c7314-f522-4c5f-addd-c0df6f73f2b5)

Gambar 1a. Analisis Genre

Pada gambar 1a. Analisis Genre ditemukan ada 20 genre film dalam dataset. Beberapa genre yang paling banyak muncul adalah Drama, Comedy, Action, Thriller, Adventure. Ada juga kategori '(no genres listed)' yang perlu diperhatikan dalam pengolahan data selanjutnya.

![dekade](https://github.com/user-attachments/assets/3eb1034f-63c3-4666-8b05-e815cea93e13)

Gambar 1b. Distribusi Film Berdasarkan Dekade

Gambar 1b. Distribusi Film Berdasarkan Dekade tersebut menunjukkan tren pertumbuhan eksponensial dalam produksi film, terutama pada dua dekade terakhir. Perkembangan teknologi dan globalisasi industri film kemungkinan menjadi faktor utama di balik tren ini.

![outlier](https://github.com/user-attachments/assets/c116870b-df74-491d-8b86-033c99727393)

Gambar 1c. Outlier 

Di gambar 1c. kita dapat melihat bahwa ada _outlier_ ekstrem dan beberapa _outlier_ kecil dalam dataset, dan mereka sangat ekstrem, sehingga akan memengaruhi kinerja model. saya perlu menghilangkannya. Saya akan menghapus apa pun yang lebih tinggi dari 1000.

![nooutlier](https://github.com/user-attachments/assets/4e3da080-e45b-447a-b78e-11f67a6aac13)

Gambar 1d. No Outlier

Data terlihat lebih baik sekarang, meski masih ada banyak _outlier_. saya akan mencoba memakainya dulu. Sebelum menggunakan data film yang sudah dibersihkan, saya akan mengubah kolom `genre` ke _one-hot encoding_ karena format _List_ (Daftar) lebih mudah untuk visualisasi, tapi kurang praktis untuk perhitungan komputer.

## Data Preparation
Tahapan _Data Preparation_ dalam proyek ini melibatkan beberapa langkah penting untuk memastikan data siap dianalisis dan digunakan dalam model rekomendasi. Langkah-langkah ini meliputi:

1. **Data Loading**

- Memuat _dataset_ film dan _rating_ menggunakan _library_ Pandas.
- Tujuannya adalah untuk membaca _dataset_ ke dalam format DataFrame yang mudah dimanipulasi dan dianalisis.

2. **Exploratory Data Analysis - EDA**

- Menganalisis kolom `genres` pada _dataset_ film untuk memahami distribusi dan variasi genre film.
- Memisahkan _string_ genre yang digabungkan menjadi list individual dan menghitung frekuensi kemunculan setiap genre.
- Memvisualisasikan distribusi genre menggunakan _bar chart_ untuk mendapatkan gambaran jelas tentang genre mana yang paling umum.
- Tujuannya adalah untuk memahami karakteristik data sebelum pemrosesan lebih lanjut, terutama dalam hal genre film.

3. **Ekstraksi Fitur dari Kolom Teks**

- Memproses kolom `title` untuk mengekstrak informasi tahun rilis film.
- Memisahkan tahun dari judul film dan menyimpannya dalam kolom baru bernama `years`.
- Tujuannya adalah untuk mengubah data tekstual menjadi data numerik yang lebih mudah diolah oleh model.

4. **Konversi Data Kategorikal**

- Mengubah kolom `genres` menjadi representasi `one-hot encoding`.
- Memisahkan setiap genre dari daftar genre film dan membuat kolom biner baru untuk setiap genre.
- Tujuannya adalah untuk mengubah data kategorikal menjadi format numerik yang sesuai untuk input model _machine learning_.

5. **Penggabungan dan Pembersihan Data**

- Menggabungkan _dataset_ film dan rating berdasarkan `movieId`.
- Menghapus kolom-kolom yang tidak relevan seperti `title`, `genres`, dan `timestamp` untuk menyederhanakan dataset.
- Tujuannya adalah untuk menyatukan informasi film dan _rating_, serta membersihkan data dari informasi yang tidak diperlukan untuk pemodelan.

6. **Pembagian Data**

- Membagi _dataset_ yang telah disiapkan menjadi data latih (_train_) dan data uji (_test_).
- Pembagian ini dilakukan untuk mengevaluasi kinerja model pada data yang belum pernah dilihat sebelumnya.
- Tujuannya adalah untuk mengukur kemampuan generalisasi model dan menghindari _overfitting_.

## Modeling
Dalam proyek ini, digunakan algoritma _clustering_ untuk mengelompokkan pengguna berdasarkan preferensi film mereka, yang kemudian digunakan untuk memprediksi _rating_ film. Berikut adalah rincian tahapan _modeling_ yang dilakukan:

1. **Pembuatan Model dengan Algoritma Clustering**

Dua algoritma _clustering_ utama yang dieksplorasi adalah _K-Means Clustering_ dan _Agglomerative Clustering_. Algoritma ini digunakan untuk mengelompokkan pengguna ke dalam beberapa _cluster_ berdasarkan _rating_ film mereka. Tujuan dari _clustering_ adalah untuk mengidentifikasi kelompok pengguna dengan preferensi film yang serupa.

2. **Pemilihan Jumlah Cluster Terbaik**

Eksperimen dilakukan dengan berbagai jumlah _cluster_ untuk menentukan konfigurasi yang menghasilkan kinerja terbaik. Jumlah _cluster_ yang optimal dipilih berdasarkan nilai RMSE terendah pada data uji, yang menunjukkan prediksi _rating_ yang paling akurat.

3. **Rekomendasi Top-N**

Setelah model _clustering_ dilatih dan dievaluasi, model digunakan untuk menghasilkan rekomendasi _top-N_ film untuk setiap pengguna.
Rekomendasi _top-N_ adalah daftar _N_ film teratas yang diprediksi akan disukai oleh pengguna, berdasarkan _cluster_ mereka dan _rating_ rata-rata film dalam _cluster_ tersebut. Output dari tahapan ini adalah daftar film yang direkomendasikan untuk setiap pengguna.

**Dua Solusi Rekomendasi dengan Algoritma yang Berbeda**

Dalam proyek ini, dua algoritma _clustering_ dieksplorasi sebagai solusi rekomendasi:

1. **_K-Means Clustering_**
Algoritma ini membagi pengguna ke dalam k _cluster_, di mana setiap pengguna termasuk dalam _cluster_ dengan rata-rata terdekat.

- Kelebihan:

    - Skalabilitas: _K-Means_ efisien dalam menangani _dataset_ besar.
    - Implementasi yang Mudah: Relatif mudah diimplementasikan dan dipahami.

- Kekurangan:

    - Sensitif terhadap Inisialisasi: Hasil dapat bervariasi tergantung pada pemilihan centroid awal.
    - Asumsi _Cluster_ Berbentuk Bulat: Kurang efektif jika _cluster_ memiliki bentuk yang kompleks atau non-bulat.

2. **_Agglomerative Clustering_**
Algoritma ini membangun hierarki _cluster_ dengan menggabungkan _cluster-cluster_ yang paling mirip secara iteratif.

- Kelebihan:

    - Tidak Membutuhkan Spesifikasi Jumlah _Cluster_ Awal: Jumlah _cluster_ dapat ditentukan secara dinamis.
    - Informasi Hierarkis: Menyediakan struktur hierarkis yang kaya tentang hubungan antar data.

- Kekurangan:

    - Komputasi yang Mahal: Bisa menjadi sangat lambat untuk _dataset_ besar.
    - Sensitif terhadap _Noise_: _Noise_ dalam data dapat memengaruhi pembentukan _cluster_.

Kedua algoritma ini memiliki kelebihan dan kekurangan masing-masing. Pemilihan algoritma yang tepat tergantung pada karakteristik data dan kebutuhan spesifik proyek. Dalam _notebook_, kedua algoritma dievaluasi dan dibandingkan untuk menentukan solusi terbaik untuk sistem rekomendasi film.

## Evaluation

Dalam proyek ini, metrik evaluasi yang digunakan adalah _Root Mean Squared Error_ (RMSE).

1. **_Root Mean Squared Error_ (RMSE)**

RMSE adalah metrik yang umum digunakan untuk mengukur perbedaan antara nilai yang diprediksi oleh model dan nilai sebenarnya. Secara matematis, RMSE dihitung dengan rumus:

![Rumus-RMSE](https://github.com/user-attachments/assets/e5a01dce-bde4-4027-9e02-ff4e7b51ce79)

Gambar 2. Rumus _Root Mean Squared Error_

Dimana:

y  = nilai hasil observasi
ŷ  = nilai hasil prediksi
i  = urutan data pada database 
n  = jumlah data

RMSE memberikan gambaran tentang seberapa besar rata-rata kesalahan prediksi model. Nilai RMSE yang lebih rendah menunjukkan bahwa model memiliki kinerja yang lebih baik dalam memprediksi nilai secara akurat. Dalam konteks sistem rekomendasi film, RMSE digunakan untuk mengukur seberapa akurat model dalam memprediksi _rating_ film yang akan diberikan oleh pengguna.

**HAsil Evaluasi**

1. Hasil evaluasi proyek menunjukkan bahwa model _clustering_ yang diimplementasikan memiliki kinerja yang cukup baik dalam memprediksi _rating_ film

2. Nilai RMSE yang diperoleh pada data uji relatif rendah, yang menunjukkan bahwa prediksi _rating_ model cukup akurat.

3. Evaluasi dilakukan dengan membandingkan nilai RMSE dari beberapa variasi model dengan jumlah _cluster_ yang berbeda.

4. Hasil ini menunjukkan bahwa model mampu mengidentifikasi pola preferensi pengguna dengan baik dan memberikan rekomendasi film yang relevan.

Dengan demikian, evaluasi menggunakan RMSE memberikan validasi kuantitatif terhadap efektivitas model rekomendasi dalam memprediksi _rating_ film, yang menjadi dasar penting dalam memberikan rekomendasi yang dipersonalisasi kepada pengguna.

## Referensi

1. THE DEVELOPMENT OF TOURISM INDUSTRY IN INDONESIA: CURRENT PROBLEMS AND CHALLENGES. https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e13eb41de769f124b3c91771167fb7b01bc85559](https://www.idpublications.org/wp-content/uploads/2017/01/Full-Paper-THE-DEVELOPMENT-OF-TOURISM-INDUSTRY-IN-INDONESIA.pdf
2. THE ROLE OF TOURISM DESTINATION AND HUMAN RESOURCES IN SUSTAINABLE TOURISM IMPLEMENTATION IN INDONESIA. https://jbhost.org/jbhost/index.php/jbhost/article/view/170
