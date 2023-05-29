# CapstoneProjectModule3-JCDSOL09
Capstone Project Module 3 - Noor Kharismawan Akbar

# A. Business Problem Understanding

## 1. Context
Tempat tinggal merupakan kebutuhan pokok setiap manusia. Namun, tidak semua orang memiliki pengetahuan tentang harga rumah yang sesuai dengan fasilitas yang tersedia. Di setiap negara cukup sulit untuk orang awam mengetahui harga rumah yang sesuai dengan berbagai aspek pendukung lain-nya. Begitupun pula di daerah California, sebuah negara bagian di Amerika Serikat yang terletak bagian Barat, yang memiliki 58 counties & 482 municipalities menurut [wikipedia](https://simple.wikipedia.org/wiki/List_of_cities_and_towns_in_California).

## 2. Problem Statement
Tantangan dari setiap developer perumahan adalah **bagaimana menentukan harga rumah yang tepat dengan memperhatikan fasilitas yang diberikan serta aspek pendukung lain**. Developer tentu saja tidak ingin membangun perumahan elit di kawasan yang notabene warga sekitarnya berpenghasilan rendah, atau membangun perumahan yang biasa saja di kawasan elit. Sehingga rumah yang di tawarkan atau di bangun oleh developer dapat memberikan keuntungan financial yang maksimal dan rumah yang di bangun juga dapat terjual dengan cepat.

## 3. Goals
Tujuan dari pemodelan ini:
1. Menentukan harga jual dari suatu rumah
2. Menentukan aspek penting dalam perhitungan harga rumah

## 4. Analytic Approach
Melihat problem yang ada, kita akan membangun **model machine learning** yang akan menjadi alat untuk memprediksi harga jual rumah, yang mana akan berguna untuk developer dalam menentukan lokasi serta harga rumah di suatu kawasan tertentu.

## 5. Modelling
Menggunakan model **Supervised Machine Learning (Regression)**, antara lain: Linear Regression, KNN, Decision Tree, Random Forest, XGBoost, AdaBoost, dan Gradient Boosting.

## 6. Evaluation Metric
Untuk model regresi, terdapat berbagai metriks evaluasi yang dapat digunakan, antara lain: RMSE, MAE, RMSLE, MAPE, dan R-square.

# B. Data Understanding

## 1. Column Description
Berikut Penjelasan kolom pada dataset.

| No | Nama Kolom | Deskripsi & Penjelasan Kolom |
| -- | -- | -- |
| 1 | longitude | Ukuran seberapa jauh barat sebuah rumah; nilai yang lebih besar lebih jauh ke barat ([Garis Bujur](https://id.wikipedia.org/wiki/Garis_bujur)) |
| 2 | latitude | Ukuran seberapa jauh utara sebuah rumah; nilai yang lebih besar lebih jauh ke utara ([Garis Lintang](https://id.wikipedia.org/wiki/Garis_lintang)) |
| 3 | housingMedianAge | Usia rata-rata sebuah rumah dalam satu blok; angka yang lebih rendah adalah bangunan yang lebih baru |
| 4 | totalRooms | Jumlah total ruangan dalam satu blok |
| 5 | totalBedrooms | Jumlah total kamar tidur dalam satu blok |
| 6 | population | Jumlah total orang yang tinggal dalam satu blok |
| 7 | households | Jumlah total rumah tangga (Jumlah kepala dalam satu [Kartu Keluarga](https://id.wikipedia.org/wiki/Kartu_Keluarga)), sekelompok orang yang tinggal di dalam satu unit rumah, untuk satu blok |
| 8 | medianIncome | Median pendapatan untuk rumah tangga dalam satu blok rumah (diukur dalam puluhan ribu Dolar AS)| |
| 9 | medianHouseValue | Median harga rumah rata-rata untuk rumah tangga dalam satu blok (diukur dalam Dolar AS) |
| 10 | oceanProximity | Jarak rumah ke pesisir pantai atau laut |

## 2. EDA
Dilakukan pada kolom numerikal & kategorikal.

# C. Data Preprocessing

## 1. Missing Value -> Listwise Deletion
Terdapat pada kolom total_bedrooms:
* Termasuk kategori MCAR
* Persentase hanya 0.95%

## 2. Data Abnormal, Outlier, Lack -> Listwise Deletion
Terdapat pada kolom:
* housing_median_age
* median_house_value
* ocean_proximity

## 3. Data Correlation
* Menambah kolom:
 * `bedrooms_per_room`
 * `population_per_household`
* Menghapus kolom:
 * `total_bedrooms`
 * `population`

# D. Machine Learning Modelling
Dibuat 2 model machine learning & di tuning parameter nya.

## 1. Data Splitting & Scaling
* Splitting: 70:30
* Scaling:
 * One Hot Encoder: kolom fitur kategorikal
 * PowerTransformer: kolom fitur numerikal

## 2. Model Selection
Berdasarkan benchmark & prediksi pada test set, dipilih 2 model terbaik yang akan di tuning, yaitu **XGBoost** & **Random Forest**.

## 3. Tuning Model
Berikut Perbandingan Hyperparameter XGBoost sebelum & setelah tuning:
| Nama Kolom | Default | Tuning Range | Best Params |
| -- | -- | -- | -- |
| max_depth | 6 | 1 - 10 | 7 |
| learning_rate | 0,3 | 0,01 - 0,10 | 0,07 |
| n_estimators | 100 | 100 - 200 | 197 |
| subsample | 1 | 0,2 - 1 | 0,9 |
| gamma | 0 | 1 - 10 | 9 |
| colsample_bytree | 1 | 0,1 - 1 | 0,7 |
| reg_alpha | 1 | logspace (-3,1,10) | 0,01 |
| reg_lambda | 1 | logspace (-3,1,10) | 1,29 |

Berikut Perbandingan Hyperparameter Random Forest sebelum & setelah tuning:
| Nama Kolom | Default | Tuning Range | Best Params |
| -- | -- | -- | -- |
| criterion | squared_error | squared_error, absolute_error, poisson | absolute_error|
| max_features | 1 | 2 - 11 | 8 |
| n_estimators | 100 | 100 - 199 | 146 |
| min_samples_split | 2 | 2 - 10 | 9 |
| min_samples_leaf | 1 | 1 - 20 | 5 |

## 4. Perbandingan Score (MAE|MAPE)
* XGBoost
 * Sebelum Tuning: 27.033 | 0.1636
 * Setelah Tuning: 26.330 | 0.1572
* Random Forest
 * Sebelum Tuning: 30.963 | 0.1887
 * Setelah Tuning: 30.573 | 0.1809

# E. Conclusion & Recommendation

## 1. Conclusion
Berdasarkan hasil pemodelan terbaik (**XGBoost**):
* Dari 7 model yang dibuat & 2 model yang di tuning, model **XGBoost** memiliki performa yang paling baik pada dataset ini dengan nilai MAE 26.330 & MAPE 15,72%. Jika dilihat dari nilai MAPE nya yaitu 15.72% untuk **XGBoost**, model ini termasuk **Good Forecast** menurut sumber dari [researchgate](https://www.researchgate.net/figure/Interpretation-of-MAPE-Results-for-Forecasting-Accuracy_tbl2_276417263).
* `ocean_proximity` dan `median_income` merupakan fitur yang korelasinya paling tinggi  dan paling penting dalam pemodelan ini, yang pengaruhnya paling besar terhadap nilai prediksi `median_house_value`.
* Model ini memiliki limitasi yaitu hanya bisa memprediksi model yang memiliki `median_house_value` pada rentang 14.999 hingga 433.800. Model XGBoost ini merupakan tree based model yang bukan merupakan model extrapolation. Sehingga model ini tidak bisa memprediksi value diluar rentang nilai datanya. Model ini juga masih belum bekerja dengan optimal pada `median_house_value` lebih dari 332.970.

## 2. Recommendation
* Untuk Developer Perumahan (Bisnis):
  * Untuk memaksimalkan keuntungan, perlu disesuaikan tempat rumah tersebut dibangun. Serta memperhatikan pendapatan orang-orang pada warga sekitar daerah tersebut (Bisa dilihat dari pendapatan perkapita).
* Untuk Model Machine Learning:
  * Bisa dilakukan optimasi menggunakan **GridSearch**. Kemudian hyperparameter tuning optimization nya bisa menggunakan **Optuna** untuk meningkatkan performa model.
  * Evaluasi fitur perlu dilakukan dengan cara yang lebih advance, misal dengan menerapkan **iterative-feature-selection**.
* Untuk dataset:
  * Melihat hanya sedikit fitur yang berkorelasi tinggi dengan target pada dataset ini, perlu ditambahkan fitur pendukung lain seperti **luas tanah**, **luas bangunan**, **parkiran mobil/garasi**, **kolam renang**, dan **jarak rumah ke fasilitas umum (rumah sakit, bandara, stasiun, mall, dll)**.
  * Untuk analisis yang terbaru, perlu dilakukan **update harga rumah** dikarenakan data yang terdapat pada dataset sudah cukup lama (Tahun 1990) yang pastinya sudah terdampak inflasi.

# F. Save Model
Simpan model yang telah di tuning menggunakan pickle.
