# Penjelasan dan Kesimpulan dari Kode 21 Cell

## 1. **Import Library**
Kode ini memuat berbagai library yang dibutuhkan, seperti `os`, `pandas`, `numpy`, hingga pustaka `torch` untuk deep learning. Library seperti `cv2` digunakan untuk pengolahan gambar, sedangkan `torchvision` menyediakan alat bantu untuk transformasi gambar.

---

## 2. **Membaca Dataset**
- Direktori gambar ditentukan (`dataset/train`).
- Dataset berbentuk CSV (`train.csv`) dibaca menggunakan `pandas`.
- Informasi dataset ditampilkan untuk memahami struktur data.

---

## 3. **Ekstraksi Data Gambar**
- Gambar dari direktori dibaca, diubah dari format BGR ke RGB, dan diubah ukurannya menjadi 244x244 piksel.
- Gambar diratakan menjadi vektor 1 dimensi dan disimpan dalam format string.
- ID gambar diekstraksi dari nama file.
- Data dikumpulkan ke dalam DataFrame `df` dengan kolom `id` dan `pixels`.

---

## 4. **Menghitung Duplikasi**
Mengidentifikasi jumlah data duplikat pada kolom `pixels` untuk memastikan kualitas dataset.

---

## 5. **Menampilkan Data Duplikat**
Menampilkan baris yang memiliki duplikasi pada kolom `pixels` untuk investigasi lebih lanjut.

---

## 6. **Mengumpulkan ID Duplikat**
Daftar ID gambar yang memiliki nilai duplikat pada kolom `pixels` dikumpulkan untuk kemungkinan pembersihan data.

---

## 7. **Menganalisis Jenis Pakaian**
- Dataset diperiksa berdasarkan kolom `jenis`.
- Jumlah pakaian dari masing-masing kategori dihitung, lalu labelnya diterjemahkan ke kategori manusiawi (`Kaos` dan `Hoodie`).

---

## 8. **Visualisasi Jenis Pakaian**
- Data distribusi kategori pakaian divisualisasikan menggunakan `barplot` dari Seaborn.
- Warna yang konsisten (`Blues`) digunakan untuk memperjelas grafik.

---

## 9. **Menganalisis Warna Pakaian**
- Kolom `warna` dalam dataset diperiksa.
- Label warna diterjemahkan menjadi nama warna manusiawi (`merah`, `kuning`, dll.).
- Data dihitung berdasarkan frekuensi kemunculan.

---

## 10. **Visualisasi Warna Pakaian**
- Distribusi warna pakaian divisualisasikan menggunakan `barplot`.
- Grafik memberikan gambaran jumlah pakaian berdasarkan warna.

---

## 11. **Fungsi Mencari Path File**
Fungsi `find_file_path` dibuat untuk mencari path file gambar berdasarkan ID. Fungsi ini memeriksa beberapa ekstensi file gambar seperti `.jpg`, `.png`, dan `.jpeg`.

---

## 12. **Membuat Dataset Kustom**
- Kelas `ClothingDataset` dibuat untuk menangani dataset gambar dengan:
  - Memuat data dari CSV.
  - Menyesuaikan label (`jenis` dan `warna`) dengan data yang disediakan.
  - Memungkinkan transformasi gambar.

---

## 13. **Membagi Dataset Training dan Validasi**
- Dataset dipecah menjadi training (80%) dan validasi (20%).
- Transformasi gambar dilakukan seperti resize, normalisasi, dan konversi ke tensor.
- Data dimuat menggunakan `DataLoader` untuk mempermudah pelatihan model.

---

## 14. **Membuat Model ViT (Vision Transformer)**
- Model `MultiLabelViT` dibangun menggunakan ViT pre-trained dari `transformers`.
- Model memiliki dua kepala klasifikasi:
  - **Jenis Pakaian**: Klasifikasi biner.
  - **Warna Pakaian**: Klasifikasi multi-kelas.

---

## 15. **Melihat Jumlah Parameter Model**
Menghitung jumlah total parameter dan parameter yang dapat dioptimasi untuk memahami kompleksitas model.

---

## 16. **Fungsi Evaluasi Model**
- Evaluasi dilakukan menggunakan:
  - **Akurasi**: Untuk jenis dan warna pakaian.
  - **Exact Match Ratio (EMR)**: Mengukur kecocokan kedua label secara bersamaan.
  - **Precision-Recall Curve**: Menentukan threshold optimal.

---

## 17. **Proses Pelatihan Model**
- Model dilatih dengan kombinasi loss:
  - `BCEWithLogitsLoss` untuk jenis pakaian.
  - `CrossEntropyLoss` untuk warna pakaian.
  - Penalti untuk EMR.
- Model dievaluasi pada setiap epoch untuk memantau performa.

---

## 18. **Menyimpan Model Terlatih**
Model terlatih disimpan ke file `.pth` agar dapat digunakan kembali tanpa melatih ulang.

---

## 19. **Memuat Model Terlatih**
Model yang telah disimpan dimuat kembali untuk prediksi pada data baru atau evaluasi tambahan.

---

## 20. **Membuat CSV untuk Data Uji**
Fungsi `create_test_csv` dibuat untuk menghasilkan file CSV kosong berdasarkan direktori data uji, dengan kolom `jenis` dan `warna` diisi dengan nilai default.

---

## 21. **Prediksi Data Uji**
- Fungsi `predict_and_save` digunakan untuk:
  - Melakukan prediksi jenis dan warna pakaian.
  - Menyimpan hasil prediksi ke dalam file CSV.
- Threshold optimal diterapkan untuk meningkatkan akurasi.

---

## **Kesimpulan**
Kode ini bertujuan untuk membangun sistem multi-label classification menggunakan Vision Transformer (ViT). Sistem ini mampu mengklasifikasikan **jenis pakaian** dan **warna pakaian** dari dataset gambar. Proses meliputi:
1. **Persiapan Data**: Membaca, membersihkan, dan memvisualisasikan dataset.
2. **Pelatihan Model**: Melatih model ViT dengan evaluasi performa pada data validasi.
3. **Prediksi Data Uji**: Memprediksi label untuk data baru dan menyimpannya dalam format CSV.

Hasil akhir berupa model yang dapat digunakan untuk mengklasifikasikan gambar pakaian secara otomatis dengan performa yang dapat diukur menggunakan akurasi dan metrik lainnya.