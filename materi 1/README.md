# Beginner linux

Di lab ini, kita akan melihat beberapa perintah Docker dasar dan alur kerja build-ship-run sederhana. Kami akan mulai dengan menjalankan beberapa wadah sederhana, lalu kami akan menggunakan Dockerfile untuk membangun aplikasi khusus. Terakhir, kita akan melihat cara menggunakan bind mounts untuk memodifikasi wadah yang sedang berjalan seperti yang Anda lakukan jika Anda secara aktif mengembangkan menggunakan Docker.

## Klon Repo GitHub Lab

Gunakan perintah berikut untuk mengkloning repo lab dari GitHub (Anda dapat mengklik perintah atau mengetiknya secara manual). Ini akan membuat salinan repo laboratorium di sub-direktori baru bernama.

## Tugas 1: Jalankan beberapa wadah Docker sederhana

Ada berbagai cara untuk menggunakan wadah. Ini termasuk:

Untuk menjalankan satu tugas: Ini bisa berupa skrip shell atau aplikasi khusus.
Interaktif: Ini menghubungkan Anda ke wadah yang mirip dengan cara Anda SSH ke server jauh.
Di latar belakang: Untuk layanan jangka panjang seperti situs web dan basis data.
Di bagian ini Anda akan mencoba masing-masing opsi tersebut dan melihat bagaimana Docker mengelola beban kerja.

## Jalankan satu tugas dalam wadah Alpine Linux
Pada langkah ini kita akan memulai sebuah wadah baru dan memerintahkannya untuk menjalankan hostnameperintah. Wadah akan mulai, jalankan hostnameperintah, lalu keluar.

1. Jalankan perintah berikut di konsol Linux Anda

2. Docker menjaga wadah berjalan selama proses itu dimulai di dalam wadah masih berjalan. Dalam hal ini hostnameproses keluar segera setelah output ditulis. Ini artinya wadah berhenti. Namun, Docker tidak menghapus sumber daya secara default, sehingga wadah masih ada di Exitednegara.

## Jalankan wadah Ubuntu interaktif
Anda dapat menjalankan sebuah wadah berdasarkan versi Linux yang berbeda dari yang dijalankan pada host Docker Anda.

Dalam contoh berikut, kita akan menjalankan wadah Ubuntu Linux di atas host Alpine Linux Docker (Play With Docker menggunakan Alpine Linux untuk node-node-nya).

1. Jalankan wadah Docker dan akses cangkangnya.

2. Jalankan perintah berikut dalam wadah.

ls /akan mencantumkan isi direktur root dalam wadah, ps auxakan menunjukkan proses yang berjalan dalam wadah, cat /etc/issueakan menunjukkan distro Linux mana wadah berjalan, dalam hal ini Ubuntu 18.04.3 LTS.

3. Ketik exituntuk meninggalkan sesi shell. Ini akan menghentikan bashproses, menyebabkan wadah untuk keluar.

4. Untuk bersenang-senang, mari kita periksa versi host VM kami.

## Jalankan latar belakang wadah MySQL

Kontainer latar belakang adalah cara Anda menjalankan sebagian besar aplikasi. Berikut ini contoh sederhana menggunakan MySQL.

1. Jalankan wadah MySQL baru dengan perintah berikut.

2. Daftar wadah yang sedang berjalan.

3. Anda dapat memeriksa apa yang terjadi di wadah Anda dengan menggunakan beberapa perintah Docker bawaan: docker container logsdan docker container top.

4. Daftar versi MySQL menggunakan docker container exec.

docker container execmemungkinkan Anda untuk menjalankan perintah di dalam sebuah wadah. Dalam contoh ini, kita akan gunakan docker container execuntuk menjalankan setara baris perintah mysql --user=root --password=$MYSQL_ROOT_PASSWORD --versiondi dalam wadah MySQL kita.

5. Anda juga dapat menggunakan docker container execuntuk terhubung ke proses shell baru di dalam wadah yang sudah berjalan. Menjalankan perintah di bawah ini akan memberi Anda shell interaktif ( sh) di dalam wadah MySQL Anda.

6. Mari kita periksa nomor versi dengan menjalankan perintah yang sama lagi, hanya kali ini dari dalam sesi shell baru di wadah.

7. Ketik exituntuk meninggalkan sesi shell interaktif.

## Tugas 2: Paket dan menjalankan aplikasi khusus menggunakan Docker

## Bangun gambar situs web sederhana
Mari kita lihat Dockerfile yang akan kita gunakan, yang membangun situs web sederhana yang memungkinkan Anda mengirim tweet.

1. Pastikan Anda berada di linux_tweet_appdirektori.

2. Tampilkan konten Dockerfile.

3. Untuk membuat perintah berikut ini lebih ramah salin / tempel, ekspor variabel lingkungan yang berisi DockerID Anda (jika Anda tidak memiliki DockerID, Anda bisa mendapatkannya secara gratis melalui Docker Hub ).

4. Gema nilai variabel kembali ke terminal untuk memastikan itu disimpan dengan benar.

5. Gunakan docker image buildperintah untuk membuat gambar Docker baru menggunakan instruksi di Dockerfile.

6. Gunakan docker container runperintah untuk memulai wadah baru dari gambar yang Anda buat

7. Klik di sini untuk memuat situs web yang seharusnya berjalan.

8. Setelah Anda mengakses situs web Anda, matikan dan hapus.

## Tugas 3: Memodifikasi situs web yang sedang berjalan

Ketika Anda secara aktif mengerjakan aplikasi, tidak nyaman harus menghentikan wadah, membangun kembali gambar, dan menjalankan versi baru setiap kali Anda membuat perubahan pada kode sumber Anda.

Salah satu cara untuk merampingkan proses ini adalah dengan memasang direktori kode sumber pada mesin lokal ke wadah yang sedang berjalan. Ini akan memungkinkan setiap perubahan yang dilakukan pada file pada host segera tercermin dalam wadah.

Kami melakukan ini menggunakan sesuatu yang disebut bind mount .

Saat Anda menggunakan bind mount, file atau direktori pada mesin host dipasang ke sebuah wadah yang berjalan di host yang sama.

## Mulai aplikasi web kami dengan bind mount

1. Mari mulai aplikasi web dan pasang direktori saat ini ke dalam wadah. Dalam contoh ini kita akan menggunakan --mountbendera untuk memasang direktori saat ini di host ke /usr/share/nginx/htmldalam wadah. Pastikan untuk menjalankan perintah ini dari dalam linux_tweet_appdirektori pada host Docker Anda.

## Ubah situs web yang berjalan
Bind mounts berarti bahwa setiap perubahan yang dilakukan pada sistem file lokal segera tercermin dalam wadah yang berjalan.

1. Salin yang baru index.htmlke dalam wadah.

Repo Git yang Anda tarik sebelumnya berisi beberapa versi file index.html yang berbeda. Anda dapat secara manual menjalankan lsperintah dari dalam ~/linux_tweet_appdirektori untuk melihat daftar mereka. Pada langkah ini kita akan ganti index.htmldengan index-new.html.


2. Buka situs web yang sedang berjalan dan segarkan halaman . Perhatikan bahwa situs tersebut telah berubah.