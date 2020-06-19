# Application Containerization and Microservice Orchestration

Dalam tutorial ini kita akan belajar tentang kontainerisasi aplikasi dasar menggunakan Docker dan menjalankan berbagai komponen aplikasi sebagai layanan microser. Kami akan menggunakan Docker Compose untuk orkestrasi selama pengembangan. Tutorial ini ditargetkan untuk pemula yang memiliki pengetahuan dasar dengan Docker. Jika Anda baru menggunakan Docker, kami sarankan Anda untuk memeriksa tutorial Docker untuk Pemula terlebih dahulu.

Kami akan mulai dari skrip Python dasar yang mengikis tautan dari laman web tertentu dan secara bertahap mengubahnya menjadi tumpukan aplikasi multi-layanan. Kode demo tersedia di repo Link Extractor . Kode ini disusun dalam langkah-langkah yang secara bertahap memperkenalkan perubahan dan konsep baru. Setelah selesai, tumpukan aplikasi akan berisi layanan microser berikut:

- Aplikasi web yang ditulis dalam PHP dan disajikan menggunakan Apache yang mengambil URL sebagai input dan merangkum tautan yang diekstrak darinya
- Aplikasi web berbicara ke server API yang ditulis dengan Python (dan Ruby) yang menangani ekstraksi tautan dan mengembalikan respons JSON
- Cache Redis yang digunakan oleh server API untuk menghindari pengambilan berulang dan ekstraksi tautan untuk halaman yang sudah dikikis

## Pengaturan Panggung

Mari kita mulai dengan kloning repositori kode demo, mengubah direktori kerja, dan memeriksa democabang keluar.

## Langkah 0: Skrip Extractor Tautan Dasar

Periksa step0cabang dan daftar file di dalamny

## Langkah 1: Script Extractor Tautan Kontainer

Dengan ini Dockerfilekita bisa menyiapkan gambar Docker untuk skrip ini. Kita mulai dari pythongambar Docker resmi yang berisi lingkungan run-time Python serta alat yang diperlukan untuk menginstal paket dan dependensi Python. Kami kemudian menambahkan beberapa metadata sebagai label (langkah ini tidak penting, tetapi merupakan praktik yang baik). Berikutnya dua instruksi menjalankan pip installperintah untuk menginstal dua paket pihak ketiga yang diperlukan agar skrip berfungsi dengan baik. Kami kemudian membuat direktori yang berfungsi /app, menyalin linkextractor.pyfile di dalamnya, dan mengubah izinnya untuk membuatnya menjadi skrip yang dapat dieksekusi. Akhirnya, kami menetapkan skrip sebagai titik masuk untuk gambar.

## Langkah 2: Tautkan Modul Extractor dengan URI Lengkap dan Anchor Text

Sejauh ini, kami telah mempelajari cara membuat wadah naskah dengan dependensi yang diperlukan untuk membuatnya lebih portabel. Kami juga telah belajar cara membuat perubahan dalam aplikasi dan membangun berbagai varian gambar Docker yang dapat hidup berdampingan. Pada langkah berikutnya kita akan membangun layanan web yang akan memanfaatkan skrip ini dan akan membuat layanan berjalan di dalam wadah Docker.

## Langkah 3: Layanan Link Extractor API

Karena kita sudah mulai menggunakan requirements.txtdependensi, kita tidak perlu lagi menjalankan pip installperintah untuk masing-masing paket. The ENTRYPOINTdirektif diganti dengan CMDdan itu mengacu pada main.pynaskah yang memiliki kode server itu karena kita tidak ingin menggunakan gambar ini untuk satu-off perintah sekarang.

Kemudian jalankan wadah dalam mode terlepas ( -dbendera) sehingga terminal tersedia untuk perintah lain saat wadah masih berjalan. Perhatikan bahwa kami memetakan port 5000wadah dengan 5000host (menggunakan -p 5000:5000argumen) untuk membuatnya dapat diakses dari host. Kami juga menetapkan nama ( --name=linkextractor) ke wadah untuk membuatnya lebih mudah untuk melihat log dan membunuh atau menghapus wadah.

## Langkah 4: Link Extractor API dan Layanan Front Web

Pada langkah ini perubahan berikut telah dilakukan sejak langkah terakhir:

- Layanan tautan extractor JSON API (ditulis dengan Python) dipindahkan dalam ./apifolder terpisah yang memiliki kode yang sama persis seperti pada langkah sebelumnya
- Aplikasi front-end web ditulis dalam PHP di bawah ./wwwfolder yang berbicara dengan API JSON
- Aplikasi PHP dipasang di dalam php:7-apachegambar Docker resmi untuk modifikasi yang lebih mudah selama pengembangan
- Aplikasi web dapat diakses di http://<hostname>[:<prt>]/?url=<url-encoded-url>
- Variabel lingkungan API_ENDPOINTdigunakan di dalam aplikasi PHP untuk mengkonfigurasinya untuk berbicara dengan server JSON API
- Sebuah docker-compose.ymlfile ditulis untuk membangun berbagai komponen dan lem mereka bersama-sama

Pada langkah ini kami berencana untuk menjalankan dua wadah terpisah, satu untuk API dan yang lainnya untuk antarmuka web. Yang terakhir membutuhkan cara untuk berbicara dengan server API. Agar kedua kontainer dapat saling berbicara, kita dapat memetakan porta mereka pada mesin host dan menggunakannya untuk meminta perutean atau kita dapat menempatkan kontainer dalam satu jaringan pribadi dan mengakses secara langsung. Docker memiliki dukungan jaringan yang sangat baik dan memberikan perintah yang bermanfaat untuk menangani jaringan. Selain itu, dalam jaringan Docker, wadah mengidentifikasi diri mereka menggunakan nama mereka sebagai nama host untuk menghindari perburuan alamat IP mereka di jaringan pribadi. Namun, kami tidak akan melakukan hal ini secara manual, sebaliknya kami akan menggunakan Docker Compose untuk mengotomatiskan banyak tugas ini.

## Langkah 5: Layanan Redis untuk Caching

Beberapa perubahan nyata dari langkah sebelumnya adalah sebagai berikut:

- Lain Dockerfileditambahkan dalam ./wwwfolder untuk aplikasi web PHP untuk membangun gambar mandiri dan menghindari pemasangan file langsung
- Wadah Redis ditambahkan untuk caching menggunakan gambar Redis Docker resmi
- Layanan API berbicara dengan layanan Redis untuk menghindari mengunduh dan mem-parsing halaman yang sudah dikerik sebelumnya
- Sebuah REDIS_URLvariabel lingkungan ditambahkan ke layanan API untuk memungkinkan untuk terhubung ke cache Redis

## Langkah 6: Tukar Layanan API Python dengan Ruby

Beberapa perubahan signifikan dari langkah sebelumnya meliputi:

- Layanan API yang ditulis dengan Python diganti dengan implementasi Ruby yang serupa
- Itu API_ENDPOINT variabel lingkungan diperbarui ke titik ke layanan API Ruby baru
- Kejadian cache ekstraksi tautan (HIT / MISS) dicatat dan dipertahankan menggunakan volume

## Kesimpulan

Kami memulai tutorial ini dengan skrip Python sederhana yang mengikis tautan dari URL laman web pemberian. Kami menunjukkan berbagai kesulitan dalam menjalankan skrip. Kami kemudian mengilustrasikan betapa mudahnya menjalankan dan membuat skrip menjadi mudah dibawa kemas. Pada langkah selanjutnya kami secara bertahap mengembangkan skrip menjadi tumpukan aplikasi multi-layanan. Dalam proses tersebut kami mengeksplorasi berbagai konsep arsitektur layanan mikro dan bagaimana alat Docker dapat membantu dalam menyusun tumpukan multi-layanan. Akhirnya, kami mendemonstrasikan kemudahan pertukaran komponen layanan mikro dan kegigihan data.