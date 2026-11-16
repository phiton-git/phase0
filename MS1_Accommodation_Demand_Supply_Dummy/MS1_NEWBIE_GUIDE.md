# Panduan Newbie — Memahami Alur MS1

## Gambaran besar
Alur MS1:
1. **Business Understanding:** pahami masalah bisnis sebelum membuka grafik.
2. **Dataset:** tentukan data yang dibutuhkan untuk menjawab pertanyaan.
3. **Data Loading:** baca CSV ke Pandas DataFrame.
4. **Data Cleaning:** periksa missing value, duplikat, tipe data, dan nilai tidak masuk akal.
5. **EDA:** cari pola dengan tabel ringkasan dan grafik.
6. **Descriptive Statistics:** jelaskan pusat dan penyebaran data.
7. **Inferential Statistics:** gunakan sampel/data untuk menguji hipotesis.
8. **Conclusion:** ubah hasil analisis menjadi jawaban dan rekomendasi.
9. **Tableau:** tampilkan visual dan ringkasan statistik dalam dashboard interaktif.

## Memahami supply dan demand proxy
- **Supply:** jumlah listing yang tersedia di dataset. Ini menghitung penawaran yang tercatat.
- **Demand aktual:** jumlah pemesanan atau malam yang benar-benar dipesan. Dataset latihan ini tidak punya booking aktual.
- **Proxy aktivitas:** `reviews_per_month`, yaitu rata-rata review per bulan. Ini dapat memberi indikasi aktivitas tamu, tetapi tidak semua tamu menulis review.
- **Aktivitas per listing:** total `reviews_per_month` di suatu lokasi dibagi jumlah listing di lokasi tersebut. Ini membantu membandingkan lokasi dengan jumlah listing berbeda.

## Mengapa perlu melihat total dan rata-rata?
Contoh ilustrasi:
| Lokasi | Listing | Total review/bulan | Review/listing/bulan |
|---|---:|---:|---:|
| A | 500 | 1.000 | 2 |
| B | 100 | 600 | 6 |
| C | 50 | 400 | 8 |

Lokasi A punya total aktivitas tertinggi, tetapi C punya rata-rata aktivitas per listing tertinggi. Dua metrik menjawab pertanyaan yang berbeda.

## Fungsi Pandas yang digunakan
- `pd.read_csv()`: membaca file CSV.
- `.head()`: melihat beberapa baris pertama.
- `.info()`: melihat tipe data dan jumlah nilai terisi.
- `.isna().sum()`: menghitung missing value.
- `.duplicated().sum()`: menghitung duplikat.
- `.drop_duplicates()`: menghapus baris duplikat.
- `.dropna()`: menghapus baris yang missing pada kolom tertentu.
- `.groupby()`: mengelompokkan data.
- `.agg()`: menghitung beberapa ringkasan sekaligus.
- `.reset_index()`: mengubah index hasil groupby menjadi kolom.
- `.sort_values()`: mengurutkan data.

## Statistik newbie
- **Mean:** rata-rata = jumlah seluruh nilai dibagi jumlah data.
- **Median:** nilai tengah setelah data diurutkan.
- **Standard deviation:** ukuran seberapa menyebar data dari rata-rata.
- **Quartile:** membagi data terurut menjadi empat bagian.
- **IQR:** Q3 - Q1; rentang 50% data tengah.
- **P-value:** ukuran kompatibilitas data dengan H0 pada model uji; bukan probabilitas H0 benar.
- **Alpha 0.05:** batas signifikansi yang ditetapkan sebelum pengujian.

## Kruskal-Wallis
Dipakai untuk membandingkan distribusi/ranking tiga kelompok independen atau lebih. H0 menyatakan distribusi kelompok sama. Jika p-value < 0.05, ada bukti setidaknya satu kelompok berbeda. Uji ini tidak memberi tahu pasangan kelompok mana yang berbeda; perlu post-hoc.

## Empat grafik notebook
1. Jumlah listing per lokasi = supply.
2. Total review bulanan per lokasi = total proxy aktivitas.
3. Rata-rata review bulanan per listing = aktivitas relatif.
4. Harga vs review bulanan = eksplorasi hubungan dua variabel.

## Kesalahan interpretasi yang harus dihindari
- Jangan menyebut review sebagai booking.
- Jangan menyebut availability sebagai occupancy.
- Jangan menyimpulkan korelasi berarti sebab-akibat.
- Jangan menyebut hasil Kruskal-Wallis sebagai bukti semua kelompok berbeda.
- Jangan memakai data dummy sebagai dataset final MS1.
