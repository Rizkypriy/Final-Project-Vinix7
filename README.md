# VINIX7 – Career Consultation Data Engineering

## 1. Project Overview

Proyek ini merupakan kegiatan pengolahan data konsultasi karier VINIX7 yang berfokus pada proses Data Engineering. Tujuannya adalah menghasilkan dataset yang lebih terstruktur, konsisten, dan siap digunakan untuk tahap analisis data selanjutnya.

Proses pengolahan mencakup Data Profiling, Data Quality Assessment, Data Cleaning, Data Filtering, Data Transformation, Data Validation, dan Data Documentation.

## 2. Dataset Overview

Dataset awal memiliki karakteristik sebagai berikut:

| Description     | Value |
| --------------- | ----: |
| Total Rows      | 2,975 |
| Total Columns   |    39 |
| Integer Columns |    18 |
| Object Columns  |    21 |

Berdasarkan pemeriksaan awal, ditemukan beberapa permasalahan seperti missing value, penggunaan placeholder, variasi penulisan kategori, dan ketidakkonsistenan format data.

Setelah proses Data Cleaning dan Filtering, dataset akhir terdiri dari **2.916 responden**.

Sebanyak 59 responden dikeluarkan karena memiliki missing value pada salah satu dari tiga kolom wajib, yaitu `Perguruan Tinggi`, `Program Studi`, dan `Dream Job`.

## 3. Data Engineering Process

### 3.1 Data Profiling

Melakukan pemeriksaan awal terhadap struktur dan karakteristik dataset, meliputi:

- Jumlah baris dan kolom.
- Tipe data setiap kolom.
- Statistik deskriptif.
- Missing value.
- Duplikasi data.
- Variasi nilai kategorikal.

Hasil pemeriksaan menunjukkan bahwa `Total Score` memiliki rata-rata 68,03, nilai minimum 24, dan maksimum 98 (sebelum baris missing value pada pergururan tinggi dan program studi dihapus).

### 3.2 Data Quality Assessment

Melakukan penilaian kualitas data berdasarkan beberapa aspek:

| Aspect       | Findings                                                                  |
| ------------ | ------------------------------------------------------------------------- |
| Completeness | Missing value pada EMAIL INFO PROGRAM, Kode Referensi, dan Tempat Magang. |
| Uniqueness   | Tidak ditemukan duplikasi baris secara keseluruhan.                       |
| Consistency  | Ditemukan spasi berlebih dan variasi penulisan pada beberapa kolom.       |
| Accuracy     | Form survei digunakan sebagai referensi untuk memeriksa kesesuaian data dengan jawaban responden. |

Timestamp yang memiliki nilai sama tidak menunjukkan bahwa respondennya sama, sehingga tidak digunakan sebagai dasar penghapusan data.

### 3.3 Data Cleaning

Proses Data Cleaning dilakukan untuk memperbaiki format, konsistensi, dan kelengkapan data.

Aktivitas yang dilakukan:

- Mengubah tipe data `Timestamp` menjadi datetime.
- Menangani placeholder dan nilai kosong pada `Kode Referensi` dan `Tempat Magang`.
- Standardisasi nama perguruan tinggi.
- Standardisasi nama program studi.
- Standardisasi kategori `Dream Job`.
- Menangani missing value pada `EMAIL INFO PROGRAM` dengan nilai `False` sesuai definisi data.
- Standardisasi urutan pilihan pada `Bidang Minat` dan `Work Values`.

Standardisasi dilakukan menggunakan mapping untuk mencocokkan nilai awal dengan nilai hasil cleaning.

### 3.4 Data Filtering and Transformation

Setelah proses cleaning, dilakukan filtering berdasarkan tiga kolom wajib, yaitu `Perguruan Tinggi`, `Program Studi`, dan `Dream Job`. Sebanyak 59 responden dikeluarkan sehingga jumlah data menjadi 2.916 responden.

Selanjutnya, dilakukan beberapa proses transformasi:

**Respondent_ID**

Membuat `Respondent_ID` sebagai identitas unik untuk setiap responden dan penghubung antar-dataset.

**Jenjang Pendidikan**

Menambahkan kolom `Jenjang Pendidikan` berdasarkan informasi pada kolom `Program Studi`.

**Label Dataset**

Delapan kolom label dibuat berdasarkan informasi survei, dimasukkan ke dataset utama, kemudian dipisahkan menjadi tabel tersendiri dengan nama `data_label.csv`.

**Long Format Transformation**

Kolom `Bidang Minat` dan `Work Values` yang berisi beberapa pilihan dalam satu sel diubah menjadi format long format. Setiap pilihan disimpan sebagai baris tersendiri dan dihubungkan menggunakan `Respondent_ID`.

## 4. Final Datasets

Hasil pengolahan menghasilkan lima dataset dalam format CSV.

| File                      |  Rows | Columns | Description                                                                       |
| ------------------------- | ----: | ------: | --------------------------------------------------------------------------------- |
| `data_main.csv`           | 2,916 |      41 | Dataset utama yang berisi informasi responden dan atribut utama.                  |
| `data_label.csv`          | 2,916 |       9 | Menyimpan delapan kolom label dan Respondent_ID.                                  |
| `data_bidang_minat.csv`   | 7,058 |       2 | Menyimpan pilihan bidang minat dalam format long format.                          |
| `data_work_values.csv`    | 8,335 |       2 | Menyimpan pilihan nilai kerja dalam format long format.                           |
| `data_backup_lengkap.csv` | 2,916 |      49 | Salinan lengkap dataset hasil pengolahan sebelum pemisahan menjadi beberapa file. |

### Dataset Explanation

**data_main.csv**

Dataset utama yang berisi informasi responden dan atribut utama. `Respondent_ID` digunakan sebagai identitas penghubung dengan dataset lainnya.

**data_label.csv**

Berisi delapan kolom label yang dibuat berdasarkan informasi survei dan dipisahkan dari dataset utama agar dapat dikelola secara terpisah.

**data_bidang_minat.csv**

Berisi pilihan bidang minat responden dalam format long format. Setiap pilihan disimpan pada baris tersendiri.

**data_work_values.csv**

Berisi pilihan nilai kerja responden dalam format long format. Struktur ini memudahkan pengelolaan setiap pilihan secara terpisah.

**data_backup_lengkap.csv**

Merupakan salinan lengkap dataset hasil pengolahan yang digunakan sebagai arsip dan referensi pemeriksaan kembali.

Seluruh dataset dihubungkan menggunakan `Respondent_ID`.

## 5. Data Documentation

Dokumentasi dibuat agar proses pengolahan dan perubahan data dapat ditelusuri kembali.

Dokumentasi terdiri dari:

| File                   | Description                                                                                   |
| ---------------------- | --------------------------------------------------------------------------------------------- |
| `Data Dictionary.docx` | Menjelaskan nama kolom, deskripsi, tipe data, dan karakteristik setiap variabel.              |
| `Log Cleaning.xlsx`    | Berisi ringkasan cleaning, mapping log, data yang memerlukan pemeriksaan, dan Data Issue Log. |

### Remaining Data Issues

Beberapa permasalahan belum dapat diselesaikan karena keterbatasan informasi:

- Masih terdapat beberapa singkatan nama perguruan tinggi yang belum dapat distandarisasi.
- Kolom `Jenjang Pendidikan` memiliki 2.792 missing value (95,75%) karena sebagian besar jenjang pendidikan tidak dapat diidentifikasi berdasarkan informasi `Program Studi`.

Nilai yang belum dapat dipastikan tetap dipertahankan tanpa imputasi atau perubahan yang tidak memiliki dasar.

## 6. Data Validation

Validasi dilakukan untuk memastikan struktur dan hubungan antar-dataset tetap konsisten.

Hasil validasi:

- Tidak ditemukan baris duplikat secara keseluruhan.
- Seluruh dataset memiliki 2.916 `Respondent_ID` unik.
- Kumpulan `Respondent_ID` konsisten pada seluruh dataset.
- Tidak ditemukan duplikasi kombinasi `Respondent_ID` dan `Bidang_Minat`.
- Tidak ditemukan duplikasi kombinasi `Respondent_ID` dan `Work_Value`.

Hasil tersebut menunjukkan bahwa hubungan antar-dataset telah sesuai dengan kebutuhan pengolahan data selanjutnya.

## 7. Project Structure

```text
VINIX7-Data-Engineering/
│
├── README.md
│
├── 01_Data_Source/
│   └── konsultasi_karir.csv
│
├── 02_Data_Processing/
│   └── notebook_data_cleaning.ipynb
│
├── 03_Dataset_Final/
│   ├── data_main.csv
│   ├── data_label.csv
│   ├── data_bidang_minat.csv
│   ├── data_work_values.csv
│   └── data_backup_lengkap.csv
│
├── 04_Documentation/
│   ├── Data Dictionary.docx
│   ├── Log Cleaning.xlsx
│   └── Laporan_Data_Engineer_VINIX7.pdf
```

## 8. Tools & Technologies

- Python
- Pandas
- Jupyter Notebook
- Microsoft Excel
- CSV

## 9. Conclusion

Proyek Data Engineering VINIX7 menghasilkan lima dataset dengan struktur yang lebih terorganisasi dan siap digunakan untuk tahap analisis data selanjutnya.

Proses pengolahan mencakup Data Profiling, Data Quality Assessment, Data Cleaning, Data Filtering, Data Transformation, Data Validation, dan Data Documentation.

Dataset akhir terdiri dari 2.916 responden dengan identitas yang konsisten pada seluruh file. Permasalahan yang belum dapat diverifikasi tetap dipertahankan dan didokumentasikan untuk menghindari perubahan data tanpa dasar yang jelas.

---

**Project:** VINIX7 Career Consultation Data Engineering
**Role:** Data Engineer
**Status:** Data Preparation Completed
