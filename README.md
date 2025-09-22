## Latar Belakang

Dalam era digital saat ini, transaksi keuangan semakin sering dilakukan secara elektronik melalui berbagai kanal seperti online banking, ATM, maupun aplikasi mobile. Peningkatan volume data transaksi ini membuka peluang besar bagi institusi keuangan untuk menganalisis perilaku nasabah secara lebih mendalam.

Analisis transaksi memungkinkan perusahaan memahami pola konsumsi, preferensi layanan, serta karakteristik demografis nasabah. Dengan pendekatan ini, lembaga keuangan dapat melakukan segmentasi pelanggan secara lebih tepat, sehingga strategi pemasaran dan pengembangan layanan dapat disesuaikan dengan kebutuhan tiap kelompok.

Clustering sebagai salah satu metode *unsupervised learning* mampu memberikan segmentasi otomatis berdasarkan kesamaan perilaku transaksi, sehingga membantu lembaga keuangan dalam mengembangkan strategi pemasaran, layanan personalisasi, serta sistem deteksi dini terhadap potensi penipuan.

## Objektif
1. **Mengeksplorasi dataset transaksi keuangan** untuk memahami pola dan distribusi data, baik numerik maupun kategorikal.
2. **Melakukan pembersihan dan pra-pemrosesan data** termasuk penanganan nilai hilang, duplikasi, outlier, serta normalisasi dan encoding fitur.
3. **Membangun model clustering (K-Means)** untuk mengelompokkan nasabah berdasarkan perilaku transaksi dan karakteristik akun.
4. **Menginterpretasikan hasil cluster** guna memberikan insight strategis, seperti segmentasi pelanggan, rekomendasi produk/layanan, dan potensi penggunaan lebih lanjut pada deteksi fraud.
5. **Menyediakan dasar analisis deskriptif** yang dapat digunakan sebagai acuan dalam pengambilan keputusan berbasis data oleh institusi keuangan.

## Informasi Dataset

Dataset ini menyediakan informasi mengenai perilaku transaksi dan pola aktivitas keuangan dengan **2.512 sampel data transaksi**. Fitur-fitur utama dataset meliputi:
- **`TransactionID`**: Pengidentifikasi unik alfanumerik untuk setiap transaksi.
- **`AccountID`**: ID unik untuk setiap akun.
- **`TransactionAmount`**: Nilai transaksi dalam mata uang.
- **`TransactionDate`**: Tanggal dan waktu transaksi.
- **`TransactionType`**: Tipe transaksi (`'Credit'` atau `'Debit'`).
- **`Location`**: Lokasi geografis transaksi.
- **`DeviceID`**: ID perangkat yang digunakan.
- **`IP Address`**: Alamat IPv4.
- **`MerchantID`**: ID unik merchant.
- **`AccountBalance`**: Saldo akun setelah transaksi.
- **`PreviousTransactionDate`**: Tanggal transaksi terakhir.
- **`Channel`**: Kanal transaksi (`Online`, `ATM`, `Branch`).
- **`CustomerAge`**: Usia pemilik akun.
- **`CustomerOccupation`**: Profesi pengguna.
- **`TransactionDuration`**: Lama waktu transaksi (dalam detik).
- **`LoginAttempts`**: Jumlah upaya login sebelum transaksi.

Dataset ini ideal untuk deteksi penipuan dan identifikasi anomali.

## Load Dataset dan Exploratory Data Analysis (EDA)

Setelah memuat dataset, dilakukan eksplorasi awal untuk memahami karakteristik data:
- **`df.head()`**: Menampilkan 5 baris pertama untuk melihat format data.
- **`df.info()`**: Memberikan ringkasan dataset, termasuk jumlah entri non-null dan tipe data setiap kolom. Ditemukan bahwa terdapat nilai-nilai yang hilang di hampir semua kolom.
- **`df.describe()`**: Menampilkan statistik deskriptif fitur numerik (mean, min, max, std, dll.). Ini memberikan gambaran tentang sebaran nilai pada kolom-kolom numerik.

EDA lebih lanjut dilakukan melalui visualisasi:
- **Heatmap Korelasi**: Menunjukkan hubungan antar fitur numerik.
- **Histogram**: Menampilkan distribusi setiap fitur numerik.
- **Scatterplot dan Boxplot**: Menjelajahi hubungan spesifik antar variabel seperti `TransactionAmount` vs `AccountBalance` dan distribusi `TransactionAmount` per `TransactionType`.
- **Countplot**: Menampilkan distribusi fitur kategorikal seperti `TransactionType`, `Channel`, `CustomerOccupation`, dan `Location`. 

## _Data Cleaning & Preprocessing_

Sebelum clustering, data dibersihkan dan diproses:
- **`isnull().sum()`**: Konfirmasi jumlah nilai yang hilang per kolom.
- **`duplicated().sum()`**: Mengecek jumlah baris duplikat.
- **Feature Scaling**: Fitur numerik dinormalisasi menggunakan `MinMaxScaler()` untuk membawa nilai ke rentang 0-1.
- **Feature Encoding**: Fitur kategorikal diubah menjadi representasi numerik menggunakan `LabelEncoder()`.
- **Drop Kolom ID dan IP Address**: Kolom pengidentifikasi unik yang tidak relevan untuk clustering dihapus.
- **Penanganan Nilai Hilang**: Baris dengan nilai yang hilang dihapus (`dropna()`).
- **Penghapusan Data Duplikat**: Baris duplikat dihapus (`drop_duplicates()`).
- **Penanganan Outlier**: Outlier pada fitur numerik ditangani menggunakan metode capping.

## Membangun Model Clustering dan Interpretasi Hasil

Model K-Means dibangun pada data yang telah diproses.
- **Elbow Method**: Digunakan untuk menentukan jumlah cluster optimal (misalnya, 4 cluster seperti yang terlihat pada visualisasi).
- **K-Means Clustering**: Data dikelompokkan menjadi 4 cluster. Kolom 'Cluster' ditambahkan ke DataFrame yang diproses.

Untuk menginterpretasikan cluster, analisis deskriptif dilakukan pada setiap cluster menggunakan fungsi agregasi (mean, min, max, median, std, count) untuk fitur numerik.

### Interpretasi Karakteristik Tiap Cluster (setelah inverse transform pada fitur numerik dan kategorikal):

Analisis ini berdasarkan data yang telah dikembalikan ke skala dan kategori aslinya untuk interpretasi yang lebih intuitif.

- **Cluster 0: (Saldo Tinggi, Durasi Transaksi Sedang, Usia Menengah)**
    - Ciri: Saldo akun relatif tinggi, jumlah transaksi kecil, durasi transaksi sedang, usia menengah.
    - Analisis: Nasabah cenderung menyimpan saldo besar dan bertransaksi moderat.
    - Rekomendasi: Tawarkan layanan tabungan premium atau investasi jangka menengah.

- **Cluster 1: (Nasabah Usia Lebih Tua dengan Saldo Menengah)**
    - Ciri: Rata-rata usia lebih tua, saldo akun menengah, transaksi moderat.
    - Analisis: Nasabah cenderung lebih konservatif, tidak menyimpan saldo terlalu besar, transaksi stabil.
    - Rekomendasi: Tawarkan produk pensiun, proteksi keuangan, atau paket layanan loyalitas.

- **Cluster 2: (Nasabah Muda dengan Saldo Rendah)**
    - Ciri: Berusia muda, saldo akun rendah, jumlah dan durasi transaksi mirip cluster lain.
    - Analisis: Nasabah baru memulai hubungan dengan layanan keuangan.
    - Rekomendasi: Dorong penggunaan produk entry-level, promosi cashback.

- **Cluster 3: (Durasi Transaksi Tinggi, Saldo Menengah)**
    - Ciri: Durasi transaksi paling lama, saldo akun menengah, usia menengah.
    - Analisis: Melakukan transaksi yang lebih kompleks atau sering menggunakan layanan digital lebih lama.
    - Rekomendasi: Tingkatkan pengalaman digital banking, percepat proses transaksi online, tawarkan layanan prioritas.

Interpretasi ini memberikan _insight_ tentang segmen nasabah yang berbeda berdasarkan perilaku transaksi dan karakteristik demografis, yang dapat digunakan untuk strategi pemasaran dan layanan yang ditargetkan.
