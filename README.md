# 📚 Data Cleansing & Data Enrichment Dataset Perpustakaan Universitas

## 📌 Deskripsi

Project ini merupakan implementasi **Data Cleansing** dan **Data Enrichment** pada dataset peminjaman buku perpustakaan universitas.

Pengolahan data dilakukan menggunakan **Python, Pandas, Regex, dan Google Colab**. Dataset dibaca dari file Excel kemudian diperiksa struktur variabelnya, dibersihkan melalui beberapa tahap standarisasi, diperiksa data kosong dan duplikat, kemudian diperkaya dengan informasi baru berupa **Kelompok Usia Buku**.

Project ini menggunakan dataset:

```text
Data-Kotor-Perpustakaan.xlsx
```

---

# 🎯 Tujuan

Project ini bertujuan untuk:

1. Membaca dan memahami struktur dataset perpustakaan.
2. Menyeragamkan format data yang tidak konsisten.
3. Menangani data kosong.
4. Mengidentifikasi dan menghapus data duplikat.
5. Menambahkan informasi baru melalui proses data enrichment.
6. Menghasilkan data yang lebih siap untuk digunakan dalam analisis.

---

# 🛠️ Teknologi yang Digunakan

| Teknologi | Fungsi |
|---|---|
| Python | Bahasa pemrograman untuk pengolahan data |
| Pandas | Membaca, membersihkan, dan mengolah dataset |
| Regex (`re`) | Membersihkan dan menyeragamkan teks |
| OpenPyXL | Membaca file Excel `.xlsx` |
| Google Colab | Lingkungan untuk menjalankan notebook |
| Google Drive | Penyimpanan dan akses dataset |

---

# 📊 Dataset

Dataset yang digunakan adalah:

**Data Kotor Perpustakaan Universitas**

File:

```text
Data-Kotor-Perpustakaan.xlsx
```

Dataset berisi informasi mengenai anggota perpustakaan, buku, penerbit, tahun terbit, tanggal peminjaman, status, dan lama peminjaman.

## Struktur Variabel

| Kolom | Deskripsi |
|---|---|
| `No` | Nomor urut data |
| `ID Anggota` | Identitas anggota perpustakaan |
| `Nama Anggota` | Nama anggota perpustakaan |
| `Program Studi` | Program studi anggota |
| `Kode Buku` | Kode buku |
| `Judul Buku` | Judul buku |
| `Penerbit` | Nama penerbit buku |
| `Tahun Terbit` | Tahun buku diterbitkan |
| `Tanggal Pinjam` | Tanggal buku dipinjam |
| `Status` | Status peminjaman buku |
| `Lama Pinjam (Hari)` | Lama peminjaman buku dalam satuan hari |

Struktur dan tipe data diperiksa menggunakan:

```python
data.info()
```

dan:

```python
data.dtypes
```

---

# 🧹 DATA CLEANSING

## Apa itu Data Cleansing?

**Data cleansing** adalah proses memperbaiki atau menyeragamkan data yang memiliki format tidak konsisten, data kosong, atau data duplikat agar data lebih terstruktur dan siap digunakan.

Pada notebook ini, proses data cleansing dilakukan dalam beberapa tahap.

---

## 1. Standarisasi Kolom Nama Anggota

Tahap pertama adalah membersihkan kolom:

```text
Nama Anggota
```

Fungsi `clean_name()` digunakan untuk:

- Memeriksa data kosong.
- Mengubah nilai menjadi string.
- Menghapus spasi di awal dan akhir.
- Menghapus spasi berlebih di tengah nama.
- Menyeragamkan huruf menggunakan format `Title Case`.

Contoh:

```text
"  Citra Lestari" → "Citra Lestari"
"BUDI SANTOSO"    → "Budi Santoso"
"andi wijaya "    → "Andi Wijaya"
```

Hasil cleansing disimpan pada kolom baru:

```text
Nama Anggota Bersih
```

Kode utama:

```python
df['Nama Anggota Bersih'] = df['Nama Anggota'].apply(clean_name)
```

> **Catatan:** Pada notebook, kolom asli `Nama Anggota` tetap digunakan dan kolom hasil cleansing ditambahkan sebagai `Nama Anggota Bersih`.

---

## 2. Standarisasi Kolom Program Studi

Tahap berikutnya adalah menyeragamkan penulisan:

```text
Program Studi
```

Beberapa variasi yang dianggap memiliki makna yang sama:

```text
TI
Informatika
Teknik informatika
```

diseragamkan menjadi:

```text
Teknik Informatika
```

Proses dilakukan menggunakan fungsi `clean_prodi()` dan hasilnya disimpan pada:

```text
Program Studi Bersih
```

Contoh:

```python
df['Program Studi Bersih'] = df['Program Studi'].apply(clean_prodi)
```

---

## 3. Standarisasi Kolom Status

Kolom:

```text
Status
```

diseragamkan agar penggunaan huruf besar dan kecil konsisten.

Contoh:

```text
dipinjam
Dipinjam
DIPINJAM
```

menjadi:

```text
Dipinjam
```

Sedangkan:

```text
dikembalikan
```

menjadi:

```text
Dikembalikan
```

Hasilnya disimpan pada:

```text
Status Bersih
```

---

## 4. Standarisasi Kolom Penerbit

Kolom:

```text
Penerbit
```

juga memiliki kemungkinan variasi penulisan.

Contoh:

```text
Andi
ANDI
Andi Publisher
```

diseragamkan menjadi:

```text
Andi Publisher
```

Contoh lainnya:

```text
Elex Media
```

diseragamkan menjadi:

```text
Elex Media Komputindo
```

Hasilnya disimpan pada:

```text
Penerbit Bersih
```

---

## 5. Standarisasi Kolom Tanggal Pinjam

Kolom:

```text
Tanggal Pinjam
```

diproses menggunakan fungsi `standardize_date()`.

Tujuannya adalah mengubah berbagai format tanggal menjadi format:

```text
DD-MM-YYYY
```

Contoh:

```text
12/09/2026 → 12-09-2026
2026-09-10 → 10-09-2026
10-09-2026 → 10-09-2026
```

Kode menggunakan:

```python
pd.to_datetime()
```

dan:

```python
strftime('%d-%m-%Y')
```

Hasil standarisasi disimpan pada kolom:

```text
Tanggal Pinjam Bersih
```

---

# 🕳️ 6. Menangani Data Kosong

Setelah proses standarisasi tanggal, dataset diperiksa untuk mengetahui jumlah data kosong menggunakan:

```python
df.isnull().sum()
```

Pada notebook, data kosong pada kolom hasil standarisasi tanggal ditangani dengan:

```python
df['Tanggal Pinjam Bersih'] = df['Tanggal Pinjam Bersih'].fillna(
    'Tidak Diketahui'
)
```

Dengan demikian, nilai tanggal yang tidak tersedia pada kolom hasil cleansing tidak ditampilkan sebagai `NaN` atau `NaT`, tetapi:

```text
Tidak Diketahui
```

Setelah penanganan, data kosong diperiksa kembali menggunakan:

```python
df.isnull().sum()
```

---

# 🔁 7. Identifikasi dan Penghapusan Duplikasi

Data duplikat dicari berdasarkan kombinasi:

```text
Nama Anggota + Judul Buku
```

Kode:

```python
duplicate_rows = df[
    df.duplicated(
        ['Nama Anggota', 'Judul Buku'],
        keep=False
    )
]
```

Parameter:

```text
keep=False
```

digunakan agar seluruh baris yang teridentifikasi sebagai duplikat dapat ditampilkan.

Setelah data duplikat diperiksa, penghapusan dilakukan menggunakan:

```python
df = df.drop_duplicates()
```

Kemudian index diatur ulang:

```python
df = df.reset_index(drop=True)
```

Nomor data juga diperbarui:

```python
df['No'] = range(1, len(df) + 1)
```

Jumlah data setelah penghapusan ditampilkan menggunakan:

```python
print("Jumlah data setelah duplikat dihapus:", len(df))
```

> **Catatan:** Identifikasi duplikat pada notebook menggunakan `Nama Anggota` dan `Judul Buku`, sedangkan `drop_duplicates()` menghapus baris yang identik secara keseluruhan.

---

# ✨ DATA ENRICHMENT

## Apa itu Data Enrichment?

**Data enrichment** adalah proses menambahkan informasi baru berdasarkan data yang sudah tersedia pada dataset.

Pada project ini, data enrichment dilakukan dengan membuat kolom:

```text
Kelompok Usia Buku
```

Kolom tersebut diperoleh dari:

```text
Tahun Terbit
```

---

# 📚 Kelompok Usia Buku

Tahun acuan yang digunakan dalam notebook adalah:

```text
2026
```

Usia buku dihitung menggunakan:

```text
Usia Buku = 2026 - Tahun Terbit
```

Kemudian buku dikelompokkan menjadi empat kategori:

| Usia Buku | Kelompok Usia Buku |
|---:|---|
| ≤ 2 tahun | Buku Baru |
| 3–6 tahun | Buku Menengah |
| 7–11 tahun | Buku Lama |
| > 11 tahun | Buku Sangat Lama |

Contoh:

```text
Tahun Terbit = 2022

2026 - 2022 = 4 tahun

Kelompok Usia Buku = Buku Menengah
```

Jika tahun terbit tidak dapat diproses, hasilnya:

```text
Tidak Diketahui
```

Hasil enrichment disimpan pada kolom:

```text
Kelompok Usia Buku
```

---

# 🔄 ALUR PENGOLAHAN DATA

```text
┌──────────────────────────────┐
│       DATA KOTOR             │
│ Data-Kotor-Perpustakaan.xlsx │
└──────────────┬───────────────┘
               ↓
      Membaca Dataset
               ↓
      Memeriksa Struktur Data
               ↓
┌──────────────────────────────┐
│       DATA CLEANSING         │
├──────────────────────────────┤
│ 1. Standarisasi Nama         │
│ 2. Standarisasi Program Studi│
│ 3. Standarisasi Status       │
│ 4. Standarisasi Penerbit     │
│ 5. Standarisasi Tanggal      │
│ 6. Menangani Data Kosong     │
│ 7. Menghapus Duplikasi       │
└──────────────┬───────────────┘
               ↓
         DATA LEBIH BERSIH
               ↓
┌──────────────────────────────┐
│       DATA ENRICHMENT        │
├──────────────────────────────┤
│ Kelompok Usia Buku           │
│ berdasarkan Tahun Terbit     │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ HASIL DATA CLEANSING &       │
│ DATA ENRICHMENT               │
└──────────────────────────────┘
```

---

# 📋 Tahapan Notebook

Notebook `2418061DataCleansing.ipynb` terdiri dari 12 bagian utama:

| No. | Tahap |
|---:|---|
| 1 | Import Library |
| 2 | Membaca Dataset Kotor Perpustakaan Universitas |
| 3 | Struktur Variabel Data Perpustakaan Universitas |
| 4 | Data Cleansing – Standarisasi Kolom Nama Anggota |
| 5 | Data Cleansing – Standarisasi Kolom Program Studi |
| 6 | Data Cleansing – Standarisasi Kolom Status |
| 7 | Data Cleansing – Standarisasi Kolom Penerbit |
| 8 | Data Cleansing – Standarisasi Kolom Tanggal Pinjam |
| 9 | Data Cleansing – Data Kosong |
| 10 | Data Cleansing – Duplikasi Data |
| 11 | Data Enrichment |
| 12 | Hasil Data Cleansing & Data Enrichment |

---

# 📈 Output

Hasil akhir ditampilkan menggunakan:

```python
df.head(37)
```

Output tersebut digunakan untuk melihat hasil keseluruhan proses **Data Cleansing dan Data Enrichment**.

Kolom hasil cleansing yang dibuat dalam notebook antara lain:

```text
Nama Anggota Bersih
Program Studi Bersih
Status Bersih
Penerbit Bersih
Tanggal Pinjam Bersih
```

Sedangkan hasil data enrichment adalah:

```text
Kelompok Usia Buku
```

---

# 📁 Struktur Project

```text
Data-Cleansing-Perpustakaan/
│
├── Data-Kotor-Perpustakaan.xlsx
├── 2418061DataCleansing.ipynb
└── README.md
```

### Penjelasan

- `Data-Kotor-Perpustakaan.xlsx` — dataset awal yang digunakan dalam proses pengolahan.
- `2418061DataCleansing.ipynb` — notebook Google Colab yang berisi seluruh proses cleansing dan enrichment.
- `README.md` — dokumentasi project.

---

# ✅ Kesimpulan

Project ini melakukan dua proses utama, yaitu **Data Cleansing** dan **Data Enrichment**.

Pada proses **Data Cleansing**, dilakukan standarisasi terhadap nama anggota, program studi, status, penerbit, dan tanggal pinjam. Selain itu, data kosong diperiksa dan ditangani, serta data duplikat diidentifikasi dan dihapus.

Pada proses **Data Enrichment**, ditambahkan informasi baru berupa **Kelompok Usia Buku** yang diperoleh dari `Tahun Terbit` dengan tahun acuan 2026.

Dengan tahapan tersebut, dataset dapat menjadi lebih **terstruktur, konsisten, dan memiliki informasi tambahan** yang dapat digunakan untuk analisis selanjutnya.

---

## 👨‍💻 Author

**Farma Ardan**

2418061

Mahasiswa Teknik Informatika  
Institut Teknologi Nasional Malang
