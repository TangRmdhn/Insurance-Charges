
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

Dataset yang digunakan adalah [Insurance Dataset](https://www.kaggle.com/datasets/mirichoi0218/insurance) dari Kaggle, yang berisi 1338 sampel. Setiap baris mewakili informasi satu individu.

### Variabel dalam dataset:

- `age` : usia individu
- `sex` : jenis kelamin
- `bmi` : indeks massa tubuh
- `children` : jumlah anak yang ditanggung
- `smoker` : status merokok (yes/no)
- `region` : wilayah tempat tinggal
- `charges` : biaya asuransi (variabel target)

EDA dilakukan dengan:
- Visualisasi boxplot dan histogram untuk mengecek distribusi dan outlier.
- Heatmap korelasi antar fitur numerik.
- Pairplot untuk melihat relasi antar fitur.
- Analisis rata-rata `charges` berdasarkan kategori seperti `smoker` dan `region`.

## Data Preparation

Langkah-langkah yang dilakukan:

1. **Pemeriksaan dan penghapusan outlier** menggunakan IQR.
2. **Encoding fitur kategorikal** dengan `pd.get_dummies` (one-hot encoding).
3. **Standarisasi** fitur numerik (`age`, `bmi`, `children`) menggunakan `StandardScaler`.
4. **Splitting data** menjadi train dan test (90% - 10%).

Alasan:  
- Penghapusan outlier dilakukan untuk menghindari distorsi terhadap model regresi.
- Normalisasi numerik penting agar model tidak bias terhadap fitur dengan skala besar.

## Modeling

Beberapa model regresi digunakan, antara lain:

**1. K-Nearest Neighbors (KNN) Regressor**
Alasan: KNN mudah dipahami dan cocok untuk melihat hubungan lokal antar data.

Kelebihan:
- Non-parametrik, tidak mengasumsikan bentuk distribusi data.
- Baik untuk data dengan hubungan non-linear sederhana.

Kekurangan:
- Sensitif terhadap skala dan outlier.
- Lambat saat prediksi jika data besar (karena perlu menghitung jarak ke semua titik).

**2. Random Forest Regressor**
Alasan: Model ensambel yang stabil dan akurat, cocok untuk data dengan banyak fitur.
  
  Kelebihan:
- Tahan terhadap overfitting dibanding decision tree tunggal.
- Dapat menangani data dengan fitur campuran (numerik dan kategorikal).

Kekurangan:
- Kurang interpretatif (model seperti "black box").
- Bisa lambat saat training jika jumlah pohon besar.

**3. Gradient Boosting (Boosting Regressor)**
Alasan: Model kuat yang membangun prediksi secara bertahap untuk mengurangi kesalahan.
  
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

Hasil evaluasi:  

| Model                 | MAE_train	| MSE_train | MAE_test |	MSE_test
|----------------------|---------|----------|----------|----------|
KNN |	2.818471 |	21797.552504 |	6.787490	| 62820.311454
RandomForest |	0.991862	| 3596.535685 |	9.384881 |	107229.404049
Boosting	| 3.330874	| 20654.091810 |	9.163674 |	102908.028757

KNN adalah pilihan terbaik untuk generalisasi karena memiliki MAE dan MSE test paling rendah, meskipun error training-nya tidak paling kecil.

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
