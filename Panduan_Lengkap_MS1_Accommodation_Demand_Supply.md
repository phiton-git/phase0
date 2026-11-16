# Panduan Lengkap Milestone 1 (MS1)
## Accommodation Market Demand & Supply Analysis
### Panduan belajar untuk pemula (Newbie Guide)

> **Tujuan dokumen:** menjelaskan keseluruhan alur pengerjaan Milestone 1 (MS1) menggunakan contoh proyek analisis pasar akomodasi. Dokumen ini dibuat agar pemula memahami *apa yang dikerjakan, mengapa dikerjakan, bagaimana cara melakukannya, dan bagaimana membaca hasilnya*.

---

## 1. Gambaran Umum Proyek

### 1.1 Judul proyek

**Accommodation Market Demand & Supply Analysis**

Dalam bahasa sederhana: analisis untuk memahami kondisi penawaran akomodasi (*supply*), indikasi aktivitas tamu (*demand/activity proxy*), dan harga pada beberapa lokasi.

### 1.2 Ide bisnis

Bayangkan ada seseorang yang ingin membuka atau mengembangkan bisnis akomodasi. Sebelum mengambil keputusan, ia ingin mengetahui:

- Lokasi mana yang memiliki banyak listing akomodasi?
- Lokasi mana yang menunjukkan aktivitas ulasan tamu relatif tinggi?
- Jenis kamar apa yang paling banyak tersedia?
- Apakah harga berkaitan dengan aktivitas ulasan?
- Bagaimana karakteristik harga dan aktivitas ulasan?
- Apakah aktivitas ulasan berbeda secara statistik antarjenis kamar?

Analisis data membantu mengubah kumpulan listing menjadi informasi yang lebih mudah dipakai untuk memahami pasar.

### 1.3 Siapa yang akan menggunakan hasil analisis?

Contoh pembaca atau *stakeholder*:

- Pemilik akomodasi.
- Calon investor properti atau akomodasi.
- Pengelola penginapan.
- Tim riset pasar.
- Analis bisnis.

### 1.4 Batasan penting: data ulasan bukan data pemesanan

Pada contoh proyek ini, kolom `number_of_reviews` dan `reviews_per_month` digunakan sebagai **indikator tidak langsung aktivitas tamu**.

Keduanya bukan jumlah booking, tingkat okupansi, atau pendapatan aktual.

Mengapa? Tidak semua tamu menulis ulasan. Ada jeda antara kunjungan dan ulasan, dan kebiasaan memberi ulasan dapat berbeda antarorang atau platform.

Karena itu, gunakan istilah:

- “indikasi aktivitas ulasan”;
- “review activity proxy”; atau
- “proksi aktivitas tamu berdasarkan ulasan”.

Hindari menyimpulkan “permintaan aktual”, “jumlah booking”, atau “okupansi” hanya dari data ulasan.

---

## 2. Memahami Tujuan Milestone 1

Secara garis besar, MS1 melatih kita untuk melakukan proses analisis data dari awal sampai akhir:

1. Menentukan masalah bisnis.
2. Menyusun pertanyaan analisis.
3. Menemukan dan memahami dataset.
4. Memuat data ke Python.
5. Memeriksa dan membersihkan data.
6. Melakukan eksplorasi dan analisis.
7. Menggunakan statistik deskriptif.
8. Melakukan statistik inferensial.
9. Membuat visualisasi dan dashboard.
10. Menulis kesimpulan dan rekomendasi.

Jadi, MS1 bukan sekadar membuat grafik. Kita harus mampu menjelaskan **mengapa analisis dilakukan, bagaimana prosesnya, apa hasilnya, dan apa arti hasil tersebut bagi kebutuhan bisnis**.

---

## 3. Problem Statement

### 3.1 Apa itu problem statement?

*Problem statement* adalah penjelasan singkat dan jelas mengenai masalah yang ingin diselesaikan melalui analisis.

Contoh:

> Calon pemilik atau investor akomodasi membutuhkan gambaran mengenai persebaran listing, indikasi aktivitas ulasan, dan variasi harga di beberapa wilayah. Tanpa analisis, mereka akan kesulitan membandingkan lokasi dan tipe akomodasi secara terstruktur. Oleh karena itu, analisis ini dilakukan untuk menggambarkan supply listing, menggunakan aktivitas ulasan sebagai proksi aktivitas tamu, serta mengevaluasi pola harga dan perbedaan aktivitas ulasan antarjenis kamar.

### 3.2 SMART

Problem statement dapat diperjelas menggunakan prinsip SMART.

| Elemen | Makna | Penerapan pada proyek |
|---|---|---|
| Specific | Spesifik | Menganalisis listing akomodasi, lokasi, tipe kamar, harga, dan ulasan |
| Measurable | Terukur | Menggunakan jumlah listing, harga, dan `reviews_per_month` |
| Achievable | Dapat dilakukan | Analisis menggunakan kolom yang tersedia pada dataset |
| Relevant | Relevan | Membantu memahami kondisi pasar akomodasi |
| Time-bound | Memiliki batas waktu | Analisis dilakukan pada periode pengerjaan MS1 |

**Catatan:** jangan mengklaim bahwa analisis ini mengukur booking atau okupansi jika dataset tidak menyediakan kolom tersebut.

---

## 4. Memahami Dataset

### 4.1 Data dummy untuk latihan

Paket latihan menggunakan data dummy/sintetis sebanyak 600 baris. Data tersebut dibuat untuk mempelajari alur pengerjaan, bukan untuk menggambarkan pasar akomodasi yang benar-benar terjadi.

Dataset latihan berisi contoh lokasi dan tipe kamar, harga simulasi, serta angka ulasan. Nilai harga pada data dummy merupakan nilai simulasi, bukan harga pasar aktual.

**Penting untuk submission:** apabila ketentuan resmi MS1 meminta dataset nyata dari sumber yang dapat dipertanggungjawabkan, dataset dummy ini tidak boleh dijadikan dataset final. Gunakan dataset nyata dan cantumkan sumber/URL-nya. Notebook dummy ini berfungsi sebagai template belajar.

### 4.2 Apa itu satu baris data?

Satu baris merepresentasikan satu listing akomodasi.

Contoh sederhana:

| id | neighbourhood | room_type | price | number_of_reviews | reviews_per_month |
|---:|---|---|---:|---:|---:|
| 101 | Area A | Entire home/apt | 120 | 35 | 1.50 |
| 102 | Area B | Private room | 75 | 18 | 0.80 |
| 103 | Area A | Private room | 60 | 12 | 0.50 |

Contoh di atas hanya ilustrasi struktur tabel.

### 4.3 Memahami kolom utama

| Kolom | Arti | Kegunaan |
|---|---|---|
| `id` | Identitas unik listing | Identifikasi listing |
| `neighbourhood` | Wilayah atau area | Analisis per lokasi |
| `room_type` | Jenis akomodasi/kamar | Perbandingan tipe |
| `price` | Harga listing | Analisis harga |
| `number_of_reviews` | Jumlah ulasan | Indikator aktivitas ulasan sepanjang periode data |
| `reviews_per_month` | Rata-rata ulasan per bulan | Proksi aktivitas ulasan bulanan |
| `availability_365` | Jumlah hari tersedia dalam kalender 365 hari | Informasi ketersediaan kalender, bukan okupansi |
| `calculated_host_listings_count` | Jumlah listing yang dimiliki host menurut data | Gambaran skala listing host |

Nama dan ketersediaan kolom harus disesuaikan dengan dataset nyata yang akhirnya dipakai.

### 4.4 Perbedaan supply, demand, dan proxy

**Supply (penawaran)**

Dalam proyek ini, supply dapat digambarkan melalui jumlah listing.

Contoh:
- Area A memiliki 150 listing.
- Area B memiliki 90 listing.

Artinya, pada dataset yang dianalisis, jumlah listing di Area A lebih banyak daripada Area B.

**Demand (permintaan)**

Demand sebenarnya dapat diukur dengan data seperti:
- jumlah pemesanan;
- jumlah malam terpesan;
- tingkat okupansi;
- transaksi atau pendapatan pemesanan.

Jika data tersebut tidak tersedia, kita tidak boleh mengaku telah mengukur demand aktual.

**Review activity proxy**

`reviews_per_month` dapat digunakan sebagai indikator aktivitas ulasan bulanan. Nilai ini bisa membantu membandingkan aktivitas listing, tetapi bukan pengganti data booking.

---

## 5. Memecah Masalah dengan 5W + 1H

5W + 1H membantu mengubah masalah bisnis menjadi pertanyaan yang dapat dianalisis.

| Komponen | Pertanyaan | Penerapan |
|---|---|---|
| What | Apa yang dianalisis? | Listing, lokasi, tipe kamar, harga, dan aktivitas ulasan |
| Who | Siapa yang membutuhkan? | Pemilik, calon investor, dan pengelola akomodasi |
| Where | Di mana analisis dilakukan? | Wilayah yang tersedia pada dataset |
| When | Kapan data dikumpulkan? | Periode yang tercantum pada sumber dataset |
| Why | Mengapa dianalisis? | Untuk memahami supply, pola harga, dan proksi aktivitas ulasan |
| How | Bagaimana menganalisisnya? | Data cleaning, EDA, statistik deskriptif, inferensial, dan dashboard |

**Untuk dataset nyata:** bagian `When` harus mengikuti keterangan sumber dataset. Jangan mengarang periode pengumpulan data.

---

## 6. Menyusun Minimal Enam Pertanyaan Analisis

Pertanyaan analisis adalah jembatan antara masalah bisnis dan kode Python.

Contoh enam pertanyaan:

| No. | Pertanyaan | Metode | Jenis output |
|---:|---|---|---|
| 1 | Wilayah mana yang memiliki jumlah listing terbanyak? | GroupBy dan agregasi | Bar chart |
| 2 | Wilayah mana yang memiliki total aktivitas ulasan bulanan tertinggi? | GroupBy dan sum | Bar chart |
| 3 | Tipe kamar mana yang memiliki rata-rata aktivitas ulasan per listing tertinggi? | GroupBy dan mean | Bar chart |
| 4 | Bagaimana hubungan harga dengan aktivitas ulasan bulanan? | Scatter plot dan korelasi | Scatter plot |
| 5 | Bagaimana distribusi harga dan aktivitas ulasan? | Statistik deskriptif | Tabel statistik |
| 6 | Apakah aktivitas ulasan berbeda antarjenis kamar? | Kruskal-Wallis | Uji inferensial |

### 6.1 Mengapa pertanyaan harus jelas?

Pertanyaan “Bagaimana kondisi pasar?” terlalu luas.

Pertanyaan “Wilayah mana yang memiliki jumlah listing terbanyak?” lebih jelas karena:
- variabelnya diketahui;
- cara menghitungnya diketahui;
- grafik yang sesuai dapat ditentukan;
- hasilnya dapat dijelaskan.

### 6.2 Jangan hanya membuat grafik

Setiap grafik harus diikuti insight.

Format sederhana:

1. **Temuan:** apa yang terlihat?
2. **Bukti:** angka mana yang mendukung?
3. **Interpretasi:** apa artinya?
4. **Batasan:** apa yang tidak dapat disimpulkan?

Contoh:

> Area A memiliki jumlah listing paling banyak pada dataset. Ini menunjukkan bahwa supply listing yang tercatat di Area A lebih besar dibandingkan area lain. Namun, jumlah listing yang tinggi tidak otomatis berarti permintaan atau keuntungan juga tinggi.

---

## 7. Alur Notebook dari Awal sampai Akhir

Notebook yang disediakan mengikuti alur kerja berikut.

### 7.1 Introduction

Bagian ini menjelaskan:
- latar belakang;
- alasan topik dipilih;
- tujuan analisis;
- calon pembaca atau stakeholder;
- ruang lingkup dan batasan analisis.

Contoh:

> Analisis ini bertujuan memahami persebaran listing akomodasi, karakteristik harga, serta aktivitas ulasan pada beberapa wilayah. Aktivitas ulasan digunakan sebagai proksi, bukan ukuran langsung booking atau okupansi.

### 7.2 Problem Statement and Dataset Description

Tuliskan:
- problem statement;
- tujuan analisis;
- pertanyaan analisis;
- sumber dataset;
- URL sumber;
- periode data;
- jumlah baris dan kolom;
- definisi setiap kolom penting;
- keterbatasan dataset.

Untuk submission final, dataset harus mengikuti ketentuan resmi dan berasal dari sumber nyata yang dapat dipertanggungjawabkan.

### 7.3 Data Loading

Data loading berarti membaca file ke dalam Python.

Contoh:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("data/P0M1_nawafi_suren_dataset.csv")

df.head()
```

Penjelasan:
- `import pandas as pd`: memanggil Pandas untuk mengolah tabel.
- `import numpy as np`: memanggil NumPy untuk operasi numerik.
- `import matplotlib.pyplot as plt`: memanggil Matplotlib untuk grafik.
- `import seaborn as sns`: memanggil Seaborn untuk visualisasi statistik.
- `pd.read_csv(...)`: membaca file CSV menjadi DataFrame.
- `df.head()`: menampilkan lima baris pertama.

Periksa ukuran data:

```python
df.shape
```

Output berupa pasangan `(jumlah_baris, jumlah_kolom)`.

Periksa nama kolom dan tipe data:

```python
df.info()
```

Lihat ringkasan statistik awal:

```python
df.describe()
```

### 7.4 Data Cleaning

Data cleaning adalah proses memeriksa dan memperbaiki masalah kualitas data.

#### A. Memeriksa missing values

```python
df.isnull().sum()
```

Artinya: hitung jumlah nilai kosong pada setiap kolom.

Jika kolom `reviews_per_month` kosong, jangan langsung mengisi nol tanpa alasan. Nilai kosong bisa berarti data tidak tersedia, bukan berarti aktivitas ulasan benar-benar nol.

#### B. Memeriksa duplikasi

```python
df.duplicated().sum()
```

Jika ingin menghapus baris duplikat:

```python
df = df.drop_duplicates()
```

Lakukan hanya setelah memahami apakah baris tersebut memang duplikat yang tidak diinginkan.

#### C. Memeriksa tipe data

```python
df.dtypes
```

Contohnya, kolom harga seharusnya numerik agar dapat dihitung rata-rata dan mediannya.

#### D. Memeriksa nilai tidak masuk akal

```python
df[["price", "number_of_reviews", "reviews_per_month"]].describe()
```

Perhatikan:
- harga nol atau negatif;
- jumlah ulasan negatif;
- nilai ulasan bulanan yang tidak masuk akal;
- nilai ekstrem yang perlu diperiksa.

Jangan otomatis menghapus semua outlier. Nilai ekstrem bisa saja merupakan listing yang benar-benar berbeda.

#### E. Memastikan perubahan

```python
df.info()
df.isnull().sum()
df.duplicated().sum()
```

Setelah cleaning, periksa kembali kondisi data.

---

## 8. Exploration and Analysis (EDA)

EDA adalah proses mengenali pola dan karakteristik data sebelum menarik kesimpulan.

### 8.1 Melihat jumlah listing per wilayah

```python
listing_per_area = (
    df.groupby("neighbourhood")["id"]
      .count()
      .sort_values(ascending=False)
)

listing_per_area
```

Penjelasan:
- `groupby("neighbourhood")`: mengelompokkan baris berdasarkan wilayah.
- `["id"]`: memilih kolom ID.
- `count()`: menghitung jumlah ID di setiap wilayah.
- `sort_values(ascending=False)`: mengurutkan dari terbesar ke terkecil.

Visualisasi:

```python
listing_per_area.plot(kind="bar", figsize=(10, 5))
plt.title("Jumlah Listing per Wilayah")
plt.xlabel("Wilayah")
plt.ylabel("Jumlah Listing")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

**Cara membaca:** batang paling tinggi menunjukkan wilayah dengan jumlah listing terbanyak pada dataset.

**Batasan:** jumlah listing menunjukkan supply yang tercatat, bukan otomatis permintaan tinggi.

### 8.2 Menghitung total aktivitas ulasan per wilayah

```python
reviews_per_area = (
    df.groupby("neighbourhood")["reviews_per_month"]
      .sum()
      .sort_values(ascending=False)
)

reviews_per_area
```

`sum()` menjumlahkan nilai aktivitas ulasan bulanan dari seluruh listing di suatu wilayah.

**Interpretasi:** total yang tinggi dapat terjadi karena jumlah listing banyak, aktivitas per listing tinggi, atau kombinasi keduanya.

### 8.3 Menghitung rata-rata aktivitas ulasan per listing

```python
avg_reviews_per_area = (
    df.groupby("neighbourhood")["reviews_per_month"]
      .mean()
      .sort_values(ascending=False)
)

avg_reviews_per_area
```

Perbedaan penting:
- `sum()` menggambarkan total aktivitas ulasan dari semua listing dalam kelompok.
- `mean()` menggambarkan rata-rata aktivitas ulasan per listing dalam kelompok.

Jangan mencampuradukkan keduanya.

Contoh sederhana:

| Wilayah | Jumlah listing | Total reviews/month | Rata-rata per listing |
|---|---:|---:|---:|
| A | 100 | 200 | 2 |
| B | 20 | 80 | 4 |

Wilayah A memiliki total lebih besar, tetapi rata-rata per listing di Wilayah B lebih tinggi.

### 8.4 Membandingkan tipe kamar

```python
room_summary = (
    df.groupby("room_type")
      .agg(
          jumlah_listing=("id", "count"),
          rata_rata_harga=("price", "mean"),
          rata_rata_ulasan=("reviews_per_month", "mean")
      )
      .sort_values("rata_rata_ulasan", ascending=False)
)

room_summary
```

`agg()` memungkinkan beberapa ringkasan dihitung sekaligus.

Contoh visualisasi:

```python
room_summary["rata_rata_ulasan"].plot(
    kind="bar",
    figsize=(9, 5)
)

plt.title("Rata-rata Aktivitas Ulasan per Tipe Kamar")
plt.xlabel("Tipe Kamar")
plt.ylabel("Rata-rata reviews per month")
plt.xticks(rotation=30)
plt.tight_layout()
plt.show()
```

### 8.5 Hubungan harga dan aktivitas ulasan

```python
plt.figure(figsize=(8, 5))

plt.scatter(
    df["price"],
    df["reviews_per_month"],
    alpha=0.5
)

plt.title("Harga vs Aktivitas Ulasan Bulanan")
plt.xlabel("Harga")
plt.ylabel("Reviews per Month")
plt.tight_layout()
plt.show()
```

Scatter plot membantu melihat pola hubungan dua variabel numerik.

Contoh interpretasi:
- titik cenderung naik dari kiri ke kanan: kemungkinan hubungan positif;
- titik cenderung turun: kemungkinan hubungan negatif;
- titik menyebar tanpa pola jelas: hubungan linear mungkin lemah.

Korelasi:

```python
correlation = df["price"].corr(df["reviews_per_month"])
print(correlation)
```

Pedoman interpretasi kasar nilai korelasi Pearson:

| Nilai absolut korelasi | Gambaran umum |
|---:|---|
| 0.00–0.19 | Sangat lemah |
| 0.20–0.39 | Lemah |
| 0.40–0.59 | Sedang |
| 0.60–0.79 | Kuat |
| 0.80–1.00 | Sangat kuat |

Pedoman ini bukan aturan universal. Korelasi juga tidak membuktikan sebab-akibat.

---

## 9. Statistik Deskriptif

Statistik deskriptif digunakan untuk meringkas data agar karakteristiknya lebih mudah dipahami.

### 9.1 Mean

Mean adalah rata-rata.

Rumus:

\[
\text{Mean} = \frac{\sum x_i}{n}
\]

Contoh:

Data harga: `100, 150, 200`

\[
\text{Mean} = \frac{100+150+200}{3}=150
\]

Python:

```python
df["price"].mean()
```

Mean mudah dipengaruhi nilai ekstrem.

### 9.2 Median

Median adalah nilai tengah setelah data diurutkan.

Contoh data:

`100, 150, 200`

Median = `150`.

Contoh dengan jumlah data genap:

`100, 150, 200, 250`

Median:

\[
\frac{150+200}{2}=175
\]

Python:

```python
df["price"].median()
```

Median sering lebih representatif untuk data harga yang memiliki nilai ekstrem.

### 9.3 Standard deviation

Standard deviation atau standar deviasi mengukur seberapa jauh nilai data menyebar dari rata-rata.

- Standar deviasi kecil: data relatif berdekatan dengan mean.
- Standar deviasi besar: data lebih menyebar.

Python:

```python
df["price"].std()
```

Secara default, Pandas menggunakan standar deviasi sampel (`ddof=1`).

### 9.4 Variance

Variance atau varians adalah kuadrat dari standar deviasi.

```python
df["price"].var()
```

Satuan variance adalah kuadrat dari satuan data, sehingga standar deviasi biasanya lebih mudah dijelaskan.

### 9.5 Quartile dan IQR

Quartile membagi data yang sudah diurutkan menjadi empat bagian.

- Q1: persentil ke-25.
- Q2: median atau persentil ke-50.
- Q3: persentil ke-75.

IQR (*Interquartile Range*):

\[
IQR = Q3-Q1
\]

Python:

```python
q1 = df["price"].quantile(0.25)
q3 = df["price"].quantile(0.75)

iqr = q3 - q1

print("Q1:", q1)
print("Q3:", q3)
print("IQR:", iqr)
```

IQR menunjukkan rentang 50% data yang berada di tengah.

### 9.6 Membuat ringkasan statistik

```python
df[["price", "reviews_per_month"]].describe()
```

Output biasanya mencakup:
- `count`: jumlah nilai yang tersedia;
- `mean`: rata-rata;
- `std`: standar deviasi;
- `min`: nilai minimum;
- `25%`: Q1;
- `50%`: median;
- `75%`: Q3;
- `max`: nilai maksimum.

### 9.7 Bagaimana menulis insight statistik deskriptif?

Contoh format:

> Rata-rata harga lebih tinggi daripada median harga. Perbedaan tersebut dapat mengindikasikan distribusi harga yang condong ke kanan, kemungkinan karena terdapat sejumlah listing dengan harga tinggi. Hal ini perlu dikonfirmasi melalui histogram atau boxplot.

Jangan menulis bahwa distribusi pasti miring hanya dari satu angka. Periksa juga visualisasi dan bentuk distribusinya.

---

## 10. Statistik Inferensial: Kruskal-Wallis

### 10.1 Apa itu statistik inferensial?

Statistik inferensial digunakan untuk menilai apakah pola yang ditemukan pada sampel memberikan bukti statistik mengenai perbedaan atau hubungan pada populasi yang menjadi sasaran inferensi.

Hasil inferensial tetap bergantung pada kualitas sampel, desain pengumpulan data, dan asumsi metode.

### 10.2 Pertanyaan pengujian

Contoh pertanyaan:

> Apakah distribusi aktivitas ulasan bulanan berbeda antarjenis kamar?

Variabel:
- numerik: `reviews_per_month`;
- kelompok: `room_type`.

### 10.3 Mengapa Kruskal-Wallis?

Kruskal-Wallis merupakan uji nonparametrik untuk membandingkan distribusi/ranking dari tiga atau lebih kelompok independen.

Uji ini sering dipilih ketika pendekatan ANOVA tidak sesuai, misalnya data sangat tidak normal atau memiliki karakteristik yang tidak cocok dengan asumsi ANOVA.

Namun, Kruskal-Wallis bukan sekadar “uji median” dalam semua kondisi. Interpretasi sebagai perbedaan median memerlukan bentuk distribusi kelompok yang relatif sebanding.

### 10.4 Hipotesis

- **H0 (hipotesis nol):** tidak ada perbedaan distribusi/ranking aktivitas ulasan antar kelompok tipe kamar.
- **H1 (hipotesis alternatif):** setidaknya ada satu kelompok yang berbeda.

### 10.5 Contoh kode

```python
from scipy.stats import kruskal

groups = []

for room in df["room_type"].dropna().unique():
    values = df.loc[
        df["room_type"] == room,
        "reviews_per_month"
    ].dropna()

    groups.append(values)

statistic, p_value = kruskal(*groups)

print("Kruskal-Wallis statistic:", statistic)
print("P-value:", p_value)
```

### 10.6 Membaca p-value

Gunakan tingkat signifikansi, misalnya:

\[
\alpha = 0.05
\]

Aturan keputusan:

- Jika `p-value < 0.05`: tolak H0. Ada bukti statistik bahwa setidaknya satu kelompok berbeda.
- Jika `p-value >= 0.05`: gagal menolak H0. Bukti yang tersedia belum cukup untuk menyatakan adanya perbedaan statistik.

**Penting:** “gagal menolak H0” bukan berarti semua kelompok terbukti sama.

### 10.7 Contoh interpretasi

Jika p-value = 0.02:

> Karena p-value 0.02 lebih kecil dari 0.05, H0 ditolak. Terdapat bukti statistik bahwa distribusi aktivitas ulasan tidak sama pada seluruh tipe kamar. Uji ini belum menunjukkan pasangan tipe kamar mana yang berbeda. Analisis lanjutan (*post-hoc test*) diperlukan untuk mengetahui pasangan kelompok yang berbeda.

Jika p-value = 0.20:

> Karena p-value 0.20 lebih besar dari 0.05, H0 gagal ditolak. Data belum memberikan bukti statistik yang cukup untuk menyatakan adanya perbedaan distribusi aktivitas ulasan antarjenis kamar.

### 10.8 Hal yang perlu diperhatikan

- Kelompok harus independen.
- Observasi yang dibandingkan harus sesuai dengan unit analisis.
- Tangani missing values secara konsisten.
- Periksa ukuran sampel setiap kelompok.
- Jika hasil signifikan, pertimbangkan post-hoc test dengan koreksi perbandingan berganda.
- Jangan menyimpulkan sebab-akibat dari uji ini.

---

## 11. Membuat Kesimpulan dan Rekomendasi

Kesimpulan harus menjawab pertanyaan analisis, bukan hanya mengulang kode.

Gunakan struktur:

1. Ringkas temuan supply.
2. Ringkas aktivitas ulasan sebagai proxy.
3. Jelaskan pola harga.
4. Jelaskan hasil statistik deskriptif.
5. Jelaskan hasil statistik inferensial.
6. Nyatakan keterbatasan.
7. Berikan rekomendasi yang sejalan dengan bukti.

Contoh template:

> Berdasarkan dataset yang dianalisis, wilayah dengan jumlah listing terbanyak adalah **[nama wilayah]**. Wilayah dengan total aktivitas ulasan bulanan tertinggi adalah **[nama wilayah]**, sedangkan rata-rata aktivitas ulasan per listing tertinggi ditemukan pada **[nama wilayah]**.
>
> Dari sisi tipe kamar, **[tipe kamar]** memiliki rata-rata aktivitas ulasan per listing tertinggi. Hubungan antara harga dan aktivitas ulasan menunjukkan korelasi sebesar **[nilai korelasi]**, yang menggambarkan hubungan **[arah dan kekuatan]**.
>
> Hasil uji Kruskal-Wallis menghasilkan p-value **[nilai]**. Berdasarkan tingkat signifikansi 0.05, keputusan pengujian adalah **[keputusan]**.
>
> Temuan ini dapat digunakan sebagai gambaran awal untuk membandingkan supply, harga, dan aktivitas ulasan. Namun, aktivitas ulasan bukan data booking atau okupansi. Untuk keputusan investasi yang lebih kuat, diperlukan data transaksi, jumlah malam terpesan, biaya operasional, dan faktor lokasi lainnya.

Isi bagian dalam tanda kurung siku menggunakan hasil aktual notebook, bukan angka rekaan.

---

## 12. Membuat Dashboard di Tableau Public

Dashboard menyajikan hasil analisis agar pembaca dapat memahami temuan tanpa membaca seluruh kode.

### 12.1 Empat visualisasi yang disarankan

1. **Listings by Neighbourhood** — jumlah listing per wilayah.
2. **Review Activity by Neighbourhood** — total aktivitas ulasan bulanan per wilayah.
3. **Average Review Activity by Room Type** — rata-rata aktivitas ulasan per tipe kamar.
4. **Price vs Reviews per Month** — scatter plot harga dan aktivitas ulasan.

Dashboard minimal harus mengikuti ketentuan tugas resmi. Dalam brief MS1 yang dibahas, dashboard diminta memiliki minimal empat visualisasi dan dua interaktivitas.

### 12.2 Dua interaktivitas

Contoh:
- Filter `neighbourhood`.
- Filter `room_type`.

Dengan filter, pembaca dapat memilih wilayah atau tipe kamar tertentu dan melihat bagaimana visualisasi berubah.

### 12.3 Statistical Analysis section

Sediakan bagian teks yang merangkum:
- statistik deskriptif;
- metode inferensial;
- hasil uji;
- interpretasi p-value;
- kesimpulan dan batasan.

Jangan hanya menempelkan p-value. Jelaskan arti hasilnya dalam bahasa bisnis.

### 12.4 Alur umum membuat dashboard

1. Siapkan file CSV yang akan digunakan.
2. Buka Tableau Public.
3. Hubungkan Tableau ke file CSV.
4. Buat worksheet untuk setiap visualisasi.
5. Atur judul, label, dan format angka.
6. Tambahkan filter.
7. Buat dashboard baru.
8. Masukkan worksheet ke dashboard.
9. Tambahkan bagian teks untuk statistical analysis.
10. Publish ke Tableau Public.
11. Salin URL dashboard.
12. Cantumkan URL dashboard di bagian atas notebook sesuai ketentuan tugas.

**Catatan:** paket latihan menyediakan CSV ringkasan untuk membantu pembuatan dashboard. File Tableau workbook (`.twb`/`.twbx`) tidak otomatis dibuat; dashboard perlu disusun dan dipublikasikan di Tableau Public.

---

## 13. Isi File dalam Paket ZIP

Paket latihan `MS1_Accommodation_Demand_Supply_Dummy.zip` berisi file-file berikut.

| File | Fungsi |
|---|---|
| `P0M1_nawafi_suren.ipynb` | Notebook utama berisi proses analisis dari awal sampai akhir |
| `data/P0M1_nawafi_suren_dataset.csv` | Dataset dummy untuk latihan |
| `data/tableau_location_summary.csv` | Ringkasan data per wilayah untuk Tableau |
| `data/tableau_room_type_summary.csv` | Ringkasan data per tipe kamar untuk Tableau |
| `README.md` | Petunjuk umum penggunaan paket |
| `description.md` | Deskripsi proyek |
| `MS1_NEWBIE_GUIDE.md` | Panduan pemula |
| `TABLEAU_GUIDE.md` | Petunjuk membuat dashboard |
| `DATA_DICTIONARY.md` | Penjelasan kolom dataset |
| `requirements.txt` | Daftar library Python yang diperlukan |

### 13.1 Urutan belajar yang disarankan

1. Baca `README.md`.
2. Baca `description.md`.
3. Baca `DATA_DICTIONARY.md`.
4. Buka `P0M1_nawafi_suren.ipynb`.
5. Jalankan notebook dari cell paling atas secara berurutan.
6. Bandingkan output dengan penjelasan pada notebook.
7. Baca `TABLEAU_GUIDE.md`.
8. Buat dashboard dari CSV ringkasan.
9. Baca kembali kesimpulan dan checklist.

---

## 14. Cara Menjalankan Notebook

### 14.1 Menyiapkan environment

Aktifkan environment Python yang digunakan untuk belajar. Jika menggunakan Conda:

```bash
conda activate h8_env
```

Masuk ke folder hasil ekstraksi ZIP:

```bash
cd /path/ke/folder/MS1_Accommodation_Demand_Supply_Dummy
```

Install library:

```bash
pip install -r requirements.txt
```

Jika menggunakan Jupyter Notebook:

```bash
jupyter notebook
```

Jika menggunakan JupyterLab:

```bash
jupyter lab
```

Buka file `P0M1_nawafi_suren.ipynb`, lalu jalankan cell dari atas ke bawah.

### 14.2 Jika menggunakan Google Colab

1. Ekstrak ZIP di komputer atau upload file yang dibutuhkan.
2. Upload notebook ke Google Colab.
3. Upload CSV ke sesi Colab atau hubungkan Google Drive.
4. Sesuaikan path pada `pd.read_csv(...)`.
5. Jalankan cell secara berurutan.

Jika path file salah, Python akan menampilkan `FileNotFoundError`. Periksa lokasi file dan path yang digunakan.

---

## 15. Checklist Submission MS1

Gunakan checklist ini sebelum mengumpulkan tugas.

### A. Problem dan dataset

- [ ] Judul proyek jelas.
- [ ] Problem statement menjelaskan masalah bisnis.
- [ ] Tujuan analisis dapat diukur.
- [ ] Dataset berasal dari sumber nyata yang dapat dipertanggungjawabkan sesuai ketentuan MS1.
- [ ] URL sumber dataset dicantumkan.
- [ ] Latar belakang dataset dijelaskan.
- [ ] Periode pengumpulan data dijelaskan jika tersedia.
- [ ] Dataset sudah dikonsultasikan dengan buddy jika diwajibkan.
- [ ] Definisi kolom dan keterbatasan data dijelaskan.

### B. Notebook

- [ ] Nama notebook mengikuti format `P0M1_<student>.ipynb`.
- [ ] URL dashboard diletakkan di bagian atas notebook.
- [ ] Terdapat Introduction.
- [ ] Terdapat Problem Statement and Dataset Description.
- [ ] Terdapat Data Loading.
- [ ] Terdapat Data Cleaning.
- [ ] Terdapat Exploration and Analysis.
- [ ] Terdapat Conclusion.
- [ ] Semua pertanyaan analisis dijawab.
- [ ] Setiap visualisasi memiliki insight.
- [ ] Statistik deskriptif dijelaskan.
- [ ] Statistik inferensial dijelaskan.
- [ ] Jika menggunakan BigQuery/SQL, query SQL disertakan sesuai ketentuan.
- [ ] Notebook dapat dijalankan dari awal sampai akhir.

### C. Dashboard

- [ ] Dashboard dipublikasikan di Tableau Public.
- [ ] Minimal empat visualisasi tersedia.
- [ ] Minimal dua interaktivitas tersedia.
- [ ] Bagian Statistical Analysis tersedia.
- [ ] Judul dan label grafik mudah dipahami.
- [ ] Angka dan interpretasi sesuai dengan hasil notebook.

### D. Pengumpulan

- [ ] Dataset final menggunakan data nyata, bukan data dummy latihan.
- [ ] Notebook menggunakan dataset final.
- [ ] URL Tableau Public sudah benar.
- [ ] README/description disiapkan sesuai instruksi.
- [ ] File diunggah ke GitHub Classroom sesuai instruksi kelas.
- [ ] Tidak ada file rahasia atau kredensial yang ikut terunggah.

---

## 16. Kesalahan Umum Pemula

### Kesalahan 1 — Menganggap banyak listing berarti demand tinggi

Tidak selalu. Banyak listing menunjukkan supply yang tercatat. Demand aktual memerlukan data pemesanan atau transaksi.

### Kesalahan 2 — Menganggap `availability_365` sebagai okupansi

Tidak tepat. Kolom tersebut menunjukkan jumlah hari listing tersedia pada kalender, bukan jumlah hari yang ditempati tamu.

### Kesalahan 3 — Menggunakan `sum()` dan `mean()` seolah sama

Tidak sama:
- `sum()` menghitung total.
- `mean()` menghitung rata-rata per baris dalam kelompok.

Keduanya menjawab pertanyaan yang berbeda.

### Kesalahan 4 — Menghapus semua outlier

Outlier belum tentu kesalahan. Periksa sumber dan konteks sebelum menghapus.

### Kesalahan 5 — Hanya menampilkan grafik tanpa insight

Grafik harus dijelaskan. Sebutkan temuan, angka pendukung, interpretasi, dan batasan.

### Kesalahan 6 — Menganggap p-value sebagai ukuran besar perbedaan

P-value bukan ukuran seberapa besar atau penting perbedaan. Untuk memahami besar perbedaan, lihat ringkasan kelompok dan ukuran efek yang sesuai.

### Kesalahan 7 — Menganggap hasil signifikan menunjukkan semua kelompok berbeda

Kruskal-Wallis hanya menunjukkan bahwa setidaknya ada satu perbedaan antarkelompok. Untuk mengetahui pasangan kelompok yang berbeda, diperlukan post-hoc test.

### Kesalahan 8 — Mengumpulkan data dummy sebagai hasil final

Data dummy hanya untuk latihan. Untuk submission, ikuti syarat dataset resmi MS1 dan gunakan sumber data nyata.

---

## 17. Glosarium untuk Newbie

| Istilah | Penjelasan sederhana |
|---|---|
| Dataset | Kumpulan data yang dianalisis |
| Row | Baris data |
| Column | Kolom/variabel |
| DataFrame | Struktur tabel pada Pandas |
| Data cleaning | Memeriksa dan memperbaiki kualitas data |
| Missing value | Nilai kosong/tidak tersedia |
| Duplicate | Data duplikat |
| Outlier | Nilai yang jauh berbeda dari sebagian besar data |
| EDA | Exploratory Data Analysis, proses mengenali data |
| GroupBy | Mengelompokkan data berdasarkan kategori |
| Aggregation | Menghitung ringkasan seperti sum, mean, count |
| Mean | Rata-rata |
| Median | Nilai tengah setelah data diurutkan |
| Standard deviation | Ukuran penyebaran data dari rata-rata |
| Variance | Kuadrat dari standar deviasi |
| Quartile | Pembagian data terurut menjadi empat bagian |
| IQR | Rentang antara Q3 dan Q1 |
| Correlation | Ukuran arah dan kekuatan hubungan dua variabel |
| Inferential statistics | Statistik untuk menguji dugaan/perbedaan berdasarkan sampel |
| P-value | Ukuran kompatibilitas hasil data dengan hipotesis nol dalam kerangka uji |
| H0 | Hipotesis nol |
| H1 | Hipotesis alternatif |
| Dashboard | Tampilan ringkasan analisis dalam satu halaman |
| Stakeholder | Pihak yang berkepentingan terhadap hasil analisis |
| Proxy | Indikator pengganti/tidak langsung untuk konsep yang tidak diukur langsung |

---

## 18. Ringkasan Flow MS1

```text
IDE BISNIS
   |
   v
PROBLEM STATEMENT
   |
   v
5W + 1H
   |
   v
ANALYSIS QUESTIONS
   |
   v
DATASET NYATA + SUMBER
   |
   v
DATA LOADING
   |
   v
DATA UNDERSTANDING
   |
   v
DATA CLEANING
   |
   v
EDA
   |
   +----------------------+
   |                      |
   v                      v
VISUAL ANALYSIS     DESCRIPTIVE STATISTICS
   |                      |
   +----------+-----------+
              |
              v
      INFERENTIAL STATISTICS
              |
              v
       BUSINESS INSIGHTS
              |
              v
        CONCLUSION
              |
              v
       TABLEAU DASHBOARD
              |
              v
       FINAL SUBMISSION
```

---

## 19. Pesan Akhir untuk Pemula

Jangan berusaha menghafal semua kode sekaligus. Pahami alur berpikirnya.

Setiap kali melihat satu cell kode, tanyakan:

1. **Apa tujuan kode ini?**
2. **Kolom apa yang sedang digunakan?**
3. **Operasi apa yang dilakukan?**
4. **Output apa yang dihasilkan?**
5. **Bagaimana output tersebut menjawab pertanyaan analisis?**
6. **Apa batasan dari kesimpulan yang bisa dibuat?**

Urutan belajar paling penting adalah:

**Problem → Data → Cleaning → EDA → Statistics → Insight → Dashboard → Conclusion.**

Notebook dummy membantu memahami proses teknis. Setelah alurnya dipahami, ganti dataset dengan dataset nyata yang memenuhi ketentuan MS1, sesuaikan pertanyaan analisis dengan kolom yang benar-benar tersedia, lalu jalankan ulang seluruh analisis.

> **Prinsip utama data analyst:** jangan memaksakan data untuk menjawab pertanyaan yang tidak dapat didukung oleh data. Kesimpulan yang jujur dan terbatas lebih baik daripada klaim besar yang tidak memiliki bukti.
