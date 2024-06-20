
# Laporan Final Project TKA

## Kelompok C8 Kelas B

## I. Introduction

Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan project untuk mendeploy sebuah aplikasi Sentiment Analysis dengan komponen Backend menggunakan python.

Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?

Pada final project TKA ini, kita diminta untuk merancang sebuah arsitektur cloud yang akan men-deploy suatu aplikasi. Digital Ocean, Microsoft Azure, dan Local Virtual Machine merupakan opsi yang diberikan untuk merancang arsitektur ini, dan kelompok kami memilih Digital Ocean dikarenakan harganya yang relatif murah.

## II. Rancangan Arsitektur & Spesifikasi

**Rancangan Arsitektur**

img

**Spesifikasi Worker 1 & 2**

img

**Spesifikasi Back-End**

img

**Spesifikasi Database (MongoDB)**

img

**Tabel Spesifikasi**

img

## III. Langkah-langkah Implementasi & Konfigurasi

**Langkah-langkah awal**

- Membuat 3 *Droplets*, 2 untuk application dan 1 untuk Back-end di Digital Ocean

- Membuat Load-Balancer dan disambungkan ke 2 application Droplets

- Terakhir, membuat Database MongoDB 

**Konfigurasi**

*Untuk dua droplet (Application):*
- `sudo apt-get install apache2`
- Konfigurasikan index.html beserta styles.css
- Gunakan command `systemctl restart apache2` untuk restart service apache2
- Akses web menggunakan IP yang ada di masing-masing droplet atau IP loadbalancer

img

*Untuk satu droplet (Back-End):*
- `sudo apt-get install python3` (ini kalau belum ada python3)
- install semua dependencies:
    - `sudo apt-get install python3-pip` (ini kalau belum ada pip)
    - `pip install flask`
    - `pip install flask_cors`
    - `pip install textblob`
    - `pip install pymongo`
    - `pip install gunicorn`
- Konfigurasikan sentiment_analysis.py
- Nyalakan menggunakan command `gunicorn -b 0.0.0.0:5000 sentiment_analysis:app --daemon` agar dapat berjalan meskipun terminal dimatikan

img

*Untuk locustfile:*
- Deploy locustfile menggunakan command `locust -f locustfile.py --host http://(IP backend):5000`

img

## IV. Hasil Pengujian Endpoint setiap API

- Get all orders

img

- Get a Specific Order by ID

img

- Create a New Order

img

- Update an Order by ID

img

- Delete an Order by ID

img 

## V. Hasil Pengujian dan Analisis Loadtesting Menggunakan Locust

- 100 User 25 Spawnrate 60 Second

img

- 100 User 50 Spawnrate 60 Second

img

- 100 User 100 Spawnrate 60 Second

img

## VI. Kesimpulan & Saran

Setelah beberapa pengujian dan analisis, dapat disimpulkan bahwa Digital Ocean memberikan hasil yang memuaskan dengan harga yang relatif murah dibandingkan dengan Cloud Provider yang lain. Selebihnya, meskipun memilih service yang paling murah, website tetap dapat berjalan dengan lancar.
