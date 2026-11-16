# MS1 — Accommodation Market Demand & Supply Analysis

Project latihan Milestone 1 Hacktiv8 dengan tema pasar penginapan.

> **PENTING: seluruh dataset di folder `data/` adalah DATA DUMMY / SINTETIS.**
> Instruksi resmi MS1 mensyaratkan dataset non-dummy yang sumbernya dapat dipertanggungjawabkan. Paket ini dibuat untuk mempelajari alur kerja end-to-end, bukan untuk langsung dikumpulkan sebagai tugas final.

## Isi paket
- `P0M1_nawafi_suren.ipynb` — notebook lengkap dari Introduction sampai Conclusion.
- `data/P0M1_nawafi_suren_dataset.csv` — 600 baris data listing sintetis.
- `data/tableau_location_summary.csv` — ringkasan per lokasi untuk Tableau.
- `data/tableau_room_type_summary.csv` — ringkasan per tipe kamar untuk Tableau.
- `TABLEAU_GUIDE.md` — langkah membuat dashboard Tableau Public.
- `MS1_NEWBIE_GUIDE.md` — penjelasan alur kerja dan konsep newbie.
- `description.md` — dokumentasi project bergaya repository.
- `requirements.txt` — library Python.
- `DATA_DICTIONARY.md` — arti kolom dan batasan interpretasi.

## Cara menjalankan
1. Extract ZIP.
2. Buka terminal pada folder hasil extract.
3. Buat/aktifkan environment Python.
4. Install library: `pip install -r requirements.txt`
5. Buka Jupyter Notebook/JupyterLab.
6. Jalankan `P0M1_nawafi_suren.ipynb` dari folder utama project.
7. Pilih **Restart Kernel and Run All** untuk memastikan semua cell berjalan.

## Struktur folder
```text
MS1_Accommodation_Demand_Supply_Dummy/
├── P0M1_nawafi_suren.ipynb
├── README.md
├── description.md
├── MS1_NEWBIE_GUIDE.md
├── TABLEAU_GUIDE.md
├── DATA_DICTIONARY.md
├── requirements.txt
└── data/
    ├── P0M1_nawafi_suren_dataset.csv
    ├── tableau_location_summary.csv
    └── tableau_room_type_summary.csv
```

## Catatan
- Harga adalah angka USD simulasi.
- `reviews_per_month` adalah proxy aktivitas review, bukan jumlah booking.
- `availability_365` bukan occupancy aktual.
- Tidak ada klaim hasil yang mewakili pasar Airbnb Singapore sungguhan.
