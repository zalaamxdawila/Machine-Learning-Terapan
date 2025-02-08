# Laporan Proyek Machine Learning - Sistem Rekomendasi Game Indie Berdasarkan Mood & Gaya Bermain 🎮🕹️

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

   ```python
   rmse = np.sqrt(mean_squared_error(y_test, y_pred))
   print(f"RMSE Score: {rmse:.2f}")
   ```

   **Hasil:** RMSE menunjukkan bahwa model memiliki tingkat kesalahan yang dapat diterima dalam merekomendasikan game.

2. **Precision-Recall Curve** untuk mengevaluasi relevansi rekomendasi yang diberikan model:

   ```python
   precision, recall, _ = precision_recall_curve(y_test, y_pred_prob)
   plt.plot(recall, precision, marker='.', label='Precision-Recall Curve')
   ```

   **Hasil:** Model memiliki keseimbangan antara *precision* dan *recall* yang baik, menunjukkan bahwa rekomendasi cukup relevan.

3. **Confusion Matrix** untuk mengevaluasi prediksi model:

   ```python
   cm = confusion_matrix(y_test, y_pred)
   sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
   ```

   **Hasil:** Model dapat membedakan game yang layak direkomendasikan dengan cukup baik.

4. **Visualisasi Top Rated Games**:

   ```python
   def plot_top_rated_games():
       top_games = df.sort_values(by='total_ratings', ascending=False).head(10)
       plt.figure(figsize=(12, 6))
       sns.barplot(x=top_games['total_ratings'], y=top_games['name'], palette='viridis')
       plt.xlabel('Total Ratings')
       plt.ylabel('Game Name')
       plt.title('Top 10 Highest Rated Indie Games')
       plt.show()
   ```

   **Hasil:** Grafik menunjukkan game indie dengan rating tertinggi, yang dapat menjadi tambahan referensi bagi sistem rekomendasi.

## Deployment

Sistem rekomendasi ini dideploy menggunakan **Flask API**, yang memungkinkan pengguna mendapatkan rekomendasi game dengan mengirimkan permintaan HTTP ke endpoint tertentu.

### Implementasi API

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/recommend', methods=['GET'])
def recommend():
    game_name = request.args.get('game_name')
    content_recommendations = get_similar_games(game_name)
    knn_recommendations = recommend_games_knn(game_name)
    return jsonify({'content': content_recommendations.to_dict(orient='records') if len(content_recommendations) > 0 else 'Game not found',
                    'collaborative': knn_recommendations.to_dict(orient='records')})

if __name__ == '__main__':
    app.run(debug=True)
```

Dengan API ini, pengguna dapat memperoleh rekomendasi dengan melakukan permintaan ke endpoint `/recommend` dengan parameter `game_name`.

## Kesimpulan

Model rekomendasi ini berhasil memberikan rekomendasi game indie berdasarkan mood dan gaya bermain pengguna dengan pendekatan Content-Based Filtering dan Collaborative Filtering. Content-Based Filtering membantu memahami hubungan antar game dari deskripsi yang ada, sementara Collaborative Filtering memberikan rekomendasi berdasarkan preferensi pemain lain yang memiliki kesamaan.

Evaluasi menunjukkan bahwa model ini memiliki tingkat kesalahan yang dapat diterima dan performa yang cukup baik dalam memberikan rekomendasi yang relevan. Dengan penerapan sistem ini dalam platform game digital, pengguna dapat menemukan game yang lebih sesuai dengan selera mereka, meningkatkan pengalaman bermain, serta memberikan eksposur lebih luas bagi pengembang game indie.

Di masa depan, sistem ini dapat ditingkatkan dengan mempertimbangkan faktor tambahan seperti interaksi waktu nyata, analisis sentimen dari ulasan pengguna, serta integrasi dengan data perilaku pemain untuk menghasilkan rekomendasi yang lebih akurat dan dinamis.
