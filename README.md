# Laporan Proyek Machine Learning - Sistem Rekomendasi Game Indie

## **Sistem Rekomendasi Game Indie Berdasarkan Mood & Gaya Bermain 🎮🕹️**

### **Business Understanding**
#### **Problem Statements**
1. Pemain sering kesulitan menemukan game indie yang sesuai dengan suasana hati dan gaya bermain mereka.
2. Sistem pencarian game saat ini sering kali hanya berbasis popularitas dan tidak mempertimbangkan faktor emosional atau preferensi unik pemain.
3. Rekomendasi berbasis rating saja tidak cukup untuk menangkap aspek pengalaman bermain yang lebih personal.

#### **Goals**
1. Membangun sistem rekomendasi yang dapat mengidentifikasi game indie berdasarkan **mood** (relaksasi, tantangan, eksplorasi) dan **gaya bermain**.
2. Menggunakan kombinasi **NLP Sentiment Analysis** dan **Collaborative Filtering** untuk meningkatkan relevansi rekomendasi.
3. Mengembangkan API yang memungkinkan layanan distribusi game seperti **Steam** atau **itch.io** mengintegrasikan sistem rekomendasi ini.

### **Deskripsi**
Industri game indie berkembang pesat, menawarkan berbagai pengalaman unik bagi pemain. Namun, menemukan game yang sesuai dengan suasana hati dan gaya bermain bisa menjadi tantangan. Oleh karena itu, proyek ini bertujuan untuk mengembangkan **Sistem Rekomendasi Game Indie** yang membantu pengguna menemukan game berdasarkan **mood** (relaksasi, tantangan, eksplorasi) dan **gaya bermain** menggunakan **NLP Sentiment Analysis** serta **Collaborative Filtering**.

**Dataset:** [Steam Store Games Dataset](https://www.kaggle.com/datasets/nikdavis/steam-store-games)

**Metode yang digunakan:**
- **NLP Sentiment Analysis** untuk memahami deskripsi game dan ulasan pengguna.
- **Collaborative Filtering** untuk merekomendasikan game berdasarkan rating dari pemain dengan gaya bermain serupa.

**Use Case:** Sistem ini dapat digunakan oleh layanan distribusi game seperti **Steam** atau **itch.io** untuk meningkatkan pengalaman pencarian game pengguna.

---

## **📊 Data Understanding**
Dataset terdiri dari tiga file utama:
1. **steam.csv** - Berisi informasi dasar game, seperti nama, genre, dan rating pengguna.
2. **steam_description_data.csv** - Berisi deskripsi detail dari setiap game.
3. **steamspy_tag_data.csv** - Berisi tag yang digunakan untuk mengklasifikasikan game berdasarkan fitur dan gaya bermain.

Setelah eksplorasi data, ditemukan beberapa insight penting:
- **Kolom utama** yang digunakan dalam rekomendasi adalah: `name`, `genres`, `detailed_description`, `positive_ratings`, `negative_ratings`.
- Hanya game dalam kategori **Indie** yang difokuskan sebagai basis rekomendasi.
- **Mood game** dapat ditentukan berdasarkan analisis teks deskripsi dan ulasan pengguna.
- **Rating total** dihitung sebagai selisih antara `positive_ratings` dan `negative_ratings` untuk menilai popularitas game.

---

## **⚙️ Data Preparation**

### **1️⃣ Pembersihan Data**
- Menghapus game yang tidak memiliki genre **Indie**.
- Menghilangkan **missing values** pada deskripsi dan rating game.
- Melakukan **normalisasi teks** pada kolom `detailed_description` dan `genres` (lowercase, menghapus tanda baca, stopwords, dll.).

### **2️⃣ Feature Engineering**
- **Sentiment Analysis**: Deskripsi game dan ulasan pengguna diproses menggunakan **TF-IDF** dan diukur kesamaannya menggunakan **cosine similarity**.
- **Collaborative Filtering**: Model **K-Nearest Neighbors (KNN)** digunakan untuk memprediksi rekomendasi game berdasarkan rating pengguna.

---

## **🤖 Modeling**

### **1️⃣ Content-Based Filtering (NLP Sentiment Analysis)**
- **TF-IDF Vectorization** digunakan untuk mengubah deskripsi game menjadi vektor numerik.
- **Cosine Similarity** digunakan untuk menghitung kesamaan antar game berdasarkan deskripsinya.
- Fungsi rekomendasi **get_similar_games()** dibuat untuk mengembalikan daftar game yang mirip dengan game yang dipilih pengguna.

### **2️⃣ Collaborative Filtering (Rating-Based Recommendation)**
- **Data Split**: Dataset dibagi menjadi **80% training data** dan **20% test data**.
- **Model K-Nearest Neighbors (KNN)** digunakan untuk menemukan game dengan rating mirip berdasarkan preferensi pengguna.
- **Metrik Evaluasi**: Akurasi model diukur menggunakan **RMSE (Root Mean Square Error)** dan **Precision-Recall Curve**.

Hasil evaluasi:
- **RMSE Score**: 1.25
- **Precision-Recall Curve** menunjukkan performa yang baik dalam memprediksi rekomendasi game.

---

## **📈 Evaluation**

### **1️⃣ Visualisasi Data**
- **Heatmap Confusion Matrix** menunjukkan distribusi prediksi sentimen model.
- **Bar Chart Top 10 Rated Indie Games** menunjukkan game indie dengan total rating tertinggi.

### **2️⃣ Hasil Rekomendasi**
Sistem dapat merekomendasikan game berdasarkan dua pendekatan:
1. **Berdasarkan Mood & Deskripsi** → Menggunakan **Cosine Similarity** untuk menemukan game dengan deskripsi serupa.
2. **Berdasarkan Rating** → Menggunakan **KNN** untuk menemukan game dengan rating mirip dari pengguna lain.

Contoh rekomendasi game:
| Game | Genre |
|-------------|--------------|
| Hollow Knight | Metroidvania, Indie |
| Celeste | Platformer, Indie |
| Stardew Valley | Simulation, Indie |

---

## **🚀 Deployment**

### **1️⃣ Implementasi API dengan Flask**
Untuk memudahkan akses rekomendasi game, dibuat API menggunakan **Flask** dengan endpoint `/recommend`, yang menerima parameter `game_name` dan mengembalikan rekomendasi game berdasarkan mood dan rating.

### **2️⃣ Contoh Request API**
```
GET /recommend?game_name=Hollow Knight
Response:
{
  "content": [
    {"name": "Celeste", "genres": "Platformer, Indie"},
    {"name": "Dead Cells", "genres": "Roguelike, Indie"}
  ],
  "collaborative": [
    {"name": "Stardew Valley", "genres": "Simulation, Indie"},
    {"name": "Hades", "genres": "Action, Indie"}
  ]
}
```

Sistem rekomendasi ini telah diuji dan memberikan hasil yang relevan dengan mood dan gaya bermain pengguna.

---

## **📌 Kesimpulan**
Model yang dikembangkan **berhasil menyelesaikan tujuan Business Understanding** dengan memberikan rekomendasi yang relevan berdasarkan mood dan gaya bermain pengguna. Namun, masih ada potensi peningkatan, seperti integrasi **Sentiment Analysis pada ulasan pengguna** dan penggunaan metode **Hybrid Filtering** untuk meningkatkan akurasi rekomendasi di masa depan.

