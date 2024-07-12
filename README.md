# PA-PC_202231017_KEVIN-NUGRAHA-SANTIKA-PERMANA_C
# Laporan Praktikum UAS Pengolahan Citra Digital

Nama  : Kevin Nugraha Santika Permana

NIM   : 202231017

Kelas : C
# A. Teori yang Mendukung Pada Project

Proyek terkait pengolahan citra digital ini melibatkan beberapa teori dasar dalam bidang pengolahan citra, yang mencakup konversi ruang warna, deteksi tepi, dan pencarian kontur. Berikut adalah penjelasan dari teori-teori yang mendukung:

### 1. Konversi Ruang Warna
Konversi ruang warna adalah proses mengubah representasi warna suatu gambar dari satu sistem koordinat warna ke sistem koordinat warna lain. Dalam konteks ini, konversi dilakukan dari ruang warna BGR (Blue, Green, Red) yang digunakan oleh OpenCV ke ruang warna RGB (Red, Green, Blue) yang lebih umum digunakan dalam visualisasi.

**Teori yang Mendukung:**

**Ruang Warna BGR vs. RGB**

 Dalam BGR, urutan penyimpanan piksel adalah Blue, Green, Red, sedangkan dalam RGB urutan penyimpanannya adalah Red, Green, Blue. Konversi ini penting untuk memastikan gambar ditampilkan dengan warna yang benar pada perangkat yang menggunakan ruang warna RGB.

### 2. Deteksi Tepi (Edge Detection)
Deteksi tepi adalah teknik dalam pengolahan citra yang digunakan untuk mengidentifikasi titik-titik dalam gambar di mana intensitas cahaya berubah tajam. Metode yang digunakan dalam proyek ini adalah algoritma Canny.

**Teori yang Mendukung:**

**Algoritma Canny**

Algoritma Canny adalah metode deteksi tepi yang populer karena mampu mendeteksi tepi yang kuat dan mengurangi noise. Algoritma ini bekerja melalui beberapa tahap: 
  1. **Gaussian Blur**: Menghaluskan gambar untuk mengurangi noise.
  2. **Gradient Calculation**: Menghitung gradien intensitas untuk mendeteksi tepi.
  3. **Non-maximum Suppression**: Menghilangkan respons tepi yang tidak maksimum.
  4. **Double Threshold**: Menggunakan dua ambang batas untuk mengekstrak tepi yang kuat dan lemah.
  5. **Edge Tracking by Hysteresis**: Menentukan tepi akhir berdasarkan hasil dari double threshold.

### 3. Pencarian Kontur (Contour Detection)
Pencarian kontur adalah proses untuk mendeteksi dan menganalisis bentuk objek dalam gambar. Kontur adalah kurva yang menghubungkan semua titik kontinu (sepanjang batas), memiliki intensitas warna yang sama.

**Teori yang Mendukung**

**Metode Pencarian Kontur**

OpenCV menyediakan fungsi `findContours` untuk mendeteksi kontur dalam gambar biner. Fungsi ini mengembalikan daftar kontur yang ditemukan, yang dapat digunakan untuk analisis lebih lanjut atau visualisasi.
  - **RETR_EXTERNAL**: Mode ini mengambil hanya kontur eksternal.
  - **CHAIN_APPROX_SIMPLE**: Menyederhanakan kontur dengan menghilangkan titik-titik redundan.

### 4. Visualisasi
Visualisasi dalam pengolahan citra digunakan untuk menampilkan hasil pengolahan gambar dengan cara yang mudah dipahami oleh manusia. Dalam proyek ini, hasil deteksi tepi dan kontur divisualisasikan menggunakan matplotlib.

**Teori yang Mendukung:**
- **Matplotlib**: Matplotlib adalah pustaka plotting di Python yang digunakan untuk membuat grafik 2D. Dalam konteks pengolahan citra, matplotlib digunakan untuk menampilkan gambar asli, hasil deteksi tepi, dan hasil pencarian kontur dalam satu plot untuk analisis komparatif.

## B. Tahapan Pembuatan Projek

### 1. Membaca Gambar dari File

Langkah pertama dalam pengolahan citra digital adalah membaca gambar dari file. Pada tahap ini, kita menggunakan pustaka OpenCV, yang menyediakan fungsi `cv2.imread()` untuk membaca gambar dari berbagai format file (seperti JPEG, PNG, dll). Fungsi ini mengembalikan gambar dalam bentuk array numpy, di mana setiap elemen array mewakili intensitas warna dari piksel gambar.

**Source Code:** 
```python
import cv2

# Membaca gambar dari file
img = cv2.imread('kevin.jpg')
```

**Penjelasan:**
- `cv2.imread('kevin.jpg')`: Membaca file gambar 'kevin.jpg' dan mengembalikan array numpy yang berisi data piksel gambar. Jika gambar tidak ditemukan, fungsi ini akan mengembalikan `None`.
- Gambar yang dibaca oleh OpenCV secara default disimpan dalam format BGR (Blue, Green, Red).

### 2. Mengubah Ukuran Gambar
Ukuran gambar yang berbeda-beda dapat menyebabkan inkonsistensi dalam pengolahan lebih lanjut. Oleh karena itu, mengubah ukuran gambar menjadi ukuran standar diperlukan. Dalam kasus ini, kita mengubah ukuran gambar sehingga tinggi gambar menjadi 1400 piksel sambil mempertahankan rasio aspek asli untuk menghindari distorsi.

**Source Code:** 
```python
# Mengubah ukuran gambar menjadi tinggi 1400 piksel
height = 1400
aspect_ratio = height / img.shape[0]
new_width = int(img.shape[1] * aspect_ratio)
img_resized = cv2.resize(img, (new_width, height))
```

**Penjelasan:**
- `height = 1400`: Menetapkan tinggi gambar yang diinginkan menjadi 1400 piksel.
- `aspect_ratio = height / img.shape[0]`: Menghitung rasio aspek berdasarkan tinggi baru dan tinggi asli gambar.
- `new_width = int(img.shape[1] * aspect_ratio)`: Menghitung lebar baru gambar berdasarkan rasio aspek dan lebar asli.
- `cv2.resize(img, (new_width, height))`: Mengubah ukuran gambar ke dimensi baru dengan mempertahankan rasio aspek.

### 3. Konversi Gambar ke Warna RGB

OpenCV menggunakan format warna BGR secara default, yang berbeda dari format RGB yang umum digunakan dalam visualisasi dan manipulasi gambar lainnya. Konversi ini diperlukan untuk menampilkan gambar dengan warna yang benar menggunakan pustaka lain seperti matplotlib, yang menggunakan format RGB.

**Source Code:** 
```python
# Konversi gambar ke warna RGB
img_rgb = cv2.cvtColor(img_resized, cv2.COLOR_BGR2RGB)
```

**Penjelasan:**
- `cv2.cvtColor(img_resized, cv2.COLOR_BGR2RGB)`: Mengonversi gambar dari format BGR ke format RGB. Fungsi ini sangat penting untuk memastikan bahwa warna gambar tampil dengan benar pada saat visualisasi.

### 4. Deteksi Tepi Menggunakan Algoritma Canny

Deteksi tepi adalah teknik untuk mengidentifikasi batas-batas objek dalam gambar. Algoritma Canny adalah salah satu metode deteksi tepi yang paling populer dan efektif. Algoritma ini terdiri dari beberapa langkah: penghalusan gambar dengan Gaussian blur, perhitungan gradien, penekanan non-maksimum, ambang batas ganda, dan pelacakan tepi dengan histeresis.

**Source Code:** 
```python
# Menggunakan Canny Edge Detection
edges = cv2.Canny(img_resized, 100, 150)
```

**Penjelasan:**
- `cv2.Canny(img_resized, 100, 150)`: Menerapkan deteksi tepi Canny pada gambar yang telah diubah ukurannya. Parameter pertama adalah gambar input, parameter kedua dan ketiga adalah nilai ambang bawah dan ambang atas untuk deteksi tepi. Algoritma Canny akan mendeteksi tepi yang kuat dan lemah berdasarkan nilai ambang ini.

### 5. Pencarian Kontur

Kontur adalah kurva yang menghubungkan semua titik yang memiliki intensitas warna yang sama dan kontinu. Pencarian kontur digunakan untuk mendeteksi dan menganalisis bentuk objek dalam gambar. Fungsi `cv2.findContours()` digunakan untuk menemukan kontur dalam gambar biner yang dihasilkan dari deteksi tepi.

**Source Code:** 
```python
# Mencari kontur dari hasil deteksi tepi
contours, _ = cv2.findContours(edges, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

# Menggambar kontur pada gambar asli dengan warna hijau (RGB: 0, 255, 0)
img_contours = img_rgb.copy()
cv2.drawContours(img_contours, contours, -1, (0, 255, 0), 2)  # Warna kontur hijau dengan ketebalan 2 piksel
```

**Penjelasan:**
- `cv2.findContours(edges, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)`: Mencari kontur dalam gambar biner. `cv2.RETR_EXTERNAL` mengambil hanya kontur eksternal, sementara `cv2.CHAIN_APPROX_SIMPLE` mengurangi jumlah titik dalam kontur dengan menghapus titik-titik yang redundan.
- `cv2.drawContours(img_contours, contours, -1, (0, 255, 0), 2)`: Menggambar kontur yang ditemukan pada gambar asli yang telah dikonversi ke RGB. Kontur digambar dengan warna hijau dan ketebalan garis 2 piksel.

### 6. Visualisasi Hasil

Visualisasi hasil sangat penting untuk memahami dan menganalisis output dari setiap tahap pengolahan citra. Dalam proyek ini, kita menggunakan matplotlib untuk menampilkan gambar asli, hasil deteksi tepi, dan hasil pencarian kontur dalam satu plot.

**Source Code:** 
```python
import matplotlib.pyplot as plt

# Menggunakan tampilan matplotlib
fig, axs = plt.subplots(1, 3, figsize=(20, 10))

# Tampilan asli
axs[0].imshow(img_rgb)
axs[0].set_title("Original Image")

# Tampilan edges
axs[1].imshow(edges, cmap='gray')
axs[1].set_title("Canny Edge Detection")

# Tampilan kontur
axs[2].imshow(img_contours)
axs[2].set_title("Contours Detection")

# Menampilkan hasil
plt.show()
```

**Penjelasan:**
- `fig, axs = plt.subplots(1, 3, figsize=(20, 10))`: Membuat subplot dengan satu baris dan tiga kolom untuk menampilkan tiga gambar secara berdampingan.
- `axs[0].imshow(img_rgb)`: Menampilkan gambar asli dalam format RGB.
- `axs[0].set_title("Original Image")`: Menambahkan judul untuk subplot pertama.
- `axs[1].imshow(edges, cmap='gray')`: Menampilkan hasil deteksi tepi dalam skala abu-abu.
- `axs[1].set_title("Canny Edge Detection")`: Menambahkan judul untuk subplot kedua.
- `axs[2].imshow(img_contours)`: Menampilkan gambar dengan kontur yang digambar.
- `axs[2].set_title("Contours Detection")`: Menambahkan judul untuk subplot ketiga.
- `plt.show()`: Menampilkan plot yang telah dibuat.

