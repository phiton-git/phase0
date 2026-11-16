# Panduan Newbie Lengkap — GC3 Set 2 (Data Dummy)

**Nama:** Nawafi Suren  
**Batch:** HCK-47  
**Kasus:** Analisis listing rumah dijual di Bekasi  
**Dataset latihan:** 120 baris, 12 halaman simulasi

> **PENTING:** Seluruh data dalam paket ini adalah dummy/sintetis. Tidak ada scraping aktual dari Rumah123. Angka 12 halaman hanya simulasi. Untuk submission GC3 resmi, tetap lakukan scraping aktual minimal 10 halaman dan kumpulkan minimal 100 listing.

## Daftar isi

1. Memahami kasus dan alur GC3
2. Menyiapkan file dan menjalankan notebook
3. A — Web scraping (simulasi)
4. B — Data preparation
5. C — Business understanding dan SMART
6. D — Analysis
7. E — Conclusion
8. Checklist submission dan troubleshooting
9. Peta hafalan fungsi penting

---

# 1. Memahami kasus dan alur GC3

## 1.1 Cerita kasus

Bayangkan kita ingin membeli rumah di Bekasi. Sebelum menentukan anggaran, kita ingin mengetahui kisaran harga rumah, apakah rumah dengan carport cenderung lebih mahal, dan apakah luas tanah atau luas bangunan memiliki hubungan yang lebih kuat dengan harga.

Data belum tersedia, sehingga dalam tugas asli kita harus mengumpulkan listing dari Rumah123. Setelah data terkumpul, kita memeriksa kualitasnya, membersihkan format, melakukan analisis statistik, lalu menjelaskan hasilnya dalam konteks keputusan pembelian.

## 1.2 Alur besar pengerjaan

```text
Memahami masalah bisnis
        ↓
Mengumpulkan listing dari website
        ↓
Menyimpan data mentah
        ↓
Memahami struktur dan kualitas data
        ↓
Membersihkan data
        ↓
Statistik deskriptif
        ↓
Confidence Interval 95%
        ↓
Uji hipotesis carport
        ↓
Korelasi luas tanah dan bangunan
        ↓
Insight dan kesimpulan
```

Cara mengingat: **Collect → Understand → Clean → Analyze → Explain.**

## 1.3 Arti kolom

| Kolom | Arti | Contoh |
|---|---|---|
| Lokasi | Area rumah di Bekasi | Jatiasih |
| Luas Tanah | Luas bidang tanah | 120 m² |
| Luas Bangunan | Luas bangunan | 90 m² |
| Carport | Jumlah carport | 0, 1, 2 |
| Jumlah Kamar Tidur | Banyak kamar tidur | 3 |
| Jumlah Kamar Mandi | Banyak kamar mandi | 2 |
| Harga Rumah | Harga penawaran | Rp 850 Juta |
| page | Nomor halaman simulasi | 1 |
| listing_id | ID unik latihan | BK0101 |

`page` dan `listing_id` adalah kolom pendukung latihan, bukan atribut utama analisis harga.

# 2. Menyiapkan file dan menjalankan notebook

## 2.1 Isi ZIP

Setelah ZIP diekstrak, susunannya:

```text
GC3_Dummy_Submission/
├── P0G3_nawafi_suren_DUMMY.ipynb
├── P0G3_data_nawafi_suren_DUMMY.csv
└── README.md
```

- `.ipynb`: kode, penjelasan, tabel, grafik, dan hasil.
- `.csv`: data mentah dummy.
- `README.md`: petunjuk singkat.

Notebook dan CSV harus berada di folder yang sama agar `read_csv()` menemukan file.

## 2.2 Menjalankan di VS Code

1. Ekstrak ZIP.
2. Buka VS Code → **File → Open Folder** → pilih folder hasil ekstrak.
3. Buka `P0G3_nawafi_suren_DUMMY.ipynb`.
4. Pastikan ekstensi Python dan Jupyter terpasang.
5. Pilih kernel Python yang benar.
6. Jalankan cell dari paling atas sampai bawah.

Install library jika diperlukan:

```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

Jika memakai Conda:

```bash
conda activate h8_env
pip install pandas numpy matplotlib seaborn scipy jupyter
```

`ModuleNotFoundError` biasanya berarti library belum terpasang pada environment/kernel yang sedang digunakan. Pastikan instalasi dan kernel menunjuk ke environment yang sama.

## 2.3 Mengenal cell

- **Markdown cell:** judul, penjelasan, rumus, dan interpretasi.
- **Code cell:** kode Python yang dijalankan.

Biasakan membaca Markdown, menjalankan kode, lalu memahami outputnya.

# 3. A — Web Scraping (simulasi)

## 3.1 Apa itu web scraping?

Web scraping adalah pengambilan informasi dari halaman website secara terprogram. Pada tugas asli, BeautifulSoup digunakan untuk membaca HTML dan mengambil atribut listing.

Contoh elemen HTML yang diberikan soal:

```html
<div class="card-featured__middle-section__price">
    <strong>Rp 770 Juta</strong>
</div>
<a title="Rumah Bagus di Bekasi Selatan" href="/properti/bekasi/hos19505931/">
    <h2>Rumah Bagus di Bekasi Selatan</h2>
</a>
<span>Jaka Mulya, Bekasi</span>
<div class="attribute-info">LT : <span>300 m²</span></div>
<div class="attribute-info">LB : <span>45 m²</span></div>
```

HTML adalah struktur elemen halaman. BeautifulSoup membantu mencari elemen berdasarkan tag, class, atribut, atau hubungan antarelemen.

## 3.2 Mengapa notebook menggunakan CSV dummy?

Agar kita dapat mempelajari semua tahap analisis tanpa tergantung pada perubahan HTML Rumah123, akses website, atau pembatasan request. Karena itu notebook membaca CSV dummy, bukan melakukan scraping aktual.

Pada tugas asli, alurnya:

```text
Request halaman 1 → parse HTML → ambil listing
Request halaman 2 → parse HTML → ambil listing
...
Minimal halaman 10 → gabungkan listing → DataFrame → CSV
```

Jangan menyebut dataset dummy sebagai hasil scraping.

## 3.3 Membaca CSV

```python
df_raw = pd.read_csv("P0G3_data_nawafi_suren_DUMMY.csv")
```

- `pd` adalah alias Pandas.
- `read_csv()` membaca file CSV.
- `df_raw` adalah DataFrame berisi data mentah.
- Data mentah belum dibersihkan.

Periksa jumlah baris dan halaman:

```python
print("Jumlah listing:", len(df_raw))
print("Jumlah halaman simulasi:", df_raw["page"].nunique())
```

- `len(df_raw)` menghitung baris.
- `df_raw["page"]` mengambil kolom page.
- `nunique()` menghitung jumlah nilai unik.

Hasil dataset latihan: 120 baris dan 12 nomor halaman simulasi.

## 3.4 Menampilkan sampel

```python
df_raw.head()
df_raw.sample(5, random_state=47)
```

- `head()` menampilkan lima baris pertama secara default.
- `sample(5)` mengambil lima baris acak.
- `random_state=47` membuat sampel dapat diulang.

Amati apakah harga masih teks, luas masih mengandung `m²`, lokasi masih memiliki `, Bekasi`, dan apakah ada nilai kosong.

**Contoh insight awal:** data masih mentah. Harga dan luas belum seluruhnya numerik, sehingga belum siap untuk analisis statistik. Kita perlu membersihkan format dan tipe datanya.

# 4. B — Data Preparation

Data preparation adalah proses menyiapkan data agar formatnya konsisten dan sesuai untuk analisis.

## 4.1 Memeriksa struktur dengan `info()`

```python
df_raw.info()
```

Informasi yang ditampilkan:
- jumlah baris;
- nama kolom;
- jumlah nilai non-null;
- tipe data;
- penggunaan memori.

Tipe data umum:

| Tipe | Makna |
|---|---|
| `int64` | Bilangan bulat |
| `float64` | Bilangan desimal |
| `object` | Umumnya teks/string |
| `bool` | True/False |

Kolom harga bertipe `object` belum bisa langsung dihitung mean atau standar deviasinya.

## 4.2 Ringkasan dengan `describe()`

```python
df_raw.describe(include="all").T
```

- `describe()` membuat ringkasan statistik.
- `include="all"` mencakup numerik dan kategorikal.
- `.T` mengubah orientasi tabel.

Arti beberapa output numerik:

| Output | Makna |
|---|---|
| `count` | Jumlah nilai tersedia |
| `mean` | Rata-rata |
| `std` | Standar deviasi |
| `min` | Nilai terkecil |
| `25%` | Kuartil pertama |
| `50%` | Median |
| `75%` | Kuartil ketiga |
| `max` | Nilai terbesar |

Perhatikan rentang harga/luas, nilai ekstrem, dan jumlah data yang tersedia pada setiap kolom.

## 4.3 Memeriksa missing value

```python
df_raw.isna().sum()
```

- `isna()` memeriksa apakah sel kosong.
- `sum()` menghitung jumlah nilai kosong pada setiap kolom.

Dalam dataset dummy, sebagian nilai `Carport` sengaja dikosongkan.

### Mengapa carport kosong diisi 0?

Instruksi soal menyebutkan bahwa carport kosong berarti rumah tidak memiliki carport. Maka:

```python
df["Carport"] = (
    pd.to_numeric(df["Carport"], errors="coerce")
      .fillna(0)
      .astype(int)
)
```

- `to_numeric()` mengubah nilai menjadi angka.
- `errors="coerce"` mengubah nilai yang gagal dikonversi menjadi NaN.
- `fillna(0)` mengganti NaN dengan nol.
- `astype(int)` mengubah hasil menjadi integer.

Jangan otomatis mengisi nol untuk semua kolom. Aturan ini hanya berlaku untuk carport sesuai instruksi. Harga atau luas yang hilang tidak berarti nilainya nol.

## 4.4 Memeriksa duplikasi

```python
df_raw.duplicated(subset=["listing_id"]).sum()
```

- `duplicated()` mendeteksi ID yang berulang.
- `subset` menentukan kolom pembanding.
- `sum()` menghitung jumlah duplikasi.

ID lebih tepat untuk pemeriksaan ini karena dua rumah berbeda bisa memiliki lokasi atau harga yang sama.

## 4.5 Membersihkan lokasi

Data awal: `Jatiasih, Bekasi`  
Target: `Jatiasih`

```python
df["Lokasi"] = (
    df["Lokasi"]
      .str.replace(r",\s*Bekasi$", "", regex=True)
      .str.strip()
)
```

- `.str` memberi akses operasi teks pada kolom.
- `replace()` mengganti pola teks.
- `,` mencari koma.
- `\s*` mencari nol atau lebih spasi.
- `Bekasi` mencari kata Bekasi.
- `$` berarti akhir teks.
- `""` berarti diganti dengan teks kosong.
- `strip()` membuang spasi di awal/akhir.

## 4.6 Membersihkan luas tanah dan luas bangunan

Data awal: `120 m²`  
Target: `120`

```python
for col in ["Luas Tanah", "Luas Bangunan"]:
    df[col] = pd.to_numeric(
        df[col].astype(str).str.replace(r"[^0-9]", "", regex=True),
        errors="coerce"
    )
```

- `for` menjalankan proses yang sama untuk dua kolom.
- `astype(str)` memastikan nilai diperlakukan sebagai teks.
- `[^0-9]` berarti karakter selain angka 0–9.
- `replace(..., "")` menghapus karakter selain angka.
- `to_numeric()` mengubah teks hasil pembersihan menjadi angka.

Contoh: `"120 m²"` → `"120"` → `120`.

## 4.7 Membersihkan harga rumah

Semua harga diseragamkan menjadi **juta Rupiah**.

| Harga mentah | Harga numerik |
|---|---:|
| Rp 1,2 Miliar | 1200 |
| Rp 1,6 Miliar | 1600 |
| Rp 600 Juta | 600 |
| Rp 1,3 MiliarHEMAT 100 Jutaan | 1300 |

Mengapa 1,2 Miliar menjadi 1200? Karena 1 Miliar = 1000 Juta.

Fungsi pembersih:

```python
def harga_ke_juta(value):
    s = str(value).split("HEMAT")[0].strip().replace("Rp", "").replace(" ", "")
    m = re.search(r"([0-9]+(?:[.,][0-9]+)?)(Miliar|Juta)", s, flags=re.I)

    if not m:
        return np.nan

    angka = float(m.group(1).replace(",", "."))
    return angka * 1000 if m.group(2).lower() == "miliar" else angka
```

Bedah fungsi:
1. `def` membuat fungsi.
2. `str(value)` memastikan input menjadi teks.
3. `split("HEMAT")[0]` mengambil harga sebelum kata promo HEMAT.
4. `strip()` membuang spasi di tepi.
5. `replace("Rp", "")` menghapus simbol Rupiah.
6. `replace(" ", "")` menghapus spasi.
7. `re.search()` mencari pola angka dan satuan.
8. Jika pola tidak ditemukan, hasilnya `NaN`.
9. `float()` mengubah angka menjadi desimal.
10. Jika satuan Miliar, kalikan 1000; jika Juta, nilainya tetap.

Contoh: `1,2 Miliar` → `1.2 × 1000` → `1200` juta Rupiah.

Terapkan fungsi ke seluruh kolom:

```python
df["Harga Rumah"] = df["Harga Rumah"].apply(harga_ke_juta)
```

`apply()` menjalankan fungsi pada setiap nilai kolom.

## 4.8 Mengubah tipe data numerik

```python
numeric_cols = [
    "Luas Tanah", "Luas Bangunan", "Carport",
    "Jumlah Kamar Tidur", "Jumlah Kamar Mandi", "Harga Rumah"
]

df[numeric_cols] = df[numeric_cols].apply(pd.to_numeric, errors="coerce")
```

Analisis statistik membutuhkan angka. Teks `"120"` berbeda dari angka `120` bagi Python.

## 4.9 Validasi hasil cleaning

```python
df.isna().sum()
df_clean = df.dropna(subset=numeric_cols).copy()
```

- Periksa missing value setelah cleaning.
- `dropna(subset=numeric_cols)` menghapus baris yang masih kosong pada kolom numerik penting.
- `copy()` membuat salinan DataFrame.

Lalu periksa:

```python
df_clean.describe().T
df_clean.dtypes
```

Pastikan tipe data benar dan jumlah baris yang tersisa masuk akal.

**Outlier tidak otomatis dihapus.** Soal meminta indikasi outlier alami dibiarkan.

# 5. C — Business Understanding dan SMART

## 5.1 Problem statement

Problem statement adalah pernyataan masalah yang ingin dijawab oleh analisis. Jangan hanya menyalin pertanyaan soal. Tuliskan dari sudut pandang calon pembeli.

Contoh satu kalimat:

> Sebelum menentukan anggaran pembelian rumah di Bekasi, saya perlu memahami estimasi rata-rata harga listing, mengetahui apakah rumah ber-carport cenderung lebih mahal daripada rumah tanpa carport, serta mengidentifikasi apakah luas tanah atau luas bangunan memiliki hubungan lebih kuat dengan harga.

## 5.2 SMART framework

| Elemen | Pertanyaan | Penerapan |
|---|---|---|
| Specific | Apa yang dianalisis? | Listing rumah dijual di Bekasi |
| Measurable | Bagaimana mengukurnya? | Minimal 100 listing/10 halaman, CI 95%, p-value, korelasi |
| Achievable | Apakah bisa dilakukan? | Atribut tersedia pada listing |
| Relevant | Mengapa penting? | Membantu memahami anggaran pembelian |
| Time-bound | Kapan diselesaikan? | Sebelum menetapkan anggaran pembelian |

SMART menjelaskan kelayakan problem statement. Soal meminta satu kalimat problem statement, kemudian lima elemen SMART dijabarkan sebagai penjelasan.

# 6. D — Analysis

## 6.1 Statistik deskriptif

Soal meminta mean, median, standar deviasi, skewness, dan kurtosis untuk seluruh kolom numerik yang relevan.

```python
stats_desc = pd.DataFrame({
    "mean": df_clean[analysis_cols].mean(),
    "median": df_clean[analysis_cols].median(),
    "std": df_clean[analysis_cols].std(),
    "skewness": df_clean[analysis_cols].skew(),
    "kurtosis": df_clean[analysis_cols].kurt()
})
```

### Mean

Mean adalah jumlah semua nilai dibagi banyaknya data.

\[
\bar{x} = \frac{\sum x_i}{n}
\]

Contoh: harga 500, 600, 700 → mean = 600 juta.

Mean sensitif terhadap nilai ekstrem. Satu rumah yang sangat mahal bisa menaikkan rata-rata.

### Median

Median adalah nilai tengah setelah data diurutkan.

Contoh: 500, 700, 600 → urut menjadi 500, 600, 700 → median 600.

Jika jumlah data genap, median adalah rata-rata dua nilai tengah. Median umumnya lebih tahan terhadap outlier dibanding mean.

### Standard deviation

Standar deviasi mengukur seberapa menyebar nilai dari rata-ratanya.

- Kecil: data cenderung berdekatan.
- Besar: data lebih bervariasi.

Untuk harga, satuannya tetap juta Rupiah.

### Skewness

Skewness menunjukkan kemencengan distribusi.

| Nilai | Interpretasi umum |
|---|---|
| Mendekati 0 | Relatif simetris |
| Positif | Ekor distribusi lebih panjang ke kanan |
| Negatif | Ekor distribusi lebih panjang ke kiri |

Harga rumah dapat memiliki skewness positif karena sebagian kecil rumah memiliki harga jauh lebih tinggi.

### Kurtosis

Pandas menghitung **excess kurtosis**:

- Mendekati 0: mirip distribusi normal dalam ukuran kurtosis.
- Positif: ekor relatif lebih berat.
- Negatif: ekor relatif lebih ringan.

Kurtosis bukan bukti tunggal ada/tidaknya outlier.

## 6.2 Histogram dan boxplot

Histogram memperlihatkan frekuensi harga pada interval tertentu. Boxplot memperlihatkan median, kuartil, rentang, dan titik yang berpotensi menjadi outlier.

Titik di luar whisker tidak otomatis salah. Rumah besar atau berada di lokasi premium memang mungkin mahal. Instruksi GC3 meminta outlier alami dibiarkan.

## 6.3 Confidence Interval 95%

### Tujuan

Mengestimasi interval yang masuk akal bagi **rata-rata populasi harga** berdasarkan sampel listing.

CI 95% bukan berarti 95% rumah pasti memiliki harga di antara batas bawah dan batas atas. Secara frequentist, jika prosedur sampling dan perhitungan diulang berkali-kali, sekitar 95% interval yang terbentuk akan mencakup rata-rata populasi sebenarnya.

### Mengapa distribusi t?

Standar deviasi populasi tidak diketahui. Kita menggunakan standar deviasi sampel dan distribusi t.

\[
CI = \bar{x} \pm t_{0.975,n-1}\frac{s}{\sqrt{n}}
\]

Keterangan:
- \(\bar{x}\): rata-rata sampel;
- \(t\): nilai kritis distribusi t;
- \(s\): standar deviasi sampel;
- \(n\): jumlah sampel;
- \(s/\sqrt{n}\): standard error.

Kode:

```python
prices = df_clean["Harga Rumah"].dropna()
n = len(prices)
mean_price = prices.mean()
std_price = prices.std(ddof=1)

t_crit = stats.t.ppf(0.975, df=n-1)
margin = t_crit * stats.sem(prices)

ci_lower = mean_price - margin
ci_upper = mean_price + margin
```

Penjelasan:
1. Ambil kolom harga dan hilangkan nilai kosong.
2. Hitung jumlah sampel.
3. Hitung mean.
4. Hitung standar deviasi sampel (`ddof=1`).
5. `stats.t.ppf(0.975, df=n-1)` mencari nilai kritis t untuk CI dua sisi 95%.
6. `stats.sem()` menghitung standard error.
7. Margin of error = nilai kritis × standard error.
8. Batas bawah = mean − margin.
9. Batas atas = mean + margin.

Template insight:

> Berdasarkan sampel listing, rata-rata harga rumah adalah Rp ... juta. Dengan confidence level 95%, estimasi interval rata-rata populasi berada pada Rp ... juta sampai Rp ... juta. Interval ini dapat menjadi referensi awal anggaran, tetapi keterwakilan sampel listing terhadap seluruh pasar perlu dipertimbangkan.

Isi titik-titik menggunakan output notebook, bukan angka tebakan.

## 6.4 Uji hipotesis carport

### Membentuk dua kelompok

- Kelompok 1: rumah dengan carport (`Carport > 0`).
- Kelompok 0: rumah tanpa carport (`Carport == 0`).

Kita membandingkan rata-rata harga dua kelompok independen.

### Hipotesis satu arah

\[
H_0: \mu_{carport} \leq \mu_{tanpa}
\]

\[
H_1: \mu_{carport} > \mu_{tanpa}
\]

- H0: rata-rata harga rumah ber-carport tidak lebih tinggi.
- H1: rata-rata harga rumah ber-carport lebih tinggi.
- Jenis: uji beda dua rata-rata independen, satu arah.
- Tingkat signifikansi: α = 0,05.

### Mengapa Welch t-test?

Welch independent two-sample t-test membandingkan dua rata-rata tanpa mengasumsikan varians kedua kelompok sama.

```python
t_stat, p_two = stats.ttest_ind(
    g1, g0, equal_var=False, nan_policy="omit"
)
```

- `ttest_ind`: uji dua kelompok independen.
- `equal_var=False`: memakai Welch test.
- `nan_policy="omit"`: mengabaikan nilai kosong.
- `t_stat`: statistik uji.
- `p_two`: p-value dua arah dari SciPy.

Karena hipotesis alternatif satu arah, p-value perlu disesuaikan:

```python
p_one = p_two / 2 if t_stat > 0 else 1 - p_two / 2
```

### Aturan keputusan

- Jika p-value satu arah < 0,05: tolak H0.
- Jika p-value satu arah ≥ 0,05: gagal menolak H0.

Gunakan istilah **gagal menolak H0**, bukan “menerima H0” secara mutlak. Bukti yang belum cukup tidak sama dengan bukti bahwa H0 pasti benar.

Template insight jika signifikan:

> Rata-rata harga rumah dengan carport lebih tinggi daripada kelompok tanpa carport. Uji Welch satu arah menghasilkan p-value ... yang lebih kecil dari 0,05. Dengan demikian, terdapat bukti statistik bahwa rata-rata harga kelompok ber-carport lebih tinggi pada dataset yang dianalisis.

Jika tidak signifikan:

> Walaupun rata-rata sampel kelompok ber-carport terlihat ..., p-value ... tidak lebih kecil dari 0,05. Bukti statistik belum cukup untuk menyatakan bahwa rumah dengan carport memiliki rata-rata harga lebih tinggi.

Perbedaan rata-rata tidak membuktikan carport menyebabkan harga naik. Lokasi, luas, kondisi, dan fasilitas lain juga mungkin berpengaruh.

## 6.5 Korelasi luas tanah dan luas bangunan

### Apa itu korelasi?

Korelasi mengukur kekuatan dan arah hubungan dua variabel. Nilainya berkisar -1 sampai +1.

| Nilai absolut | Pedoman umum |
|---:|---|
| 0,00–0,19 | Sangat lemah |
| 0,20–0,39 | Lemah |
| 0,40–0,59 | Sedang |
| 0,60–0,79 | Kuat |
| 0,80–1,00 | Sangat kuat |

Ini hanya pedoman praktis, bukan batas universal.

- Positif: satu variabel meningkat, variabel lain cenderung meningkat.
- Negatif: satu meningkat, variabel lain cenderung menurun.
- Mendekati nol: hubungan monotonik lemah/tidak tampak.

### Pearson atau Spearman?

- **Pearson:** mengukur hubungan linear dan sensitif terhadap outlier.
- **Spearman:** mengukur hubungan monotonik berdasarkan ranking, berguna ketika distribusi menceng atau terdapat outlier.

Notebook menggunakan Spearman sebagai pilihan latihan. Pemilihan metode tetap perlu didukung pemeriksaan karakter data.

### Hipotesis korelasi

\[
H_0: \rho = 0
\]

\[
H_1: \rho \neq 0
\]

Ini pengujian dua arah: menguji apakah ada hubungan, tanpa menentukan arah sejak awal.

Kode:

```python
rho_lt, p_lt = stats.spearmanr(
    df_clean["Luas Tanah"], df_clean["Harga Rumah"]
)

rho_lb, p_lb = stats.spearmanr(
    df_clean["Luas Bangunan"], df_clean["Harga Rumah"]
)
```

- `spearmanr()` menghasilkan koefisien dan p-value.
- `rho_lt` dan `p_lt`: korelasi serta p-value luas tanah dengan harga.
- `rho_lb` dan `p_lb`: korelasi serta p-value luas bangunan dengan harga.

Contoh angka untuk belajar membaca (BUKAN hasil dataset):

```text
Luas Tanah     rho = 0.70, p = 0.001
Luas Bangunan  rho = 0.82, p = 0.001
```

Interpretasi contoh:
- Keduanya memiliki hubungan positif.
- Hubungan luas bangunan lebih kuat karena 0,82 lebih besar daripada 0,70.
- Keduanya signifikan pada α = 0,05 karena p-value < 0,05.

Untuk data aktual, bandingkan nilai absolut `rho` untuk melihat hubungan mana yang lebih kuat. Jangan membandingkan p-value saja.

**Korelasi bukan sebab-akibat.** Korelasi luas yang tinggi tidak membuktikan bahwa memperbesar luas pasti menyebabkan kenaikan harga dengan jumlah tertentu.

Template insight:

> Spearman menunjukkan korelasi luas tanah sebesar ... dengan p-value ..., sedangkan luas bangunan sebesar ... dengan p-value .... Variabel dengan nilai absolut rho lebih besar memiliki hubungan monotonik lebih kuat dengan harga pada sampel. Hasil ini menunjukkan asosiasi, bukan sebab-akibat.

## 6.6 Scatterplot

Scatterplot membantu melihat arah dan pola hubungan antara luas tanah/harga dan luas bangunan/harga. Grafik menjadi pendamping koefisien korelasi, bukan pengganti pengujian statistik.

# 7. E — Conclusion

## 7.1 Bedakan hasil, insight, dan kesimpulan

- **Hasil:** angka yang dikeluarkan kode.
- **Insight:** arti angka dalam konteks pembelian rumah.
- **Kesimpulan:** jawaban ringkas atas tujuan analisis.

| Hasil | Contoh insight |
|---|---|
| Mean lebih besar dari median | Distribusi mungkin menceng ke kanan |
| CI 95% memiliki lower dan upper | Memberi estimasi interval rata-rata harga |
| p-value carport < 0,05 | Ada bukti rata-rata kelompok ber-carport lebih tinggi |
| rho bangunan lebih besar dari rho tanah | Hubungan monotonik bangunan-harga lebih kuat pada sampel |

## 7.2 Template kesimpulan

Isi angka berdasarkan output notebook:

> Berdasarkan analisis terhadap [jumlah] listing, harga rumah memiliki rata-rata Rp [mean] juta dan median Rp [median] juta. Nilai skewness menunjukkan distribusi harga [interpretasi]. Estimasi confidence interval 95% untuk rata-rata harga berada pada Rp [lower] juta hingga Rp [upper] juta.
>
> Uji Welch menunjukkan rata-rata harga rumah dengan carport [lebih tinggi/belum terbukti lebih tinggi] dibandingkan rumah tanpa carport, dengan p-value [nilai]. Pada tingkat signifikansi 5%, keputusan pengujian adalah [tolak/gagal menolak] H0.
>
> Korelasi Spearman luas tanah dengan harga adalah [rho], sedangkan luas bangunan dengan harga adalah [rho]. Variabel dengan hubungan lebih kuat adalah [variabel].
>
> Dengan demikian, analisis ini dapat menjadi referensi awal dalam menyusun anggaran dan menentukan atribut rumah yang diprioritaskan. Hasil tetap perlu ditafsirkan dengan mempertimbangkan keterbatasan sampel listing dan faktor lain yang tidak dianalisis.

Jangan membuat klaim berlebihan seperti:
- “Harga ini pasti harga pasar seluruh Bekasi.”
- “Carport menyebabkan harga naik.”
- “Luas bangunan satu-satunya penentu harga.”
- “95% rumah berada di dalam confidence interval.”

Gunakan istilah yang sesuai bukti: “pada sampel ini”, “terdapat bukti statistik”, “berhubungan”, dan “estimasi”.

# 8. Checklist akhir dan troubleshooting

## 8.1 Checklist pemahaman

- [ ] Memahami fungsi library yang digunakan.
- [ ] Bisa menjelaskan DataFrame mentah dan bersih.
- [ ] Memahami `head()`, `info()`, `describe()`, `isna()`, dan `duplicated()`.
- [ ] Memahami alasan carport kosong diisi nol.
- [ ] Bisa menjelaskan konversi Miliar/Juta ke juta Rupiah.
- [ ] Bisa membedakan mean dan median.
- [ ] Bisa membaca standar deviasi, skewness, dan kurtosis.
- [ ] Memahami confidence interval 95%.
- [ ] Bisa menulis H0 dan H1 uji carport.
- [ ] Memahami alasan penggunaan Welch t-test.
- [ ] Bisa membaca rho dan p-value Spearman.
- [ ] Bisa mengubah hasil menjadi insight bisnis.

## 8.2 Checklist submission resmi

- [ ] Scraping aktual Rumah123 dengan BeautifulSoup.
- [ ] Minimal 10 halaman website benar-benar diakses.
- [ ] Minimal 100 listing aktual terkumpul.
- [ ] Tujuh atribut wajib tersedia.
- [ ] Data scraping disimpan ke CSV dengan nama sesuai instruksi.
- [ ] Notebook memiliki judul, nama, batch, dan pengenalan program.
- [ ] Setiap tahap memiliki penjelasan Markdown.
- [ ] Semua cell berjalan tanpa error.
- [ ] Insight dan kesimpulan berasal dari data aktual.
- [ ] Nama file submission mengikuti format yang ditentukan.

## 8.3 Troubleshooting umum

| Masalah | Penyebab umum | Solusi |
|---|---|---|
| `FileNotFoundError` | CSV tidak ditemukan | Letakkan CSV dan notebook di folder sama; cek nama file |
| `ModuleNotFoundError` | Library belum terpasang di kernel | Install library pada environment kernel aktif |
| `KeyError` | Nama kolom tidak cocok | Periksa `df.columns` |
| Harga menjadi NaN | Format harga tidak cocok dengan parser | Tampilkan harga mentah dan sesuaikan fungsi |
| Terlalu banyak baris terhapus | Banyak missing pada kolom penting | Periksa missing sebelum/sesudah cleaning |
| Notebook berhenti | Ada cell error | Baca traceback, perbaiki cell, lalu jalankan ulang |

# 9. Peta hafalan fungsi penting

| Tahap | Fungsi/kode | Kegunaan |
|---|---|---|
| Baca data | `pd.read_csv()` | Membaca CSV |
| Lihat data | `.head()`, `.sample()` | Melihat sampel |
| Struktur | `.info()`, `.dtypes` | Tipe data dan non-null |
| Ringkasan | `.describe()` | Statistik ringkas |
| Missing | `.isna().sum()` | Menghitung nilai kosong |
| Duplikat | `.duplicated()` | Mendeteksi duplikasi |
| Cleaning teks | `.str.replace()` | Mengganti/menghapus teks |
| Konversi angka | `pd.to_numeric()` | Mengubah menjadi numerik |
| Isi missing | `.fillna(0)` | Mengisi nilai kosong |
| Statistik | `.mean()`, `.median()`, `.std()`, `.skew()`, `.kurt()` | Statistik deskriptif |
| Confidence interval | `stats.t.ppf()`, `stats.sem()` | Menghitung CI |
| Uji beda | `stats.ttest_ind(..., equal_var=False)` | Welch t-test |
| Korelasi | `stats.spearmanr()` | Korelasi Spearman |
| Grafik | `sns.histplot()`, `sns.boxplot()`, `sns.scatterplot()` | Visualisasi |

---

## Penutup: pola berpikir yang harus dikuasai

1. **Pahami masalah bisnis:** calon pembeli membutuhkan informasi harga dan atribut rumah.
2. **Kumpulkan data:** pada tugas asli, scraping listing aktual dari website.
3. **Periksa data:** pahami baris, kolom, tipe data, missing value, dan duplikasi.
4. **Bersihkan data:** ubah teks menjadi angka dan samakan satuan.
5. **Analisis:** statistik deskriptif, confidence interval, uji hipotesis, dan korelasi.
6. **Jelaskan:** hubungkan hasil statistik dengan keputusan pembeli.
7. **Simpulkan:** jawab problem statement menggunakan bukti.

**Pegangan utama:** jangan mulai dari menghafal kode. Mulailah dari memahami pertanyaan bisnis, lalu pahami mengapa setiap kode diperlukan untuk menjawab pertanyaan tersebut.
