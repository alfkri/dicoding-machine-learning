# Laporan Proyek Machine Learning - Muhammad Al Fikri 

## Project Overview
Pada proyek ini akan membahas tentang sebuah perusahaan yang bergerak di industri perfilman yang ingin meningkatkan *traffic platform film streaming* mereka, oleh karena itu perusahaan akan mencoba membuat sistem rekomendasi dengan menerapkan pendekatan *Machine Learning* untuk merekomendasi film-film yang mereka sediakan berdasarkan genre film untuk para customer mereka.  

Tujuan dirancangnya sebuah sistem rekomendasi adalah untuk memberikan rekomendasi yang sesuai dengan kebutuhan pengguna. Berdasarkan Jurnal Teknik Informatika dan Sistem Informasi Fakultas Teknologi Informasi, Universitas Kristen Maranatha menyatakan bahwa *"Sistem rekomendasi adalah fitur-fitur dan teknik-teknik pada perangkat lunak yang menyediakan sesuatu hal yang berguna untuk user. Sistem rekomendasi juga menyediakan rekomendasi-rekomendasi dari beberapa item yang berpotensi menarik untuk pengguna"*[[1]](https://www.neliti.com/id/publications/140821/penerapan-metode-content-based-filtering-pada-sistem-rekomendasi-kegiatan-ekstra). Hal ini menjadi penting mengingat tidak semua pengguna dapat menentukan pilihannya saat pertama kali menggunakan suatu aplikasi atau layanan.

### Latar belakang  
Film merupakan salah satu bentuk media komunikasi massa dari berbagai macam teknologi dan berbagai unsur-unsur kesenian.  Perpaduan yang seimbang dan harmonis antara seni sastra, seni musik, seni peran dan komedi dikemas menjadi satu dalam bentuk film[[2]](http://books.uinsby.ac.id/id/eprint/216/3/Yoyon%20Mudjiono_Kajian%20Semiotika%20dalam%20Film.pdf). Menonton film adalah alternatif hiburan yang sering dipilih ketika merasa penat atau bosan dengan rutinitas. Apalagi kini banyak tersedia *platform digital* dari yang gratis hingga berbayar yang menawarkan beragam genre film dan bisa ditonton di mana saja melalui ponsel.     

Salah satu faktor yang mempengaruhi seseorang untuk menonton film adalah melihat dari kategori genrenya. Contohnya orang yang menyukai film Avengers kemungkinan juga akan menyukai film Thor, karena kedua film tersebut memiliki genre yang sama yaitu *Action*. Oleh karena itu di proyek kali ini perusahaan akan membuat sebuah sistem rekomendasi menggunakan pendekatan *Machine Learning* guna meningkatkan *traffic platform* mereka.  

## Bussiness Understanding 

### Problem Statements  
Berdasarkan latar belakang di atas, rincian masalahnya adalah sebagai berikut:
- Model *Machine Learning* apa yang cocok untuk menyelesaikan permasalahan tersebut?
- Bagaimana cara menentukan hasil rekomendasi suatu model *Machine Learning* dapat dikatakan baik?

### Goals
Untuk menjawab pertanyaan di atas, maka akan dijabarkan sebagai berikut:  
- Model yang cocok untuk menyelesaikan masalah tersebut adalah model yang berbasis dengan konten atau biasa disebut *Content-Based Filtering*.
- Melakukan evaluasi terhadap metrik dari model *Machine Learning* tersebut.  

## Data Understanding
Berikut merupakan informasi dari dataset yang digunakan:

|           Jenis         |  Keterangan |
| ----------------------- | ----------- |
|           Sumber        | [Movie Recommender System Dataset](https://www.kaggle.com/datasets/gargmanas/movierecommenderdataset)|
| Pemilik | [SHINIGAMI](https://www.kaggle.com/gargmanas) |
|          Lisensi        | [GPL 2](http://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html) |
| Jenis dan Ukuran Berkas | zip (846KB) |   

Tabel 1. Informasi Dataset  

Pada berkas tersebut terdapat 2 file, yaitu movies.csv dan ratings.csv

### Deskripsi Variabel

 - **movies.csv**  
 ![image](https://user-images.githubusercontent.com/72861300/191751696-2c39e3bf-cdb1-4d78-8b2b-159cea2b80ed.png)  
 Gambar 1. Informasi variabel movies  
  Variabel-variabel yang terdapat pada file movies.csv adalah sebagai berikut:
	 - *movieId*: id film
	 - *title*: Judul film
	 - *genres*: genre film  

 - **ratings.csv**  
 ![image](https://user-images.githubusercontent.com/72861300/191753064-336ef509-f116-40d2-8728-127867e8bea2.png)  
 Gambar 2. Informasi variabel ratings    
 Variabel-variabel yang terdapat pada file ratings.csv adalah sebagai berikut:  
   - *userId*: id user
   - *movieId*: id film
   - *rating*: rating yang diberikan user
   - *timestamp*: waktu user memberikan rating  

### Exploratory Data Analysis - Univariate Analysis

 - **Movies**  
 ![unv-2](https://user-images.githubusercontent.com/72861300/191754798-c47faf4d-1429-43a4-962c-b18330766bc4.png)  
 Gambar 3. Distribusi fitur genre
 Dari hasil visualisasi pada Gambar 3 dapat disimpulkan bahwa:
   -   Sebagian besar sampel film dari dataset movies ber-genre *drama* dan *comedy*, hal tersebut menunjukkan bahwa film yang tersedia lebih banyak ber-genre  _drama_  dan  _comedy_.  
   
 - **Rating**
 ![unv-3](https://user-images.githubusercontent.com/72861300/191754808-adb2dcf3-6755-40ef-b64f-e5d181db7ae9.png)    
 Gambar 4. Visualisasi fitur numerik rating  
   Dari hasil visualisasi pada gambar 4 dapat disimpulkan bahwa:  
   - Rentang rating film adalah 0,5 hingga 5
   - Jumlah sampel terbanyak adalah film yang memiliki rating 4, hal ini menunjukkan bahwa banyak user yang menilai film dengan nilai 4.  

## Data Preparation  
Berikut merupakan tahapan-tahapan dalam melakukan data preparation:  
 -  *Menggabungkan Dataset dan Menangani Missing Value*  
Proses ini dilakukan dengan menggabungkan kedua dataset movies.csv dan ratings.csv menggunakan fungsi [*merge()*](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html) dan setelah digabungkan data yang memiliki nilai kosong/*null* akan dihapus menggunakan fungsi [*dropna()*](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.dropna.html) dengan tujuan agar mudah untuk diproses.  

 - *Menghapus Data Duplikat*  
Proses ini dilakukan dengan menggunakan fungsi [*drop_duplicates()*](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html) agar tidak ada data yang memiliki nilai sama untuk mencegah kekeliruan.   

 - *Mengonversi Data Series Menjadi Bentuk List*  
Proses ini dilakukan dengan menggunakan fungsi [tolist()](https://pandas.pydata.org/docs/reference/api/pandas.Series.tolist.html) agar data lebih mudah diproses pada tahap pemodelan.  

## Modeling  
Setelah data selesai disiapkan, proses selanjutnya adalah membuat model adapun tahap-tahapnya diantaranya sebagai berikut:  

 - *Melakukan Vektorisasi dengan TF-IDF*  
Pada tahap ini data yang telah disiapkan dikonversi menjadi bentuk vektor menggunakan fungsi [tfidfvectorizer()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) dari library sklearn untuk mengidentifikasi korelasi antara judul film dengan kategori genrenya.  
 - *Mengukur tingkat kesamaan dengan [Cosine Similarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html)*  
 Setelah data dikonversi menjadi bentuk vektor, selanjutnya ukur tingkat kesamaan antara dua vektor dan menentukan apakah kedua vektor tersebut menunjuk ke arah yang sama. Semakin kecil sudut cosinus, semakin besar nilai cosine similarity.  
 
 - *Membuat Fungsi movie_recommendations()*
Tahap terakhir dari proses modeling adalah membuat fungsi untuk mendapatkan hasil *top-N recommendation*, kali ini fungsinya dinamakan *movie_recommendations()*. Cara kerja dari fungsi ini yaitu menggunakan fungsi [argpartition](https://numpy.org/doc/stable/reference/generated/numpy.argpartition.html) untuk mengambil sejumlah nilai k tertinggi dari similarity data (dalam kasus ini: dataframe **cosine_sim_df**). Kemudian mengambil data dari bobot (tingkat kesamaan) tertinggi ke terendah. Data ini lalu dimasukkan ke dalam variabel closest. Berikutnya menghapus movie_title yang dicari menggunakan fungsi [drop()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html) agar tidak muncul dalam daftar rekomendasi.
 Penjelasan parameter dari fungsi *movie_recommendations()* adalah sebagai berikut:  
   - movie_title : Judul film (index kemiripan dataframe) (str)
   -  similarity_data : Kesamaan dataframe simetrik dengan judul film sebagai indeks dan kolom (object)
   -  items : Mengandung kedua nama dan fitur lainnya yang digunakan untuk mendefinisikan kemiripan (object)
   -  k : Banyaknya jumlah rekomendasi yang diberikan (int)  
  
### Result  
Setelah model selesai dibuat, panggil model untuk menampilkan hasil rekomendasi, sebagai contoh kita gunakan judul film *Piper (2016)* untuk menguji model.  


|    |   id   | movie_title  |   genre   |
|----|--------|--------------|-----------|
|2565| 160718 | Piper (2016) | Animation |  

Tabel 2. Informasi judul film uji  
 
Dapat terlihat pada Tabel 2 bahwa film *Piper (2016)* merupakan film dengan genre Animation.  Selanjutnya kita lihat rekomendasi film yang sesuai dengan genre yang sama dengan film tersebut.  


|| movie_title  |   genre   |
|--|--------------|-----------|
|0| A Plasticine Crow (1981) | Animation | 
|1| The Red Turtle (2016) | Animation | 
|2| The Monkey King (1964) | Animation | 
|3| Winter in Prostokvashino (1984) | Animation | 
|4| Vacations in Prostokvashino (1980) | Animation | 
|5| Garfield's Pet Force (2009) | Animation | 
|6| Nasu: Summer in Andalusia (2003) | Animation | 
|7| Three from Prostokvashino (1978) | Animation | 
|8| Investigation Held by Kolobki (1986) | Animation | 
|9| Fireworks, Should We See It from the Side or t... | Animation |    

Tabel 3. Hasil rekomendasi   

Seperti terlihat pada Tabel 3, model berhasil menampilkan rekomendasi film berdasarkan genrenya.  

## Evaluation 
Karena model yang digunakan untuk proyek kali ini adalah ***Content-Based Filtering***, maka metrik yang cocok untuk digunakan adalah *Precision*. Secara matematis dapat dirumuskan sebagai berikut:  
![img](https://dicoding-web-img.sgp1.cdn.digitaloceanspaces.com/original/academy/dos:819311f78d87da1e0fd8660171fa58e620211012160253.png)    
Gambar 5. Rumus *precision* sistem rekomendasi  

Berdasarkan hasil yang telah ditampilkan Tabel 3 pada bagian [*Result*](#result) dapat disimpulkan bahwa dari 10 judul film yang direkomendasikan, ada 10 film yang relevan oleh karena itu nilai *Precision* dari model ini adalah 10/10 atau 100%.  

## Conclusion  
Setelah melalui proses yang panjang, mulai dari mempersiapkan dataset hingga melakukan evaluasi, akhirnya sistem rekomendasi dengan pendekatan *Machine Learning Content-Based Filtering* pun selesai dirancang dan hasilnya pun cukup memuaskan yaitu dari 10 judul film yang direkomendasikan, terdapat 10 film yang relevan dengan judul film yang diuji yang menandakan *precision* dari model ini adalah 100%. Diharapkan dengan dirancangnya sistem rekomendasi ini, *traffic platform film streaming* perusahaan dapat naik dengan signifikan.

## Referensi  
[[1]](https://www.neliti.com/id/publications/140821/penerapan-metode-content-based-filtering-pada-sistem-rekomendasi-kegiatan-ekstra) Firmahsyah, Firmahsyah dan Tiur Gantini. "Penerapan Metode Content-Based Filtering Pada Sistem Rekomendasi Kegiatan Ekstrakulikuler (Studi Kasus Di Sekolah ABC)." _Jurnal Teknik Informatika dan Sistem Informasi_, vol. 2, no. 3, 2016, doi:[10.28932/jutisi.v2i3.548](https://dx.doi.org/10.28932/jutisi.v2i3.548).  

[[2]](http://books.uinsby.ac.id/id/eprint/216/3/Yoyon%20Mudjiono_Kajian%20Semiotika%20dalam%20Film.pdf) Mudjiono, Yoyon (2020) _Kajian semiotika dalam film._ Jurnal Ilmu Komunikasi, 1 (1). pp. 125-138. ISSN 2088-981X; 2723-2557  


[[3]](https://chaitanyabelhekar.medium.com/recommender-system-metrics-clearly-explained-1f2ba6690216)   Belhekar, Chaitanya . (2020, Agustus 29).  Recommender System Metrics — Clearly Explained. [https://chaitanyabelhekar.medium.com/recommender-system-metrics-clearly-explained-1f2ba6690216](https://chaitanyabelhekar.medium.com/recommender-system-metrics-clearly-explained-1f2ba6690216)
