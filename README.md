# Analisis Sentimen Ulasan Duolingo: Pendekatan Komparatif Machine Learning & Deep Learning

Repositori resmi untuk naskah [konferensi nasional](https://colab.research.google.com/drive/1xJ8k_9vhsVzK5W8qpbTfhLI38GlkpWR7#notebook-main) yang mengevaluasi performa klasifikasi sentimen pada ulasan pengguna [Duolingo](https://3uv7q232ixk-496ff2e9c6d22116-0-colab.googleusercontent.com/outputframe.html?vrz=colab-external_20261111-060054_RC00_1018888155) dari Google Play Store dengan menerapkan protokol pencegahan kebocoran data (*no-leakage protocol*).

---

## Ringkasan Metodologi
1. **Pencegahan Data Leakage:** Pelatihan model menggunakan *Early Stopping* yang memantau *Validation Set* independen, menjaga data uji tetap murni (*out-of-domain holdout*).
2. **Sanitasi Data Uji:** Melakukan deduplikasi teks untuk menjamin independensi sampel (*i.i.d.*) pada 750 ulasan unik yang dikumpulkan pada November 2026.
3. **Model yang Diuji:**
   - *Majority Baseline* (Dummy Classifier - selalu memprediksi kelas mayoritas positif)
   - *Multinomial Naive Bayes* (TF-IDF unigram & bigram)
   - *Logistic Regression* (TF-IDF unigram & bigram dengan regularisasi L2)
   - *Bidirectional LSTM* (BiLSTM dengan *SpatialDropout1D* dan *Embedding* 64-dimensi)

---

## Hasil Evaluasi Metrik Komparatif
Berikut adalah hasil pengujian performa model pada dataset eksternal Google Play Store ($n = 750$):

| Model | Accuracy | Balanced Acc | Macro Precision | Macro Recall | Macro F1 | F1 Negatif | F1 Positif |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Majority Baseline** | 0.885 | 0.500 | 0.443 | 0.500 | 0.470 | 0.000 | 0.939 |
| **Multinomial NB** | 0.887 | 0.845 | 0.737 | 0.845 | 0.774 | 0.615 | 0.934 |
| **Logistic Regression** | **0.928** | **0.888** | **0.810** | **0.888** | **0.843** | **0.727** | **0.959** |
| **BiLSTM** | 0.919 | 0.883 | 0.791 | 0.883 | 0.828 | 0.702 | 0.953 |

*Uji statistik McNemar menunjukkan tidak ada perbedaan signifikan antara Logistic Regression dan BiLSTM ($p = 0.189$). Oleh karena itu, model linier yang lebih murah dan mudah diinterpretasikan (*Logistic Regression*) sangat memadai untuk tugas ini.*

---

## Prasyarat & Instalasi
Pastikan pustaka berikut terinstal di lingkungan Python/Google Colab Anda:
```bash
pip install numpy pandas scikit-learn tensorflow matplotlib seaborn
