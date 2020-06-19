# Praktikum Pertemuan Ke 12 (UAS)

## Step 1 orkestrasi

Jadi, apa itu orkestrasi? Yah, orkestrasi mungkin paling baik digambarkan menggunakan contoh. Katakanlah Anda memiliki aplikasi yang memiliki lalu lintas tinggi bersama dengan persyaratan ketersediaan tinggi. Karena persyaratan ini, Anda biasanya ingin menggunakan setidaknya 3+ mesin, sehingga jika tuan rumah gagal, aplikasi Anda masih dapat diakses dari setidaknya dua lainnya. Jelas, ini hanya sebuah contoh dan kasus penggunaan Anda kemungkinan akan memiliki persyaratan sendiri, tetapi Anda mendapatkan idenya.

Menyebarkan aplikasi Anda tanpa Orkestrasi biasanya sangat memakan waktu dan rawan kesalahan, karena Anda harus secara manual SSH ke setiap mesin, mulai aplikasi Anda, dan kemudian terus mengawasi hal-hal untuk memastikan itu berjalan seperti yang Anda harapkan.

Tetapi, dengan Perkakas orkestrasi, Anda biasanya dapat melepas sebagian besar pekerjaan manual ini dan membiarkan otomatisasi melakukan pekerjaan berat. Salah satu fitur keren Orkestrasi dengan Docker Swarm, adalah Anda dapat menggunakan aplikasi di banyak host dengan hanya satu perintah (setelah mode Swarm diaktifkan).

## Step 2 konfigurasi mode gerombolan

Aplikasi dunia nyata biasanya digunakan di beberapa host seperti yang dibahas sebelumnya. Ini meningkatkan kinerja aplikasi dan ketersediaan, serta memungkinkan komponen aplikasi individu untuk skala secara mandiri. Docker memiliki alat asli yang tangguh untuk membantu Anda melakukan ini.

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%201.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%201.png)

Tapi, ini hanya pada satu node. Apa yang terjadi jika simpul ini turun? Nah, aplikasi kita baru saja mati dan tidak pernah restart. Untuk memulihkan layanan, kita harus masuk secara manual ke mesin ini, dan mulai mengubah hal-hal untuk membuatnya kembali dan berjalan. Jadi, akan sangat membantu jika kita memiliki beberapa jenis sistem yang memungkinkan kita menjalankan aplikasi / layanan "tidur" ini di banyak mesin.

Di bagian ini Anda akan mengkonfigurasi Mode Swarm . Ini adalah mode opsional baru di mana beberapa host Docker membentuk kelompok mesin yang mengatur sendiri disebut swarm . Mode Swarm memungkinkan fitur baru seperti layanan dan bundel yang membantu Anda menyebarkan dan mengelola aplikasi multi-kontainer di beberapa host Docker

## Step 2.1 buat simpul manager

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%203.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%204.png)

## Step 2.2 bergabung dengan node pekerja ke swarm

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%205.png)

## Step 3 menyebarkan aplikasi di beberapa host

Sekarang setelah Anda memiliki swarm up dan running, sekarang saatnya untuk menggunakan aplikasi tidur kami yang sangat sederhana.

## Step 3.1 menyebarkan komponen aplikasi sebagai layanan Docker

Aplikasi menjadi sangat populer di internet (karena memukul Reddit dan HN). Orang suka itu. Jadi, Anda harus mengukur aplikasi Anda untuk memenuhi permintaan puncak. Anda harus melakukan ini di beberapa host untuk ketersediaan tinggi juga. Kami akan menggunakan konsep Layanan untuk mengukur aplikasi kami dengan mudah dan mengelola banyak wadah sebagai satu kesatuan.

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%207.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%206.png)

Status layanan dapat berubah beberapa kali hingga berjalan. Gambar sedang diunduh dari Docker Store ke mesin lain di Swarm. Setelah gambar diunduh, wadah masuk ke keadaan berjalan di salah satu dari tiga node. Pada titik ini mungkin tidak tampak bahwa kami telah melakukan sesuatu yang sangat berbeda dari hanya menjalankan a docker run .... Kami sekali lagi menggunakan satu wadah pada satu host. Perbedaannya di sini adalah bahwa wadah telah dijadwalkan pada gugus gerombolan. Sudah selesai dilakukan dengan baik. Anda telah menyebarkan aplikasi tidur ke Swarm baru Anda menggunakan layanan Docker.

## Step 4 skala aplikasi

Salah satu hal hebat tentang layanan adalah Anda dapat menaikkan dan menurunkannya untuk memenuhi permintaan. Pada langkah ini Anda akan meningkatkan skala layanan dan kemudian kembali.

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%208.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%209.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2010.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2011.png)

## Step 5 kuras simpul dan jadwalkan ulang wadah

Aplikasi tidur Anda telah melakukan yang luar biasa setelah mencapai Reddit dan HN. Sekarang nomor 1 di App Store! Anda telah meningkatkan selama liburan dan turun selama musim yang lambat. Sekarang Anda sedang melakukan pemeliharaan di salah satu server Anda sehingga Anda harus dengan anggun mengeluarkan server dari kerumunan tanpa mengganggu layanan kepada pelanggan Anda.

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2012.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2013.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2014.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2015.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2016.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2017.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2018.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2018.png)

## membersihkan

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2019.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2020.png)

![hasil](https://github.com/azizandika/tekn-cloud-computing/blob/master/minggu_12/gambar%2021.png)


