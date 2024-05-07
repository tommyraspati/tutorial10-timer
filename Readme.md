# Tutorial 10 - Advanced Programming - Timer

## Understanding How It Works

<img src="/images/1.2 tutorial10.png">

Meskipun `println!()` dipanggil setelah pernyataan `spawner.spawn(async { ... });`, output "hey hey" dicetak sebelum "howdy!". Hal ini terjadi karena fungsi `async` berjalan secara asinkronus di luar alur utama eksekusi program, yang berarti program utama tidak menunggu sampai fungsi `async` selesai sebelum melanjutkan eksekusi berikutnya. Pencetakan "hey hey" terjadi di luar fungsi `async` dan dieksekusi langsung oleh program utama, sementara fungsi `async` menunggu hasil dari operasi `future`. Oleh karena itu, "hey hey" dicetak terlebih dahulu sebelum tugas asinkronus dijalankan oleh executor.

## Multiple Spawn and Removing Drop
<img src="/images/1.3.png">

Dari hasil output yang terlihat, terlihat bahwa penggunaan banyak spawner menghasilkan lebih banyak tugas dilakukan, karena setiap spawner menambahkan tugas baru ke dalam antrian untuk dieksekusi. Tidak melakukan drop pada spawner mengakibatkan program tidak pernah berakhir karena program menganggap bahwa masih akan ada pengiriman data oleh spawner. Penggunaan drop(spawner) menandakan bahwa interaksi sudah selesai dan spawner akan ditutup.

Ketika suatu spawner memanggil fungsi spawn, tugas baru akan dibuat dan dimasukkan ke dalam antrian. Executor kemudian akan mengambil satu tugas dari antrian tersebut, mengeksekusinya, dan mengulang proses tersebut hingga tidak ada tugas lagi dalam antrian. Penggunaan drop(spawner) menandakan bahwa interaksi telah selesai dan spawner dapat ditutup.
