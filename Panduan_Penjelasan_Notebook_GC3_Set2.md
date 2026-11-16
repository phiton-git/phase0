# Panduan Notebook GC3 – Set 2 Hacktiv8 FTDS

## Simulasi Analisis Harga Rumah di Bekasi

**Nama:** Nawafi Suren  
**Batch:** HCK-47  
**Notebook pendamping:** `GC3_Set2_Notebook_Final_Dummy_Nawafi_Suren.ipynb`

---

## 1. Tujuan notebook

Notebook ini dibuat untuk mempelajari keseluruhan alur pengerjaan **Graded Challenge 3 – Set 2 (GC3)** dari awal sampai akhir, menggunakan contoh analisis harga rumah di Bekasi.

Fokus pertanyaan bisnis:

> Apakah luas tanah, luas bangunan, dan jumlah carport berkaitan dengan harga rumah?

Notebook disusun untuk pemula (*newbie*). Setiap tahap memiliki kode Python, penjelasan tujuan, dan panduan membaca hasilnya.

## 2. Penting: dataset yang digunakan adalah data dummy

Notebook menggunakan **100 listing sintetis**, dengan simulasi:
- 10 halaman.
- 10 listing per halaman.
- Total 100 listing.

Data tersebut **bukan hasil scraping Rumah123 dan bukan data listing rumah aktual**. Data sengaja dibuat secara matematis agar menunjukkan pola yang ingin dipelajari:
- Luas tanah yang lebih besar cenderung memiliki harga lebih tinggi.
- Luas bangunan yang lebih besar cenderung memiliki harga lebih tinggi.
- Jumlah carport yang lebih banyak cenderung berkaitan dengan harga lebih tinggi.

Data dummy berguna untuk memahami alur pemrograman dan statistik tanpa bergantung pada perubahan tampilan situs web. Data ini tidak boleh diklaim sebagai hasil scraping aktual atau digunakan sebagai dataset final submission GC3.

## 3. Peta alur pengerjaan

1. Import library Python.
2. Simulasi proses scraping 10 halaman.
3. Menggabungkan listing menjadi DataFrame.
4. Memahami struktur data (*data understanding*).
5. Membersihkan data (*data cleaning*).
6. Memvalidasi dan menyimpan dataset bersih.
7. Melakukan Exploratory Data Analysis (EDA).
8. Menghitung statistik deskriptif.
9. Menghitung Confidence Interval 95%.
10. Melakukan Hypothesis Testing.
11. Melakukan Correlation Analysis.
12. Menulis Business Insight dan Conclusion.

---

## 4. Penjelasan setiap tahap

### Tahap 1 — Import library

| Library | Kegunaan |
|---|---|
| `pandas` | Membaca, membersihkan, dan mengolah data berbentuk tabel |
| `numpy` | Operasi numerik dan membuat angka acak untuk data simulasi |
| `matplotlib` | Membuat visualisasi/grafik |
| `seaborn` | Membuat visualisasi statistik |
| `scipy.stats` | Menghitung uji statistik, distribusi t, dan korelasi |

Contoh:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats
```

`as pd`, `as np`, dan seterusnya adalah alias agar nama library lebih singkat ketika dipanggil.

### Tahap 2 — Simulasi scraping 10 halaman

Pada scraping aktual:
- `requests` digunakan untuk mengambil HTML halaman web.
- `BeautifulSoup` digunakan untuk membaca dan mengambil elemen HTML.
- Hasil parsing disimpan sebagai kumpulan data listing.

Pada notebook dummy, fungsi `scrape_page_dummy()` menggantikan proses mengambil HTML. Fungsi tersebut menghasilkan data sintetis dengan struktur kolom seperti hasil scraper.

```python
all_listings = []

for page in range(1, 11):
    page_data = scrape_page_dummy(
        page,
        n_listing=10,
        seed=42
    )
    all_listings.extend(page_data)
```

Penjelasan:
- `range(1, 11)` menghasilkan angka halaman 1 sampai 10.
- `scrape_page_dummy()` membuat 10 listing untuk setiap halaman.
- `extend()` menambahkan seluruh listing dari halaman tersebut ke daftar utama.
- Setelah 10 iterasi, targetnya adalah 100 listing.

**Penting:** angka 10 halaman dan 100 listing di sini adalah simulasi untuk belajar. Ketentuan submission aktual harus mengikuti brief GC3.

### Tahap 3 — Mengubah hasil menjadi DataFrame

```python
df_raw = pd.DataFrame(all_listings)
```

`DataFrame` adalah struktur data berbentuk tabel pada Pandas:
- Baris mewakili satu listing rumah.
- Kolom mewakili atribut rumah.
- `df_raw` adalah dataset mentah sebelum cleaning.

Untuk melihat lima baris pertama:

```python
df_raw.head()
```

Untuk melihat ukuran data:

```python
df_raw.shape
```

Output `shape` berbentuk `(jumlah_baris, jumlah_kolom)`.

### Tahap 4 — Data understanding

Sebelum cleaning, kita perlu memahami isi dataset.

```python
df_raw.info()
df_raw.describe(include="all")
df_raw.isna().sum()
df_raw.duplicated(subset=["listing_id"]).sum()
```

Penjelasan:
- `info()` menampilkan jumlah baris, nama kolom, dan tipe data.
- `describe()` memberikan ringkasan statistik.
- `isna().sum()` menghitung nilai kosong pada setiap kolom.
- `duplicated()` memeriksa data duplikat berdasarkan ID listing.

Tujuannya adalah mengetahui kondisi awal data sebelum mengubah atau menghapus apa pun.

### Tahap 5 — Data cleaning

Data hasil scraping sering kali masih berbentuk teks.

Contoh:
- `"120 m²"` masih berupa string.
- `"2"` pada kolom carport juga bisa terbaca sebagai string.
- `"Rp 1,250 juta"` merupakan teks, bukan angka.

Agar bisa dihitung, data harus dibersihkan dan dikonversi menjadi tipe numerik.

Contoh membersihkan luas tanah:

```python
df["luas_tanah"] = (
    df["luas_tanah"]
    .str.replace("m²", "", regex=False)
    .str.strip()
)

df["luas_tanah"] = pd.to_numeric(
    df["luas_tanah"],
    errors="coerce"
)
```

Penjelasan:
- `str.replace()` menghapus satuan `m²`.
- `str.strip()` menghapus spasi di awal dan akhir teks.
- `pd.to_numeric()` mengubah teks angka menjadi angka.
- `errors="coerce"` mengubah nilai yang gagal dikonversi menjadi `NaN`.

Untuk harga, notebook mengubah format `"Rp 1,250 juta"` menjadi `1250`, dengan satuan juta rupiah.

Setelah cleaning, periksa kembali:

```python
df.isna().sum()
df.dtypes
df.head()
```

**Catatan:** pada data aktual, format harga bisa berbeda-beda. Parser harus disesuaikan dengan format yang benar-benar ditemukan di halaman sumber.

### Tahap 6 — Validasi dan menyimpan dataset bersih

Validasi memastikan hasil cleaning masuk akal.

```python
assert (df["luas_tanah"] > 0).all()
assert (df["harga_juta"] > 0).all()
```

`assert` akan menghentikan eksekusi jika kondisi tidak terpenuhi.

Dataset bersih disimpan dengan:

```python
df.to_csv(
    "gc3_dummy_clean_100_listings.csv",
    index=False
)
```

`index=False` berarti nomor indeks DataFrame tidak ikut ditulis sebagai kolom tambahan.

### Tahap 7 — Exploratory Data Analysis (EDA)

EDA adalah proses mengenali karakteristik data melalui ringkasan angka dan visualisasi.

Notebook membuat beberapa grafik:
1. Histogram distribusi harga.
2. Scatter plot luas tanah terhadap harga.
3. Scatter plot luas bangunan terhadap harga.
4. Boxplot harga berdasarkan jumlah carport.

**Histogram harga** membantu melihat rentang harga yang paling banyak muncul, bentuk distribusi, dan kemungkinan kemencengan.

**Scatter plot luas tanah dan harga** menampilkan satu titik untuk satu listing. Jika titik cenderung naik dari kiri ke kanan, terdapat indikasi hubungan positif.

**Scatter plot luas bangunan dan harga** memeriksa apakah rumah dengan luas bangunan lebih besar cenderung memiliki harga lebih tinggi.

**Boxplot carport dan harga** membandingkan distribusi harga berdasarkan jumlah carport:
- Garis di dalam kotak: median.
- Kotak: rentang kuartil pertama sampai kuartil ketiga.
- Whisker: jangkauan data menurut aturan boxplot.
- Titik di luar whisker: kandidat outlier.

Perbedaan pada grafik belum otomatis berarti perbedaan tersebut signifikan secara statistik.

### Tahap 8 — Descriptive Statistics

| Ukuran | Makna sederhana |
|---|---|
| Mean | Rata-rata |
| Median | Nilai tengah setelah data diurutkan |
| Modus | Nilai yang paling sering muncul |
| Minimum | Nilai terkecil |
| Maximum | Nilai terbesar |
| Range | Maximum dikurangi minimum |
| Variance | Ukuran penyebaran dalam satuan kuadrat |
| Standard deviation | Ukuran penyebaran dalam satuan yang sama dengan data |

Contoh:

```python
harga = df["harga_juta"]

mean_harga = harga.mean()
median_harga = harga.median()
std_harga = harga.std(ddof=1)
variance_harga = harga.var(ddof=1)
```

`ddof=1` digunakan untuk menghitung standar deviasi dan variance sampel.

Interpretasi:
- Mean jauh lebih besar dari median dapat mengindikasikan beberapa harga tinggi menarik rata-rata ke atas.
- Standard deviation besar menunjukkan harga lebih menyebar dari mean.
- Range menunjukkan jarak antara harga terendah dan tertinggi.

Notebook juga menghitung:
- **Skewness:** mengukur kemencengan distribusi. Nilai positif menunjukkan ekor lebih panjang ke kanan.
- **Kurtosis:** mengukur karakteristik ekor distribusi. Pandas memberikan *excess kurtosis*, sehingga distribusi normal bernilai sekitar 0.

### Tahap 9 — Confidence Interval 95%

Confidence Interval (CI) digunakan untuk mengestimasi rentang mean populasi berdasarkan sampel.

Rumus:

\[
CI = \bar{x} \pm t_{\alpha/2, n-1}
\times \frac{s}{\sqrt{n}}
\]

Keterangan:
- \(\bar{x}\): mean sampel.
- \(t\): nilai kritis distribusi t.
- \(s\): standar deviasi sampel.
- \(n\): jumlah sampel.
- \(s/\sqrt{n}\): standard error.

Notebook menggunakan distribusi t karena standar deviasi populasi tidak diketahui.

**Interpretasi yang benar:** CI 95% adalah prosedur estimasi yang dalam pengambilan sampel berulang akan mencakup mean populasi sekitar 95% dari waktu. CI bukan rentang yang berisi 95% listing rumah.

### Tahap 10 — Hypothesis Testing berdasarkan carport

Notebook membandingkan dua kelompok:
- **Kelompok A:** rumah dengan 0–1 carport.
- **Kelompok B:** rumah dengan 2 atau lebih carport.

Hipotesis:
- **H0 (null hypothesis):** rata-rata harga kelompok A dan kelompok B sama.
- **H1 (alternative hypothesis):** rata-rata harga kedua kelompok berbeda.

Uji yang digunakan adalah **Welch's Independent Two-Sample t-Test**. Welch t-test membandingkan mean dua kelompok independen dan tidak mengharuskan varians kedua kelompok sama.

Kode inti:

```python
t_stat, p_value = stats.ttest_ind(
    group_a,
    group_b,
    equal_var=False
)
```

Taraf signifikansi:

```python
alpha = 0.05
```

Aturan keputusan:

| Kondisi | Keputusan |
|---|---|
| p-value < 0.05 | Tolak H0 |
| p-value ≥ 0.05 | Gagal menolak H0 |

Jika p-value < 0.05, terdapat bukti statistik bahwa rata-rata harga kedua kelompok berbeda.

Jika p-value ≥ 0.05, bukti belum cukup untuk menyatakan rata-rata harga kedua kelompok berbeda.

**Penting:** uji ini tidak membuktikan bahwa carport menyebabkan harga rumah naik. Perbedaan harga mungkin juga berkaitan dengan luas, lokasi, atau karakteristik rumah lainnya.

### Tahap 11 — Correlation Analysis

Korelasi mengukur arah dan kekuatan hubungan dua variabel.

**Pearson correlation** mengukur hubungan linear:
- Mendekati +1: hubungan linear positif kuat.
- Mendekati -1: hubungan linear negatif kuat.
- Mendekati 0: hubungan linear lemah.

**Spearman correlation** mengukur hubungan monoton berdasarkan peringkat. Spearman berguna ketika hubungan tidak linear tetapi tetap monoton, atau ketika data memiliki outlier.

```python
pearson_corr = df[corr_cols].corr(method="pearson")
spearman_corr = df[corr_cols].corr(method="spearman")
```

Heatmap digunakan agar hubungan antarvariabel lebih mudah dilihat.

**Korelasi bukan kausalitas.** Korelasi positif antara luas tanah dan harga tidak membuktikan luas tanah sebagai satu-satunya penyebab harga lebih tinggi.

### Tahap 12 — Business Insight & Conclusion

Tahap terakhir mengubah hasil statistik menjadi penjelasan yang berguna untuk konteks bisnis.

Gunakan hasil aktual dari notebook untuk menjawab:
1. Berapa mean dan median harga rumah?
2. Bagaimana tingkat variasi harga berdasarkan standard deviation?
3. Berapa rentang Confidence Interval 95%?
4. Apakah rata-rata harga kelompok carport berbeda secara statistik?
5. Variabel mana yang memiliki korelasi paling kuat dengan harga?
6. Apa rekomendasi yang masuk akal berdasarkan temuan?

Jangan menulis angka kesimpulan sebelum menjalankan notebook. Isi kesimpulan menggunakan output yang benar-benar muncul.

---

## 5. Fokus belajar: luas tanah, luas bangunan, dan carport

Data dummy sengaja dibentuk dengan pola harga:

\[
Harga \approx 180 + 2.7(LT) + 1.9(LB) + 95(Carport) + Noise
\]

Keterangan:
- `LT`: luas tanah dalam m².
- `LB`: luas bangunan dalam m².
- `Carport`: jumlah kapasitas carport.
- `Noise`: variasi acak agar data tidak terlalu sempurna.

Koefisien dalam formula tersebut **hanya aturan pembentuk data dummy**, bukan hasil estimasi pasar properti Bekasi dan bukan rekomendasi harga nyata.

Dengan desain ini, notebook dapat dipakai untuk berlatih membaca pola scatter plot, menghitung korelasi, membandingkan harga menurut kelompok carport, serta membedakan hubungan statistik dan sebab-akibat.

## 6. Cara menjalankan notebook

1. Download file `GC3_Set2_Notebook_Final_Dummy_Nawafi_Suren.ipynb`.
2. Buka [Google Colab](https://colab.research.google.com/).
3. Pilih **File → Upload notebook**.
4. Upload file `.ipynb`.
5. Jalankan cell secara berurutan dari atas ke bawah.
6. Amati output setelah setiap cell.
7. Pastikan file CSV dummy berhasil dibuat.

Jika menjalankan di komputer sendiri, install library yang diperlukan:

```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

## 7. Strategi belajar yang disarankan

Jangan langsung menjalankan semua cell sekaligus.

1. Baca penjelasan Markdown.
2. Jalankan satu cell kode.
3. Amati output.
4. Pastikan memahami fungsi setiap baris.
5. Coba ubah satu parameter kecil.
6. Jalankan ulang dan amati perubahan hasil.

Contoh eksperimen:
- Ubah koefisien luas tanah dari `2.7` menjadi `4.0`.
- Ubah koefisien carport dari `95` menjadi `150`.
- Jalankan ulang notebook.
- Amati perubahan grafik, mean, dan korelasi.

Eksperimen ini membantu memahami bagaimana pola data yang dibentuk memengaruhi hasil analisis. Jangan mengubah parameter untuk membuat hasil statistik tertentu lalu menganggapnya sebagai temuan dunia nyata.

## 8. Batasan dan persiapan submission GC3 aktual

Notebook ini merupakan **notebook pembelajaran**, bukan pengganti notebook submission final.

Sebelum submission GC3:
- [ ] Ganti generator dummy dengan scraper aktual.
- [ ] Ikuti jumlah halaman dan jumlah listing pada brief resmi.
- [ ] Simpan raw data hasil scraping.
- [ ] Bersihkan harga, luas tanah, luas bangunan, carport, dan kolom lain sesuai brief.
- [ ] Validasi hasil parsing secara manual pada beberapa listing.
- [ ] Periksa missing values dan duplikasi.
- [ ] Jalankan ulang EDA dan statistik menggunakan data aktual.
- [ ] Tulis business insight berdasarkan hasil aktual.
- [ ] Cantumkan sumber data dan keterbatasan analisis.
- [ ] Jalankan seluruh notebook dari awal hingga akhir tanpa error.

**Kesimpulan utama:** gunakan notebook dummy untuk memahami *flow* dan konsep. Gunakan data hasil scraping aktual untuk analisis dan submission GC3.
