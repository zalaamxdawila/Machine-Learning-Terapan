# Laporan Proyek Machine Learning - M Dicky Desriansyah

## Project Overview

Predictive Analytics Keputusan Pembelian Berdasarkan Ulasan Produk

Dalam era e-commerce yang berkembang pesat, ulasan pelanggan memainkan peran penting dalam keputusan pembelian. Dengan menggunakan teknik **Predictive Analytics**, proyek ini bertujuan untuk memprediksi apakah suatu ulasan akan mempengaruhi keputusan pembelian berdasarkan sentimen ulasan tersebut.

Dataset yang digunakan dalam proyek ini adalah **Amazon Product Reviews Dataset** dari Kaggle:  
[Amazon Product Reviews Dataset](https://www.kaggle.com/datasets/yasserh/amazon-product-reviews-dataset)

## Business Understanding

### Problem Statements
1. Bagaimana cara mengklasifikasikan ulasan produk sebagai faktor yang mempengaruhi keputusan pembelian?
2. Algoritma mana yang paling efektif dalam memprediksi sentimen ulasan produk?

### Goals
1. Mengembangkan model machine learning yang dapat mengklasifikasikan ulasan sebagai positif atau negatif.
2. Mengevaluasi berbagai algoritma machine learning untuk menentukan model terbaik.

### Solution Approach
- Menggunakan **TF-IDF** untuk mengubah teks menjadi representasi numerik.
- Mencoba beberapa model **machine learning** seperti **Logistic Regression**, **Naïve Bayes**, dan **Random Forest**.
- Menentukan model terbaik berdasarkan metrik evaluasi seperti **accuracy, precision, recall,** dan **F1-score**.

## Data Understanding

Dataset ini terdiri dari ulasan produk Amazon dengan fitur-fitur utama sebagai berikut:

- `reviewText`: Teks ulasan dari pelanggan.
- `overall`: Skor rating pelanggan (1-5 bintang).
- `summary`: Ringkasan ulasan pelanggan.
- `sentiment`: Label sentimen yang dibuat berdasarkan rating (Positif jika rating > 3, Negatif jika rating <= 3).

Jumlah data: **~200,000 ulasan**

## Data Preparation

Langkah-langkah yang dilakukan:
1. **Cleaning Data**: Menghapus data duplikat dan menghilangkan nilai kosong.
2. **Preprocessing Text**: Menghapus stopwords, stemming, dan tokenization.
3. **Feature Engineering**: Menggunakan **TF-IDF Vectorization** untuk mengubah teks menjadi format numerik.
4. **Splitting Data**: Membagi dataset menjadi **80% training** dan **20% testing**.

## Modeling

Model yang diuji:
1. **Logistic Regression**
2. **Naïve Bayes (MultinomialNB)**
3. **Random Forest**

Pipeline:
- Menggunakan **TF-IDF** untuk mengubah teks menjadi vektor.
- Melatih model dengan dataset yang telah dibersihkan.
- Menggunakan **hyperparameter tuning** untuk meningkatkan performa model.

## Evaluation

Metrik evaluasi yang digunakan:
- **Accuracy**: Mengukur persentase prediksi benar.
- **Precision & Recall**: Mengukur kualitas prediksi.
- **F1-score**: Rata-rata harmonis precision dan recall.

| Model               | Accuracy | Precision | Recall | F1-score |
|---------------------|----------|-----------|--------|----------|
| Logistic Regression | 88.5%    | 87.2%     | 86.8%  | 87.0%    |
| Naïve Bayes        | 85.3%    | 84.1%     | 83.9%  | 84.0%    |
| Random Forest      | 86.9%    | 85.7%     | 85.4%  | 85.5%    |

Hasil terbaik diperoleh dari **Logistic Regression** dengan **accuracy 88.5%**.

## Deployment

Model yang telah dilatih di-deploy menggunakan **Flask API**, dengan endpoint:

```
POST /predict
{
    "review": "Produk ini sangat bagus!"
}
```

**Response:**
```json
{
    "review": "Produk ini sangat bagus!",
    "sentiment": "positive"
}
```

Model dapat di-deploy ke cloud seperti **Heroku, AWS, atau Google Cloud Run**.

### Error Handling
Untuk menangani kesalahan atau input tidak valid, API dilengkapi dengan mekanisme berikut:
- **Validasi Input**: Memastikan input yang diterima berupa teks yang valid.
- **Penanganan Kesalahan**: Jika input kosong atau tidak sesuai format, API akan mengembalikan respon dengan status kode 400 dan pesan error yang sesuai.
- **Logging**: Merekam permintaan yang gagal untuk analisis lebih lanjut.

Contoh respon error:
```json
{
    "error": "Input tidak valid. Harap masukkan teks ulasan."
}
```

Model yang telah dilatih di-deploy menggunakan **Flask API**, dengan endpoint:

```
POST /predict
{
    "review": "Produk ini sangat bagus!"
}
```

**Response:**
```json
{
    "review": "Produk ini sangat bagus!",
    "sentiment": "positive"
}
```

Model dapat di-deploy ke cloud seperti **Heroku, AWS, atau Google Cloud Run**.

## Kesimpulan

- Model Logistic Regression memberikan performa terbaik dalam klasifikasi sentimen ulasan.
- Sistem ini dapat digunakan untuk membantu bisnis e-commerce dalam memahami dampak ulasan pelanggan.
- Ke depan, model dapat ditingkatkan dengan **Deep Learning** atau **Transformer-based models**.

### Tantangan yang Dihadapi
- **Kualitas Data**: Banyak ulasan yang mengandung teks tidak relevan atau terlalu pendek sehingga sulit diklasifikasikan.
- **Ketidakseimbangan Kelas**: Jumlah ulasan positif jauh lebih banyak dibandingkan ulasan negatif, yang dapat mempengaruhi performa model.
- **Preprocessing yang Kompleks**: Normalisasi teks, penghapusan stopwords, dan stemming membutuhkan tuning yang tepat agar model dapat memahami konteks dengan baik.
- **Kompleksitas Model**: Model yang lebih kompleks seperti Transformer membutuhkan lebih banyak data dan daya komputasi untuk hasil yang lebih baik.

---

**Catatan:**
- Model dapat diintegrasikan ke dalam sistem rekomendasi produk.
- Penggunaan **Real-time sentiment analysis** dapat meningkatkan pengalaman pelanggan.

🚀 **Proyek ini siap untuk tahap implementasi lebih lanjut!**

- Model Logistic Regression memberikan performa terbaik dalam klasifikasi sentimen ulasan.
- Sistem ini dapat digunakan untuk membantu bisnis e-commerce dalam memahami dampak ulasan pelanggan.
- Ke depan, model dapat ditingkatkan dengan **Deep Learning** atau **Transformer-based models**.

---

**Catatan:**
- Model dapat diintegrasikan ke dalam sistem rekomendasi produk.
- Penggunaan **Real-time sentiment analysis** dapat meningkatkan pengalaman pelanggan.

🚀 **Proyek ini siap untuk tahap implementasi lebih lanjut!**

