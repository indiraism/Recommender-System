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
Dalam pengembangan model machine learning untuk sistem rekomendasi, teknik _content-based filtering_ dan _collaborative filtering_ digunakan untuk merekomendasikan tempat terbaik kepada pengguna berdasarkan _rating_ atau penilaian yang mereka berikan. Tujuannya adalah menyajikan rekomendasi yang akurat sesuai preferensi pengguna.

1. _Content-based Filtering Recommendation_

Beberapa tahap yang dilakukan untuk membuat sistem rekomendasi dengan pendekatan _content-based filtering_ adalah _TF-IDF Vectorizer_, _cosine similarity_, dan pengujian sistem rekomendasi.

- **_TF-IDF Vectorizer_**

_TF-IDF Vectorizer_ akan melakukan transformasi teks nama tempat menjadi bentuk angka berupa matriks.

- **_Cosine Similarity_**

_Cosine similarity_ adalah metode untuk mengukur kemiripan antara dua data place dengan mengevaluasi sudut di antara keduanya. Teknik ini menentukan tingkat kesamaan berdasarkan sudut antara data _place_ yang dibandingkan. Hasil perhitungannya menghasilkan nilai yang mencerminkan tingkat kemiripan, di mana nilai mendekati 1 menandakan kemiripan yang kuat, sedangkan nilai mendekati 0 menunjukkan kemiripan yang lemah.

- **Hasil _Top-N Recommendation_**

Dengan menerapkan _TF-IDF Vectorizer_ untuk mengonversi data objek wisata ke dalam bentuk matriks dan mengukur kesamaan antar tempat menggunakan _cosine similarity_, pengujian sistem rekomendasi _content-based filtering_ dilakukan. Hasil evaluasinya adalah seperti berikut:

Diambil sebuah nama tempat yang dipilih oleh pengguna.

 **Tabel 5. Nama Tempat yang Dipilih Pengguna**
  | Place_Id | Place_Name | Category |
  | -------- | ---------- | ----------- |
  | 1 | Monumen Nasional | Budaya |
  | 2 | Kota Tua | Budaya |
  | 3 | Dunia Fantasi | Taman Hiburan |
  | 4 | Taman Mini Indonesia Indah (TMII) | Taman Hiburan |
  | 5 | Atlantis Water Adventure | Taman Hiburan |

Berikut adalah hasil rekomendasi nama tempat berdasarkan kategori yang sama.

**Tabel 6. Hasil Rekomendasi _Content-based Filtering_**

  | Place_Name                          | Category |
  | ----------------------------------- | -------- |
  | Candi Sewu                          | Budaya   |
  | Museum Benteng Vredeburg Yogyakarta | Budaya   |
  | Museum Satria Mandala               | Budaya   |
  | Kyotoku Floating Market             | Budaya   |
  | Bandros City Tour                   | Budaya   |

Dari hasil rekomendasi tersebut, terlihat bahwa sistem berhasil menyarankan lokasi-lokasi terkait berdasarkan input 'Monumen Nasional', dengan rekomendasi yang masih dalam kategori serupa, yakni budaya.

2. **_Collaborative Filtering Recommendation_**

Alur kerja pengembangan sistem rekomendasi _collaborative filtering_ terdiri dari tiga tahap utama: data preparation, pembagian data latih dan data validasi, serta konstruksi model dan evaluasi hasil rekomendasi.

- Dalam tahap _data preparation_, fitur `User_Id` dan `Place_Id` pada dataset _ratings_ di-encode menjadi _array_, kemudian dipetakan ulang ke dataset. Hasilnya mengungkapkan total 300 user, 437 tempat, dengan skor _rating_ berkisar dari 1.0 sampai 5.0.

- Proses pembagian dataset dimulai dengan pengacakan data _ratings_, kemudian membaginya menjadi data _training_ dan data validasi dengan perbandingan 80% untuk pelatihan dan 10% untuk validasi.

- Model Development dan Hasil Rekomendasi

Dari model _machine learning_ yang telah dibangun menggunakan _layer embedding_ dan _regularizer_, serta _adam optimizer_, _binary crossentropy loss function_, dan metrik RMSE (_Root Mean Squared Error_), diperoleh hasil pengujian sistem rekomendasi tempat wisata dengan pendekatan _collaborative filtering_.

Hasil rekomendasi menunjukkan bahwa sistem mengambil sampel acak _user_ dan merekomendasikan tempat-tempat yang mendapat penilaian terbaik dari _user_ tersebut.

- Curug Cilengkrang: Cagar Alam
- Gunung Lalakon: Cagar Alam
- Taman Hutan Raya Ir. H. Juanda: Cagar Alam
- Grand Maerakaca: Taman Hiburan
- Food Junction Grand Pakuwon: Pusat Perbelanjaan

Pada tahap berikutnya, sistem memperlihatkan daftar 10 tempat rekomendasi yang disesuaikan dengan kategori preferensi pengguna acak ini. Dapat diamati bahwa beberapa rekomendasi memiliki kategori yang mirip, contohnya:

- Taman Vanda: Taman Hiburan
- Taman Sejarah Bandung: Budaya
- Puspa Iptek Sundial: Taman Hiburan
- Museum Pendidikan Nasional: Budaya
- Water Park Bandung Indah: Taman Hiburan


## Evaluation

1. **_Content-based Filtering Recommendation_**

Tahap evaluasi untuk sistem rekomendasi dengan _content-based filtering_ dapat menggunakan metrik _precision_.

   $$precision = \frac{TP}{TP + FP}$$

   Di mana:
   $TP =$ _True Positive_; rekomendasi yang sesuai
   $FP =$ _False Positive_; rekomendasi yang tidak sesuai

   Berdasarkan hasil rekomendasi tempat wisata dengan pendekatan _content-based filtering_ dapat dilihat bahwa hasil yang diberikan oleh sistem rekomendasi berdasarkan tempat wisata **Monumen Nasional** dengan kategori **Budaya**, menghasilkan 5 rekomendasi judul tempat wisata yang tepat. Tetapi secara keseluruhan sistem merekomendasikan tempat wisata dengan tepat.

   $$precision = \frac{5}{5 + 0} = 100\%$$

   Dengan begitu, diperoleh nilai _precision_ sebesar **100%**.

2. **_Collaborative Filtering Recommendation_**

Tahap evaluasi untuk sistem rekomendasi dengan _collaborative filtering_ menggunakan metrik RMSE (_Root Mean Squared Error_). Rumus untuk mencari nilai RMSE sebagai berikut,

   $$RMSE=\sqrt{\sum^{n}_{i=1} \frac{y_i - y\\_pred_i}{n}}$$

   Di mana:
   $n =$ jumlah _dataset_
   $i =$ urutan data dalam _dataset_
   $y_i =$ nilai yang sebenarnya
   $y_{pred} =$ nilai prediksi terhadap $i$

Nilai RMSE dari sistem rekomendasi dengan pendekatan _collaborative filtering_ adalah 0.2526 pada _Training RMSE_, dan 0.2562 pada _Validation RMSE_. Sedangkan untuk nilai _training loss_ sebesar 0.0638, dan _validation loss_ sebesar 0.0656.


## Referensi

1. THE DEVELOPMENT OF TOURISM INDUSTRY IN INDONESIA: CURRENT PROBLEMS AND CHALLENGES. https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e13eb41de769f124b3c91771167fb7b01bc85559](https://www.idpublications.org/wp-content/uploads/2017/01/Full-Paper-THE-DEVELOPMENT-OF-TOURISM-INDUSTRY-IN-INDONESIA.pdf
2. THE ROLE OF TOURISM DESTINATION AND HUMAN RESOURCES IN SUSTAINABLE TOURISM IMPLEMENTATION IN INDONESIA. https://jbhost.org/jbhost/index.php/jbhost/article/view/170
