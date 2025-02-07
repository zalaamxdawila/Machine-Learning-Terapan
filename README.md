# Laporan Proyek Machine Learning - M Dicky Desriansyah

## Project Overview

### **Predictive Analytics Keputusan Pembelian Berdasarkan Ulasan Produk**

Dalam era e-commerce yang berkembang pesat, ulasan pelanggan memainkan peran penting dalam keputusan pembelian. Ulasan produk, yang sering kali mencerminkan pengalaman dan kepuasan pelanggan, dapat memberikan wawasan berharga bagi bisnis e-commerce untuk meningkatkan penjualan dan layanan mereka. Oleh karena itu, dengan menggunakan teknik **Predictive Analytics**, proyek ini bertujuan untuk memprediksi apakah suatu ulasan produk akan mempengaruhi keputusan pembelian berdasarkan sentimen ulasan tersebut.

Dataset yang digunakan dalam proyek ini adalah **Amazon Product Reviews Dataset** dari Kaggle, yang dapat diakses melalui tautan berikut:  
[Amazon Product Reviews Dataset](https://www.kaggle.com/datasets/yasserh/amazon-product-reviews-dataset)

**Referensi terkait**:  
- "Sentiment Analysis and Opinion Mining" oleh Bing Liu, seorang pakar dalam analisis sentimen.
- Riset sebelumnya menunjukkan bahwa ulasan pelanggan secara langsung mempengaruhi keputusan pembelian, dengan analisis sentimen yang memungkinkan untuk memahami sentimen pelanggan lebih dalam.

---

## Business Understanding

### **Problem Statements**
1. Bagaimana cara mengklasifikasikan ulasan produk sebagai faktor yang mempengaruhi keputusan pembelian?
2. Algoritma machine learning mana yang paling efektif dalam memprediksi sentimen ulasan produk?

### **Goals**
- Mengembangkan model machine learning yang dapat mengklasifikasikan ulasan produk sebagai **positif** atau **negatif** berdasarkan sentimen yang terkandung di dalamnya.
- Mengevaluasi berbagai algoritma machine learning untuk menentukan model terbaik berdasarkan metrik evaluasi seperti **accuracy**, **precision**, **recall**, dan **F1-score**.

### **Solution Statement**
Untuk mencapai tujuan ini, dilakukan pendekatan berikut:
1. Menggunakan **TF-IDF** untuk mengubah teks ulasan menjadi representasi numerik yang dapat diproses oleh algoritma machine learning.
2. Mencoba beberapa model **machine learning** seperti **Logistic Regression**, **Naïve Bayes**, dan **Random Forest** untuk menentukan algoritma yang paling efektif dalam memprediksi sentimen ulasan.

---

## **Data Understanding**

Dataset yang digunakan dalam proyek ini adalah **Amazon Product Reviews Dataset**, yang berisi **27 kolom** ulasan produk Amazon. Dataset ini diunduh dari platform Kaggle melalui tautan berikut: [Amazon Product Reviews Dataset](https://www.kaggle.com/datasets/yasserh/amazon-product-reviews-dataset).

### Kondisi Data

Sebelum dilakukan analisis lebih lanjut, dataset dievaluasi untuk mengidentifikasi potensi masalah kualitas data. Berikut adalah temuan-temuan kunci:

*   **Missing Value**: Terdapat nilai-nilai yang hilang (*missing value*) pada beberapa kolom, terutama pada kolom `reviews.text` dan `reviews.rating`. Proporsi *missing value* bervariasi antar kolom.
*   **Data Duplikat**: Ditemukan adanya data duplikat dalam dataset. Data duplikat adalah baris dengan nilai yang persis sama di seluruh kolom.
*   **Outlier**: Identifikasi *outlier* atau nilai ekstrem dilakukan pada kolom `reviews.rating`. Beberapa ulasan memiliki rating yang tidak biasa, seperti rating 1 bintang untuk produk yang direkomendasikan.

### Deskripsi Fitur

Berikut adalah deskripsi lengkap untuk setiap fitur dalam dataset:

*   **`reviews.text`**: Berisi teks ulasan yang ditulis oleh pelanggan. Fitur ini merupakan data teks utama yang digunakan dalam analisis sentimen.
*   **`reviews.rating`**: Menunjukkan skor rating yang diberikan oleh pelanggan, dengan skala 1 hingga 5 bintang. Rating ini digunakan untuk membuat label sentimen.
*   **`reviews.doRecommend`**: Menunjukkan apakah pelanggan merekomendasikan produk tersebut (True/False). Fitur ini dapat digunakan sebagai label alternatif atau fitur tambahan dalam analisis.
*   **`reviews.numHelpful`**: Menunjukkan jumlah orang yang menganggap ulasan tersebut bermanfaat.
*   **`reviews.date`**: Tanggal ulasan dipublikasikan.
*   **`reviews.title`**: Judul ulasan.
*   **`reviews.username`**: Nama pengguna yang menulis ulasan.
*   **`sentiment`**: Label sentimen yang diturunkan dari `reviews.rating`. Ulasan dikategorikan sebagai **"Positif"** jika rating lebih dari 3, dan **"Negatif"** jika rating 3 atau kurang.

### Visualisasi Data

Beberapa visualisasi awal, seperti yang ditunjukkan pada Gambar 1.1 dan 1.2, memberikan gambaran tentang distribusi ulasan berdasarkan rating dan sentimen, serta frekuensi kata-kata yang muncul dalam ulasan. Visualisasi ini membantu memahami keseimbangan kelas dan pola teks dalam dataset.

---

## **Data Preparation**

Tahap persiapan data adalah fondasi penting dalam proyek ini. Berikut adalah tahapan-tahapan yang telah dilakukan secara rinci dan berurutan:

### Data Cleaning

Langkah awal melibatkan pembersihan data untuk memastikan kualitas dan konsistensi.

- **Menghapus Data Kosong:** Baris dengan nilai kosong (*missing value*) pada kolom `reviews.text` (teks ulasan) dan `reviews.rating` (rating) dihapus menggunakan fungsi `dropna()`. Hal ini penting untuk menghindari error dan memastikan data yang diolah lengkap.

### Text Preprocessing

Tahap ini bertujuan untuk menyiapkan teks ulasan agar lebih mudah diolah oleh model NLP.

- **Case Folding:** Seluruh teks ulasan diubah menjadi huruf kecil menggunakan fungsi `lower()`. Tujuannya adalah untuk menyeragamkan teks dan menghindari perbedaan interpretasi kata hanya karena kapitalisasi (misalnya, "Bagus" dan "bagus" dianggap sama).
- **Membersihkan Tanda Baca:** Tanda baca seperti koma, titik, tanda seru, dan lain-lain dihapus dari teks menggunakan *regular expression* atau ekspresi reguler. Tanda baca seringkali tidak memiliki nilai informasi dalam analisis sentimen.
- **Tokenisasi:** Teks ulasan dipecah menjadi unit kata individual atau token menggunakan fungsi `word_tokenize()` dari library NLTK. Tokenisasi ini menghasilkan daftar kata yang lebih mudah dianalisis.
- **Stop Words Removal:** Kata-kata umum yang sering muncul tetapi kurang informatif seperti "the", "a", "is", dan "are" dihapus menggunakan daftar *stop words* yang tersedia di NLTK. Penghapusan *stop words* membantu memfokuskan analisis pada kata-kata yang lebih bermakna.
- **Stemming:** Setiap kata diubah ke bentuk dasarnya atau *stem* menggunakan algoritma Porter Stemmer. *Stemming* bertujuan untuk menyatukan kata-kata yang memiliki akar kata yang sama (misalnya, "running", "runs", dan "run" menjadi "run").

![download](https://github.com/user-attachments/assets/d8d79188-ec94-41a1-85c1-34e559fb61e5)


### Labeling (Pelabelan)

Tahap ini membuat label sentimen berdasarkan rating ulasan.

- **Membuat Kolom Sentimen:** Kolom baru bernama `sentiment` dibuat berdasarkan nilai pada kolom `reviews.rating`. Ulasan dengan rating lebih dari 3 diberi label "positive", sedangkan ulasan dengan rating 3 atau kurang diberi label "negative".

### Feature Engineering (Rekayasa Fitur)

Tahap ini mengubah teks yang telah diproses menjadi fitur numerik yang dapat dipahami oleh model *machine learning*.

- **TF-IDF Vectorization:** Teks ulasan yang telah diproses diubah menjadi representasi numerik menggunakan metode *Term Frequency-Inverse Document Frequency* (TF-IDF). TF-IDF memberikan bobot pada setiap kata berdasarkan frekuensi kemunculannya dalam dokumen dan seberapa jarang kata tersebut muncul di seluruh korpus.

### Data Splitting (Pembagian Data)

Dataset yang telah diproses dan direkayasa fiturnya dibagi menjadi dua bagian utama.

- **Data Training:** 80% dari data digunakan untuk melatih model *machine learning*.
- **Data Testing:** 20% dari data digunakan untuk menguji kinerja model setelah dilatih. Pembagian data ini menggunakan fungsi `train_test_split` dari library Scikit-learn.

Dengan mengikuti tahapan persiapan data yang runtut dan lengkap ini, data menjadi lebih siap untuk digunakan dalam pemodelan *machine learning* untuk analisis sentimen.


![download](https://github.com/user-attachments/assets/b43fd993-63fc-4236-85e4-fd578653afa2)



---

## Modeling

Pada tahap ini, tiga model *machine learning* dievaluasi untuk memprediksi sentimen ulasan. Kode yang digunakan untuk pelatihan dan evaluasi model adalah sebagai berikut:

```python
models = {
    "Logistic Regression": LogisticRegression(),
    "Naïve Bayes": MultinomialNB(),
    "Random Forest": RandomForestClassifier()
}

best_model = None
best_accuracy = 0

for name, model in models.items():
    model.fit(X_train_tfidf, y_train)
    y_pred = model.predict(X_test_tfidf)
    acc = accuracy_score(y_test, y_pred)
    print(f"{name} Accuracy: {acc:.4f}")
    print(classification_report(y_test, y_pred))

    if acc > best_accuracy:
        best_accuracy = acc
        best_model = model
```

### 1. Regresi Logistik (Logistic Regression)

*   **Cara Kerja:** Regresi Logistik adalah model linier yang digunakan untuk klasifikasi biner. Model ini bekerja dengan memprediksi probabilitas sebuah *instance* termasuk dalam kelas tertentu menggunakan fungsi logistik. Batas keputusan kemudian ditetapkan untuk mengklasifikasikan *instance* ke dalam salah satu kelas.
*   **Parameter:**
    *   Semua parameter yang digunakan adalah *default*.

### 2. Naive Bayes (MultinomialNB)

*   **Cara Kerja:** Multinomial Naive Bayes adalah model probabilistik yang didasarkan pada teorema Bayes. Model ini sangat efisien untuk klasifikasi teks karena mengasumsikan bahwa fitur (kata-kata) dalam dokumen independen satu sama lain. Model ini menghitung probabilitas setiap kelas diberikan fitur dan memilih kelas dengan probabilitas tertinggi.
*   **Parameter:**
    *   Semua parameter yang digunakan adalah *default*.

### 3. Random Forest

*   **Cara Kerja:** Random Forest adalah model *ensemble* yang menggunakan banyak pohon keputusan untuk membuat prediksi. Setiap pohon dilatih pada subset data yang berbeda dan menggunakan fitur yang berbeda. Prediksi akhir dibuat dengan menggabungkan prediksi dari semua pohon, biasanya melalui *voting* mayoritas untuk klasifikasi.
*   **Parameter:**
    *   Semua parameter yang digunakan adalah *default*.
 
![download](https://github.com/user-attachments/assets/a52f6e4b-385f-49a6-acda-9eb4bdc57f3f)



### Proses Pelatihan

1.  Model dilatih menggunakan data pelatihan yang telah diubah menjadi representasi TF-IDF (`X_train_tfidf`) dan label pelatihan (`y_train`) dengan perintah `model.fit(X_train_tfidf, y_train)`.
2.  Model yang telah dilatih kemudian digunakan untuk membuat prediksi pada data uji TF-IDF (`X_test_tfidf`) dengan perintah `y_pred = model.predict(X_test_tfidf)`.
3.  Akurasi model dihitung menggunakan `accuracy_score(y_test, y_pred)` dan ditampilkan.
4.  Laporan klasifikasi, termasuk *precision*, *recall*, dan *F1-score*, ditampilkan menggunakan `classification_report(y_test, y_pred)`.
5.  Model dengan akurasi tertinggi disimpan sebagai `best_model`.

---

## Evaluasi

Beberapa metrik evaluasi digunakan untuk mengukur kinerja model secara komprehensif:

### Metrik Evaluasi

*   **Akurasi (Accuracy):** Persentase prediksi yang benar dari total prediksi. Mengukur seberapa sering model membuat prediksi yang tepat.
*   **Presisi (Precision):** Proporsi prediksi positif yang benar dari semua prediksi positif. Mengukur seberapa akurat model dalam memprediksi sentimen positif.
*   **Recall (Recall):** Proporsi prediksi positif yang benar dari semua data positif yang sebenarnya. Mengukur seberapa baik model dalam menemukan semua ulasan positif yang sebenarnya.
*   **F1-Score (F1-Score):** Rata-rata harmonik antara presisi dan *recall*. Memberikan ukuran keseimbangan antara presisi dan *recall*.

### Hasil Evaluasi

![download](https://github.com/user-attachments/assets/16fd767e-5799-4f24-8310-9679aeb4afca)


| Model                | Akurasi | Presisi | *Recall*  | F1-Score |
| ---------------------- | -------- | ------- | --------- | -------- |
| Regresi Logistik        | 87.7%    | 87.3%     | 100%    | 93.2%    |
| *Naive Bayes*           | 87.2%    | 86.9%     | 100%    | 93%      |
| *Random Forest*         | 89.8%    | 89.6%     | 99.5%   | 94.3%    |

Berdasarkan hasil evaluasi, model *Random Forest* menunjukkan performa terbaik dengan akurasi **89.8%** dan F1-Score tertinggi **94.3%**. Model ini dipilih sebagai model terbaik dalam proyek ini.

### Dampak Terhadap *Business Understanding*

Model *Random Forest* yang dihasilkan dalam proyek ini memiliki potensi besar untuk memberikan dampak positif bagi bisnis. Dengan kemampuan untuk mengklasifikasikan sentimen ulasan produk secara akurat, bisnis dapat memperoleh wawasan berharga tentang preferensi dan opini pelanggan. Informasi ini dapat digunakan untuk:

*   **Meningkatkan Kualitas Produk:** Ulasan negatif yang diidentifikasi oleh model dapat memberikan masukan berharga untuk perbaikan produk.
*   **Meningkatkan Kepuasan Pelanggan:** Dengan memahami sentimen pelanggan, bisnis dapat menyesuaikan layanan dan produk mereka untuk memenuhi harapan pelanggan.
*   **Mengoptimalkan Strategi Pemasaran:** Ulasan positif dapat digunakan dalam kampanye pemasaran untuk menyoroti keunggulan produk.

### Keterkaitan dengan *Problem Statement*, *Goals*, dan *Solution Statement*

Model *Random Forest* yang dihasilkan secara efektif menjawab *problem statement* kedua, yaitu "Algoritma *machine learning* mana yang paling efektif dalam memprediksi sentimen ulasan produk?". Model ini terbukti menjadi yang terbaik di antara model yang dievaluasi berdasarkan metrik evaluasi yang relevan.

Model ini juga mencapai *goals* yang ditetapkan, yaitu mengembangkan model *machine learning* yang dapat mengklasifikasikan ulasan produk sebagai positif atau negatif, dan mengevaluasi berbagai algoritma untuk menentukan model terbaik.

Solusi yang diusulkan, yaitu menggunakan TF-IDF untuk representasi numerik teks dan mencoba beberapa model *machine learning*, terbukti efektif. TF-IDF memungkinkan model untuk memahami konteks kata dalam ulasan, dan pemilihan model *Random Forest* sebagai yang terbaik menghasilkan performa yang optimal.

### Deployment

Model terbaik, yaitu *Random Forest*, telah di-*deploy* sebagai REST API menggunakan Flask. API ini memungkinkan aplikasi lain untuk mengakses dan menggunakan model untuk prediksi sentimen.

#### API Endpoint

*   **Endpoint:** `/predict`
*   ***Request***:

```json
POST /predict
{
  "review": "Produk ini sangat bagus!"
}
```

*   **Response**:

```json
{
  "review": "Produk ini sangat bagus!",
  "sentiment": "positive"
}
```

#### Opsi Deployment

Beberapa opsi *deployment* yang umum meliputi:

*   **Heroku:** Platform *cloud* yang mudah digunakan untuk *deployment* aplikasi sederhana.
*   **AWS:** Platform *cloud* dengan berbagai layanan untuk *deployment* dan skalabilitas aplikasi.
*   **Google Cloud Run:** Platform *serverless* untuk *deployment* dan eksekusi *container*.

#### Penanganan Error

*   **Validasi Input:** API memastikan input teks ulasan sesuai format yang diinginkan.
*   **Logging:** Semua *request* dan *error* dicatat untuk memudahkan *debugging*.


---

## Kesimpulan

Proyek ini berhasil membangun sistem analisis sentimen untuk ulasan produk Amazon menggunakan teknik Machine Learning. Model terbaik yang digunakan adalah **Random Forest**, dengan akurasi **89.8%**. Sistem ini memberikan wawasan berharga bagi bisnis e-commerce untuk memahami sentimen pelanggan.

### **Pengembangan Masa Mendatang**:
- Eksplorasi model **Deep Learning** dan **Transformer-based models** seperti **BERT** untuk meningkatkan performa prediksi.

---

## Tantangan yang Dihadapi

Beberapa tantangan yang dihadapi dalam proyek ini meliputi:
- **Kualitas Data**: Teks yang tidak relevan atau informal sulit diproses dengan baik.
- **Ketidakseimbangan Kelas**: Data ulasan positif lebih banyak dibandingkan ulasan negatif.
- **Keterbatasan Sumber Daya**: Penggunaan model Deep Learning membutuhkan banyak data dan daya komputasi.

---

🚀 **Proyek ini siap untuk tahap implementasi lebih lanjut!**

- **Real-time sentiment analysis** dapat meningkatkan pengalaman pelanggan.
- Sistem ini dapat diintegrasikan ke dalam **sistem rekomendasi produk** untuk bisnis e-commerce.

---
