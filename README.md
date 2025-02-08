# Laporan Proyek Machine Learning - Sistem Rekomendasi Game Indie 🎮🕹️

## Project Overview

Game adalah bentuk hiburan yang sangat populer dengan berbagai genre dan pengalaman bermain. Pemain memiliki preferensi unik berdasarkan suasana hati (mood) dan gaya bermain mereka. Oleh karena itu, sistem rekomendasi yang dapat memahami kebutuhan pemain sangat diperlukan untuk meningkatkan pengalaman bermain.

Sistem rekomendasi ini bertujuan untuk membantu pemain menemukan game indie yang sesuai dengan preferensi mereka, baik berdasarkan mood (relaksasi, tantangan, eksplorasi) maupun gaya bermain yang disukai. Dengan menggunakan teknik *Natural Language Processing* (NLP) dan *Collaborative Filtering*, sistem ini dapat memberikan rekomendasi yang lebih personal.

Referensi terkait:
- [Recommender Systems in Gaming](https://dl.acm.org/doi/10.1145/3340631.3394856)

## Business Understanding

### Problem Statements

1. Bagaimana cara merekomendasikan game indie yang sesuai dengan mood pengguna berdasarkan deskripsi game dan ulasan pemain?
2. Bagaimana cara memberikan rekomendasi game yang relevan berdasarkan kesamaan gaya bermain antar pengguna?

### Goals

1. Mengembangkan sistem yang dapat memahami mood pengguna dari deskripsi dan ulasan game menggunakan teknik NLP.
2. Mengimplementasikan sistem *Collaborative Filtering* untuk memberikan rekomendasi berdasarkan kesamaan preferensi pemain.

### Solution Approach

Untuk mencapai tujuan di atas, sistem rekomendasi dikembangkan menggunakan dua pendekatan utama:

1. **Content-Based Filtering (NLP Sentiment Analysis)** - Memanfaatkan *TF-IDF Vectorization* dan *Cosine Similarity* untuk memahami hubungan antar game berdasarkan deskripsi dan ulasan pengguna.
2. **Collaborative Filtering (K-Nearest Neighbors - KNN)** - Menggunakan pendekatan berbasis tetangga terdekat untuk merekomendasikan game yang dimainkan oleh pemain dengan preferensi serupa.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah **Steam Store Games Dataset** dari Kaggle:

- Link: [Steam Store Games Dataset](https://www.kaggle.com/datasets/nikdavis/steam-store-games)

Dataset terdiri dari beberapa file utama:

1. `steam.csv` - Data utama yang mencakup informasi game seperti nama, genre, dan rating.
2. `steam_description_data.csv` - Berisi deskripsi game yang digunakan untuk analisis NLP.
3. `steamspy_tag_data.csv` - Berisi informasi tag untuk memahami preferensi genre.

Visualisasi awal dilakukan untuk memahami distribusi rating, genre, dan popularitas game indie.

## Data Preparation

Beberapa langkah *data preparation* yang dilakukan:

1. **Cleaning Text**: Membersihkan deskripsi game dari karakter khusus dan stopwords.
2. **Merging Data**: Menggabungkan informasi dari beberapa file dataset berdasarkan *appid*.
3. **Filtering Indie Games**: Memfilter hanya game dengan genre "Indie".
4. **Feature Engineering**: Membuat fitur tambahan seperti *total\_ratings*.

## Modeling and Result

Model yang digunakan untuk sistem rekomendasi ini terdiri dari dua pendekatan:

1. **Content-Based Filtering**:
   - Menggunakan *TF-IDF Vectorizer* untuk mengekstrak fitur dari deskripsi game.
   - Menghitung kesamaan antar game menggunakan *Cosine Similarity*.
   - Menghasilkan rekomendasi berdasarkan game yang memiliki deskripsi serupa.
2. **Collaborative Filtering (KNN)**:
   - Menggunakan algoritma *K-Nearest Neighbors (KNN)* untuk mencari game dengan gaya bermain serupa.
   - Menggunakan *Cosine Distance* sebagai metrik kesamaan antar game.

**Kelebihan dan Kekurangan Pendekatan:**

| Pendekatan              | Kelebihan                                                        | Kekurangan                                                       |
| ----------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| Content-Based Filtering | Memberikan rekomendasi yang sangat relevan dengan deskripsi game | Tidak dapat merekomendasikan game baru tanpa deskripsi yang kaya |
| Collaborative Filtering | Memahami hubungan antar pemain berdasarkan preferensi mereka     | Bergantung pada data rating yang cukup untuk bekerja dengan baik |

## Evaluation

Evaluasi dilakukan menggunakan beberapa metrik:

1. **Root Mean Squared Error (RMSE)** untuk mengukur kesalahan model pada prediksi rating:

     RMSE Score: 0.43

   **Hasil:** RMSE menunjukkan bahwa model memiliki tingkat kesalahan yang dapat diterima dalam merekomendasikan game.

2. **Precision-Recall Curve** untuk mengevaluasi relevansi rekomendasi yang diberikan model:

   ![download](https://github.com/user-attachments/assets/83a8f9b2-a341-4011-bcb5-1c8b0d737cf2)

   **Hasil:** Model memiliki keseimbangan antara *precision* dan *recall* yang baik, menunjukkan bahwa rekomendasi cukup relevan.

3. **Confusion Matrix** untuk mengevaluasi prediksi model:

   ![download](https://github.com/user-attachments/assets/0f8d0a0a-cae9-4b21-ab13-22ffca2ba123)

   **Hasil:** Model dapat membedakan game yang layak direkomendasikan dengan cukup baik.

4. **Visualisasi Top Rated Games**:

   ![download](https://github.com/user-attachments/assets/c9e6b651-993a-43ce-a7d0-61e05c4b7bd5)

   **Hasil:** Grafik menunjukkan game indie dengan rating tertinggi, yang dapat menjadi tambahan referensi bagi sistem rekomendasi.
   
## Deployment

Sistem rekomendasi ini dideploy menggunakan **Flask API**, yang memungkinkan pengguna mendapatkan rekomendasi game dengan mengirimkan permintaan HTTP ke endpoint tertentu.

### API Endpoint

- **Endpoint:** `/recommend`
- **Request:**

  ```json
  GET /recommend?game_name=Stardew Valley
  ```

- **Response:**

  ```json
  {
    "content": [ {"name": "Terraria", "genres": "indie adventure"}, ...],
    "collaborative": [ {"name": "Hollow Knight", "genres": "indie action"}, ...]
  }
  ```

### Opsi Deployment

Beberapa opsi *deployment* yang umum meliputi:

- **Heroku:** Platform *cloud* yang mudah digunakan untuk *deployment* aplikasi sederhana.
- **AWS:** Platform *cloud* dengan berbagai layanan untuk *deployment* dan skalabilitas aplikasi.
- **Google Cloud Run:** Platform *serverless* untuk *deployment* dan eksekusi *container*.

### Penanganan Error

- **Validasi Input:** API memastikan input game sesuai format yang diinginkan.
- **Logging:** Semua *request* dan *error* dicatat untuk memudahkan *debugging*.

## Kesimpulan

Proyek ini berhasil membangun sistem rekomendasi game indie berdasarkan mood dan gaya bermain pemain. Dengan menerapkan pendekatan *Content-Based Filtering* menggunakan NLP serta *Collaborative Filtering*, sistem dapat memberikan rekomendasi yang lebih personal dan relevan. 

Evaluasi menunjukkan bahwa model dapat memberikan rekomendasi dengan akurasi yang cukup baik, namun masih ada beberapa keterbatasan, seperti ketergantungan pada kualitas deskripsi game dan data rating pengguna.

Untuk pengembangan lebih lanjut, sistem ini dapat ditingkatkan dengan:
- Menggunakan model *Deep Learning* seperti *Transformers* untuk meningkatkan pemahaman teks.
- Memanfaatkan data perilaku pemain untuk rekomendasi yang lebih akurat.
- Mengembangkan fitur rekomendasi berbasis *real-time* untuk pengalaman pengguna yang lebih dinamis.


