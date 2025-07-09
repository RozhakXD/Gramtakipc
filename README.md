# Fuzzy Logic: Segmentasi Potensi Ekonomi Regional

## Latar Belakang

Penilaian potensi ekonomi suatu wilayah adalah masalah kompleks yang dipengaruhi oleh banyak faktor yang tidak pasti dan bersifat linguistik. Proyek ini mengimplementasikan **Fuzzy Inference System (FIS)** untuk memodelkan ketidakpastian ini dan mengklasifikasikan potensi ekonomi 17 kabupaten/kota di Provinsi Sulawesi Tenggara ke dalam kategori **Rendah**, **Sedang**, atau **Tinggi**.

Tujuan utamanya adalah untuk membandingkan dua metode defuzzifikasi fundamental dalam logika fuzzy: **Mamdani** dan **Sugeno**, serta menganalisis perbedaan karakteristik output dari keduanya pada studi kasus nyata.

## 📂 Struktur Proyek

```text
../FuzzyEconomicPotential
├── data
│   ├── processed
│   │   └── cleaned_data_segmentasi_ekonomi.csv
│   └── raw
│       └── raw_data_segmentasi_ekonomi.csv
├── images
│   └── perbandingan_mamdani_sugeno.png
├── notebooks
│   └── Main_FuzzyLogic_Segmentasi_ipynb.ipynb
└── README.md
```

## 🛠️ Metodologi

Sistem ini dibangun menggunakan Python dengan library `scikit-fuzzy`.

1. **Variabel & Fuzzifikasi**
    Empat variabel input digunakan untuk menentukan potensi ekonomi:
   
   * Input:
     
     1. `Upah Minimum Kabupaten/Kota (UMK)`    
     2. `Indeks Pembangunan Manusia (IPM)` 
     3. `Tingkat Pengangguran Terbuka (TPT)`   
     4. `Jumlah Penduduk`  
   
   * Output:
     
     1. `Potensi Ekonomi` (Skor 0-100)

Setiap variabel dipetakan ke dalam himpunan fuzzy (misal: `Rendah`, `Sedang`, `Tinggi`) menggunakan fungsi keanggotaan berbentuk segitiga dan trapesium.

2. **Rule Base**
   Sebanyak 15 aturan `IF-THEN` dirancang berdasarkan pengetahuan umum untuk menjadi dasar pengambilan keputusan. Contoh:
   
   * **Aturan Ideal:** `IF IPM adalah Tinggi AND UMK adalah Tinggi AND TPT adalah Rendah THEN Potensi Ekonomi adalah Tinggi`
   * **Aturan Pesimis:** `IF IPM adalah Rendah AND UMK adalah Rendah AND TPT adalah Tinggi THEN Potensi Ekonomi adalah Rendah`

3. **Inferensi & Defuzzifikasi**
   Dua sistem kontrol dibangun dan dibandingkan:
* **Mamdani:** Menghitung output berdasarkan **pusat massa (centroid)** dari gabungan area fuzzy yang diaktifkan oleh aturan.
* **Sugeno (Orde Nol):** Menghitung output sebagai **rata-rata terbobot** dari nilai-nilai konstanta yang telah ditetapkan untuk setiap kategori output (misal: Rendah=25, Sedang=50, Tinggi=75).

## 📊 Hasil & Analisis

Kedua metode berhasil mengklasifikasikan daerah sesuai logika aturan. Namun, perbandingan keduanya menyoroti perbedaan fundamental yang menjadi temuan utama proyek ini.

#### Analisis Perbandingan

1. **Karakteristik Output:**
   
   * **Mamdani** menghasilkan skor yang lebih **halus dan bervariasi**, merefleksikan interaksi kompleks antar aturan. Ini cocok untuk analisis yang membutuhkan gradasi.
   * **Sugeno** menghasilkan skor yang lebih **tegas dan terpusat** pada nilai output yang telah ditentukan, menjadikannya lebih cepat dan cocok untuk sistem kontrol diskrit.

2. **Studi Kasus Divergensi:** Perbedaan paling signifikan terlihat pada **Kota Kendari** (Mamdani: `48.89` Rendah, Sugeno: `75.00` Tinggi). Ini **bukanlah error**, melainkan bukti bahwa:
   
   * Pada **Sugeno**, aktivasi kuat dari satu aturan `Tinggi` sudah cukup untuk "menarik" skor akhir secara drastis ke atas.
   * Pada **Mamdani**, pengaruh aturan `Tinggi` yang sama diseimbangkan oleh aturan-aturan `Rendah` lain yang mungkin juga aktif, sehingga pusat massa area gabungannya tetap rendah.

3. **Temuan Utama:** Tidak ada satu pun daerah yang mencapai klasifikasi 'Tinggi' pada metode Mamdani. Ini mengindikasikan bahwa, berdasarkan empat variabel yang digunakan, tidak ada daerah yang memenuhi semua kriteria ideal secara bersamaan.

## 🚀 Cara Menjalankan Proyek

1. **Clone Repositori**
   
   ```bash
   git clone https://github.com/RozhakDev/FuzzyEconomicPotential.git
   cd FuzzyEconomicPotential
   ```

2. **Buka Notebook**
   Buka file `notebooks/Main_FuzzyLogic_Segmentasi.ipynb` menggunakan Jupyter Notebook atau Google Colab.

3. **Jalankan Semua Sel**
   Notebook ini dirancang untuk dijalankan secara berurutan dari atas ke bawah. Library yang dibutuhkan akan diinstal secara otomatis pada sel pertama.

## 📄 Lisensi

Proyek ini dirilis di bawah lisensi MIT. Silakan lihat file [LICENSE](LICENSE) untuk detail lebih lanjut.

---

Terima kasih telah mengunjungi repositori ini! Jika Anda memiliki pertanyaan, saran, atau ingin berkontribusi, silakan buka _issue_ atau _pull request_. Semoga proyek ini bermanfaat untuk riset dan pengembangan sistem berbasis logika fuzzy di bidang ekonomi regional.