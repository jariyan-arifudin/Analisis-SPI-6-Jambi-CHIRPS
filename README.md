# Analisis Kekeringan Meteorologis (SPI-6) Provinsi Jambi

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini berisi implementasi kode Python untuk menghitung **Standardized Precipitation Index 6-Bulanan (SPI-6)** di Provinsi Jambi menggunakan data CHIRPS v3.0.

SPI-6 mengukur anomali curah hujan dalam periode 6 bulan. Indeks ini sangat berguna untuk memonitor ketersediaan air pada skala musim tanam (*agricultural drought*) dan potensi kekeringan hidrologis awal (debit sungai/air tanah dangkal).

## Metodologi: Temporal Buffering

Perhitungan SPI-6 membutuhkan akumulasi curah hujan selama 6 bulan berturut-turut.
* Contoh: SPI bulan **Juni** membutuhkan data hujan **Januari s.d. Juni**.
* Contoh: SPI bulan **Januari** membutuhkan data hujan **Agustus s.d. Desember (tahun lalu) + Januari (tahun ini)**.

Maka, script ini menerapkan mekanisme **Temporal Buffering** otomatis:
1.  Saat memproses tahun `N`, script memuat data tahun `N-1`.
2.  Perhitungan SPI-6 dilakukan secara *seamless* lintas tahun.
3.  Hasil akhir dipotong (*slicing*) agar sesuai dengan tahun target.

## Klasifikasi Kekeringan

Nilai SPI diklasifikasikan menjadi 5 kelas tingkat kekeringan berdasarkan standar BMKG/WMO:

| Nilai SPI | Kelas Kekeringan | Label |
| :--- | :--- | :--- |
| $\geq$ -1.00 | **Tidak Ada Kekeringan** | 1 (Biru) |
| -0.99 s.d 0.99 | **Mendekati Normal** | 2 (Hijau Muda) |
| -1.00 s.d -1.49 | **Agak Kering** | 3 (Kuning) |
| -1.50 s.d -1.99 | **Kering** | 4 (Oranye) |
| $\leq$ -2.00 | **Sangat Kering** | 5 (Merah) |

*> Catatan: Dalam script ini, nilai SPI positif (Basah) disederhanakan menjadi satu kelas "Tidak Ada Kekeringan" karena fokus studi adalah bencana kekeringan.*

## Struktur Direktori Data

Script ini dirancang untuk berjalan di **Google Colab** dengan struktur folder Google Drive sebagai berikut:

```text
/My Drive/Colab Notebooks/Skripsi/
├── Batas Adm Jambi/            # Shapefile Batas Provinsi
├── CHIRPS v3/
│   └── bias_corrected_regression/
│       └── downscaled_1km_metric/  # Input: Raster CHIRPS 1km
└── SPI6-Output/                # Output: Folder hasil (Otomatis dibuat)

```

## Prasyarat Instalasi

Script membutuhkan pustaka geospasial Python:

```bash
pip install rioxarray xarray geopandas rasterio scipy leafmap

```

## Cara Penggunaan

1. Pastikan data CHIRPS 1km tersedia.
2. Jalankan script `analisis_spi6.ipynb`.
3. Script akan memproses data per periode secara otomatis.
4. Hasil peta (`.tif` dan `.shp`) tersimpan di folder `SPI6-Output`.

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Analisis Kekeringan Meteorologis (SPI-6) Provinsi Jambi*. GitHub Repository.
