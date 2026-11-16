# Tableau Public — Panduan Dashboard MS1

Dashboard MS1 sesuai instruksi terdiri dari dua bagian:
1. **Data Visualization:** minimal 4 visualisasi dan minimal 2 interaktivitas.
2. **Statistical Analysis:** tampilkan statistik deskriptif, inferensial, analisis, dan kesimpulan.

> File CSV dalam paket ini adalah dummy untuk latihan Tableau. Hasil dashboard harus diberi label SIMULASI.

## Opsi paling mudah: gunakan dataset utama
1. Buka Tableau Public.
2. Connect → To a File → Text File.
3. Pilih `data/P0M1_nawafi_suren_dataset.csv`.
4. Buat worksheet:
   - `Supply by Neighbourhood`: Columns = neighbourhood; Rows = COUNT(id); Marks = Bar.
   - `Total Review Activity`: Columns = neighbourhood; Rows = SUM(reviews_per_month); Marks = Bar.
   - `Average Review per Listing`: Columns = neighbourhood; Rows = AVG(reviews_per_month); Marks = Bar.
   - `Price vs Review Activity`: Columns = price; Rows = reviews_per_month; Marks = Circle; Color = room_type.
5. Buat dashboard baru dan masukkan empat worksheet tersebut.
6. Tambahkan filter `neighbourhood` dan `room_type`. Pada menu filter pilih **Apply to Worksheets → All Using This Data Source**.
7. Tambahkan judul dan catatan: `SIMULATED DATA — NOT REAL MARKET FINDINGS`.

## Interaktivitas minimum
- Filter lokasi (`neighbourhood`).
- Filter tipe penginapan (`room_type`).
- Opsional: aktifkan Use as Filter pada grafik lokasi agar klik bar memfilter grafik lain.

## Bagian Statistical Analysis
Tambahkan Text objects yang diisi setelah menjalankan notebook:
- Descriptive statistics: mean, median, std, IQR untuk price dan reviews_per_month.
- Inferential statistics: H-statistic, p-value, alpha 0.05, keputusan Kruskal-Wallis.
- Interpretasi singkat dan keterbatasan proxy review.

## Publish
1. Sign in Tableau Public.
2. Server → Tableau Public → Save to Tableau Public.
3. Salin URL dashboard.
4. Tempel URL tersebut di bagian paling atas notebook.

## Penting
Jangan mengklaim dashboard ini menggambarkan pasar nyata. Untuk submission, ganti data dengan dataset asli yang telah disetujui buddy.
