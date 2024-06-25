
# Laporan Final Project TKA

## Kelompok C8 Kelas B

**Anggota**
- Nathan Kho Pancras 5027231002
  
- Amoes Noland 5027231028
  
- Fico Simhanandi 5027231030

- Johanes Edward Nathanael 5027231067

- Muhammad Kenas Galeno Putra 5027231069

## I. Introduction

Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan project untuk mendeploy sebuah aplikasi Sentiment Analysis dengan komponen Backend menggunakan python.

Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?

Pada final project TKA ini, kita diminta untuk merancang sebuah arsitektur cloud yang akan men-deploy suatu aplikasi. Digital Ocean, Microsoft Azure, dan Local Virtual Machine merupakan opsi yang diberikan untuk merancang arsitektur ini, dan kelompok kami memilih Digital Ocean dikarenakan harganya yang relatif murah.

## II. Rancangan Arsitektur & Spesifikasi

**Rancangan Arsitektur**

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/RA.png)

**Spesifikasi Worker 1 (Application)**

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/frontendDO.png)

**Spesifikasi Worker 2 & 3 (Back-End)**

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/backendDO1.png)

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/backendDO2.png)

**Spesifikasi Database (MongoDB)**

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/mongodb.png)

**Tabel Spesifikasi**

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/R_Arsi.png)

## III. Langkah-langkah Implementasi & Konfigurasi

**Langkah-langkah awal**

- Membuat 3 *Droplets*, 1 untuk application dan 2 untuk Back-end di Digital Ocean

- Membuat Load-Balancer dan disambungkan ke 2 backend Droplets (Pastikan saat membuat loadbalancer, health-check diganti menjadi TCP)

- Terakhir, membuat Database MongoDB 

**Konfigurasi**

*Untuk satu droplet (Application):*
- `sudo apt-get install apache2`
- Konfigurasikan index.html beserta styles.css
- Gunakan command `systemctl restart apache2` untuk restart service apache2
- Akses web menggunakan IP yang ada di masing-masing droplet atau IP loadbalancer

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/frontend_1.png)

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/frontend_2.png)

*Untuk dua droplet (Back-End):*
- `sudo apt-get install python3` (ini kalau belum ada python3)
- install semua dependencies:
    - `sudo apt-get install python3-pip` (ini kalau belum ada pip)
    - `pip install flask`
    - `pip install flask_cors`
    - `pip install textblob`
    - `pip install pymongo`
    - `pip install gunicorn`
- Konfigurasikan sentiment_analysis.py
- Nyalakan menggunakan command `gunicorn -b 0.0.0.0:80 sentiment_analysis:app --daemon` agar dapat berjalan meskipun terminal dimatikan

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/backend_1.png)

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/isi_sentiment_nano.png)

*Untuk locustfile:*
- Deploy locustfile menggunakan command `locust -f locustfile.py --host http://(IP backend)`

## IV. Hasil Pengujian Endpoint setiap API

- Get /analyze (Menggunakan Web)

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/analyze.png)

- Get /history (Menggunakan Postman)

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/history.png)

## V. Hasil Pengujian dan Analisis Loadtesting Menggunakan Locust

**Important!**

Sebelum melakukan tes locust, selalu hapus isi dari database terlebih dahulu, disini menggunakan MongoDB Compass

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/mongocompass.png)

**Max RPS**
- Konfigurasi awal

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/RPS_tes.png)

- Hasil Max RPS

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/RPS_res.png)

Dilihat dari hasil diatas, maka Max RPS yang didapatkan berkisaran di sekitar **271** RPS

**Peak User Concurrency**

- 50 Spawnrate 60 Second

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/50UPS.png)

Mulai mengalami failure di 3950 User sehingga Peak Concurrency berada di sekitar **3950**

- 100 Spawnrate 60 Second

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/100UPS.png)

Mulai mengalami failure di 3500 User sehingga Peak Concurrency berada di sekitar **3500**

- 200 Spawnrate 60 Second

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/200UPS.png)

Mulai mengalami failure di 6400 User sehingga Peak Concurrency berada di sekitar **6400**

- 500 Spawnrate 60 Second

![github-small](https://github.com/PuroFuro/FP_TKA/blob/main/img/500UPS.png)

Mulai mengalami failure di 8500 User sehingga Peak Concurrency berada di sekitar **8500**

## VI. Kesimpulan & Saran

Setelah beberapa pengujian dan analisis, dapat disimpulkan bahwa Digital Ocean memberikan hasil yang memuaskan dengan harga yang relatif murah dibandingkan dengan Cloud Provider yang lain. Selebihnya, meskipun memilih service yang paling murah, website tetap dapat berjalan dengan lancar.

## VII. Revisi

Revisi yang dilakukan berupa mengganti pekerjaan worker, yakni dari 2 Application dan 1 Backend menjadi 2 Backend dan 1 Application. Hal ini terjadi dikarenakan saat dilakukan pengecekan, ditemukan bahwa bagian back-end memiliki workload yang lebih besar daripada frontend, sehinggan yang akan di-loadbalance merupakan backend. Hasil yang didapatkan jauh lebih bagus, seperti tidak adanya failure di Peak User Concurrency yang cukup besar (dapat dilihat di hasil pengujian).
