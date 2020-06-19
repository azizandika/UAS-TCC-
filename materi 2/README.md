# Docker Networking Hands-on Lab

Di lab ini Anda akan belajar tentang konsep-konsep utama Docker Networking. Anda akan mendapatkan tangan Anda kotor dengan melihat contoh-contoh beberapa konsep jaringan dasar, belajar tentang jaringan Bridge, dan akhirnya Overlay networking.

# Bagian # 1 - Dasar-Dasar Jaringan

## Langkah 1: Perintah Jaringan Docker

Output perintah menunjukkan cara menggunakan perintah serta semua docker networksub-perintah. Seperti yang dapat Anda lihat dari output, docker networkperintah ini memungkinkan Anda untuk membuat jaringan baru, daftar jaringan yang ada, memeriksa jaringan, dan menghapus jaringan. Ini juga memungkinkan Anda untuk menghubungkan dan memutuskan wadah dari jaringan.

## Langkah 2: Daftar jaringan

## Langkah 3: Periksa jaringan

The docker network inspectPerintah ini digunakan untuk melihat rincian konfigurasi jaringan. Rincian ini meliputi; nama, ID, driver, driver IPAM, info subnet, wadah terhubung, dan banyak lagi.

Gunakan docker network inspect <network>untuk melihat detail konfigurasi jaringan kontainer pada host Docker Anda. Perintah di bawah ini menunjukkan perincian jaringan yang disebut bridge.

## Langkah 4: Daftar plugin driver jaringan

The docker infoperintah menunjukkan banyak informasi menarik tentang instalasi Docker.

Jalankan docker infoperintah dan temukan daftar plugin jaringan.

# Bagian # 2 - Jembatan Jaringan

## Langkah 1: Dasar-Dasarnya
Setiap instalasi Docker yang bersih dilengkapi dengan jaringan yang disebut jembatan . Verifikasi ini dengan docker network ls.

Output di atas menunjukkan bahwa jaringan jembatan dikaitkan dengan driver jembatan . Penting untuk dicatat bahwa jaringan dan driver terhubung, tetapi tidak sama. Dalam contoh ini jaringan dan driver memiliki nama yang sama - tetapi mereka bukan hal yang sama!

Output di atas juga menunjukkan bahwa jaringan jembatan dicakup secara lokal. Ini berarti bahwa jaringan hanya ada pada host Docker ini. Ini berlaku untuk semua jaringan yang menggunakan driver jembatan - driver jembatan menyediakan jaringan host tunggal.

## Langkah 2: Hubungkan wadah

Jaringan jembatan adalah jaringan default untuk wadah baru. Ini berarti bahwa kecuali Anda menentukan jaringan yang berbeda, semua kontainer baru akan terhubung ke jaringan jembatan .

Buat wadah baru dengan menjalankan docker run -dt ubuntu sleep infinity.

## Langkah 3: Uji konektivitas jaringan

Output ke docker network inspectperintah sebelumnya menunjukkan alamat IP wadah baru. Dalam contoh sebelumnya ini adalah "172.17.0.2" tetapi milik Anda mungkin berbeda.

Ping alamat IP wadah dari prompt shell host Docker Anda dengan menjalankan ping -c5 <IPv4 Address>. Ingatlah untuk menggunakan IP wadah di lingkungan Anda .

## Langkah 4: Konfigurasikan NAT untuk konektivitas eksternal

# Bagian # 3 - Overlay Networking

## Langkah 1: Dasar-Dasarnya

Pada langkah ini Anda akan menginisialisasi Swarm baru, bergabung dengan node pekerja tunggal, dan memverifikasi operasi berhasil.

## Langkah 2: Buat jaringan overlay

Sekarang Anda memiliki Swarm yang diinisialisasi, saatnya untuk membuat jaringan overlay .

## Langkah 3: Buat layanan

Sekarang kami memiliki jaringan Swarm yang diinisialisasi dan overlay, saatnya untuk membuat layanan yang menggunakan jaringan.

## Langkah 4: Uji jaringan

Untuk menyelesaikan langkah ini, Anda memerlukan alamat IP dari tugas layanan yang berjalan pada node2 yang Anda lihat pada langkah sebelumnya ( 10.0.0.3).

## Langkah 5: Uji penemuan layanan

Sekarang setelah Anda memiliki layanan yang berfungsi menggunakan jaringan overlay, mari kita uji penemuan layanan.

Jika Anda masih di dalam wadah, masuk kembali ke dalamnya dengan docker exec -it <CONTAINER ID> /bin/bash perintah.