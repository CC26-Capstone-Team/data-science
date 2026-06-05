# 💼 CarPathMu — Data Science Pipeline

> **Capstone Project** · DBS Foundation · 2026  
> Prediksi `job_title` & analisis tren karir berbasis data lowongan kerja nyata.

---

## 📋 Daftar Isi

- [Gambaran Umum](#gambaran-umum)
- [Pertanyaan Bisnis](#pertanyaan-bisnis)
- [Struktur Proyek](#struktur-proyek)
- [Dataset](#dataset)
- [Alur Pipeline](#alur-pipeline)
- [Visualisasi Dashboard](#visualisasi-dashboard)
- [Cara Menjalankan](#cara-menjalankan)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Tim](#tim)

---

## 🎯 Gambaran Umum

CarPathMu adalah platform rekomendasi karir berbasis AI. Repositori ini berisi komponen **Data Science** yang mencakup:

1. **Analisis Data & EDA** — eksplorasi mendalam terhadap 50.000+ lowongan kerja.
2. **Analisis Bisnis (Explanatory Analysis)** — menjawab 5 pertanyaan bisnis terukur.
3. **Feature Engineering & Preprocessing** — mempersiapkan data untuk model deep learning.
4. **Streamlit Dashboard** — visualisasi interaktif tren karir dan skill.

---

## ❓ Pertanyaan Bisnis

| # | Pertanyaan | Metrik Keberhasilan |
|---|-----------|---------------------|
| 1 | Bagaimana tren pertumbuhan rata-rata salary **Tech vs Non-Tech** (level Mid, 2020–2024)? | Δ rata-rata salary tahunan (%) per kelompok |
| 2 | Industri apa saja yang menawarkan rata-rata salary **tertinggi**? | Top-5 industri berdasarkan rata-rata salary |
| 3 | Job title mana yang memiliki rata-rata salary **tertinggi**? | Top-5 job title berdasarkan rata-rata salary |
| 4 | Skill apa yang **paling sering** diminta oleh perusahaan? | Top-10 skill berdasarkan frekuensi kemunculan |
| 5 | Skill apa yang memberikan **rata-rata salary tertinggi**? | Top-10 skill berdasarkan rata-rata salary |

---

## 📁 Struktur Proyek

```
data-science/
│
├── 📓 Carpathmu_Data_Science.ipynb   # Notebook utama (EDA, analisis, preprocessing)
├── 📦 job_posting_carpathmu.csv      # Dataset mentah (raw)
├── 📋 requirements.txt               # Dependensi Python
│
└── 📂 dashboard/
    ├── 🐍 dashboard.py               # Aplikasi Streamlit interaktif
    └── 📊 job_posting_clean.csv      # Dataset bersih untuk dashboard
```

---

## 📊 Dataset

| Atribut | Detail |
|---------|--------|
| **File** | `job_posting_carpathmu.csv` |
| **Ukuran Mentah** | 50.743 baris × 26 kolom |
| **Ukuran Bersih** | ~50.000 baris × 15 kolom |
| **Periode** | 2020 – 2024 |

### Kolom Utama

| Kolom | Deskripsi |
|-------|-----------|
| `job_title` | Judul pekerjaan (20 kategori unik) |
| `industry` | Industri perusahaan (11 kategori) |
| `work_type` | On-Site / Remote / Hybrid |
| `experience_level` | Entry / Mid / Senior / Lead / Executive |
| `education_required` | Bootcamp / High School / Associate / Bachelor's / Master's / PhD |
| `skills_required` | Keahlian yang dibutuhkan (82 skill unik, dipisah `\|`) |
| `education_background` | Latar belakang pendidikan (13 kategori, multi-label) |
| `salary_midpoint` | Titik tengah rentang gaji (USD) |
| `min_GPA_requirement` | Minimal IPK yang dibutuhkan |
| `post_date` | Tanggal posting lowongan |

### Statistik Salary

| Metrik | Nilai |
|--------|-------|
| Mean | ~$162,575 |
| Median | ~$147,000 |
| Min | $36,000 |
| Max | $781,000 |

---

## 🔄 Alur Pipeline

```
Raw CSV (50,743 rows × 26 columns)
    │
    ▼  [3] Data Wrangling
    │   • Drop 12 kolom tidak relevan (company, location, benefits, dll.)
    │   • Fix tipe data: post_date → datetime
    │   • Drop missing values (~1%) & 223 duplikat
    │
    ▼  Clean DataFrame (~50,000 rows × 15 columns)
    │
    ▼  [4] EDA — 9 kolom dieksplorasi (distribusi + insight)
    │
    ▼  [5] Explanatory Analysis — 5 business questions terjawab
    │
    ▼  [7] Feature Importance (Random Forest)
    │   • Fitur terpilih: skills_required, education_background,
    │     min_GPA_requirement, education_required
    │
    ▼  [9] Feature Engineering
    │   • Fitur temporal: post_month, post_quarter, post_year
    │   • skill_count, edu_bg_count
    │   • salary_vs_industry_mean
    │
    ▼  [10] Preprocessing untuk Deep Learning
    │   • OrdinalEncoder  → education_required, seniority, work_type, industry
    │   • MultiLabelBinarizer → skills_required, education_background
    │   • LabelEncoder    → job_title (target)
    │   • MinMaxScaler    → min_GPA_requirement
    │
    ▼  Train / Val / Test Split (60% / 20% / 20%, stratified)
    │
    ▼  Artefak tersimpan (.pkl, .csv, .npy)
```

---

## 📈 Visualisasi Dashboard

Dashboard Streamlit menyajikan 5 visualisasi interaktif yang dapat difilter berdasarkan **Tahun**, **Level Pendidikan**, dan **Level Pengalaman**:

| # | Visualisasi |
|---|-------------|
| 1 | 📉 Tren rata-rata salary: **Tech vs Non-Tech** (line chart per tahun) |
| 2 | 💰 **Top 5 Job Title** berdasarkan rata-rata salary tertinggi |
| 3 | 🏭 **Top 5 Industri** berdasarkan rata-rata gaji tertinggi |
| 4 | 🛠️ **Top 10 Keahlian** dengan frekuensi permintaan tertinggi |
| 5 | 💵 **Top 10 Skill** berdasarkan rata-rata salary tertinggi |

### KPI Scorecard
- 📊 **Rata-rata Gaji** (USD)
- 📋 **Total Lowongan** aktif
- 🌐 **Persentase Remote Jobs**

---

## 🚀 Cara Menjalankan

### Prasyarat

- Python 3.8+
- pip

### 1. Clone Repositori

```bash
git clone <repository-url>
cd data-science
```

### 2. Install Dependensi

```bash
pip install -r requirements.txt
```

### 3. Jalankan Notebook (Analisis & Preprocessing)

Buka dan jalankan `Carpathmu_Data_Science.ipynb` menggunakan Jupyter Notebook atau VS Code:

```bash
jupyter notebook Carpathmu_Data_Science.ipynb
```

### 4. Jalankan Dashboard Streamlit

```bash
streamlit run dashboard/dashboard.py
```

Dashboard akan terbuka di browser pada `http://localhost:8501`.

---

## 🛠️ Teknologi yang Digunakan

| Library | Versi | Kegunaan |
|---------|-------|----------|
| `streamlit` | ≥ 1.32.0 | Dashboard interaktif |
| `pandas` | ≥ 2.0.0 | Manipulasi & analisis data |
| `plotly` | ≥ 5.18.0 | Visualisasi interaktif |
| `numpy` | — | Komputasi numerik |
| `matplotlib` / `seaborn` | — | Visualisasi EDA di notebook |
| `scikit-learn` | — | Encoding, scaling, feature importance |
| `joblib` | — | Serialisasi artefak model |

---

## 👥 Tim

**© 2026 Capstone Team CarPathMu** · DBS Foundation Coding Camp
