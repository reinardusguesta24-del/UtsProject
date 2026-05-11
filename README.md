
  # Rancangan Antarmuka Pengguna

```mermaid
flowchart TD

    A[Halaman Utama Sistem]

    A --> B[Header Aplikasi]
    B --> B1[Judul Analisis Sentimen]
    B --> B2[Metode Naive Bayes dan TF-IDF]

    A --> C[Statistik Model]
    C --> C1[Akurasi Model]
    C --> C2[Jumlah Data Latih]
    C --> C3[Jumlah Fitur TF-IDF]
    C --> C4[Kelas Sentimen]

    A --> D[Input Teks]
    D --> D1[Textarea Komentar]

    A --> E[Contoh Kalimat]
    E --> E1[Contoh Positif]
    E --> E2[Contoh Negatif]
    E --> E3[Contoh Netral]

    A --> F[Tombol Analisis]

    F --> G[Preprocessing]
    G --> G1[Case Folding]
    G --> G2[Tokenizing]
    G --> G3[Stopword Removal]
    G --> G4[Stemming]

    G4 --> H[TF-IDF]

    H --> I[Naive Bayes Classifier]

    I --> J[Hasil Analisis]

    J --> J1[Sentimen Positif]
    J --> J2[Sentimen Negatif]
    J --> J3[Sentimen Netral]

    J --> K[Confidence Score]

    J --> L[Distribusi Probabilitas]

    J --> M[Token Hasil Preprocessing]

    J --> N[Informasi Pipeline Sistem]
