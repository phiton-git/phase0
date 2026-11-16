# Panduan Lengkap MS1 untuk Pemula

## Property Market Opportunity Analysis

**Student:** Nawafi Suren\
**Program:** Hacktiv8 Data Science Fulltime Program\
**Milestone:** 1 -- Learning Simulation\
**Notebook utama:** `P0M1_nawafi_suren.ipynb`\
**Dataset:** `P0M1_nawafi_suren_dataset.csv`

> **Penting:** Panduan ini dibuat berdasarkan isi file
> `MS1_Property_Market_Opportunity_Analysis.zip`. Dataset di dalamnya
> adalah data dummy/sintetis untuk latihan. Instruksi resmi MS1
> mensyaratkan dataset non-dummy yang sumbernya dapat
> dipertanggungjawabkan dan dikonsultasikan terlebih dahulu kepada
> buddy. Jadi, gunakan paket ini untuk belajar alur pengerjaan, bukan
> langsung sebagai submission resmi.

------------------------------------------------------------------------

# Daftar Isi

1.  Gambaran besar proyek
2.  Memahami business case
3.  Memahami SMART Problem Statement
4.  Identifikasi masalah dengan 5W+1H
5.  Memahami dataset dan data dictionary
6.  Persiapan environment dan folder
7.  Step 1 -- Introduction
8.  Step 2 -- Problem Statement and Dataset Description
9.  Step 3 -- Data Loading
10. Step 4 -- Data Cleaning
11. Step 5 -- Exploration and Analysis
12. Empat visualisasi dan cara membaca insight
13. Statistik deskriptif
14. Statistik inferensial: Welch t-test
15. Menyiapkan data untuk Tableau
16. Membuat dashboard Tableau Public
17. Step 6 -- Conclusion
18. Checklist pengerjaan
19. Kesalahan pemula yang perlu dihindari
20. Batasan proyek dan persiapan submission resmi
21. Glosarium singkat

------------------------------------------------------------------------

# 1. Gambaran Besar Proyek

## 1.1 Apa yang sedang kita kerjakan?

Bayangkan kamu bekerja sebagai Data Analyst di perusahaan properti.
Perusahaan ingin memahami gambaran pasar rumah di beberapa area Bekasi.

Perusahaan mempunyai beberapa pertanyaan:

-   Area mana yang memiliki listing rumah paling banyak?
-   Area mana yang mempunyai median harga penawaran paling tinggi?
-   Apakah rumah dengan tanah lebih luas cenderung mempunyai harga lebih
    tinggi?
-   Apakah rumah dengan bangunan lebih luas cenderung mempunyai harga
    lebih tinggi?
-   Apakah harga rumah dengan carport berbeda dari rumah tanpa carport?

Tugas Data Analyst adalah mengubah data listing menjadi informasi yang
dapat membantu client melakukan investigasi pasar lebih lanjut.

## 1.2 Makna judul

**Property Market Opportunity Analysis** dapat dipecah menjadi:

  Istilah       Arti sederhana   Dalam proyek
  ------------- ---------------- ----------------------------------------------
  Property      Properti         Rumah yang ditawarkan
  Market        Pasar            Kumpulan listing rumah di area yang diamati
  Opportunity   Peluang          Area/segmen yang layak diteliti lebih lanjut
  Analysis      Analisis         Mengolah data, statistik, dan visualisasi

Inti proyek: **mengubah data listing properti menjadi insight awal
tentang karakteristik penawaran rumah.**

Jangan langsung menyimpulkan bahwa area dengan listing terbanyak pasti
mempunyai demand tertinggi. Listing adalah penawaran yang terlihat pada
dataset, sedangkan demand membutuhkan data lain seperti pencarian,
inquiry, booking, transaksi, atau waktu sampai properti terjual.

## 1.3 Alur besar MS1

``` text
Business Understanding
        |
        v
SMART Problem Statement
        |
        v
Identifikasi Masalah (5W+1H)
        |
        v
Dataset Understanding
        |
        v
Data Loading
        |
        v
Data Cleaning
        |
        v
Exploration and Analysis
        |
        +------> 4 Visualisasi
        |
        +------> Statistik Deskriptif
        |
        +------> Statistik Inferensial
        |
        v
Tableau Dashboard
        |
        v
Conclusion & Recommendation
```

**Mengapa urutan ini penting?** Kita harus memahami masalah bisnis
sebelum memilih kolom dan membuat grafik. Grafik yang bagus tetapi tidak
menjawab pertanyaan client belum menyelesaikan masalah bisnis.

------------------------------------------------------------------------

# 2. Memahami Business Case

## 2.1 Siapa client-nya?

Dalam simulasi ini, client adalah perusahaan properti. Pembaca laporan
yang dituju:

-   Manajer pemasaran properti.
-   Tim akuisisi properti.
-   Tim riset pasar.

Mereka tidak membutuhkan kode Python semata. Mereka membutuhkan jawaban
yang mudah dipahami untuk membantu menentukan area atau segmen yang
perlu diperiksa lebih lanjut.

## 2.2 Apa keputusan yang ingin dibantu?

Contoh keputusan yang mungkin dibantu oleh analisis:

-   Memilih area yang akan diteliti lebih dalam.
-   Memahami rentang harga penawaran pada beberapa area.
-   Memahami karakteristik ukuran rumah yang berkaitan dengan harga.
-   Membandingkan karakteristik listing dengan dan tanpa carport.

Analisis ini belum cukup untuk memutuskan pembelian properti atau
menghitung keuntungan investasi. Untuk itu dibutuhkan data transaksi,
biaya, sewa, okupansi, risiko, dan informasi pasar lain.

------------------------------------------------------------------------

# 3. SMART Problem Statement

SMART membantu membuat problem statement yang tidak terlalu luas.

## 3.1 Problem statement proyek

> Dalam simulasi ini, analis mengevaluasi 2.000 listing sintetis dari 10
> area di Bekasi dengan tanggal listing selama 1 Januari--31 Desember
> 2025. Analisis dilakukan untuk mengukur distribusi jumlah listing dan
> median harga per area, hubungan luas tanah dan luas bangunan dengan
> harga, serta perbedaan harga berdasarkan kepemilikan carport. Hasil
> disajikan melalui minimal empat visualisasi, satu analisis statistik
> deskriptif, satu uji statistik inferensial, dan rekomendasi awal.

## 3.2 Arti setiap unsur SMART

  -----------------------------------------------------------------------
  Unsur                   Pertanyaan              Penerapan
  ----------------------- ----------------------- -----------------------
  Specific                Apa yang dianalisis?    Harga dan karakteristik
                                                  listing rumah di 10
                                                  area Bekasi

  Measurable              Bagaimana keberhasilan  2.000 listing, 4
                          diukur?                 grafik, 1 analisis
                                                  deskriptif, 1 uji
                                                  inferensial

  Achievable              Apakah bisa dikerjakan? Python, Pandas,
                                                  Matplotlib, Seaborn,
                                                  SciPy

  Relevant                Mengapa penting?        Membantu menentukan
                                                  prioritas investigasi
                                                  pasar

  Time-bound              Batas periode apa?      Listing 1 Januari--31
                                                  Desember 2025
  -----------------------------------------------------------------------

**Tips pemula:** SMART bukan sekadar menulis lima kata. Setiap unsur
harus terlihat jelas di problem statement.

------------------------------------------------------------------------

# 4. Identifikasi Masalah dengan 5W+1H

5W+1H digunakan untuk membedah masalah sebelum menentukan kolom dataset.

  -----------------------------------------------------------------------
  Elemen                  Pertanyaan proyek       Jawaban
  ----------------------- ----------------------- -----------------------
  What                    Apa yang ingin          Distribusi listing,
                          diketahui?              harga, dan
                                                  karakteristik rumah

  Where                   Di mana?                10 area di Kota Bekasi

  When                    Kapan?                  Periode listing tahun
                                                  2025

  Who                     Siapa pengguna hasil    Manajer pemasaran dan
                          analisis?               tim akuisisi

  Why                     Mengapa dianalisis?     Menentukan area/segmen
                                                  untuk investigasi lebih
                                                  lanjut

  How                     Bagaimana caranya?      Cleaning, statistik,
                                                  visualisasi, dan uji
                                                  beda
  -----------------------------------------------------------------------

## 4.1 Enam penjabaran masalah yang harus dijawab

Instruksi MS1 meminta minimal enam penjabaran masalah: empat untuk
visualisasi, satu untuk statistik deskriptif, dan satu untuk statistik
inferensial.

  -----------------------------------------------------------------------
  No.               Penjabaran        Metode            Output
                    masalah                             
  ----------------- ----------------- ----------------- -----------------
  1                 Area mana yang    Bar plot          Jumlah listing
                    mempunyai listing                   per area
                    terbanyak?                          

  2                 Bagaimana         Bar plot          Median harga per
                    perbandingan                        area
                    median harga                        
                    antararea?                          

  3                 Apakah luas tanah Scatter plot +    Pola land area vs
                    berhubungan       korelasi          price
                    dengan harga?                       

  4                 Apakah luas       Scatter plot +    Pola building
                    bangunan          korelasi          area vs price
                    berhubungan                         
                    dengan harga?                       

  5                 Bagaimana pusat   Statistik         Mean, median,
                    data, penyebaran, deskriptif        std, IQR, outlier
                    dan outlier                         
                    harga?                              

  6                 Apakah rata-rata  Welch independent t-statistic,
                    harga berbeda     two-sample t-test p-value,
                    antara rumah                        confidence
                    dengan dan tanpa                    interval
                    carport?                            
  -----------------------------------------------------------------------

Empat pertanyaan pertama harus memiliki grafik dan insight
masing-masing. Pertanyaan kelima dan keenam harus memiliki perhitungan
serta interpretasi statistik.

------------------------------------------------------------------------

# 5. Memahami Dataset dan Data Dictionary

## 5.1 File dataset

Nama file: `P0M1_nawafi_suren_dataset.csv`

Karakteristik dataset:

-   2.008 baris data mentah.
-   2.000 ID listing unik sebelum proses pembersihan.
-   10 area di Kota Bekasi.
-   Satu baris mewakili satu listing rumah.
-   Periode tanggal listing tahun 2025.
-   Data dibuat secara sintetis.

Delapan baris duplikat sengaja ditambahkan. Beberapa nilai kosong dan
nilai tidak valid juga disisipkan agar kita bisa berlatih data cleaning.

## 5.2 Penjelasan kolom

  -----------------------------------------------------------------------
  Kolom                   Arti                    Contoh penggunaan
  ----------------------- ----------------------- -----------------------
  `listing_id`            ID unik listing         Menghitung jumlah
                                                  listing unik

  `area`                  Nama area               Membandingkan pasar
                                                  antararea

  `city`                  Kota                    Mengetahui wilayah

  `land_area_m2`          Luas tanah dalam m²     Analisis ukuran tanah

  `building_area_m2`      Luas bangunan dalam m²  Analisis ukuran
                                                  bangunan

  `bedrooms`              Jumlah kamar tidur      Karakteristik rumah

  `bathrooms`             Jumlah kamar mandi      Karakteristik rumah

  `carport`               Jumlah kapasitas        Membuat kelompok
                          carport                 dengan/tanpa carport

  `renovated`             1 jika direnovasi, 0    Karakteristik renovasi
                          jika tidak              

  `property_age_years`    Usia properti dalam     Karakteristik usia
                          tahun                   

  `price_idr`             Harga simulasi dalam    Analisis harga
                          rupiah                  

  `latitude`              Koordinat lintang       Potensi analisis peta
                          sintetis                

  `longitude`             Koordinat bujur         Potensi analisis peta
                          sintetis                

  `listing_date`          Tanggal listing         Analisis waktu
  -----------------------------------------------------------------------

## 5.3 Memahami unit observasi

Unit observasi adalah satu objek yang direpresentasikan oleh satu baris.

Dalam dataset ini:

-   Satu baris = satu listing rumah.
-   Satu `listing_id` seharusnya hanya muncul satu kali.
-   Harga merupakan harga penawaran simulasi, bukan harga transaksi.

Jangan menganggap 2.008 baris berarti 2.008 properti unik. Ada duplikasi
yang harus diperiksa.

------------------------------------------------------------------------

# 6. Persiapan Environment dan Folder

## 6.1 Struktur folder

Ekstrak file ZIP sehingga struktur folder menjadi:

``` text
MS1_Property_Market_Opportunity_Analysis/
├── P0M1_nawafi_suren.ipynb
├── P0M1_nawafi_suren_dataset.csv
├── README.md
├── TABLEAU_GUIDE.md
└── DATA_DICTIONARY.md
```

Notebook dan CSV harus berada dalam folder yang sama karena kode membaca
file dengan nama relatif.

## 6.2 Mengaktifkan environment

Jika menggunakan environment Conda yang sebelumnya dipakai:

``` bash
conda activate h8_env
```

Install library yang dibutuhkan:

``` bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

Arti library:

  Library      Fungsi
  ------------ -------------------------------
  Pandas       Membaca dan mengolah tabel
  NumPy        Operasi numerik
  Matplotlib   Membuat grafik
  Seaborn      Membuat visualisasi statistik
  SciPy        Uji statistik
  Jupyter      Menjalankan notebook

Masuk ke folder proyek, lalu jalankan:

``` bash
cd ~/Downloads/H8/MS1_Property_Market_Opportunity_Analysis
jupyter notebook
```

Buka `P0M1_nawafi_suren.ipynb`.

**Run All** berarti menjalankan seluruh cell dari atas ke bawah.
Jalankan secara berurutan karena cell berikutnya menggunakan variabel
yang dibuat oleh cell sebelumnya.

------------------------------------------------------------------------

# 7. Step 1 -- Introduction

Bagian pertama notebook berisi identitas dan pengenalan proyek.

Contoh:

``` markdown
# P0M1 – Property Market Opportunity Analysis

Student: Nawafi Suren
Program: Hacktiv8 Data Science Fulltime Program
Milestone: 1 – Learning Simulation
Dashboard URL: Belum dipublikasikan
```

## Mengapa Introduction diperlukan?

Agar pembaca mengetahui: - Siapa yang mengerjakan proyek. - Program atau
milestone yang dikerjakan. - Apa judul proyek. - Di mana dashboard dapat
dilihat setelah dipublikasikan.

Pada instruksi MS1, URL dashboard harus dicantumkan di bagian paling
atas notebook. Setelah Tableau Public dipublikasikan, ganti teks
sementara dengan URL dashboard yang sebenarnya.

------------------------------------------------------------------------

# 8. Step 2 -- Problem Statement and Dataset Description

Bagian ini menjelaskan alasan proyek dilakukan, masalah yang hendak
dijawab, serta dataset yang digunakan.

## 8.1 Tulis topik dan latar belakang

Topik:

**Property Market Opportunity Analysis**

Latar belakang sederhana:

Perusahaan properti membutuhkan gambaran tentang distribusi listing,
harga penawaran, dan karakteristik rumah di beberapa area. Analisis data
listing dapat membantu perusahaan menentukan area atau segmen yang perlu
diteliti lebih lanjut.

## 8.2 Tulis SMART

Gunakan lima unsur SMART pada Bab 3. Jangan hanya menyalin definisi
SMART; jelaskan penerapannya pada proyek.

## 8.3 Tulis 5W+1H

Gunakan tabel pada Bab 4. Pastikan pertanyaan yang dibuat dapat dijawab
oleh kolom yang tersedia.

## 8.4 Tulis enam penjabaran masalah

Pastikan jumlah dan jenisnya sesuai:

-   Empat pertanyaan visualisasi.
-   Satu pertanyaan statistik deskriptif.
-   Satu pertanyaan statistik inferensial.

## 8.5 Jelaskan dataset dan sumbernya

Untuk paket latihan ini, tuliskan secara jujur bahwa dataset dibuat
secara sintetis menggunakan NumPy. Jangan mencantumkan sumber eksternal
palsu.

Pada submission resmi, sumber dataset nyata harus disertakan, misalnya
URL Kaggle, BigQuery, BPS, atau sumber lain yang dapat
dipertanggungjawabkan. Ikuti instruksi konsultasi dataset kepada buddy.

------------------------------------------------------------------------

# 9. Step 3 -- Data Loading

Data Loading berarti membaca dataset ke dalam Python agar dapat
diperiksa dan dianalisis.

## 9.1 Import library

``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats
from pathlib import Path
```

Penjelasan:

-   `import pandas as pd`: memanggil Pandas dengan alias `pd`.
-   `import numpy as np`: memanggil NumPy dengan alias `np`.
-   `import matplotlib.pyplot as plt`: memanggil fungsi plotting
    Matplotlib.
-   `import seaborn as sns`: memanggil Seaborn.
-   `from scipy import stats`: mengambil modul statistik SciPy.
-   `from pathlib import Path`: membantu mengelola path file.

Alias hanya nama pendek agar kode lebih praktis. `pd` tetap berarti
Pandas, sedangkan `sns` berarti Seaborn.

## 9.2 Atur tampilan Pandas

``` python
pd.set_option("display.max_columns", None)
pd.set_option("display.float_format", lambda x: f"{x:,.2f}")
sns.set_theme(style="whitegrid")
```

Penjelasan:

-   `display.max_columns`: menampilkan semua kolom DataFrame.
-   `display.float_format`: menampilkan angka desimal dengan dua digit
    dan pemisah ribuan.
-   `sns.set_theme`: mengatur tampilan grafik Seaborn.

## 9.3 Tentukan lokasi dataset

``` python
DATA_PATH = Path("P0M1_nawafi_suren_dataset.csv")
```

`Path()` membentuk alamat file. Karena CSV berada di folder yang sama
dengan notebook, cukup tulis nama file.

## 9.4 Baca CSV

``` python
df_raw = pd.read_csv(
    DATA_PATH,
    parse_dates=["listing_date"]
)
```

Penjelasan:

-   `pd.read_csv()` membaca file CSV.
-   Hasilnya disimpan ke variabel `df_raw`.
-   `parse_dates` meminta Pandas membaca `listing_date` sebagai tanggal,
    bukan sekadar teks.

Mengapa nama variabel `df_raw`? `raw` berarti data mentah. Kita
menyimpan data awal agar dapat membandingkannya dengan data setelah
cleaning.

## 9.5 Lihat lima baris pertama

``` python
display(df_raw.head())
```

`head()` secara default menampilkan lima baris pertama.

Tujuannya: - Melihat nama kolom. - Melihat contoh isi data. - Memastikan
file berhasil dibaca. - Memeriksa format data secara visual.

## 9.6 Periksa ukuran data

``` python
print(df_raw.shape)
```

`shape` menghasilkan tuple:

``` text
(jumlah_baris, jumlah_kolom)
```

Pada dataset ini:

``` text
(2008, 14)
```

Artinya 2.008 baris dan 14 kolom.

## 9.7 Periksa tipe data

``` python
df_raw.info()
```

Informasi yang ditampilkan: - Jumlah baris. - Nama kolom. - Jumlah nilai
non-null. - Tipe data. - Penggunaan memori.

Contoh tipe data: - `int64`: bilangan bulat. - `float64`: bilangan
desimal. - `object`: biasanya teks. - `datetime64`: tanggal/waktu.

## 9.8 Statistik awal

``` python
df_raw.describe(include="all").T
```

`describe()` memberikan ringkasan statistik. `.T` melakukan transpose
agar kolom statistik tampil sebagai baris.

Untuk kolom numerik biasanya muncul: - `count`: jumlah data yang
tersedia. - `mean`: rata-rata. - `std`: standar deviasi. - `min`:
minimum. - `25%`: kuartil pertama. - `50%`: median. - `75%`: kuartil
ketiga. - `max`: maksimum.

## 9.9 Periksa missing values

``` python
df_raw.isna().sum()
```

Urutan proses:

1.  `isna()` memeriksa apakah nilai kosong.
2.  Hasilnya berupa True atau False.
3.  `sum()` menghitung jumlah True pada setiap kolom.

## 9.10 Periksa duplikasi

``` python
df_raw.duplicated().sum()
```

`duplicated()` memeriksa baris yang sama persis dengan baris sebelumnya.
`sum()` menghitung jumlah duplikasi.

Untuk memeriksa ID listing berulang:

``` python
df_raw["listing_id"].duplicated().sum()
```

Keduanya tidak selalu sama. Dua baris bisa mempunyai ID sama tetapi isi
kolom lain berbeda.

------------------------------------------------------------------------

# 10. Step 4 -- Data Cleaning

Data Cleaning adalah proses memperbaiki atau menyiapkan data agar layak
dianalisis.

Contoh masalah: - Data duplikat. - Nilai kosong. - Tipe data tidak
tepat. - Harga negatif. - Luas tanah nol atau negatif. - Nilai yang
tidak masuk akal menurut konteks.

## 10.1 Salin data mentah

``` python
df = df_raw.copy()
```

Mengapa menggunakan `.copy()`?

Agar proses cleaning dilakukan pada DataFrame baru bernama `df`. Data
asli `df_raw` tetap disimpan sebagai pembanding.

## 10.2 Hapus duplikasi berdasarkan ID

``` python
df = df.drop_duplicates(
    subset="listing_id",
    keep="first"
).copy()
```

Penjelasan:

-   `drop_duplicates()` menghapus duplikasi.
-   `subset="listing_id"` berarti pemeriksaan dilakukan berdasarkan ID.
-   `keep="first"` mempertahankan kemunculan pertama.
-   `.copy()` membuat salinan hasil.

Mengapa berdasarkan ID? Karena asumsi proyek adalah satu ID
merepresentasikan satu listing. Jika data nyata memiliki aturan ID
berbeda, strategi ini harus ditinjau kembali.

## 10.3 Pastikan tipe data numerik

``` python
numeric_cols = [
    "land_area_m2",
    "building_area_m2",
    "bedrooms",
    "bathrooms",
    "carport",
    "renovated",
    "property_age_years",
    "price_idr",
    "latitude",
    "longitude"
]

for col in numeric_cols:
    df[col] = pd.to_numeric(
        df[col],
        errors="coerce"
    )
```

Penjelasan:

-   `numeric_cols` adalah daftar nama kolom numerik.
-   `for col in numeric_cols` mengulang proses untuk setiap nama kolom.
-   `pd.to_numeric()` mengubah nilai menjadi numerik.
-   `errors="coerce"` mengubah nilai yang gagal dikonversi menjadi
    `NaN`.

`NaN` berarti nilai tidak tersedia atau tidak dapat dibaca sebagai
angka.

## 10.4 Hapus nilai tidak valid

Kode pada notebook:

``` python
df = df[
    (df["price_idr"] > 0) &
    (df["land_area_m2"] > 0) &
    (df["building_area_m2"] > 0) &
    (df["bedrooms"] >= 0) &
    (df["bathrooms"] >= 0) &
    (df["carport"] >= 0)
].copy()
```

Penjelasan operator:

  Operator          Arti
  ----------------- -------------------------------------
  `>`               Lebih besar dari
  `>=`              Lebih besar dari atau sama dengan
  `&`               AND, semua syarat harus benar
  `df[condition]`   Memilih baris yang memenuhi kondisi

Contoh:

``` python
df[df["price_idr"] > 0]
```

Artinya hanya ambil baris dengan harga lebih besar dari nol.

**Catatan penting:** perbandingan terhadap `NaN` tidak menghasilkan
True. Oleh karena itu, kode di atas juga membuang baris yang memiliki
nilai kosong pada kolom yang diuji sebelum proses imputasi berikutnya.

## 10.5 Isi missing values dengan median

Kode pada notebook:

``` python
for col in numeric_cols:
    if df[col].isna().any():
        df[col] = df[col].fillna(
            df[col].median()
        )
```

Penjelasan:

-   `df[col]` memilih satu kolom.
-   `.isna()` mendeteksi nilai kosong.
-   `.any()` memeriksa apakah ada setidaknya satu nilai kosong.
-   `.median()` menghitung median kolom.
-   `.fillna()` mengisi nilai kosong dengan median.

Mengapa median? Median relatif tahan terhadap pengaruh nilai ekstrem
dibanding mean.

**Catatan audit notebook:** karena filter invalid values pada langkah
10.4 dilakukan sebelum imputasi, missing values pada `land_area_m2`,
`building_area_m2`, `bedrooms`, dan `bathrooms` sudah ikut terbuang pada
dataset latihan ini. Akibatnya, loop imputasi tidak mengisi 48 sel
kosong tersebut karena barisnya sudah tidak ada. Ini penting dipahami
agar tidak mengira imputasi benar-benar terjadi.

Jika tujuanmu adalah mempertahankan baris yang memiliki nilai kosong,
strategi yang lebih masuk akal adalah: 1. Hapus duplikasi. 2. Ubah tipe
data. 3. Periksa nilai invalid yang benar-benar tidak masuk akal. 4.
Putuskan apakah nilai kosong akan diimputasi atau baris dihapus. 5. Baru
jalankan filter validasi akhir.

Jangan mengubah urutan secara membabi buta. Tentukan strategi
berdasarkan arti kolom dan kebutuhan analisis.

## 10.6 Validasi luas bangunan

``` python
df = df[
    df["building_area_m2"] <= df["land_area_m2"]
].copy()
```

Dalam simulasi ini, luas bangunan tidak boleh melebihi luas tanah. Baris
yang tidak memenuhi asumsi tersebut dihapus.

Pada dunia nyata, aturan ini harus diperiksa dengan definisi luas yang
digunakan. Bangunan bertingkat, misalnya, dapat memiliki total luas
lantai lebih besar daripada luas tanah. Jadi, aturan validasi harus
sesuai konteks.

## 10.7 Membuat kolom harga dalam juta rupiah

``` python
df["price_million"] = (
    df["price_idr"] / 1_000_000
)
```

Contoh:

``` text
Rp950.000.000 / 1.000.000 = 950 juta
```

Kolom baru membuat angka harga lebih mudah dibaca pada grafik.

Kolom asli `price_idr` tetap dipertahankan.

## 10.8 Membuat kelompok carport

``` python
df["carport_group"] = np.where(
    df["carport"] > 0,
    "With carport",
    "No carport"
)
```

`np.where(kondisi, nilai_jika_benar, nilai_jika_salah)` bekerja seperti
IF sederhana.

Artinya: - Jika jumlah carport lebih dari 0 → `With carport`. - Jika
tidak → `No carport`.

Contoh:

    carport carport_group
  --------- ---------------
          0 No carport
          1 With carport
          2 With carport

## 10.9 Mengambil nama bulan

``` python
df["listing_month"] = (
    df["listing_date"].dt.month_name()
)
```

-   `.dt` mengakses komponen tanggal.
-   `.month_name()` mengubah bulan menjadi nama bulan.

Kolom ini disiapkan sebagai opsi filter dashboard.

## 10.10 Periksa hasil cleaning

``` python
print("Clean shape:", df.shape)
print("Remaining missing values:", df.isna().sum().sum())
print("Remaining duplicate IDs:",
      df["listing_id"].duplicated().sum())
display(df.head())
```

Pada urutan kode notebook saat ini, hasil yang diharapkan dari dataset
paket adalah:

  Tahap                              Jumlah baris
  -------------------------------- --------------
  Data mentah                               2.008
  Setelah duplikasi ID dihapus              2.000
  Setelah filter nilai invalid              1.950
  Setelah validasi luas bangunan            1.950

Mengapa turun menjadi 1.950? Dataset sengaja berisi 8 baris duplikat, 2
baris dengan nilai invalid, dan nilai kosong pada empat kolom yang ikut
terbuang oleh filter sebelum imputasi.

## 10.11 Cleaning summary

``` python
cleaning_summary = pd.DataFrame({
    "stage": ["Raw data", "After cleaning"],
    "rows": [len(df_raw), len(df)]
})

display(cleaning_summary)
```

Tujuannya membandingkan jumlah baris sebelum dan sesudah cleaning.

**Insight yang bisa ditulis:** data mentah memiliki 2.008 baris,
sedangkan data bersih memiliki 1.950 baris. Pengurangan terjadi karena
duplikasi dan nilai yang tidak memenuhi aturan validasi pada simulasi.

------------------------------------------------------------------------

# 11. Step 5 -- Exploration and Analysis

EDA adalah singkatan dari Exploratory Data Analysis. Artinya, kita
mengeksplorasi data untuk memahami pola, distribusi, hubungan, dan
kejanggalan.

MS1 meminta analisis yang menjawab enam penjabaran masalah.

## 11.1 Visualisasi 1 -- Jumlah listing per area

### Pertanyaan bisnis

Area mana yang mempunyai jumlah listing rumah paling banyak?

### Kode

``` python
listing_by_area = (
    df.groupby("area")["listing_id"]
      .nunique()
      .sort_values(ascending=False)
)
```

### Bedah kode

**`groupby("area")`**

Mengelompokkan baris berdasarkan area. Semua listing Bekasi Timur berada
dalam satu kelompok, semua listing Jatiasih dalam kelompok lain, dan
seterusnya.

**`["listing_id"]`**

Memilih kolom ID listing yang akan dihitung.

**`.nunique()`**

Menghitung jumlah ID unik, bukan jumlah seluruh kemunculan.

**`.sort_values(ascending=False)`**

Mengurutkan jumlah listing dari terbesar ke terkecil.

### Membuat grafik

``` python
plt.figure(figsize=(12, 6))

sns.barplot(
    x=listing_by_area.values,
    y=listing_by_area.index
)

plt.title("Number of Property Listings by Area")
plt.xlabel("Number of listings")
plt.ylabel("Area")

plt.tight_layout()
plt.show()
```

Bedah fungsi: - `plt.figure(figsize=(12, 6))`: menentukan ukuran
gambar. - `sns.barplot()`: membuat grafik batang. - `x`: nilai jumlah
listing. - `y`: nama area. - `title()`: judul grafik. - `xlabel()` dan
`ylabel()`: nama sumbu. - `tight_layout()`: merapikan jarak elemen. -
`show()`: menampilkan grafik.

### Menampilkan area teratas

``` python
print(
    "Area dengan listing terbanyak:",
    listing_by_area.index[0],
    "dengan",
    listing_by_area.iloc[0],
    "listing."
)
```

-   `.index[0]` mengambil nama area pertama setelah sorting.
-   `.iloc[0]` mengambil nilai jumlah listing pertama.

### Contoh hasil dari dataset paket

Area dengan listing terbanyak adalah Bantargebang, sebanyak 223 listing
setelah cleaning.

### Contoh insight

> Bantargebang memiliki jumlah listing paling banyak pada dataset
> simulasi. Hal ini menunjukkan jumlah penawaran yang teramati lebih
> tinggi pada area tersebut. Temuan ini belum membuktikan bahwa
> permintaan pembeli di Bantargebang paling tinggi.

**Perbedaan penting:** supply/penawaran tidak sama dengan
demand/permintaan.

------------------------------------------------------------------------

## 11.2 Visualisasi 2 -- Median harga per area

### Pertanyaan bisnis

Bagaimana perbandingan median harga rumah antararea?

### Mengapa menggunakan median?

Harga properti dapat memiliki nilai sangat tinggi. Mean dapat tertarik
oleh harga ekstrem. Median adalah nilai tengah setelah harga diurutkan,
sehingga sering lebih representatif untuk menggambarkan harga tipikal.

### Kode

``` python
median_price_area = (
    df.groupby("area")["price_million"]
      .median()
      .sort_values(ascending=False)
)
```

Bedah: - `groupby("area")`: kelompokkan berdasarkan area. -
`["price_million"]`: pilih harga dalam juta rupiah. - `.median()`:
hitung median setiap kelompok. - `.sort_values(ascending=False)`:
urutkan dari median tertinggi.

### Membuat grafik

``` python
plt.figure(figsize=(12, 6))

sns.barplot(
    x=median_price_area.values,
    y=median_price_area.index
)

plt.title("Median Asking Price by Area")
plt.xlabel("Median price (million IDR)")
plt.ylabel("Area")

plt.tight_layout()
plt.show()
```

### Mengambil area tertinggi dan terendah

``` python
print(
    "Median harga tertinggi:",
    median_price_area.index[0],
    median_price_area.iloc[0]
)

print(
    "Median harga terendah:",
    median_price_area.index[-1],
    median_price_area.iloc[-1]
)
```

-   Index `0` adalah posisi pertama atau tertinggi.
-   Index `-1` adalah posisi terakhir atau terendah.

### Contoh hasil dari dataset paket

-   Median tertinggi: Pondok Gede, sekitar Rp1.105,74 juta.
-   Median terendah: Bantargebang, sekitar Rp744,33 juta.

Angka di atas berasal dari data sintetis dan akan muncul kembali ketika
notebook dijalankan.

### Contoh insight

> Median harga penawaran di Pondok Gede lebih tinggi daripada area lain
> pada dataset simulasi. Bantargebang memiliki median terendah.
> Perbedaan ini dapat menjadi dasar segmentasi harga untuk investigasi
> lebih lanjut, tetapi belum menjelaskan penyebab perbedaan harga.

------------------------------------------------------------------------

## 11.3 Visualisasi 3 -- Luas tanah vs harga

### Pertanyaan bisnis

Apakah rumah dengan luas tanah lebih besar cenderung memiliki harga
penawaran lebih tinggi?

### Mengapa menggunakan scatter plot?

Scatter plot cocok untuk membandingkan dua variabel numerik: - Sumbu X:
luas tanah. - Sumbu Y: harga rumah.

Setiap titik mewakili satu listing rumah.

### Kode

``` python
plt.figure(figsize=(9, 6))

sns.regplot(
    data=df,
    x="land_area_m2",
    y="price_million",
    scatter_kws={"alpha": 0.35},
    line_kws={"color": "red"}
)

plt.title("Land Area vs Asking Price")
plt.xlabel("Land area (m²)")
plt.ylabel("Price (million IDR)")

plt.tight_layout()
plt.show()
```

Penjelasan: - `regplot()` membuat scatter plot dan garis regresi. -
`data=df` menentukan DataFrame sumber. - `x` menentukan variabel sumbu
horizontal. - `y` menentukan variabel sumbu vertikal. - `alpha=0.35`
membuat titik transparan agar tumpang tindih lebih mudah dilihat. -
`line_kws` mengatur tampilan garis regresi.

### Menghitung korelasi

``` python
land_corr = df["land_area_m2"].corr(
    df["price_million"]
)

print(land_corr)
```

`.corr()` secara default menghitung korelasi Pearson.

Panduan membaca nilai korelasi:

    Nilai absolut korelasi Interpretasi umum
  ------------------------ -------------------
                0,00--0,19 Sangat lemah
                0,20--0,39 Lemah
                0,40--0,59 Sedang
                0,60--0,79 Kuat
                0,80--1,00 Sangat kuat

Kategori tersebut adalah pedoman praktis, bukan aturan universal.

### Contoh hasil dari dataset paket

Korelasi luas tanah dengan harga sekitar **0,855**, yaitu hubungan
positif sangat kuat pada data simulasi.

### Contoh insight

> Pada dataset simulasi terdapat hubungan positif yang kuat antara luas
> tanah dan harga penawaran. Listing dengan tanah lebih luas cenderung
> mempunyai harga lebih tinggi. Korelasi ini tidak membuktikan luas
> tanah menjadi satu-satunya penyebab harga meningkat.

------------------------------------------------------------------------

## 11.4 Visualisasi 4 -- Luas bangunan vs harga

### Pertanyaan bisnis

Apakah rumah dengan luas bangunan lebih besar cenderung memiliki harga
penawaran lebih tinggi?

### Kode

``` python
plt.figure(figsize=(9, 6))

sns.regplot(
    data=df,
    x="building_area_m2",
    y="price_million",
    scatter_kws={"alpha": 0.35},
    line_kws={"color": "red"}
)

plt.title("Building Area vs Asking Price")
plt.xlabel("Building area (m²)")
plt.ylabel("Price (million IDR)")

plt.tight_layout()
plt.show()
```

### Korelasi

``` python
building_corr = df["building_area_m2"].corr(
    df["price_million"]
)

print(building_corr)
```

### Contoh hasil dari dataset paket

Korelasi luas bangunan dengan harga sekitar **0,871**, yaitu hubungan
positif sangat kuat pada data simulasi.

### Contoh insight

> Luas bangunan menunjukkan hubungan positif kuat dengan harga
> penawaran. Pada dataset simulasi, korelasinya sedikit lebih tinggi
> dibanding korelasi luas tanah. Temuan ini hanya menggambarkan hubungan
> statistik, bukan bukti sebab-akibat.

### Membandingkan dua korelasi

``` python
print("Land area correlation:", land_corr)
print("Building area correlation:", building_corr)
```

Jangan hanya membandingkan angka lalu menyatakan variabel dengan
korelasi lebih tinggi pasti paling penting. Korelasi sederhana belum
mengontrol area, usia rumah, fasilitas, dan variabel lain.

------------------------------------------------------------------------

# 12. Statistik Deskriptif -- Memahami Distribusi Harga

## 12.1 Apa itu statistik deskriptif?

Statistik deskriptif digunakan untuk meringkas karakteristik data. Kita
tidak sedang menguji hipotesis; kita sedang menggambarkan data yang
tersedia.

Pada proyek ini, variabel yang dianalisis adalah `price_million`.

## 12.2 Mean

Mean adalah rata-rata.

Rumus:

``` text
Mean = jumlah seluruh nilai / banyaknya nilai
```

Python:

``` python
price.mean()
```

Contoh sederhana:

Harga: 500, 600, 700 juta.

Mean:

``` text
(500 + 600 + 700) / 3 = 600 juta
```

## 12.3 Median

Median adalah nilai tengah setelah data diurutkan.

``` python
price.median()
```

Contoh:

``` text
Data awal: 700, 500, 600
Setelah sorting: 500, 600, 700
Median: 600
```

Jika jumlah data genap, median adalah rata-rata dari dua nilai tengah.

Median berguna ketika data memiliki nilai ekstrem.

## 12.4 Mode

Mode adalah nilai yang paling sering muncul.

``` python
price.mode()
```

Jika terdapat lebih dari satu mode, Pandas dapat mengembalikan lebih
dari satu nilai.

Notebook mengambil mode pertama dengan:

``` python
price.mode().iloc[0]
```

## 12.5 Variance

Variance mengukur penyebaran data terhadap mean.

``` python
price.var()
```

Variance memiliki satuan kuadrat. Karena itu, standar deviasi sering
lebih mudah diinterpretasikan.

## 12.6 Standard deviation

Standard deviation adalah akar kuadrat variance.

``` python
price.std()
```

Semakin besar standar deviasi, semakin tersebar harga dari rata-ratanya.

## 12.7 Quartile

Quartile membagi data terurut menjadi empat bagian.

-   Q1: batas sekitar 25% data terbawah.
-   Q2: median atau 50%.
-   Q3: batas sekitar 75% data terbawah.

Python:

``` python
q1 = price.quantile(0.25)
q2 = price.quantile(0.50)
q3 = price.quantile(0.75)
```

## 12.8 IQR

IQR adalah jarak antara Q3 dan Q1.

``` python
iqr = q3 - q1
```

Rumus:

``` text
IQR = Q3 - Q1
```

IQR menggambarkan rentang 50% data bagian tengah.

## 12.9 Outlier dengan aturan 1,5 × IQR

Batas bawah:

``` text
Lower Bound = Q1 - 1.5 × IQR
```

Batas atas:

``` text
Upper Bound = Q3 + 1.5 × IQR
```

Kode:

``` python
lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr
```

Data di luar batas tersebut ditandai sebagai calon outlier.

``` python
outliers = df[
    (price < lower_bound) |
    (price > upper_bound)
]
```

Operator `|` berarti OR. Baris dipilih jika harga berada di bawah batas
bawah ATAU di atas batas atas.

**Ingat:** outlier tidak otomatis berarti kesalahan data. Rumah premium
dapat memiliki harga yang jauh lebih tinggi dan tetap valid.

## 12.10 Kode ringkasan statistik notebook

``` python
price = df["price_million"]

desc = price.describe()

q1 = price.quantile(0.25)
q3 = price.quantile(0.75)
iqr = q3 - q1

lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr

outliers = df[
    (price < lower_bound) |
    (price > upper_bound)
]
```

Kemudian notebook menyusun ringkasan:

``` python
descriptive_summary = pd.DataFrame({
    "metric": [
        "Mean", "Median", "Mode", "Variance",
        "Std deviation", "Minimum", "Q1", "Q3",
        "Maximum", "IQR", "Lower outlier bound",
        "Upper outlier bound", "Outlier count"
    ],
    "value": [
        price.mean(),
        price.median(),
        price.mode().iloc[0],
        price.var(),
        price.std(),
        price.min(),
        q1,
        q3,
        price.max(),
        iqr,
        lower_bound,
        upper_bound,
        len(outliers)
    ]
})

display(descriptive_summary)
```

## 12.11 Contoh hasil dataset paket

Angka berikut merupakan hasil menjalankan logika notebook pada dataset
yang disertakan:

  Statistik                           Hasil
  ----------------------- -----------------
  Jumlah listing bersih               1.950
  Mean harga                  Rp960,00 juta
  Median harga                Rp936,71 juta
  Standard deviation          Rp310,42 juta
  Minimum                        Rp250 juta
  Q1                          Rp730,23 juta
  Q3                        Rp1.164,21 juta
  Maximum                   Rp2.108,57 juta
  IQR                         Rp433,99 juta
  Jumlah calon outlier                   14

Angka dapat ditampilkan dengan pembulatan berbeda tergantung format
notebook.

### Contoh interpretasi

> Mean harga lebih tinggi daripada median, yang mengindikasikan
> distribusi harga dapat memiliki ekor ke arah nilai tinggi. Terdapat 14
> observasi di luar batas 1,5 × IQR. Observasi tersebut perlu diperiksa
> lebih lanjut, bukan langsung dihapus.

------------------------------------------------------------------------

# 13. Statistik Inferensial -- Welch Independent Two-Sample T-Test

## 13.1 Apa itu statistik inferensial?

Statistik inferensial menggunakan data sampel untuk melakukan pengujian
atau membuat perkiraan mengenai perbedaan/hubungan pada populasi.

Pada proyek simulasi, kita menguji apakah rata-rata harga listing rumah
dengan carport berbeda dari rumah tanpa carport.

Karena dataset ini sintetis dan bukan sampel acak pasar sebenarnya,
kesimpulan inferensial hanya untuk latihan metode.

## 13.2 Mengapa membandingkan carport?

Kolom `carport` menunjukkan jumlah kapasitas carport.

Kita membentuk dua kelompok:

-   Rumah tanpa carport: `carport == 0`.
-   Rumah dengan carport: `carport > 0`.

Pertanyaan statistik:

> Apakah rata-rata harga penawaran kedua kelompok berbeda?

## 13.3 Hipotesis

**H0 (null hypothesis):**

Rata-rata harga kelompok dengan carport sama dengan kelompok tanpa
carport.

``` text
H0: mean_with = mean_without
```

**H1 (alternative hypothesis):**

Rata-rata harga kedua kelompok berbeda.

``` text
H1: mean_with != mean_without
```

Karena pertanyaan menggunakan kata "berbeda", pengujian dilakukan dua
arah (`two-sided`).

## 13.4 Alpha 0,05

Alpha adalah batas signifikansi yang ditentukan sebelum pengujian.

``` python
alpha = 0.05
```

Aturan keputusan berdasarkan p-value:

-   Jika p-value \< 0,05: tolak H0.
-   Jika p-value \>= 0,05: gagal menolak H0.

Jangan menulis "H0 terbukti benar". Gagal menolak H0 berarti bukti yang
tersedia belum cukup untuk menyatakan terdapat perbedaan.

## 13.5 Memisahkan kelompok

``` python
with_carport = df.loc[
    df["carport"] > 0,
    "price_million"
]

without_carport = df.loc[
    df["carport"] == 0,
    "price_million"
]
```

Penjelasan:

-   `.loc[baris, kolom]` memilih baris dan kolom berdasarkan
    label/kondisi.
-   Kondisi pertama mengambil rumah dengan carport.
-   Kondisi kedua mengambil rumah tanpa carport.
-   Kolom yang diambil adalah harga dalam juta rupiah.

## 13.6 Menghitung uji t

``` python
t_stat, p_value = stats.ttest_ind(
    with_carport,
    without_carport,
    equal_var=False,
    alternative="two-sided"
)
```

Penjelasan parameter:

-   `with_carport`: sampel kelompok pertama.
-   `without_carport`: sampel kelompok kedua.
-   `equal_var=False`: menggunakan Welch t-test, sehingga tidak
    mengasumsikan varians kedua kelompok sama.
-   `alternative="two-sided"`: menguji apakah kedua rata-rata berbeda ke
    arah mana pun.

Hasil: - `t_stat`: nilai statistik t. - `p_value`: nilai p untuk
pengujian hipotesis.

## 13.7 Hitung mean dan selisih mean

``` python
mean_with = with_carport.mean()
mean_without = without_carport.mean()

difference = mean_with - mean_without
```

Interpretasi: - Jika selisih positif, mean kelompok dengan carport lebih
tinggi. - Jika selisih negatif, mean kelompok dengan carport lebih
rendah. - Jika mendekati nol, kedua mean berdekatan.

Selisih mean bukan bukti bahwa carport menyebabkan perubahan harga.

## 13.8 Confidence interval 95%

Confidence interval memberikan rentang estimasi selisih mean.

Notebook menghitung standard error dan derajat kebebasan Welch:

``` python
n1 = len(with_carport)
n0 = len(without_carport)

v1 = with_carport.var(ddof=1)
v0 = without_carport.var(ddof=1)

se = np.sqrt(v1/n1 + v0/n0)

df_welch = (v1/n1 + v0/n0)**2 / (
    (v1/n1)**2/(n1-1) +
    (v0/n0)**2/(n0-1)
)
```

Kemudian mencari nilai kritis distribusi t:

``` python
critical_t = stats.t.ppf(
    0.975,
    df_welch
)
```

Mengapa 0,975? Untuk confidence interval dua arah 95%, sisa probabilitas
5% dibagi dua: 2,5% di setiap ekor. Batas kuantil atas adalah 97,5%.

Menghitung batas interval:

``` python
ci_low = difference - critical_t * se
ci_high = difference + critical_t * se
```

Confidence interval:

``` text
Selisih mean ± nilai kritis × standard error
```

## 13.9 Contoh hasil dataset paket

  Ukuran                                             Hasil simulasi
  ----------------------------- -----------------------------------
  Jumlah rumah dengan carport                                 1.518
  Jumlah rumah tanpa carport                                    432
  Mean dengan carport                                 Rp960,64 juta
  Mean tanpa carport                                  Rp957,76 juta
  Selisih mean                                          Rp2,89 juta
  95% CI selisih                  -Rp30,52 juta sampai Rp36,29 juta
  t-statistic                                                 0,170
  p-value                                                    0,8654

## 13.10 Interpretasi hasil

Karena p-value sekitar 0,8654 lebih besar daripada alpha 0,05, keputusan
pengujian adalah **gagal menolak H0**.

Confidence interval selisih mean juga mencakup angka nol.

Contoh penulisan:

> Pada dataset simulasi, belum terdapat bukti statistik yang cukup untuk
> menyatakan rata-rata harga penawaran rumah dengan carport berbeda dari
> rumah tanpa carport pada taraf signifikansi 5%. Selisih rata-rata
> kedua kelompok relatif kecil dan confidence interval 95% mencakup nol.
> Hasil ini tidak membuktikan bahwa carport tidak mempunyai nilai pasar;
> analisis ini hanya membandingkan dua kelompok tanpa mengontrol faktor
> lain seperti lokasi dan luas rumah.

## 13.11 Mengapa tidak boleh menyimpulkan sebab-akibat?

Rumah dengan carport mungkin juga memiliki: - Luas tanah lebih besar. -
Luas bangunan lebih besar. - Lokasi berbeda. - Usia bangunan berbeda.

Welch t-test pada notebook hanya membandingkan rata-rata dua kelompok.
Uji tersebut tidak mengisolasi pengaruh carport dari faktor lainnya.

------------------------------------------------------------------------

# 14. Menyiapkan Data untuk Tableau

Notebook menghasilkan dua file tambahan setelah cell ekspor dijalankan.

## 14.1 Membuat ringkasan per area

``` python
area_summary = (
    df.groupby("area")
      .agg(
          listing_count=("listing_id", "nunique"),
          median_price_million=("price_million", "median"),
          mean_price_million=("price_million", "mean"),
          median_land_area=("land_area_m2", "median"),
          median_building_area=("building_area_m2", "median"),
          carport_share=(
              "carport",
              lambda s: (s > 0).mean()
          )
      )
      .reset_index()
)
```

Bedah: - `groupby("area")`: kelompokkan per area. - `.agg()`: hitung
beberapa ringkasan sekaligus. - `listing_count`: jumlah ID unik. -
`median_price_million`: median harga. - `mean_price_million`: mean
harga. - `median_land_area`: median luas tanah. -
`median_building_area`: median luas bangunan. - `carport_share`:
proporsi listing dengan carport. - `reset_index()`: mengembalikan `area`
menjadi kolom biasa.

## 14.2 Ekspor ringkasan

``` python
area_summary.to_csv(
    "tableau_area_summary.csv",
    index=False
)
```

`index=False` berarti indeks DataFrame tidak ikut ditulis sebagai kolom
tambahan.

## 14.3 Ekspor data bersih

``` python
df.to_csv(
    "tableau_clean_listings.csv",
    index=False
)
```

File ini berisi data bersih pada tingkat listing, sehingga cocok untuk
scatter plot dan filter dashboard.

## 14.4 Dua file output Tableau

  ------------------------------------------------------------------------------
  File                           Isi                     Kegunaan
  ------------------------------ ----------------------- -----------------------
  `tableau_area_summary.csv`     Ringkasan per area      Ringkasan KPI atau
                                                         perbandingan area

  `tableau_clean_listings.csv`   Data bersih per listing Empat visualisasi dan
                                                         filter
  ------------------------------------------------------------------------------

------------------------------------------------------------------------

# 15. Membuat Dashboard Tableau Public

Instruksi MS1 membagi dashboard menjadi dua bagian.

## 15.1 Bagian A -- Data Visualization

Minimal empat grafik yang sama dengan notebook:

1.  Listing Count by Area.
2.  Median Asking Price by Area.
3.  Land Area vs Asking Price.
4.  Building Area vs Asking Price.

Pada dashboard, insight masing-masing grafik tidak wajib ditulis. Namun
insight tetap wajib ada pada notebook Python.

## 15.2 Membuka data di Tableau

1.  Buka Tableau Public.
2.  Pilih koneksi **Text File**.
3.  Pilih `tableau_clean_listings.csv`.
4.  Pastikan kolom terbaca dengan tipe data yang sesuai.
5.  Masuk ke worksheet untuk mulai membuat grafik.

## 15.3 Worksheet 1 -- Listing Count by Area

-   Drag `area` ke Rows.
-   Drag `listing_id` ke Columns.
-   Ubah agregasi menjadi **Count (Distinct)** jika diperlukan.
-   Pilih Marks: Bar.
-   Urutkan dari jumlah terbesar ke terkecil.

Tujuan: membandingkan jumlah listing unik setiap area.

## 15.4 Worksheet 2 -- Median Price by Area

-   Drag `area` ke Rows.
-   Drag `price_million` ke Columns.
-   Ubah agregasi menjadi **Median**.
-   Marks: Bar.
-   Urutkan berdasarkan median harga.

Tujuan: membandingkan harga tipikal antararea.

## 15.5 Worksheet 3 -- Land Area vs Price

-   Drag `land_area_m2` ke Columns.
-   Drag `price_million` ke Rows.
-   Marks: Circle.
-   Drag `listing_id` ke Detail agar satu titik mewakili satu listing.
-   Tambahkan trend line bila diperlukan.

Tujuan: melihat pola hubungan luas tanah dengan harga.

## 15.6 Worksheet 4 -- Building Area vs Price

-   Drag `building_area_m2` ke Columns.
-   Drag `price_million` ke Rows.
-   Marks: Circle.
-   Drag `listing_id` ke Detail.
-   Tambahkan trend line bila diperlukan.

Tujuan: melihat pola hubungan luas bangunan dengan harga.

## 15.7 Interaktivitas dashboard

Instruksi MS1 mewajibkan minimal dua interaktivitas.

Rekomendasi:

**Filter 1: Area** - Tampilkan filter `area`. - Pilih multiple values
dropdown. - Pengguna dapat memilih satu atau beberapa area.

**Filter 2: Carport Group** - Tampilkan filter `carport_group`. -
Pengguna dapat memilih `With carport` atau `No carport`.

Jika tersedia, atur **Apply to Worksheets → All Using This Data Source**
agar filter memengaruhi seluruh worksheet yang menggunakan sumber data
yang sama.

Alternatif filter kedua pada panduan paket adalah `listing_month`.

## 15.8 Bagian B -- Statistical Analysis

Bagian ini harus berisi hasil statistik yang sama dengan notebook.

### Statistik deskriptif

Tampilkan: - Mean. - Median. - Standard deviation. - IQR. - Jumlah
outlier.

Tuliskan penjelasan singkat mengenai pusat data, penyebaran, dan
outlier.

### Statistik inferensial

Tampilkan: - Jumlah sampel dengan carport. - Jumlah sampel tanpa
carport. - Mean kedua kelompok. - Selisih mean. - Confidence interval
95%. - t-statistic. - p-value. - Keputusan H0.

Tambahkan interpretasi dan kesimpulan.

## 15.9 Menyusun dashboard

Contoh tata letak:

``` text
+-------------------------------------------------------+
| PROPERTY MARKET OPPORTUNITY ANALYSIS                  |
| KPI: Total Listings | Median Price | Median Land Area |
+-------------------------------------------------------+
| Filter Area              | Filter Carport Group      |
+--------------------------+----------------------------+
| Listing Count by Area    | Median Price by Area       |
+--------------------------+----------------------------+
| Land Area vs Price       | Building Area vs Price     |
+-------------------------------------------------------+
| STATISTICAL ANALYSIS                                 |
| Descriptive Statistics | Inferential Statistics     |
| Interpretation and Conclusion                       |
+-------------------------------------------------------+
```

## 15.10 Publish dashboard

1.  Simpan pekerjaan ke Tableau Public.
2.  Login ke akun Tableau Public.
3.  Publish workbook.
4.  Buka halaman dashboard yang sudah dipublikasikan.
5.  Salin URL publik.
6.  Masukkan URL ke bagian paling atas notebook.

**Penting:** notebook tidak otomatis mempublikasikan dashboard. Proses
publish dilakukan melalui Tableau Public.

------------------------------------------------------------------------

# 16. Step 6 -- Conclusion

Conclusion bukan sekadar mengulang grafik. Bagian ini harus menjawab
problem statement dan memberikan rekomendasi yang didukung hasil
analisis.

## 16.1 Struktur kesimpulan

Gunakan enam bagian:

1.  **Market distribution:** area dengan listing terbanyak.
2.  **Price comparison:** area dengan median harga tertinggi dan
    terendah.
3.  **Property size:** arah dan kekuatan korelasi luas tanah/bangunan
    dengan harga.
4.  **Descriptive statistics:** mean, median, penyebaran, dan jumlah
    outlier.
5.  **Inferential statistics:** hasil Welch t-test, p-value, confidence
    interval, dan keputusan H0.
6.  **Recommendation:** area atau segmen yang perlu diteliti lebih
    lanjut.

## 16.2 Contoh kesimpulan berdasarkan data dummy

> Berdasarkan dataset simulasi yang telah dibersihkan, terdapat 1.950
> listing yang dianalisis. Bantargebang memiliki jumlah listing
> terbanyak, sedangkan Pondok Gede memiliki median harga penawaran
> tertinggi. Luas tanah dan luas bangunan sama-sama menunjukkan korelasi
> positif kuat dengan harga penawaran. Statistik deskriptif menunjukkan
> mean harga sekitar Rp960 juta dan median sekitar Rp936,71 juta, dengan
> 14 calon outlier berdasarkan metode IQR.
>
> Hasil Welch t-test menunjukkan p-value sekitar 0,8654, lebih besar
> dari 0,05. Dengan demikian, belum terdapat bukti statistik yang cukup
> untuk menyatakan rata-rata harga kedua kelompok carport berbeda pada
> dataset simulasi.
>
> Untuk tindak lanjut, perusahaan dapat meneliti lebih jauh area dengan
> jumlah listing tinggi dan area dengan median harga tinggi. Analisis
> lanjutan perlu menggunakan data transaksi, data permintaan, waktu
> listing hingga terjual, serta faktor lokasi dan karakteristik rumah
> yang lebih lengkap.

## 16.3 Batasan kesimpulan

Selalu cantumkan batasan berikut:

-   Dataset adalah dummy/sintetis.
-   Harga adalah harga penawaran simulasi, bukan harga transaksi.
-   Jumlah listing bukan ukuran langsung demand.
-   Korelasi tidak membuktikan sebab-akibat.
-   Hasil uji statistik tidak dapat digeneralisasi ke seluruh pasar
    properti Bekasi.
-   Perbedaan harga dapat dipengaruhi variabel lain yang belum
    dikontrol.

------------------------------------------------------------------------

# 17. Checklist Pengerjaan MS1

## Python Notebook

-   [ ] Introduction berisi identitas student.
-   [ ] Dashboard URL dicantumkan di bagian paling atas.
-   [ ] Problem statement menggunakan SMART.
-   [ ] Latar belakang dijelaskan.
-   [ ] Dataset dan sumber dataset dijelaskan.
-   [ ] Identifikasi masalah menggunakan metode seperti 5W+1H.
-   [ ] Minimal enam penjabaran masalah tersedia.
-   [ ] Data loading dan eksplorasi awal dilakukan.
-   [ ] Data cleaning dijelaskan.
-   [ ] Empat visualisasi dibuat.
-   [ ] Setiap visualisasi memiliki insight.
-   [ ] Statistik deskriptif dihitung dan dijelaskan.
-   [ ] Statistik inferensial dihitung dan dijelaskan.
-   [ ] Conclusion menjawab problem statement.

## Tableau Public

-   [ ] Minimal empat visualisasi.
-   [ ] Minimal dua interaktivitas.
-   [ ] Bagian Statistical Analysis tersedia.
-   [ ] Statistik deskriptif sama dengan notebook.
-   [ ] Statistik inferensial sama dengan notebook.
-   [ ] Analisis dan kesimpulan ditulis.
-   [ ] Dashboard dipublikasikan.
-   [ ] URL dashboard dimasukkan ke notebook.

## Dataset dan dokumentasi

-   [ ] Dataset disertakan.
-   [ ] Sumber data dapat dipertanggungjawabkan.
-   [ ] Dataset dikonsultasikan kepada buddy.
-   [ ] `description.md`/README menjelaskan tujuan dan cara menjalankan
    proyek.
-   [ ] Nama notebook dan dataset mengikuti ketentuan MS1.

------------------------------------------------------------------------

# 18. Kesalahan Pemula yang Perlu Dihindari

## 18.1 Langsung membuat grafik tanpa problem statement

Salah: langsung membuat grafik harga karena kolom harga tersedia.

Benar: tentukan dulu pertanyaan bisnis, misalnya "Bagaimana perbandingan
median harga antararea?"

## 18.2 Menggunakan mean untuk semua perbandingan

Mean dapat dipengaruhi nilai ekstrem. Untuk harga properti, median
sering lebih representatif sebagai harga tipikal.

## 18.3 Menganggap korelasi sebagai sebab-akibat

Korelasi positif tidak membuktikan bahwa perubahan satu variabel
menyebabkan perubahan variabel lain.

## 18.4 Menghapus semua outlier

Outlier bisa merupakan properti premium yang valid. Periksa konteks
sebelum menghapus.

## 18.5 Menganggap p-value sebagai probabilitas H0 benar

p-value bukan probabilitas bahwa H0 benar. p-value mengukur seberapa
ekstrem hasil data jika H0 dan asumsi pengujian berlaku.

## 18.6 Menganggap listing terbanyak berarti demand tertinggi

Listing adalah penawaran yang terlihat. Demand harus diukur dengan
indikator permintaan yang sesuai.

## 18.7 Tidak menulis insight di bawah grafik

Grafik harus menjawab pertanyaan. Jelaskan apa yang terlihat dan apa
implikasinya bagi client.

## 18.8 Menyalin angka tanpa memahami satuan

`price_idr` menggunakan rupiah, sedangkan `price_million` menggunakan
juta rupiah. Jangan mencampur keduanya ketika menulis kesimpulan.

------------------------------------------------------------------------

# 19. Batasan Proyek dan Persiapan Submission Resmi

Paket ini dibuat sebagai simulasi pembelajaran yang mengikuti workflow
MS1, tetapi ada perbedaan penting dari ketentuan submission resmi.

  -----------------------------------------------------------------------
  Aspek                   Paket latihan           Submission resmi
  ----------------------- ----------------------- -----------------------
  Dataset                 Dummy/sintetis          Non-dummy

  Sumber                  Dibuat dengan NumPy     URL sumber harus
                                                  dicantumkan

  Konsultasi buddy        Tidak dilakukan         Wajib

  Notebook                Sudah disiapkan         Harus disesuaikan
                                                  dengan data nyata

  Dashboard               Panduan, belum          Tableau Public
                          dipublish               dipublish dan URL
                                                  dicantumkan

  Insight                 Contoh berdasarkan data Berdasarkan data nyata
                          simulasi                yang dianalisis
  -----------------------------------------------------------------------

Instruksi resmi juga menyebutkan: - Format notebook:
`P0M1_<nama-student>.ipynb`. - Format dataset:
`P0M1_<nama-student>_dataset.csv`. - URL dashboard harus ada di bagian
paling atas notebook. - Dataset harus disertakan. - `description.md`
perlu diedit dan dikumpulkan sesuai ketentuan repository tugas. -
Ketentuan repository pribadi mengikuti waktu yang ditetapkan dalam
instruksi MS1.

Jangan mengirimkan dataset dummy ini sebagai dataset resmi MS1.

------------------------------------------------------------------------

# 20. Glosarium Singkat

  Istilah                  Arti sederhana
  ------------------------ ------------------------------------------------
  Business Understanding   Memahami kebutuhan bisnis
  Problem Statement        Pernyataan masalah yang ingin dijawab
  SMART                    Framework membuat tujuan spesifik dan terukur
  Dataset                  Kumpulan data
  DataFrame                Struktur tabel pada Pandas
  Data Loading             Membaca data ke program
  Data Cleaning            Memperbaiki/menyiapkan data
  Missing Values           Nilai kosong
  Duplicate                Data duplikat
  EDA                      Eksplorasi data
  GroupBy                  Mengelompokkan data
  Aggregation              Menghitung ringkasan kelompok
  Mean                     Rata-rata
  Median                   Nilai tengah
  Mode                     Nilai paling sering muncul
  Variance                 Ukuran penyebaran kuadrat
  Standard Deviation       Ukuran penyebaran dalam satuan asli
  Quartile                 Pembagian data menjadi empat bagian
  IQR                      Q3 dikurangi Q1
  Outlier                  Nilai yang jauh dari pola umum
  Correlation              Ukuran arah dan kekuatan hubungan
  Hypothesis               Pernyataan yang diuji
  H0                       Hipotesis nol
  H1                       Hipotesis alternatif
  Alpha                    Batas signifikansi
  p-value                  Ukuran bukti terhadap H0 berdasarkan model uji
  Confidence Interval      Rentang estimasi parameter
  t-test                   Uji perbedaan rata-rata
  Tableau Dashboard        Tampilan visualisasi interaktif
  Insight                  Temuan yang bermakna dari analisis

------------------------------------------------------------------------

# 21. Cara Belajar Paling Efektif

Jangan hanya menekan **Run All** lalu menganggap proyek selesai.

Gunakan pola belajar berikut untuk setiap tahap:

1.  **Baca pertanyaan bisnis.** Pahami apa yang ingin diketahui.
2.  **Baca kode.** Identifikasi fungsi yang digunakan.
3.  **Jalankan cell.** Amati output.
4.  **Jelaskan output dengan kata-kata sendiri.** Jangan hanya menyalin
    angka.
5.  **Hubungkan ke bisnis.** Apa arti hasil tersebut bagi client?
6.  **Catat keterbatasan.** Apa yang belum dapat dijawab oleh data?

Target akhirnya adalah mampu menjelaskan satu alur utuh:

> Client memiliki masalah → Data Analyst merumuskan pertanyaan → data
> dipahami dan dibersihkan → analisis statistik dilakukan → visualisasi
> dibuat → hasil diterjemahkan menjadi insight → rekomendasi diberikan
> kepada client.

**Jika kamu mampu menjelaskan alur tersebut dan mempertanggungjawabkan
setiap grafik serta perhitungan statistik, berarti kamu sudah memahami
fondasi pengerjaan Milestone 1.**
