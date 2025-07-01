# Analisis Sentimen Ulasan Aplikasi Kopi Kenangan
Nama  : Soraya Indah Setiani

NIM   : M0722074
## Project Overview
Kopi Kenangan merupakan perusahaan asal Indonesia yang menjual produk utama berupa minuman kopi. Kopi susu gula aren sebagai menu andalan Kopi Kenangan. Selain itu, perusahaan ini juga menjual minuman non-kopi seperti susu, teh, dan sari, beserta makanan ringan seperti kukis dan roti. Kopi Kenangan meluncurkan aplikasi untuk mempermudah konsumen dalam membeli produknya. Konsumen dapat menggunakan aplikasi Kopi Kenangan untuk membeli produk tanpa harus datang ke gerai, sehingga menghemat waktu dan terhindar dari antre. Lebih dari 800 gerai tersebar, aplikasi ini membantu konsumen menemukan gerai terdekat dari lokasinya. 

Aplikasi Kopi Kenangan dapat diunduh melalui Google Play Store (https://play.google.com/store/apps/details?id=com.kopikenangan). Aplikasi ini telah diunduh lebih dari 1 juta pengguna. Berdasarkan penilaian rating dan ulasan yang terlihat pada Google Play Store, aplikasi ini memperoleh rating 4.9 dan terdapat 142000 ulasan.

Untuk mengetahui bagaimana sentimen konsumen terhadap aplikasi Kopi Kenangan, perlu dilakukan analisis sentimen agar mengetahui apakah aplikasi ini memiliki penilaian positif, netral, atau negatif di mata konsumen.

## Pengambilan Data
Data yang digunakan adalah data ulasan aplikasi Kopi Kenangan di Google Play Store. Pengambilan data dilakukan dengan _scraping_ dengan jumlah maksimum data yang diambil adalah 142000 data.

    # Proses Scraping
    scrapreview = reviews_all(
    'com.kopikenangan',      # ID aplikasi
    lang='id',               # Bahasa ulasan (default: 'en')
    country='id',            # Negara (default: 'us')
    sort=Sort.MOST_RELEVANT, # Urutan ulasan (default: Sort.MOST_RELEVANT)
    count=142000             # Jumlah maksimum ulasan yang ingin diambil
    ) 
## Data Loading
Data hasil scraping memiliki 24837 baris dan 11 variabel. Setelah itu, mengidentifikasi dan membersihkan _missing value_ dan duplikasi data.

## Pre-Processing Text
Tahapan ini dilakukan pada variabel content yang mana berisi ulasan pengunjung. Terdapat beberapa hal yang dilakukan, yaitu.
* Menghapus mention, hashtag, RT, link, angka, karakter selain huruf & angka.
* Mengganti baris baru dengan spasi, menghapus tanda baca, dan karakter spasi dari kiri & kanan.
* Case Folding: Mengubah semua karakter dalam teks menjadi kecil (bukan kapital).
* Tokenizing: Memecah atau membagi string, teks menjadi daftar token.
* Kata-kata yang tidak mengandung arti, tidak menunjukkan sentimen, dan berjumlah banyak, perlu dijadikan stopwords kemudian dihapus.
* Stemming: Mengurangi kata ke bentuk dasarnya yang menghilangkan imbuhan awalan dan akhiran atau ke akar kata.
* Mengganti slangwords menjadi kata yang baku.

## Pelabelan
Pelabelan dilakukan dengan membaca data kamus kata-kata positif dan negatif dari GitHub. Setelah itu, mendistribusikan sentimen. Berikut hasil distribusi sentimen:

        polarity
        positive    18818
        negative     2378
        Name: count, dtype: int64
Terlihat bahwa  aplikasi Kopi Kenangan cenderung positif. Sebanyak 88.8% pengguna aplikasi memberi penilaian positif yang artinya mereka puas dengan aplikasi ini.

## Eksplorasi Data
Word Cloud menunjukkan bebrapa kata yang menonjol, yaitu "promo", "enak", "bagus", "suka", "oke", "keren", "mudah", "keren". Kata-kata tersebut menunjukkan bahwa pengguna aplikasi senang dengan promo yang diberikan, puas terhadap fitur dan pelayanan aplikasi yang memudahkan, menyukai menu yang telah dibeli.

## Data Splitting dan Ekstraksi Fitur
### Data Splitting (80:20) dan Ekstraksi Fitur dengan TF-IDF
Berikut tahapan yang dilakukan.
* Data Splitting: 80% data _training_ dan 20% data _testing_
* Ekstraksi Fitur dengan TF-IDF
* Menerapkan algoritma Naive Bayes Random Forest, Regresi Logistik, dan Decision Tree.
### Data Splitting (80:20) dan Ekstraksi Fitur dengan Word2Vec
Berikut tahapan yang dilakukan.
* Data Splitting: 80% data _training_ dan 20% data _testing_
* Ekstraksi Fitur dengan Word2Vec
* Menerapkan algoritma Naive Bayes Random Forest, Regresi Logistik, dan Decision Tree.
### Data Splitting (70:30) dan Ekstraksi Fitur dengan TF-IDF
Berikut tahapan yang dilakukan.
* Data Splitting: 70% data _training_ dan 30% data _testing_
* Ekstraksi Fitur dengan TF-IDF
* Menerapkan algoritma Naive Bayes Random Forest, Regresi Logistik, dan Decision Tree.
### Data Splitting (70:30) dan Ekstraksi Fitur dengan TF-IDF
Berikut tahapan yang dilakukan.
* Data Splitting: 70% data _training_ dan 30% data _testing_
* Ekstraksi Fitur dengan Word2Vec
* Menerapkan algoritma Naive Bayes Random Forest, Regresi Logistik, dan Decision Tree.

## Kesimpulan
Berikut ringkasan akurasi data testing pada setiap algoritma.
    
    | Model             | TF-IDF(80:20)         | Word2Vec(80:20)      | TF-IDF(70:30)         | Word2Vec(70:30)      |
    |-------------------|-----------------------|----------------------|-----------------------|----------------------|
    | Naive Bayes       | 0.8882075471698113    | 0.6674528301886793   | 0.8886617392671804    | 0.6713319704356031   |
    | Random Forest     | 0.9415094339622642    | 0.9235849056603773   | 0.9403994338732505    | 0.9237301462494103   |
    | Regresi Logistik  | 0.9459905660377359    | 0.9113207547169812   | 0.9441736122031766    | 0.9091052052209467   |
    | Decision Tree     | 0.9363207547169812    | 0.8924528301886793   | 0.9275043245793364    | 0.8900770561409027   |
* Ekstraksi fitur menggunakan metode TF-IDF menghasilkan akurasi yang lebih tinggi dibandingkan dengan Word2Vec.
* Model terbaik untuk analisis sentimen aplikasi Kopi Kenangan adalah Regresi Logistik dan Random Forest.
* Menggunakan Regresi Logistik dan Random Forest, ekstraksi TF-IDF, serta pembagian data 80:20 dapat disimpulkan bahwa aplikasi Kopi Kenangan memiliki sentimen positif dengan akurasi > 94%

