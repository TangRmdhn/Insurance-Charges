
# Laporan Proyek Machine Learning - Bintang Ramadhan

## Domain Proyek

Asuransi kesehatan merupakan instrumen penting dalam sistem pelayanan kesehatan, berfungsi sebagai mekanisme perlindungan finansial terhadap risiko biaya pengobatan yang tidak terduga. Di Indonesia, implementasi asuransi kesehatan nasional melalui Badan Penyelenggara Jaminan Sosial (BPJS) Kesehatan bertujuan untuk memastikan akses layanan kesehatan yang merata dan terjangkau bagi seluruh lapisan masyarakat. Namun, tantangan seperti keterbatasan infrastruktur, ketimpangan distribusi layanan, dan kendala administratif masih menjadi hambatan dalam mencapai tujuan tersebut.

Studi oleh Suryono (2008) menyoroti pentingnya regulasi pemerintah dalam pengelolaan asuransi kesehatan, khususnya terkait dengan Peraturan Pemerintah Nomor 69 Tahun 1991, yang mengatur penyelenggaraan asuransi kesehatan di Indonesia. Sementara itu, penelitian oleh Miller dan Wherry (2017) di Amerika Serikat menunjukkan bahwa perluasan cakupan asuransi kesehatan melalui Medicaid berkontribusi pada peningkatan akses layanan kesehatan dan perbaikan status kesehatan masyarakat.
Google Scholar

Dalam konteks global, asuransi kesehatan tidak hanya berperan dalam aspek finansial, tetapi juga berdampak pada peningkatan kualitas hidup dan produktivitas masyarakat. Oleh karena itu, pengembangan sistem asuransi kesehatan yang efektif dan inklusif menjadi prioritas dalam upaya mencapai tujuan pembangunan berkelanjutan di sektor kesehatan.

**Mengapa masalah ini penting?**

- Perusahaan asuransi memerlukan metode estimasi yang akurat untuk menentukan premi asuransi secara personal.
- Dengan adanya prediksi biaya yang lebih tepat, perusahaan dapat mengurangi kerugian akibat prediksi biaya yang salah.

**Referensi**:
- Suryono, A. (2008). Asuransi Kesehatan Menurut Peraturan Pemerintah Nomor 69 Tahun 1991. HUMANIS, Jurnal Sosial Ekonomi Humaniora, 2.

- Miller, S., & Wherry, L. R. (2017). Health and access to care during the first 2 years of the ACA Medicaid expansions. New England Journal of Medicine, 376(10), 947–956.

## Business Understanding

### Problem Statements

- Bagaimana cara memprediksi biaya asuransi kesehatan (`charges`) berdasarkan informasi pribadi seseorang?
- Apakah fitur seperti status merokok, usia, dan BMI memiliki pengaruh signifikan terhadap biaya asuransi?

### Goals

- Mengembangkan model prediktif untuk memperkirakan nilai `charges` pada data baru.
- Mengidentifikasi fitur mana yang paling berkontribusi dalam menentukan biaya asuransi.

### Solution Statements

- Menerapkan beberapa model regresi, seperti KNN, Random Forest, dan Boosting Algorithm.
- Melakukan improvement melalui teknik preprocessing (penghapusan outlier dan normalisasi), dan evaluasi menggunakan metrik regresi seperti MAE, MSE, dan R².

## Data Understanding

Dataset yang digunakan adalah [Insurance Dataset](https://www.kaggle.com/datasets/mirichoi0218/insurance)

### Jumlah data : 
- Baris : 1338
- Kolom : 7

### Kondisi data
- Tidak ada missing value
  
![Data hilang](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/data%20hilang.png "a title")

- Terdapat 1 data duplikat sehingga harus dihapus

![DAta duplikat](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/duplikat%20data.png "a title")
  
- Banyak outlier pada data sehingga harus dihapus menggunakan teknik IQR

### Variabel dalam dataset:

- `age` : usia individu
- `sex` : jenis kelamin
- `bmi` : indeks massa tubuh
- `children` : jumlah anak yang ditanggung
- `smoker` : status merokok (yes/no)
- `region` : wilayah tempat tinggal
- `charges` : biaya asuransi (variabel target)

### Visualisasi boxplot untuk mengecek outlier.

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/outlier%20age.png "a title")
![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/outlier%20bmi.png "a title")
![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/outlier%20childern.png "a title")
![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/outlier%20charges.png "a title")

Dari visual boxplot terdapat beberapa data yang outlier sehingga harus di hapus menggunakan teknik IQR

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/hapus%20outlier "a title")

### Analisis total sample

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/sample%20kel.png "a title")

Pada data ini perempuan lebih banyak daripada laki-laki

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/sample%20smoke.png "a title")

Pada data ini orang yang tidak merokok jauh lebih banyak daripada orang yang merokok

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/sample%20region.png "a title")

Pada data ini orang yang dari berbeda beda daerah, daerah southwest memiliki data yang paling sedikit

### Penyebaran data

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/histogram.png "a title")

Dari visual tersebut umur yang paling banyak di data adalah sekitar 20 tahun, bmi paling banyak adalah 30 kg/m², kemudian orang yang tidak memiliki anak adalah data terbanyak, dan charges yang murah yang paling banyak.

### Analisis rata-rata `charges` berdasarkan kategori seperti `smoker` dan `region`.

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/rata-rata%20charges.png "a title")

Data orang yang merokok memiliki rata-rata harga asuransi yang sangat tinggi

### Pairplot untuk melihat relasi antar fitur.

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/pairplot.png "a title")

Data `age` menunjukan semakin tinggi umur maka `charges` nya juga semakin naik

### Heatmap korelasi antar fitur numerik.

![Alt text](https://github.com/TangRmdhn/Insurance-Charges/blob/main/Assets/correlation.png "a title")

Data `age` memiliki korelasi terhadap `charges` yang paling tinggi

## Data Preparation

Langkah-langkah yang dilakukan:

1. **Pemeriksaan missing values** Tidak ada missing values
2. **Pemeriksaan duplikat data** Terdapat 1 data yang duplikat sehingga harus dihapus
3. **Pemeriksaan dan penghapusan outlier** menggunakan IQR.
4. **Encoding fitur kategorikal** dengan `pd.get_dummies` (one-hot encoding).
5. **Splitting data** menjadi train dan test (80% - 20%).
6. **Standarisasi** fitur numerik (`age`, `bmi`, `children`) menggunakan `StandardScaler`.

Alasan:  
- Penghapusan outlier dilakukan untuk menghindari distorsi terhadap model regresi.
- Normalisasi numerik penting agar model tidak bias terhadap fitur dengan skala besar.

## Modeling

Beberapa model regresi digunakan, antara lain:

**1. K-Nearest Neighbors (KNN) Regressor**
KNN (K-Nearest Neighbors) bekerja dengan mencari sejumlah tetangga terdekat (misalnya K = 3) dari data yang ingin diprediksi, lalu hasil prediksi diambil dari rata-rata nilai tetangga tersebut (untuk regresi) atau mayoritas kelasnya (untuk klasifikasi). Pada kasus ini menggunakan regresi

Kelebihan:
- Non-parametrik, tidak mengasumsikan bentuk distribusi data.
- Baik untuk data dengan hubungan non-linear sederhana.

Kekurangan:
- Sensitif terhadap skala dan outlier.
- Lambat saat prediksi jika data besar (karena perlu menghitung jarak ke semua titik).

**2. Random Forest**
Random Forest adalah gabungan banyak pohon keputusan (decision tree) yang dilatih pada bagian-bagian acak dari data; hasil prediksinya diambil dari rata-rata semua prediksi pohon-pohon tersebut agar lebih akurat dan tahan terhadap overfitting.
  
Kelebihan:
- Tahan terhadap overfitting dibanding decision tree tunggal.
- Dapat menangani data dengan fitur campuran (numerik dan kategorikal).

Kekurangan:
- Kurang interpretatif (model seperti "black box").
- Bisa lambat saat training jika jumlah pohon besar.

**3. Boosting Algorithm**
Boosting Algorithm seperti Gradient Boosting bekerja dengan melatih model secara bertahap, di mana setiap model baru fokus memperbaiki kesalahan dari model sebelumnya; hasil akhirnya merupakan gabungan dari semua model untuk memberikan prediksi yang lebih kuat dan akurat.
  
Kelebihan:
- Performa tinggi, sering unggul dalam kompetisi.
- Menangkap pola kompleks secara bertahap.

Kekurangan:
- Rentan overfitting jika tidak dituning dengan baik.
- Proses training bisa memakan waktu lebih lama.

Model-model ini dilatih dengan data train dan dievaluasi dengan data test. Tuning dilakukan melalui:
- `n_neighbors=10` untuk model KNN
- `n_estimators=50, max_depth=16, random_state=55` untuk model Random Forest
- `learning_rate=0.05, random_state=55` untuk model Boosting

## Evaluation

### Metrik yang digunakan:
- **Mean Absolute Error (MAE)**
- **Mean Squared Error (MSE)**

Penjelasan:
- MAE: Rata-rata kesalahan absolut
- MSE: Rata-rata kuadrat kesalahan (lebih sensitif terhadap outlier)

### Evaluasi Performa Model

Model dievaluasi menggunakan metrik regresi Mean Absolute Error (MAE) dan Mean Squared Error (MSE). MAE mengukur rata-rata selisih absolut antara nilai prediksi dan nilai aktual, sedangkan MSE menghitung rata-rata kuadrat dari selisih tersebut. MAE lebih mudah diinterpretasikan dan tidak terlalu sensitif terhadap outlier, sedangkan MSE memberikan penalti lebih besar terhadap kesalahan besar, sehingga cocok untuk mengevaluasi seberapa jauh prediksi meleset dari nilai sebenarnya.

Berikut adalah hasil evaluasi dari tiga model yang digunakan:

| Model         | MAE (Train) | MSE (Train) | MAE (Test) | MSE (Test) |
|---------------|-------------|-------------|------------|------------|
| KNN           | 2.818       | 21,797.55   | **6.787**  | 62,820.31 |
| Random Forest | 0.991  | 3,596.54| 9.384      | 107,229.40  |
| Boosting      | 3.330       | 20,654.09   | 9.163      | 102,908.03  |

Model KNN menunjukkan performa terbaik dalam hal generalisasi karena memiliki nilai MAE dan MSE paling rendah pada data uji. Meskipun model Random Forest memiliki error paling kecil pada data latih, performanya pada data uji kurang baik, yang mengindikasikan kemungkinan overfitting. Sementara itu, model Boosting berada di antara keduanya, dengan performa uji yang cukup baik, tetapi tidak sebaik KNN.

### Evaluasi Kesesuaian dengan Business Understanding

Model yang dikembangkan dalam proyek ini telah berhasil menjawab problem statements yang dirumuskan sebelumnya. Model mampu memprediksi biaya asuransi (`charges`) berdasarkan atribut pribadi seseorang seperti usia, BMI, jumlah anak, status merokok, dan lainnya. Selain itu, hasil eksplorasi data menunjukkan bahwa fitur seperti `smoker`, `age`, dan `bmi` memiliki kontribusi signifikan terhadap nilai `charges`, sesuai dengan hipotesis awal. Fitur yang paling berkontribusi adalah fitur `age` karena pada correlation matriks memiliki nilai korelasi 0.44 dan menempati yang tertinggi.

Tujuan utama proyek ini, yaitu membangun model prediktif yang mampu memperkirakan `charges` untuk data baru, telah tercapai. Ketiga model yang digunakan mampu mempelajari pola dari data historis dan melakukan prediksi pada data uji dengan tingkat kesalahan yang dapat diterima. Selain itu, proyek ini juga berhasil mengidentifikasi fitur yang memengaruhi biaya asuransi yaitu fitur `age`, yang sangat berguna bagi perusahaan dalam menyusun strategi penetapan premi secara lebih personal dan akurat. 

Secara keseluruhan, model KNN dipilih sebagai model akhir karena memiliki performa terbaik pada data uji, yang menunjukkan kemampuannya dalam melakukan generalisasi terhadap data baru. Model ini dinilai mampu memenuhi kebutuhan bisnis dalam memprediksi biaya asuransi secara lebih akurat, sehingga dapat membantu perusahaan dalam mengelola risiko finansial dan meningkatkan efisiensi dalam penetapan premi.

### Metrik Evaluasi Model

Dalam proyek ini digunakan tiga metrik evaluasi regresi, yaitu **MAE**, dan **MSE** untuk mengukur seberapa akurat model dalam memprediksi biaya asuransi:

#### 1. Mean Absolute Error (MAE)
MAE mengukur rata-rata selisih absolut antara nilai prediksi dan nilai aktual. Rumusnya:

MAE = (1/n) * Σ |yᵢ - ŷᵢ|

- Nilai MAE lebih rendah menunjukkan prediksi lebih akurat.
- MAE tidak sensitif terhadap outlier karena tidak memanfaatkan kuadrat selisih.

---

#### 2. Mean Squared Error (MSE)
MSE menghitung rata-rata kuadrat selisih antara nilai prediksi dan nilai aktual:

MSE = (1/n) * Σ (yᵢ - ŷᵢ)²

- MSE memberikan penalti lebih besar terhadap kesalahan besar.
- Semakin kecil nilai MSE, semakin baik performa model.

---
