# Penerapan Algoritma LexRank dalam Peringkasan Otomatis Teks Dokumen Akademik

Thesis project — Silvana Maretha Br. Simbolon, Universitas Negeri Medan (2026)

---

## Overview

Dokumen regulatif akademik memiliki struktur yang berbeda dari teks naratif biasa, kalimatnya berdiri sendiri, padat informasi, dan minim konteks antar-kalimat. Penelitian ini menguji sejauh mana algoritma Continuous LexRank mampu mengekstrak kalimat-kalimat paling sentral dari dokumen Pedoman Akademik Unimed 2019–2020 yang terdiri dari 1.414 kalimat.

LexRank bekerja dengan membangun graf kemiripan antar kalimat menggunakan IDF-Modified Cosine Similarity, lalu meranking kalimat menggunakan PageRank. Tidak ada pemahaman semantik, murni berbasis kemiripan leksikal. Itu yang membuat hasilnya menarik sekaligus terbatas.

---

## Pipeline

```
PDF (Scanned) → OCR (Tesseract) → Preprocessing → LexRank Model → Evaluation
```

1. OCR: Ekstraksi teks dari PDF hasil scan menggunakan Tesseract OCR (Bahasa Indonesia)
2. Preprocessing: Case folding, cleaning, tokenisasi, stopword removal (NLTK), stemming (Sastrawi)
3. Modeling: Perhitungan TF-IDF, IDF-Modified Cosine Similarity, transition matrix, konvergensi PageRank (damping factor 0.85)
4. Evaluasi: Precision, Recall, F1-score terhadap gold summary yang disusun manual dan divalidasi ahli

---

## Results

| Ukuran Ringkasan | Precision | Recall | F1-Score | Keterwakilan Informasi |
| ---------------- | --------- | ------ | -------- | ---------------------- |
| Top-40           | -         | -      | 0.2821   | 57.5%                  |
| Top-70           | -         | -      | 0.3768   | 64.28%                 |

Peningkatan F1-Score dari Top-40 ke Top-70: 33.6%
Validasi ahli: rata-rata skor 3.00/4.00 (kategori "Baik")

---

## Key Finding

Ada gap yang konsisten antara skor komputasional dan penilaian manusia. LexRank memilih kalimat berdasarkan sentralitas graf (seberapa mirip kalimat itu dengan kalimat lain), sementara manusia memilih berdasarkan relevansi semantik (seberapa penting informasi itu secara makna). Dua kriteria ini tidak selalu menghasilkan pilihan yang sama.

---

## Limitations

Penelitian ini secara eksplisit mengakui beberapa keterbatasan:

1. Bias repetitif: LexRank cenderung memilih kalimat yang sering muncul secara leksikal, bukan yang paling informatif secara semantik
2. Dangling reference: Kalimat yang dipilih kadang kehilangan konteks karena kalimat rujukannya tidak ikut terpilih
3. Single validator: Gold summary divalidasi oleh satu ahli, yang merupakan keterbatasan metodologis
4. Single corpus: Hanya diuji pada satu dokumen (Pedoman Akademik Unimed), sehingga generalisabilitas terbatas

---

## Possible Improvements

1. Mengganti atau melengkapi LexRank dengan pendekatan semantik (IndoBERT, SimCSE)
2. Menambahkan post-processing untuk reduksi redundansi (MMR)
3. Memperluas corpus ke dokumen regulatif lain
4. Menambah jumlah validator untuk gold summary yang lebih robust

---

## How to Run

Semua notebook dikembangkan di Google Colab. Untuk menjalankan:

1. Upload notebook ke Google Colab
2. Ikuti instruksi di setiap cell
3. Siapkan file input dokumen dalam format `.txt`

Urutan eksekusi:

1. `OCR_Pedoman_Akademik.ipynb` — hanya diperlukan jika input berupa PDF scan
2. `Preprocessing_Pedoman_Akademik.ipynb`
3. `Continuous_Lexrank_Modelling_PA.ipynb`

---

## Tech Stack

Python · NLTK · Sastrawi · NumPy · Pandas · NetworkX · Matplotlib · Tabulate

---

## Notes

F1-score yang relatif rendah (0.3768) bukan indikasi kegagalan, ini mencerminkan kesulitan inheren dalam mengevaluasi peringkasan ekstraktif pada dokumen regulatif menggunakan exact match. Skor validasi ahli (3.00/4.00) menunjukkan bahwa ringkasan yang dihasilkan tetap dinilai baik secara kualitatif.
