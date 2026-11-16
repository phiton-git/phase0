# phase0

# Pentingnya Sort (`sort()`) Sebelum Menentukan Median

**Materi:** Statistics – Descriptive Statistics  
**Level:** Beginner / Newbie  
**Bahasa:** Python  
**Konteks:** Analisis Data Bike Sharing

---

## 1. Memahami Apa Itu Median

Median adalah nilai tengah dari sekumpulan data yang sudah **diurutkan dari nilai terkecil ke terbesar**.

Artinya, sebelum menentukan median, kita harus mengurutkan data terlebih dahulu.

Contoh data durasi perjalanan (menit):

```python
data = [30, 10, 50, 20, 40]
```

Data tersebut belum berurutan.

Kita harus mengurutkannya:

```python
data.sort()

print(data)
```

Hasil:

```text
[10, 20, 30, 40, 50]
```

Sekarang kita bisa melihat bahwa angka yang berada di tengah adalah `30`.

**Median = 30**

### Visualisasi

| Posisi | Nilai |
|---|---:|
| 1 | 10 |
| 2 | 20 |
| 3 | **30 (Median)** |
| 4 | 40 |
| 5 | 50 |

---

## 2. Mengapa Tidak Boleh Langsung Mengambil Nilai Tengah?

Perhatikan contoh berikut:

```python
data = [100, 10, 50, 20, 40]
```

Jika kita langsung mengambil elemen tengah menggunakan indeks:

```python
print(data[2])
```

Hasil:

```text
50
```

Apakah 50 merupakan median?

**Belum tentu!**

Karena data belum diurutkan.

Sekarang kita urutkan:

```python
data.sort()

print(data)
```

Hasil:

```text
[10, 20, 40, 50, 100]
```

Median sebenarnya adalah:

```text
40
```

Bukan 50.

**Kesimpulan:**

Posisi tengah dalam urutan data asli belum tentu merupakan median. Posisi tengah harus ditentukan setelah data diurutkan.

---

## 3. Memahami `sort()` dan `sorted()`

Python memiliki dua cara umum untuk mengurutkan data.

### A. Menggunakan `sort()`

```python
data = [30, 10, 50, 20, 40]

data.sort()

print(data)
```

Hasil:

```text
[10, 20, 30, 40, 50]
```

Karakteristik:

- Mengurutkan data secara langsung.
- Mengubah urutan list asli.
- Tidak menghasilkan list baru.
- Return value-nya adalah `None`.

Contoh:

```python
data = [30, 10, 50]

hasil = data.sort()

print(hasil)
```

Hasil:

```text
None
```

Mengapa `None`?

Karena `sort()` mengubah list secara langsung, bukan mengembalikan list baru.

### B. Menggunakan `sorted()`

```python
data = [30, 10, 50, 20, 40]

data_urut = sorted(data)

print(data_urut)
print(data)
```

Hasil:

```text
[10, 20, 30, 40, 50]
[30, 10, 50, 20, 40]
```

Karakteristik:

- Menghasilkan list baru yang sudah diurutkan.
- Data asli tidak berubah.
- Cocok ketika kita ingin mempertahankan data asli.

### Perbedaan `sort()` dan `sorted()`

| Fungsi | Mengubah data asli? | Menghasilkan list baru? |
|---|---|---|
| `sort()` | Ya | Tidak |
| `sorted()` | Tidak | Ya |

**Tips untuk Data Science:**

Gunakan `sorted()` jika ingin mempertahankan data asli selama proses analisis.

---

## 4. Menentukan Median Jika Jumlah Data Ganjil

Contoh:

```python
data = [10, 20, 30, 40, 50]

data.sort()

n = len(data)

median = data[n // 2]

print("Jumlah data:", n)
print("Median:", median)
```

Hasil:

```text
Jumlah data: 5
Median: 30
```

### Memahami setiap baris kode

**1. Menghitung jumlah data**

```python
n = len(data)
```

`len()` digunakan untuk menghitung jumlah elemen dalam list.

Hasil:

```text
5
```

**2. Menghitung indeks tengah**

```python
n // 2
```

Hasil:

```text
2
```

Mengapa menggunakan `//`?

Karena `//` adalah operator pembagian bulat (floor division).

Contoh:

```python
print(5 // 2)
```

Hasil:

```text
2
```

**3. Mengambil nilai tengah**

```python
median = data[n // 2]
```

Sama dengan:

```python
median = data[2]
```

Hasil:

```text
30
```

### Memahami indeks Python

Python menggunakan indeks yang dimulai dari nol.

| Nilai | Indeks |
|---|---:|
| 10 | 0 |
| 20 | 1 |
| 30 | 2 |
| 40 | 3 |
| 50 | 4 |

Jadi `data[2]` menghasilkan angka `30`.

---

## 5. Menentukan Median Jika Jumlah Data Genap

Contoh:

```python
data = [10, 20, 30, 40, 50, 60]

data.sort()

n = len(data)

median = (data[n // 2 - 1] + data[n // 2]) / 2

print("Median:", median)
```

Hasil:

```text
Median: 35.0
```

Mengapa 35?

Karena jumlah data genap, sehingga terdapat dua nilai tengah.

| Nilai | Indeks |
|---|---:|
| 10 | 0 |
| 20 | 1 |
| 30 | 2 |
| 40 | 3 |
| 50 | 4 |
| 60 | 5 |

Dua nilai tengah adalah:

- 30 (indeks 2)
- 40 (indeks 3)

Rumus:

\[
Median = \frac{30+40}{2}
\]

\[
Median = 35
\]

### Memahami kode

```python
data[n // 2 - 1]
```

Mengambil nilai tengah sebelah kiri.

```python
data[n // 2]
```

Mengambil nilai tengah sebelah kanan.

Kemudian kedua nilai dijumlahkan dan dibagi dua.

---

## 6. Menghitung Median Menggunakan NumPy

Dalam Data Science, kita sering menggunakan NumPy untuk menghitung median secara otomatis.

```python
import numpy as np

data = [30, 10, 50, 20, 40]

median = np.median(data)

print("Median:", median)
```

Hasil:

```text
Median: 30.0
```

Menariknya, `np.median()` tidak mengharuskan kita memanggil `sort()` terlebih dahulu.

NumPy menangani proses pengurutan secara internal ketika menghitung median.

Namun, memahami proses manual tetap penting agar kita mengerti bagaimana median diperoleh.

### Perbandingan

| Cara | Keterangan |
|---|---|
| `sort()` + perhitungan manual | Memahami proses menentukan median |
| `sorted()` + perhitungan manual | Menghitung median tanpa mengubah data asli |
| `np.median()` | Menghitung median secara otomatis |

---

## 7. Contoh Lengkap: Analisis Durasi Perjalanan Bike Sharing

Kita gunakan contoh yang relevan dengan analisis statistik Bike Sharing.

Misalnya terdapat data durasi perjalanan dalam detik:

```python
import numpy as np

# 1. Data durasi perjalanan (detik)
duration = [100, 500, 200, 300, 900, 400, 600]

# 2. Mengurutkan data
duration_sorted = sorted(duration)

# 3. Menghitung jumlah data
n = len(duration_sorted)

# 4. Menghitung median
if n % 2 == 1:

    # Jika jumlah data ganjil
    median = duration_sorted[n // 2]

else:

    # Jika jumlah data genap
    median = (
        duration_sorted[n // 2 - 1] +
        duration_sorted[n // 2]
    ) / 2

# 5. Menampilkan hasil
print("Data asli:", duration)
print("Data terurut:", duration_sorted)
print("Jumlah data:", n)
print("Median:", median)
```

Hasil:

```text
Data asli: [100, 500, 200, 300, 900, 400, 600]

Data terurut: [100, 200, 300, 400, 500, 600, 900]

Jumlah data: 7

Median: 400
```

### Memahami alur berpikir

1. **Mengumpulkan data**

   `[100, 500, 200, 300, 900, 400, 600]`

2. **Mengurutkan data**

   `[100, 200, 300, 400, 500, 600, 900]`

3. **Menghitung jumlah data**

   `n = 7`

4. **Memeriksa jumlah data**

   `7 % 2 == 1`, berarti jumlah data ganjil.

5. **Mengambil nilai tengah**

   `duration_sorted[7 // 2]`

   `duration_sorted[3]`

6. **Mendapatkan median**

   `400 detik`

---

## 8. Memahami Operator `%` (Modulus)

Pada kode sebelumnya kita menggunakan:

```python
if n % 2 == 1:
```

Apa maksudnya?

Operator `%` digunakan untuk mendapatkan sisa hasil pembagian.

Contoh:

```python
print(7 % 2)
print(6 % 2)
```

Hasil:

```text
1
0
```

Mengapa?

- 7 dibagi 2 menghasilkan sisa 1.
- 6 dibagi 2 menghasilkan sisa 0.

Dalam menentukan median:

| Kondisi | Artinya |
|---|---|
| `n % 2 == 1` | Jumlah data ganjil |
| `n % 2 == 0` | Jumlah data genap |

Inilah alasan kita menggunakan `if` dan `else` untuk menentukan rumus median.

---

## 9. Latihan Mandiri

Sekarang coba tentukan median dari data berikut.

```python
data = [80, 20, 60, 40, 100]
```

**Pertanyaan:**

1. Bagaimana hasil data setelah diurutkan?
2. Berapa jumlah datanya?
3. Apakah jumlah data ganjil atau genap?
4. Berapa nilai mediannya?

### Solusi

```python
data = [80, 20, 60, 40, 100]

# Mengurutkan data
data_sorted = sorted(data)

# Menghitung jumlah data
n = len(data_sorted)

# Menghitung median
median = data_sorted[n // 2]

print("Data asli:", data)
print("Data terurut:", data_sorted)
print("Jumlah data:", n)
print("Median:", median)
```

Hasil:

```text
Data asli: [80, 20, 60, 40, 100]
Data terurut: [20, 40, 60, 80, 100]
Jumlah data: 5
Median: 60
```

**Jawaban: Median = 60**

---

## 10. Ringkasan Materi

| Konsep | Fungsi |
|---|---|
| `sort()` | Mengurutkan list asli |
| `sorted()` | Mengurutkan dan menghasilkan list baru |
| `len()` | Menghitung jumlah data |
| `//` | Pembagian bulat |
| `%` | Menghitung sisa pembagian |
| `np.median()` | Menghitung median otomatis |

### Rumus Median

**Jika jumlah data ganjil:**

\[
Median = X_{\frac{n+1}{2}}
\]

**Jika jumlah data genap:**

\[
Median = \frac{X_{\frac{n}{2}}+X_{\frac{n}{2}+1}}{2}
\]

Catatan: \(X\) menunjukkan data yang sudah diurutkan.

### Hal yang Wajib Diingat

1. Median bukan sekadar mengambil elemen yang berada di tengah list.
2. Data harus diurutkan terlebih dahulu.
3. Jika jumlah data ganjil, ambil satu nilai tengah.
4. Jika jumlah data genap, hitung rata-rata dua nilai tengah.
5. `np.median()` dapat menghitung median secara otomatis.
6. `sorted()` berguna jika kita ingin mempertahankan data asli.

---

## 11. Hubungan Median dengan Outlier dalam Data Science

Median sangat berguna ketika dataset memiliki **outlier (nilai ekstrem)**.

Contoh:

```python
data = [10, 15, 15, 20, 25, 30, 60]

import numpy as np

print("Mean:", np.mean(data))
print("Median:", np.median(data))
```

Hasil:

```text
Mean: 25.0
Median: 20.0
```

Nilai 60 merupakan nilai yang relatif jauh dari sebagian besar data.

Nilai ekstrem tersebut dapat menarik mean ke arah yang lebih tinggi.

Sementara median lebih tahan terhadap pengaruh outlier.

**Interpretasi:**

- Mean menggambarkan rata-rata aritmetika.
- Median menggambarkan nilai tengah data.
- Median sering lebih representatif untuk menggambarkan pusat data ketika terdapat outlier.

### Kesimpulan Akhir

Dalam analisis statistik, pengurutan data merupakan konsep dasar yang wajib dipahami sebelum menentukan median.

Urutan berpikir yang benar:

```text
DATA MENTAH
    |
    v
URUTKAN DATA (sort / sorted)
    |
    v
HITUNG JUMLAH DATA (len)
    |
    v
PERIKSA GANJIL ATAU GENAP (%)
    |
    v
TENTUKAN NILAI TENGAH
    |
    v
MEDIAN
    |
    v
INTERPRETASI HASIL
```


**Ingat:** Median adalah nilai tengah dari data yang sudah diurutkan, bukan nilai yang kebetulan berada di tengah urutan data mentah.


import pandas as pd

# Contoh data kotor
df = pd.DataFrame({
    'price': ['Rp 150.000', 'Rp 25.000,-', 'IDR 1.200.000', 'Gratis', None]
})

# 1. Hapus semua karakter selain angka (0-9)
df['price_clean'] = df['price'].astype(str).str.replace(r'[^0-9]', '', regex=True)

# 2. Ubah string kosong menjadi NaN (jika ada teks seperti 'Gratis') lalu ubah ke tipe float/int
df['price_clean'] = pd.to_numeric(df['price_clean'], errors='coerce')

print

(df)
