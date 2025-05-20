# Laporan Proyek Machine Learning - Indira Aline

## Latar Belakang

![Movies Recommendation System](https://blog.lancedb.com/content/images/2024/05/1_AatBvnpVpEPoQvZAMeqU-A.webp)

*Sumber: [Google](https://blog.lancedb.com/content/images/2024/05/1_AatBvnpVpEPoQvZAMeqU-A.webp)*

Di era digital ini, jumlah film yang tersedia sangatlah banyak dan terus bertambah. Layanan streaming film menawarkan ribuan judul, membuat penonton seringkali kesulitan menemukan film yang benar-benar sesuai dengan preferensi mereka. Hal ini mengarah pada beberapa masalah:

- **_Overchoice_:** Penonton kewalahan dengan banyaknya pilihan, yang dapat menyebabkan frustrasi dan kesulitan dalam membuat keputusan.

- **Waktu yang terbuang:** Penonton menghabiskan waktu yang signifikan untuk mencari film, alih-alih menikmati film itu sendiri.

- **Penemuan film yang terbatas:** Penonton cenderung menonton film-film populer atau yang sudah mereka kenal, sehingga melewatkan film-film berkualitas dari genre atau negara lain yang mungkin mereka sukai.

Sistem rekomendasi hadir sebagai solusi untuk mengatasi masalah-masalah ini.  Sistem rekomendasi film bertujuan untuk:

- **Mempersonalisasi pengalaman menonton:** Dengan menganalisis preferensi dan perilaku pengguna, sistem dapat merekomendasikan film yang kemungkinan besar akan disukai oleh setiap individu.

**Memudahkan penemuan film:** Sistem dapat membantu pengguna menemukan film-film baru yang mungkin belum mereka ketahui, memperluas cakrawala sinematik mereka.

- **Meningkatkan kepuasan pengguna:** Dengan memberikan rekomendasi yang relevan, sistem dapat meningkatkan kepuasan pengguna terhadap layanan streaming film.

Proyek ini akan mengembangkan sebuah sistem rekomendasi film yang menggunakan pendekatan _user clustering_ dan prediksi rating berdasarkan genre dan tahun rilis. Pendekatan ini dipilih karena memiliki beberapa keunggulan:

- **_Cold start problem_:** Tidak seperti metode _collaborative filtering_ yang membutuhkan rating dari pengguna lain, pendekatan ini dapat memberikan rekomendasi bahkan kepada pengguna baru yang belum memberikan rating apapun.

- **Efisiensi:** _User clustering_ memungkinkan sistem untuk membuat rekomendasi yang dipersonalisasi secara efisien, tanpa perlu membandingkan setiap pengguna dengan semua film yang tersedia.

Dengan demikian, proyek ini diharapkan dapat memberikan kontribusi dalam meningkatkan pengalaman menonton film di era digital ini.

Referensi:

[A Movie Recommender System: MOVREC](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e13eb41de769f124b3c91771167fb7b01bc85559)


## Business Understanding

### Problem Statements

1. Bagaimana dapat secara efisien merekomendasikan film kepada pengguna tanpa bergantung pada peringkat pengguna eksplisit?
2. Dapatkah memanfaatkan genre film dan tahun rilis untuk memprediksi preferensi pengguna dan memberikan rekomendasi yang dipersonalisasi?
3. Bagaimana dapat mengelompokkan pengguna dengan preferensi film yang serupa untuk meningkatkan akurasi rekomendasi?

### Goals

1. Mengembangkan sistem rekomendasi film yang menyarankan film berdasarkan genre dan tahun rilisnya, sehingga menghilangkan kebutuhan akan peringkat pengguna sebelumnya.
2. Memprediksi peringkat film untuk pengguna secara akurat dengan menganalisis hubungan antara genre film, tahun rilis, dan kelompok pengguna.
3. Mengidentifikasi kelompok pengguna yang berbeda dengan preferensi film yang serupa untuk mempersonalisasi rekomendasi dan meningkatkan akurasi prediksi.

    ### Solution statements
    - **Pengelompokan Pengguna:** Menggunakan algoritma pengelompokan (_DBSCAN, Agglomerative Clustering, KMeans_) untuk mengelompokkan pengguna berdasarkan preferensi film mereka yang disimpulkan dari data film. Ini akan membantu dalam mengidentifikasi pengguna dengan selera yang sama.
    - **Pemodelan Prediktif:** Mengembangkan model yang memprediksi peringkat film menggunakan genre film dan tahun rilis sebagai fitur. Model ini akan dilatih pada kelompok pengguna untuk menangkap preferensi kelompok tertentu dan membuat rekomendasi yang dipersonalisasi.


## Data Understanding

**Informasi Datasets**

| Jenis | Keterangan |
| ------ | ------ |
| Title | _Movie Recommendation System_ |
| Source | [Kaggle](https://www.kaggle.com/datasets/parasharmanas/movie-recommendation-system) |
| Maintainer | [Manas Parashar ⚡](https://www.kaggle.com/parasharmanas) |
| License | Other (specified in description) |
| Visibility | Publik |
| Tags | _Arts and Entertainment, Movies and TV Shows_ |
| Usability | 10.00 |

Berikut informasi pada dataset: 

| movieId | tittle | genres |
| ------ | ------ | ------ |
| 1 | Toy Story (1995) | Adventure|Animation|Children|Comedy|Fantasy |
| 2 | Jumanji (1995) | Adventure|Children|Fantasy |
| 3 | Grumpier Old Men (1995) | Comedy|Romance |
| 4 | Waiting to Exhale (1995) | Comedy|Drama|Romance |
| 5 | Father of the Bride Part II (1995) | Comedy |

Tabel 1a. _Movies_

`movies.csv` berisi informasi mengenai film-film, dengan total 62.423 entri dan 3 kolom. Kondisi data pada file ini menunjukkan bahwa tidak ada nilai yang hilang (non-null count sesuai dengan jumlah entri) dan tipe data yang digunakan adalah integer untuk `movieId` dan object untuk title dan genres.

| userId | movieId | rating | timestamp |
| ------ | ------ | ------ | ------ |
| 1 | 296 | 5 | 1.14788e+09 |
| 1 | 306 | 3.5 | 1.14787e+09 |
| 1 | 307 | 5 | 1.14787e+09 |
| 1 | 665 | 5 | 1.14788e+09 |
| 1 | 899 | 3.5 | 1.14787e+09 |

Tabel 1b. _Ratings_

`ratings.csv` berisi informasi mengenai rating film, dengan total 25.000.095 entri dan 4 kolom. Kondisi data pada file ini juga menunjukkan tidak ada nilai yang hilang, dengan tipe data integer untuk userId, movieId, dan _timestamp_, serta float untuk rating.

**Fitur pada Dataset**
1. `movies.csv`
- `movieId`: Merupakan _ID_ unik untuk setiap film.
- `title`: Judul film, yang juga mencakup tahun rilis.
- `genres`: Kategori atau genre film, dipisahkan dengan tanda "|".

2. `rating.csv`:
- `userId`: _ID_ unik untuk setiap pengguna.
- `movieId`: _ID film_ yang diberi rating (berkorespondensi dengan `movieId` di file `movies.csv`).
- `rating`: Rating yang diberikan pengguna untuk film tersebut (dalam skala numerik).
- `_timestamp_`: _Timestamp_ yang menunjukkan waktu pemberian rating.

## Exploratory Data Analysis (EDA)

![Genres](/image/genres.png)

Gambar 1a. Analisis Genre

Pada gambar 1a. Analisis Genre ditemukan ada 20 genre film dalam dataset. Beberapa genre yang paling banyak muncul adalah Drama, Comedy, Action, Thriller, Adventure. Ada juga kategori '(no genres listed)' yang perlu diperhatikan dalam pengolahan data selanjutnya.

![Distribusi Film Berdasarkan Dekade](/image/dekade.png)

Gambar 1b. Distribusi Film Berdasarkan Dekade

Gambar 1b. Distribusi Film Berdasarkan Dekade tersebut menunjukkan tren pertumbuhan eksponensial dalam produksi film, terutama pada dua dekade terakhir. Perkembangan teknologi dan globalisasi industri film kemungkinan menjadi faktor utama di balik tren ini.

![Outlier](/image/outlier.png)

Gambar 1c. Outlier 

Di gambar 1c. kita dapat melihat bahwa ada _outlier_ ekstrem dan beberapa _outlier_ kecil dalam dataset, dan mereka sangat ekstrem, sehingga akan memengaruhi kinerja model. saya perlu menghilangkannya. Saya akan menghapus apa pun yang lebih tinggi dari 1000.

![No Outlier](/image/nooutlier.png)

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

![Rumus RMSE](/image/Rumus-RMSE.png)

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

1. A Movie Recommender System: MOVREC. https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e13eb41de769f124b3c91771167fb7b01bc85559
