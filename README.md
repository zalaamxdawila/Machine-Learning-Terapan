# Laporan Proyek Machine Learning - M Dicky Desriansyah

## Project Overview

### Sistem Rekomendasi Game Indie 🎮🕹️

Game adalah bentuk hiburan yang sangat populer dengan berbagai genre dan pengalaman bermain. Pemain memiliki preferensi unik berdasarkan suasana hati (*mood*) dan gaya bermain mereka. Oleh karena itu, sistem rekomendasi yang dapat memahami kebutuhan pemain sangat diperlukan untuk meningkatkan pengalaman bermain.

Sistem rekomendasi ini bertujuan untuk membantu pemain menemukan game indie yang sesuai dengan preferensi mereka, baik berdasarkan *mood* (relaksasi, tantangan, eksplorasi) maupun gaya bermain yang disukai. Dengan menggunakan teknik *Natural Language Processing* (NLP) dan *Collaborative Filtering*, sistem ini dapat memberikan rekomendasi yang lebih personal.

**Referensi terkait:**  
- [Recommender Systems in Gaming](https://dl.acm.org/doi/10.1145/3340631.3394856)

---

## Business Understanding

### Problem Statements

1. Bagaimana cara merekomendasikan game indie yang sesuai dengan *mood* pengguna berdasarkan deskripsi game dan ulasan pemain?
2. Bagaimana cara memberikan rekomendasi game yang relevan berdasarkan kesamaan gaya bermain antar pengguna?

### Goals

1. Mengembangkan sistem yang dapat memahami *mood* pengguna dari deskripsi dan ulasan game menggunakan teknik NLP.
2. Mengimplementasikan sistem *Collaborative Filtering* untuk memberikan rekomendasi berdasarkan kesamaan preferensi pemain.

### Solution Approach

Untuk mencapai tujuan di atas, sistem rekomendasi dikembangkan menggunakan dua pendekatan utama:

1. **Content-Based Filtering (NLP Sentiment Analysis)**  
   - Memanfaatkan *TF-IDF Vectorization* dan *Cosine Similarity* untuk memahami hubungan antar game berdasarkan deskripsi dan ulasan pengguna.

---

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah *Steam Store Games Dataset* dari Kaggle. Dataset ini berisi informasi tentang berbagai game yang tersedia di platform Steam.

### Sumber Data
- Dataset: [*Steam Store Games Dataset*](https://www.kaggle.com/datasets/nikdavis/steam-store-games)

### Struktur Dataset
Dataset yang digunakan terdiri dari tiga file:

1. **steam.csv**
   - Jumlah data: 27.075 baris, 18 kolom
   - Kondisi data: Terdapat *missing values* pada beberapa kolom seperti `release_date`, `english`, dan `required_age`.
   - Fitur utama: `appid`, `name`, `release_date`, `genres`, `positive_ratings`, `negative_ratings`, `price`.

2. **steam_description_data.csv**
   - Jumlah data: 27334 baris, 4 kolom
   - Fitur utama: `steam_appid`, `short_description`, `detailed_description`, `about_the_game`.

3. **steamspy_tag_data.csv**
   - Jumlah data: 29022 baris, 372 kolom
   - Kondisi data: Sebagian besar kolom berisi jumlah tag yang diberikan oleh pengguna.

### Analisis Awal
Visualisasi awal dilakukan untuk memahami distribusi rating, genre, dan popularitas game indie.

---

## Data Preparation

Langkah-langkah *data preparation* yang dilakukan:

1. **Merging Data**: Menggabungkan informasi dari tiga file dataset (`steam.csv`, `steam_description_data.csv`, dan `steamspy_tag_data.csv`) berdasarkan `appid` untuk mendapatkan dataset yang lebih komprehensif.
2. **Filtering Indie Games**: Memfilter data untuk hanya menyertakan game dengan genre *Indie*.
3. **Data Cleaning**:
   - Membersihkan teks pada kolom `genres` dan `detailed_description`.
   - Menghapus *stop words* dan tanda baca.
   - Mengubah teks menjadi huruf kecil.
4. **Feature Engineering**:
   - Membuat fitur baru `total_ratings` dengan mengurangkan `negative_ratings` dari `positive_ratings`.
5. **TF-IDF Vectorization**:
   - Menerapkan *TF-IDF Vectorization* pada kolom `detailed_description` untuk mengukur relevansi kata dalam deskripsi game relatif terhadap keseluruhan dokumen.
   - Ekstraksi fitur dengan *TF-IDF* (*Term Frequency-Inverse Document Frequency*) adalah metode pemrosesan data yang dilakukan untuk mengukur pentingnya sebuah kata dalam sebuah dokumen relatif terhadap kumpulan dokumen lainnya.
6. **Perhitungan Cosine Similarity**:
   - Digunakan untuk mengukur kemiripan antara deskripsi game berdasarkan vektor *TF-IDF*.

---

## Modeling and Results

Sistem rekomendasi ini dibangun menggunakan pendekatan *Content-Based Filtering* dengan memanfaatkan *Cosine Similarity* untuk menemukan game yang memiliki kemiripan konten dengan game yang disukai pengguna.

### Cara Kerja Sistem Rekomendasi

1. **Ekstraksi Fitur dan Pembobotan**: Deskripsi setiap game diubah menjadi representasi numerik menggunakan metode *Term Frequency-Inverse Document Frequency* (TF-IDF). TF-IDF memberikan bobot pada setiap kata dalam deskripsi game, yang mencerminkan seberapa penting kata tersebut dalam game tertentu dan dalam keseluruhan koleksi game.
2. **Perhitungan Cosine Similarity**: Setelah deskripsi game diubah menjadi vektor TF-IDF, *cosine similarity* dihitung antara setiap pasangan game. *Cosine similarity* mengukur kemiripan antara dua vektor berdasarkan sudut di antara keduanya. Semakin tinggi nilai *cosine similarity*, semakin mirip kedua game tersebut.
3. **Pemberian Rekomendasi**: Ketika pengguna ingin mendapatkan rekomendasi game yang mirip dengan game tertentu, sistem akan mencari game lain dengan nilai *cosine similarity* tertinggi terhadap game tersebut. Game-game dengan nilai similarity tertinggi kemudian direkomendasikan kepada pengguna.

### Cara Kerja
1. Representasi teks game menggunakan TF-IDF.
2. Perhitungan *Cosine Similarity* untuk menemukan game yang memiliki kemiripan konten.
3. Game dengan nilai *similarity* tertinggi direkomendasikan kepada pengguna.

### Contoh Rekomendasi untuk *Hollow Knight*

![download](https://github.com/user-attachments/assets/31abf9dc-0c0d-42f7-86a3-db9cf27e21b4)

| Rank | Game | Genres |
|------|-----------------------|--------------------------|
| 1 | Omega Strike | Action, Adventure, Indie |
| 2 | SWARMRIDER OMEGA | Action, Indie |
| 3 | Ares Omega | Action, Indie, RPG |
| 4 | ATOMEGA | Action, Indie |
| 5 | Survivor of Eschewal | Action, Adventure, Indie |

### Kelebihan dan Kekurangan *Content-Based Filtering*

| Pendekatan | Kelebihan | Kekurangan |
|------------|--------------------------------------|------------------------------------------------|
| Content-Based Filtering | Relevansi tinggi dengan deskripsi game | Tidak dapat merekomendasikan game baru tanpa deskripsi yang kaya |
| | Mudah diimplementasikan | Terbatas pada informasi deskripsi game |

---

# Evaluation

Sistem rekomendasi dievaluasi kinerjanya menggunakan dua metrik, yaitu **Precision@5** dan **Recall@5**.

- **Precision@5** mengukur seberapa akurat sistem dalam merekomendasikan game yang relevan di antara 5 game teratas yang direkomendasikan.  
- **Recall@5** mengukur seberapa lengkap sistem dalam merekomendasikan game yang relevan dari keseluruhan game yang relevan yang ada.

Game yang dianggap relevan dalam evaluasi ini adalah game-game yang memiliki total rating (`total_ratings`) di atas **persentil ke-75** dari seluruh game dalam dataset.

## Hasil Evaluasi

Hasil evaluasi untuk game contoh *"Hollow Knight"* adalah sebagai berikut:

![download](https://github.com/user-attachments/assets/9697357d-805f-4f1b-a102-2798e7b2af9e)

| Metrik        | Skor        |
|--------------|------------|
| Precision@5  | 0.2        |
| Recall@5     | 0.000206016 |

### Interpretasi Hasil:

- **Precision@5 = 0.2** → Artinya, 20% dari 5 game teratas yang direkomendasikan untuk *"Hollow Knight"* oleh sistem adalah game yang relevan.  
- **Recall@5 = 0.000206016** → Artinya, sistem hanya merekomendasikan sebagian kecil (sekitar **0.02%**) dari keseluruhan game yang relevan untuk *"Hollow Knight"*.

## Visualisasi Game dengan Rating Tertinggi

Selain metrik **Precision@5** dan **Recall@5**, bagian evaluasi juga menyertakan visualisasi **10 game dengan rating tertinggi** dalam dataset. Visualisasi ini dibuat menggunakan fungsi `plot_top_rated_games()`:

![download](https://github.com/user-attachments/assets/7b898c7f-febd-4c00-a153-af9a5ab85b48)

Fungsi ini mengurutkan game berdasarkan total rating (total_ratings) secara menurun (descending) dan mengambil 10 game teratas. Kemudian, fungsi ini membuat bar plot menggunakan library seaborn untuk menampilkan game-game tersebut beserta total rating mereka. Visualisasi ini memberikan gambaran tentang game-game yang paling populer dan memiliki rating tertinggi dalam dataset.


---

## Deployment

Sistem rekomendasi di-*deploy* menggunakan *Flask API*.

### API Endpoint
**Endpoint**: `/recommend`  
**Metode**: `GET`  
**Parameter**: `game_name` (wajib)

**Contoh Permintaan:**
```http
GET /recommend?game_name=Stardew Valley
```

**Contoh Respons:**
```json
{
  "content": [
    {"name": "Terraria", "genres": "Action, Adventure, Indie, RPG"},
    {"name": "Starbound", "genres": "Action, Adventure, Indie, RPG, Simulation"}
  ]
}
```

### Opsi *Deployment*
- **Heroku**
- **AWS**
- **Google Cloud Run**

### Penanganan Error
- **Validasi Input**: API memastikan input game sesuai format yang diinginkan.
- **Logging**: Semua *request* dan *error* dicatat untuk memudahkan *debugging*.

---

## Kesimpulan

Proyek ini berhasil membangun sistem rekomendasi game indie berdasarkan *mood* dan gaya bermain pemain. Sistem ini menggunakan pendekatan *Content-Based Filtering* dan *Collaborative Filtering* untuk memberikan rekomendasi yang lebih personal.

### Rencana Pengembangan Selanjutnya
- Menggunakan model *Deep Learning* seperti *Transformers* untuk meningkatkan pemahaman teks.
- Memanfaatkan data perilaku pemain.
- Mengembangkan fitur rekomendasi berbasis *real-time* untuk pengalaman pengguna yang lebih dinamis.

