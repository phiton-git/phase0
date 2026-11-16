
# LC3 – Analisis Durasi Perjalanan Bike Share San Francisco

Panduan penyelesaian **Live Code 3 (LC3) Hacktiv8** dengan penjelasan untuk pemula (*newbie*).

> **Tujuan proyek:** mengambil 3.000 data durasi perjalanan sepeda dari BigQuery, memahami distribusi data melalui statistik deskriptif, mendeteksi dan menangani outlier dengan metode IQR, membandingkan statistik sebelum dan sesudah pembersihan, lalu menyediakan data bersih melalui API menggunakan FastAPI.

---

## 0. Gambaran besar alur pengerjaan

1. Menghubungkan Google Colab ke Google BigQuery.
2. Mengambil 3.000 nilai `duration_sec`.
3. Membuat DataFrame dan memeriksa kualitas data.
4. Menghitung statistik deskriptif sebelum pembersihan.
5. Menghitung Q1, Q2, Q3, IQR, dan batas outlier.
6. Mengidentifikasi outlier dan membuat dataset bersih.
7. Menghitung ulang statistik setelah pembersihan.
8. Membandingkan kondisi sebelum dan sesudah.
9. Menyimpan data bersih sebagai CSV.
10. Membuat API FastAPI untuk melihat data dan menghapus record berdasarkan ID.

**Alur berpikir:** ambil data → pahami data → deteksi nilai ekstrem → bersihkan → ukur dampaknya → sajikan data melalui API.

---

## 1. Persiapan dan koneksi ke BigQuery

Jalankan bagian ini di Google Colab.

```python
from google.colab import auth
from google.cloud import bigquery

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

auth.authenticate_user()
print("Authenticated")

project_id = "phiton-514618"
client = bigquery.Client(project=project_id)
```

### Penjelasan newbie

- `auth.authenticate_user()` meminta izin agar Colab dapat mengakses akun Google.
- `bigquery.Client()` membuat koneksi untuk menjalankan query ke BigQuery.
- `project_id` adalah ID Google Cloud Project.
- `pandas` dipakai untuk mengolah data berbentuk tabel.
- `numpy` membantu perhitungan numerik.
- `matplotlib` dan `seaborn` digunakan untuk visualisasi.

## 2. Mengambil 3.000 data dari BigQuery

```python
query = """
SELECT duration_sec
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
WHERE duration_sec IS NOT NULL
LIMIT 3000
"""

df = client.query(query).to_dataframe()

print("Jumlah baris:", len(df))
df.head()
```

### Penjelasan newbie

- `SELECT duration_sec` mengambil kolom durasi perjalanan.
- `FROM` menunjukkan tabel sumber.
- `WHERE duration_sec IS NOT NULL` menghindari nilai kosong.
- `LIMIT 3000` membatasi hasil maksimal 3.000 baris.
- `to_dataframe()` mengubah hasil query menjadi DataFrame Pandas.

**Catatan:** `LIMIT 3000` tidak menjamin sampel acak. Ikuti query dan metode pengambilan sampel yang diminta instruksi resmi LC3.

Periksa hasil:

```python
print("Shape:", df.shape)
df.info()
print(df.isnull().sum())
print(df.describe())
```

`shape` menunjukkan jumlah baris dan kolom. `info()` menunjukkan tipe data dan jumlah nilai non-null. `isnull().sum()` menghitung data kosong. `describe()` memberi ringkasan statistik awal.

---

## 3. Memahami statistik deskriptif

Statistik deskriptif membantu kita memahami karakteristik data tanpa membaca setiap baris satu per satu.

### 3.1 Central Tendency (ukuran pemusatan)

Central tendency menjawab pertanyaan: **nilai data biasanya berada di sekitar angka berapa?**

| Ukuran | Arti sederhana | Fungsi |
|---|---|---|
| Mean | Rata-rata | Jumlah seluruh nilai dibagi banyak data |
| Median | Nilai tengah | Membagi data terurut menjadi dua bagian |
| Mode | Nilai paling sering muncul | Menunjukkan nilai dengan frekuensi tertinggi |

Contoh:

```python
contoh = pd.Series([10, 15, 15, 20, 25, 30, 60])

print("Mean:", contoh.mean())
print("Median:", contoh.median())
print("Mode:", contoh.mode().tolist())
```

Hasil: Mean = 25, Median = 20, Mode = 15.

**Mengapa mean dan median bisa berbeda?** Mean sensitif terhadap nilai yang sangat besar atau sangat kecil. Median lebih tahan terhadap nilai ekstrem.

### 3.2 Menghitung central tendency sebelum cleaning

```python
data = df["duration_sec"].dropna()

mean_before = data.mean()
median_before = data.median()
mode_before = data.mode().tolist()

print("MEAN sebelum cleaning:", mean_before)
print("MEDIAN sebelum cleaning:", median_before)
print("MODE sebelum cleaning:", mode_before)
```

`mode()` dapat menghasilkan lebih dari satu nilai jika beberapa nilai sama-sama paling sering muncul. Karena itu hasilnya berupa Series; `.tolist()` mengubahnya menjadi list Python.

### 3.3 Dispersion (ukuran penyebaran)

Dispersion menjawab pertanyaan: **seberapa menyebar data dari pusatnya?**

| Ukuran | Arti sederhana |
|---|---|
| Range | Jarak nilai maksimum dan minimum |
| Variance | Rata-rata kuadrat penyimpangan dari mean |
| Standard deviation | Ukuran penyebaran dalam satuan yang sama dengan data |
| IQR | Jarak antara Q3 dan Q1 |
| Coefficient of Variation (CV) | Standar deviasi dibandingkan mean, dalam persen |

```python
range_before = data.max() - data.min()
variance_before = data.var()
std_before = data.std()
cv_before = (std_before / data.mean()) * 100

print("Range:", range_before)
print("Variance:", variance_before)
print("Standard deviation:", std_before)
print("Coefficient of Variation:", cv_before, "%")
```

**Catatan:** `pandas.Series.var()` dan `.std()` secara default memakai rumus sampel (`ddof=1`). Untuk populasi gunakan `ddof=0`. Gunakan pendekatan yang konsisten saat membandingkan hasil.

### 3.4 Skewness dan kurtosis

- **Skewness** mengukur kemencengan distribusi.
  - Mendekati 0: relatif simetris.
  - Positif: ekor distribusi lebih panjang ke kanan.
  - Negatif: ekor distribusi lebih panjang ke kiri.
- **Kurtosis** pada Pandas menggunakan *excess kurtosis*.
  - Mendekati 0: mirip kurtosis distribusi normal menurut definisi excess.
  - Positif: ekor relatif lebih berat dibanding normal.
  - Negatif: ekor relatif lebih ringan dibanding normal.

```python
print("Skewness:", data.skew())
print("Kurtosis:", data.kurt())
```

Jangan menyimpulkan bentuk distribusi hanya dari satu angka. Periksa juga histogram dan boxplot.

---

## 4. Visualisasi data sebelum cleaning

```python
plt.figure(figsize=(10, 5))
sns.histplot(data, bins=50, kde=True)
plt.title("Distribusi Durasi Perjalanan Sebelum Cleaning")
plt.xlabel("Durasi (detik)")
plt.ylabel("Frekuensi")
plt.show()
```

```python
plt.figure(figsize=(10, 3))
sns.boxplot(x=data)
plt.title("Boxplot Durasi Perjalanan Sebelum Cleaning")
plt.xlabel("Durasi (detik)")
plt.show()
```

Histogram memperlihatkan bentuk distribusi dan konsentrasi nilai. Boxplot membantu melihat sebaran dan titik yang berada di luar batas whisker.

---

## 5. Deteksi outlier dengan metode IQR

### 5.1 Memahami quartile

Quartile membagi data yang sudah diurutkan menjadi empat bagian:

- **Q1 (25%)**: sekitar 25% data berada di bawah atau sama dengan nilai ini.
- **Q2 (50%)**: median, titik tengah data.
- **Q3 (75%)**: sekitar 75% data berada di bawah atau sama dengan nilai ini.

**IQR** adalah jarak antara Q3 dan Q1:

\[
IQR = Q3 - Q1
\]

Batas bawah dan batas atas outlier:

\[
Lower\ Bound = Q1 - 1.5 \times IQR
\]

\[
Upper\ Bound = Q3 + 1.5 \times IQR
\]

Nilai di bawah lower bound atau di atas upper bound ditandai sebagai outlier menurut aturan IQR.

### 5.2 Contoh kecil agar konsepnya mudah dipahami

```python
contoh = pd.Series([100, 200, 300, 400, 500, 550, 600,
                    700, 800, 900, 1000, 1200, 1300, 1600, 5000])

q1 = contoh.quantile(0.25)
q2 = contoh.quantile(0.50)
q3 = contoh.quantile(0.75)

iqr = q3 - q1
lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr

outlier = contoh[(contoh < lower_bound) | (contoh > upper_bound)]

print("Q1:", q1)
print("Q2 / Median:", q2)
print("Q3:", q3)
print("IQR:", iqr)
print("Lower Bound:", lower_bound)
print("Upper Bound:", upper_bound)
print("Outlier:", outlier.tolist())
```

Pada contoh ini, angka `5000` berada jauh di atas kumpulan nilai lainnya sehingga terdeteksi sebagai outlier.

### 5.3 Menghitung IQR pada data LC3

```python
q1 = data.quantile(0.25)
q2 = data.quantile(0.50)
q3 = data.quantile(0.75)

iqr = q3 - q1

lower_bound = q1 - (1.5 * iqr)
upper_bound = q3 + (1.5 * iqr)

print("Q1:", q1)
print("Q2 / Median:", q2)
print("Q3:", q3)
print("IQR:", iqr)
print("Lower Bound:", lower_bound)
print("Upper Bound:", upper_bound)
```

### 5.4 Mengambil baris outlier

```python
outlier_mask = (data < lower_bound) | (data > upper_bound)
outliers = data[outlier_mask]

print("Jumlah outlier:", len(outliers))
print("Persentase outlier:", len(outliers) / len(data) * 100, "%")
```

Operator:
- `data < lower_bound` mencari nilai di bawah batas bawah.
- `data > upper_bound` mencari nilai di atas batas atas.
- `|` berarti **OR**: salah satu kondisi benar sudah cukup.
- `outlier_mask` adalah kumpulan nilai True/False untuk menandai setiap baris.

Pada pengerjaan LC3 yang telah kita bahas, hasil yang tercatat adalah **177 outlier dari 3.000 baris (5,90%)**, sehingga tersisa **2.823 baris** setelah outlier dihapus. Angka aktual dapat berbeda jika query atau sampel berubah.

---

## 6. Menangani outlier (cleaning)

Untuk latihan LC3 ini, kita menggunakan pendekatan menghapus baris yang berada di luar batas IQR.

```python
df_clean = df[
    (df["duration_sec"] >= lower_bound) &
    (df["duration_sec"] <= upper_bound)
].copy()

print("Jumlah data sebelum cleaning:", len(df))
print("Jumlah data sesudah cleaning:", len(df_clean))
print("Jumlah data yang dihapus:", len(df) - len(df_clean))
```

### Mengapa menggunakan `.copy()`?

`.copy()` membuat salinan DataFrame hasil filter. Ini membantu menghindari masalah ketika kita mengubah DataFrame hasil penyaringan.

**Penting untuk business knowledge:** outlier tidak otomatis berarti data salah. Perjalanan yang sangat lama bisa saja valid, misalnya sepeda belum dikembalikan. Penghapusan outlier harus dijelaskan sebagai keputusan analisis, bukan aturan universal. Dalam proyek latihan ini kita mengikuti pendekatan cleaning IQR yang digunakan pada pembahasan.

### Verifikasi setelah cleaning

```python
print(df_clean.info())
print(df_clean["duration_sec"].describe())

plt.figure(figsize=(10, 5))
sns.histplot(df_clean["duration_sec"], bins=50, kde=True)
plt.title("Distribusi Durasi Setelah Cleaning")
plt.xlabel("Durasi (detik)")
plt.ylabel("Frekuensi")
plt.show()

plt.figure(figsize=(10, 3))
sns.boxplot(x=df_clean["duration_sec"])
plt.title("Boxplot Durasi Setelah Cleaning")
plt.show()
```

---

## 7. Membandingkan statistik sebelum dan sesudah cleaning

Kita perlu menghitung ulang statistik agar tahu dampak penghapusan outlier.

```python
before = df["duration_sec"].dropna()
after = df_clean["duration_sec"].dropna()

comparison = pd.DataFrame({
    "Before Cleaning": {
        "count": before.count(),
        "mean": before.mean(),
        "median": before.median(),
        "std": before.std(),
        "variance": before.var(),
        "min": before.min(),
        "max": before.max(),
        "range": before.max() - before.min(),
        "skewness": before.skew(),
        "kurtosis": before.kurt(),
        "CV (%)": before.std() / before.mean() * 100
    },
    "After Cleaning": {
        "count": after.count(),
        "mean": after.mean(),
        "median": after.median(),
        "std": after.std(),
        "variance": after.var(),
        "min": after.min(),
        "max": after.max(),
        "range": after.max() - after.min(),
        "skewness": after.skew(),
        "kurtosis": after.kurt(),
        "CV (%)": after.std() / after.mean() * 100
    }
})

comparison
```

### Cara membaca hasil

Dalam hasil LC3 yang sebelumnya kita bahas, contoh angka mean adalah:

- Mean sebelum cleaning: sekitar **854,45 detik**.
- Mean sesudah cleaning: sekitar **549,86 detik**.

Gunakan angka dari output notebook milikmu sebagai hasil final karena hasil bisa berubah sesuai sampel dan query.

Interpretasi yang perlu dipahami:

1. **Mean turun** jika nilai ekstrem yang besar sebelumnya menarik rata-rata ke atas.
2. **Median** biasanya berubah lebih kecil dibanding mean karena median lebih tahan terhadap outlier.
3. **Standard deviation dan variance** dapat turun karena penyebaran data berkurang.
4. **Range** biasanya turun karena nilai maksimum ekstrem dihapus.
5. **Skewness** dapat menjadi lebih dekat ke nol, tetapi tidak wajib.
6. **CV** menunjukkan penyebaran relatif terhadap mean.

### Memahami Coefficient of Variation (CV)

\[
CV = \frac{Standard\ Deviation}{Mean} \times 100\%
\]

CV berguna untuk membandingkan variasi relatif, terutama ketika rata-rata dua kelompok berbeda.

Contoh interpretasi yang pernah kita hitung:

- CV sebelum cleaning: **67,32%**.
- CV sesudah cleaning: **38,39%**.

Artinya variasi relatif menurun. Dengan klasifikasi sederhana yang digunakan pada latihan, CV 67,32% termasuk **sangat tinggi**, sedangkan 38,39% termasuk **tinggi**. Jadi bukan berarti variasinya sudah rendah; variasinya hanya lebih kecil dibanding kondisi awal.

> Kategori CV seperti rendah/tinggi/sangat tinggi tidak memiliki satu standar universal untuk semua bidang. Jika tugas memberikan batas kategori tertentu, ikuti batas dari materi tersebut dan tuliskan sumber kategorinya.

### Membuat ringkasan perubahan

```python
summary = pd.DataFrame({
    "Metric": ["Jumlah data", "Mean", "Median", "Std", "Variance",
               "Range", "Skewness", "Kurtosis", "CV (%)"],
    "Before": [
        before.count(), before.mean(), before.median(), before.std(),
        before.var(), before.max() - before.min(), before.skew(),
        before.kurt(), before.std() / before.mean() * 100
    ],
    "After": [
        after.count(), after.mean(), after.median(), after.std(),
        after.var(), after.max() - after.min(), after.skew(),
        after.kurt(), after.std() / after.mean() * 100
    ]
})

summary["Change"] = summary["After"] - summary["Before"]
summary
```

---

## 8. Menyiapkan dataset untuk API

Agar setiap baris memiliki identitas unik, kita tambahkan `record_id`.

```python
df_api = df_clean[["duration_sec"]].reset_index(drop=True)
df_api.insert(0, "record_id", range(1, len(df_api) + 1))

print(df_api.head())
print(df_api.shape)
```

Simpan sebagai CSV:

```python
df_api.to_csv("bike_duration_clean.csv", index=False)
```

Download dari Colab:

```python
from google.colab import files
files.download("bike_duration_clean.csv")
```

File yang dihasilkan memiliki dua kolom: `record_id` dan `duration_sec`.

`record_id` adalah ID buatan untuk keperluan API, bukan ID perjalanan asli dari sumber BigQuery.

---

## 9. Membuat FastAPI

FastAPI digunakan agar dataset bisa diakses melalui endpoint HTTP.

### 9.1 Struktur folder

```text
LC3/
├── api.py
└── bike_duration_clean.csv
```

Pastikan `api.py` dan CSV berada dalam folder yang sama.

### 9.2 Install library

Di terminal environment Python yang digunakan:

```bash
pip install fastapi uvicorn pandas
```

### 9.3 Isi file `api.py`

```python
from fastapi import FastAPI, HTTPException
import pandas as pd
from pathlib import Path

app = FastAPI(
    title="Bike Share Duration API",
    description="API untuk mengakses data durasi perjalanan sepeda yang sudah dibersihkan.",
    version="1.0.0"
)

BASE_DIR = Path(__file__).resolve().parent
CSV_PATH = BASE_DIR / "bike_duration_clean.csv"


def load_data():
    """Membaca dataset CSV dan mengembalikannya sebagai DataFrame."""
    if not CSV_PATH.exists():
        raise FileNotFoundError(f"File tidak ditemukan: {CSV_PATH}")

    return pd.read_csv(CSV_PATH)


@app.get("/")
def home():
    """Menampilkan informasi utama API."""
    return {
        "message": "Welcome to Bike Share Duration API",
        "documentation": "/docs",
        "endpoints": [
            "GET /trips",
            "GET /trips/{record_id}",
            "DELETE /trips/{record_id}"
        ]
    }


@app.get("/trips")
def get_trips():
    """Mengambil seluruh data perjalanan."""
    df = load_data()
    return {
        "total_data": len(df),
        "data": df.to_dict(orient="records")
    }


@app.get("/trips/{record_id}")
def get_trip_by_id(record_id: int):
    """Mengambil satu perjalanan berdasarkan record_id."""
    df = load_data()
    result = df[df["record_id"] == record_id]

    if result.empty:
        raise HTTPException(
            status_code=404,
            detail="Record ID tidak ditemukan"
        )

    return result.to_dict(orient="records")[0]


@app.delete("/trips/{record_id}")
def delete_trip(record_id: int):
    """Menghapus satu record berdasarkan record_id dari file CSV."""
    df = load_data()
    result = df[df["record_id"] == record_id]

    if result.empty:
        raise HTTPException(
            status_code=404,
            detail="Record ID tidak ditemukan"
        )

    df = df[df["record_id"] != record_id]
    df.to_csv(CSV_PATH, index=False)

    return {
        "message": "Record berhasil dihapus",
        "deleted_record_id": record_id,
        "remaining_data": len(df)
    }
```

### Penjelasan newbie untuk bagian API

- `FastAPI(...)` membuat aplikasi API.
- `@app.get("/")` berarti fungsi di bawahnya dijalankan ketika endpoint `/` dipanggil dengan metode GET.
- `@app.get("/trips")` mengambil seluruh data.
- `@app.get("/trips/{record_id}")` mengambil satu data berdasarkan ID.
- `@app.delete("/trips/{record_id}")` menghapus data berdasarkan ID.
- `HTTPException(404, ...)` mengirim respons bahwa data tidak ditemukan.
- `to_dict(orient="records")` mengubah DataFrame menjadi list berisi dictionary agar mudah dikirim sebagai JSON.
- `to_csv()` menyimpan perubahan ke file CSV.

**Catatan:** endpoint DELETE di atas mengubah file CSV secara langsung. Ini cocok untuk latihan lokal sederhana, tetapi belum cocok untuk API multi-user atau produksi. Untuk penggunaan nyata, gunakan database dan pertimbangkan autentikasi, validasi, serta transaksi.

### 9.4 Menjalankan API

Jalankan terminal dari folder `LC3`:

```bash
uvicorn api:app --reload
```

Buka alamat berikut:

- `http://127.0.0.1:8000/` — informasi API.
- `http://127.0.0.1:8000/trips` — seluruh data.
- `http://127.0.0.1:8000/docs` — dokumentasi interaktif Swagger.

### 9.5 Menguji endpoint

| Endpoint | Method | Fungsi |
|---|---|---|
| `/` | GET | Informasi API |
| `/trips` | GET | Mengambil semua data |
| `/trips/1` | GET | Mengambil record dengan ID 1 |
| `/trips/1` | DELETE | Menghapus record dengan ID 1 |

Untuk mencoba DELETE:
1. Buka `/docs`.
2. Pilih endpoint `DELETE /trips/{record_id}`.
3. Klik **Try it out**.
4. Masukkan ID yang memang ada, misalnya `1`.
5. Klik **Execute**.
6. Panggil kembali `GET /trips` untuk memeriksa bahwa jumlah data berkurang.

Simpan backup CSV sebelum menguji DELETE agar data awal tetap tersedia.

---

## 10. Kesimpulan analisis (contoh narasi laporan)

Berdasarkan analisis terhadap data durasi perjalanan Bike Share San Francisco, dilakukan pemeriksaan statistik deskriptif untuk memahami pemusatan dan penyebaran data. Outlier dideteksi menggunakan metode Interquartile Range (IQR), yaitu nilai yang berada di bawah `Q1 - 1.5 × IQR` atau di atas `Q3 + 1.5 × IQR`.

Pada hasil pengerjaan yang telah dibahas, ditemukan 177 outlier dari 3.000 data (5,90%). Setelah baris outlier dihapus, tersisa 2.823 data. Penghapusan nilai ekstrem berdampak pada ukuran statistik, terutama mean, standard deviation, variance, dan range. Mean menjadi lebih rendah karena nilai durasi yang sangat besar tidak lagi menarik rata-rata ke atas.

Perbandingan CV juga menunjukkan penurunan dari 67,32% menjadi 38,39%. Artinya, penyebaran relatif data setelah cleaning lebih kecil daripada sebelum cleaning. Namun, CV 38,39% masih dapat dikategorikan tinggi berdasarkan klasifikasi sederhana yang digunakan dalam latihan. Hasil ini menunjukkan bahwa cleaning mengurangi variasi relatif, tetapi tidak membuat data menjadi seragam.

Dataset yang telah dibersihkan kemudian diberi `record_id`, disimpan sebagai CSV, dan diintegrasikan dengan FastAPI. API menyediakan endpoint untuk menampilkan seluruh data, mengambil satu record berdasarkan ID, dan menghapus record berdasarkan ID.

**Ganti angka pada narasi ini dengan output aktual notebook** apabila hasil query atau sampel yang kamu gunakan berbeda.

---

## 11. Checklist sebelum mengumpulkan LC3

- [ ] Query BigQuery berhasil mengambil 3.000 baris sesuai instruksi.
- [ ] Data diperiksa dari nilai kosong dan tipe data.
- [ ] Mean, median, dan mode dihitung.
- [ ] Range, variance, dan standard deviation dihitung.
- [ ] Skewness dan kurtosis dihitung.
- [ ] Q1, Q2, Q3, IQR, lower bound, dan upper bound dihitung.
- [ ] Outlier diidentifikasi dan jumlah/persentasenya dilaporkan.
- [ ] Data dibersihkan dengan metode yang dijelaskan.
- [ ] Statistik sebelum dan sesudah cleaning dibandingkan.
- [ ] Visualisasi sebelum dan sesudah cleaning dibuat.
- [ ] Dataset bersih disimpan menjadi `bike_duration_clean.csv`.
- [ ] CSV memiliki `record_id` dan `duration_sec`.
- [ ] FastAPI dapat dijalankan.
- [ ] Endpoint GET `/`, GET `/trips`, GET `/trips/{record_id}`, dan DELETE `/trips/{record_id}` diuji.
- [ ] Kesimpulan menjelaskan dampak cleaning, bukan hanya menyalin angka.

---

## 12. Kamus istilah singkat

| Istilah | Arti newbie |
|---|---|
| DataFrame | Tabel data di Pandas |
| `mean()` | Menghitung rata-rata |
| `median()` | Menghitung nilai tengah |
| `mode()` | Mencari nilai yang paling sering muncul |
| `quantile()` | Mengambil posisi persentil/quartile |
| IQR | Jarak Q3 dengan Q1 |
| Outlier | Nilai yang jauh dari pola umum menurut kriteria tertentu |
| `dropna()` | Menghapus nilai kosong |
| `copy()` | Membuat salinan DataFrame |
| `skew()` | Mengukur kemencengan distribusi |
| `kurt()` | Mengukur karakteristik ekor distribusi |
| API | Cara aplikasi saling meminta dan mengirim data |
| Endpoint | Alamat/fungsi yang disediakan API |
| GET | Meminta atau membaca data |
| DELETE | Meminta penghapusan data |
| JSON | Format pertukaran data yang umum dipakai API |
| CSV | Format file tabel berbasis teks |

**Prinsip belajar:** jangan hanya menghafal kode. Untuk setiap baris, tanyakan: data apa yang masuk, operasi apa yang dilakukan, dan informasi apa yang kita peroleh dari hasilnya.
