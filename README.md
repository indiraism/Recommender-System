# Laporan Proyek Machine Learning - Indira Aline

## Project Overview

![Indonesia Tourism Destination](https://www.balidiscovery.com/wp-content/uploads/2024/10/widiyanti3.jpg)

*Sumber: [Bali Discovery](https://www.balidiscovery.com/ri-identifies-top-5-tourism-destinations/)*

Sistem rekomendasi telah menjadi bagian integral dari kehidupan digital kita, membantu pengguna menavigasi lautan informasi dan pilihan yang tersedia. Dari platform _e-commerce_ hingga layanan _streaming_, sistem ini berperan penting dalam meningkatkan pengalaman pengguna dengan menyajikan konten atau produk yang relevan dan personal.

Indonesia, dengan kekayaan budaya dan keindahan alamnya, menawarkan beragam destinasi wisata yang menarik. Namun, bagi wisatawan, baik domestik maupun mancanegara, menemukan destinasi yang sesuai dengan preferensi pribadi bisa menjadi tantangan. Informasi yang tersebar, pilihan yang melimpah, dan kurangnya rekomendasi yang dipersonalisasi seringkali menyulitkan proses perencanaan perjalanan. Tanpa panduan yang efektif, wisatawan mungkin melewatkan permata tersembunyi atau menghabiskan waktu pada destinasi yang tidak sesuai dengan minat mereka, yang pada akhirnya dapat mengurangi kepuasan pengalaman berwisata. [1](https://www.idpublications.org/wp-content/uploads/2017/01/Full-Paper-THE-DEVELOPMENT-OF-TOURISM-INDUSTRY-IN-INDONESIA.pdf)

Masalah ini dapat diselesaikan melalui pengembangan sistem rekomendasi destinasi wisata yang cerdas. Sistem ini akan menganalisis preferensi pengguna dan karakteristik destinasi untuk menyajikan rekomendasi yang paling relevan. Pendekatan ini tidak hanya akan mempermudah wisatawan dalam menemukan tempat-tempat menarik, tetapi juga akan berkontribusi pada peningkatan jumlah kunjungan ke berbagai destinasi wisata di Indonesia. [2](https://jbhost.org/jbhost/index.php/jbhost/article/view/170)


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

Dataset yang digunakan yaitu `tourism_with_id.csv` dan `tourism_rating.csv` sebagai dataset.

Berikut adalah _Exploratory Data Analysis_ (EDA) yang merupakan proses investigasi awal pada data untuk menganalisis karakteristik, menemukan pola, anomali, dan memeriksa asumsi pada data.

- **Place**

  **Tabel 1. Informasi Dataset tourism_with_id**

  | #   | Column       | Non-Null Count | Dtype   |
  | --- | ------------ | -------------- | ------- |
  | 0   | Place_Id     | 437 non-null   | int64   |
  | 1   | Place_Name   | 437 non-null   | object  |
  | 2   | Description  | 437 non-null   | object  |
  | 3   | Category     | 437 non-null   | object  |
  | 4   | City         | 437 non-null   | object  |
  | 5   | Price        | 437 non-null   | int64   |
  | 6   | Rating       | 437 non-null   | float64 |
  | 7   | Time_Minutes | 205 non-null   | float64 |
  | 8   | Coordinate   | 437 non-null   | object  |
  | 9   | Lat          | 437 non-null   | float64 |
  | 10  | Long         | 437 non-null   | float64 |

  tourism_with_id terdiri dari 437 baris dan 10 kolom sebagai berikut:

  - Place_Id: kolom yang menunjukkan id dari setiap tempat wisata.
  - Place_Name: kolom yang menunjukkan nama dari setiap tempat wisata.
  - Description: kolom yang menunjukkan deskripsi dari setiap tempat wisata.
  - Category: kolom yang menunjukkan kategori dari setiap tempat wisata.
  - City: kolom yang menunjukkan kota dimana tempat wisata tersebut berada.
  - Price: kolom yang menunjukkan harga tiket masuk ke tempat wisata tersebut.
  - Rating: kolom yang menunjukkan rating dari setiap tempat wisata.
  - Time_Minutes: kolom yang menunjukkan waktu yang diperlukan untuk mengunjungi tempat wisata tersebut.
  - Coordinate: kolom yang menunjukkan koordinat dari setiap tempat wisata.
  - Lat: kolom yang menunjukkan latitude dari setiap tempat wisata.
  - Long: kolom yang menunjukkan longitude dari setiap tempat wisata.

Berikut adalah visualisasi dari dataset tourism_with_id:

  **Tabel 2. Sampel Dataset tourism_with_id**

  | #   | Place_Id | Place_Name                        | Category      |
  | --- | -------- | --------------------------------- | ------------- |
  | 0   | 1        | Monumen Nasional                  | Budaya        |
  | 1   | 2        | Kota Tua                          | Budaya        |
  | 2   | 3        | Dunia Fantasi                     | Taman Hiburan |
  | 3   | 4        | Taman Mini Indonesia Indah (TMII) | Taman Hiburan |
  | 4   | 5        | Atlantis Water Adventure          | Taman Hiburan |

- **Rating**

  **Tabel 3. Informasi Dataset tourism_rating**

  | #   | Column        | Non-Null Count | Dtype |
  | --- | ------------- | -------------- | ----- |
  | 0   | User_Id       | 10000 non-null | int64 |
  | 1   | Place_Id      | 10000 non-null | int64 |
  | 2   | Place_Ratings | 10000 non-null | int64 |

  tourism_rating terdiri dari 10000 baris dan 3 kolom sebagai berikut:

  - User_Id: identitas unik dari setiap pengguna.
  - Place_Id: identitas unik dari setiap tempat wisata.
  - Place_Ratings: penilaian atau rating yang diberikan oleh pengguna terhadap tempat wisata tertentu.

  Berikut adalah visualisasi dari dataset tourism_rating:

  **Tabel 4. Sampel Dataset tourism_rating**

  | #   | User_Id | Place_Id | Place_Ratings |
  | --- | ------- | -------- | ------------- |
  | 0   | 1       | 179      | 3             |
  | 1   | 1       | 344      | 2             |
  | 2   | 1       | 5        | 5             |
  | 3   | 1       | 373      | 3             |
  | 4   | 1       | 101      | 4             |


## Data Preparation
Tahap ini bertujuan untuk mempersiapkan data yang akan digunakan untuk proses training model. Di sini dilakukan penghapusan kolom yang tidak diperlukan, pembersihkan data _missing value_, dan melakukan pengecekan dan penghapusan data duplikat.

1. **Menghilangkan Kolom yang Tidak Relevan**

   Untuk dataset **tourism_with_id**, hanya kolom `Place_Id`, `Place_Name`, dan `Category` yang dibutuhkan, ehingga    kolom lainnya dihapus. Sementara pada dataset **tourism_rating**, seluruh kolom diperlukan, sehingga tidak ada      kolom yang dihilangkan.

2. **Pengecekan Missing Values**

   Proses pengecekan data yang hilang atau _missing value_ dilakukan pada masing-masing dataset **tourism_with_id**    dan **tourism_rating**. Berdasarkan hasil pengecekan, ternyata tidak ada data yang hilang dari kedua dataset        tersebut.


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
