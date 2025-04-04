# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Menurut saya sendiri dalam konteks BambangShop sekarang, saat ini penggunaan struct model `Subscriber` sudah cukup karena fungsionalitas yang dibutuhkan relatif sederhana. Tidak ada banyak perilaku yang harus di-_override_ atau diperluas, sehingga _polymorphism_ via trait tidak benar-benar diperlukan. Namun, jika di masa depan sistem berkembang dan terdapat jenis-jenis _subscriber_ dengan perilaku berbeda, maka mengimplementasikan trait bisa mempermudah pengelolaan perbedaan tersebut dan meningkatkan fleksibilitas.

2. Meskipun atribut seperti `id` pada `Product` dan `url` pada `Subscriber` bersifat unik, kita membutuhkan struktur data yang memungkinkan pencarian dan penghapusan dengan efisiensi yang lebih baik. Jika menggunakan `Vec`, pencarian untuk menemukan `Subscriber` tertentu akan membutuhkan pencarian linear yang berpotensi memakan waktu lebih lama. `DashMap` memberikan keuntungan karena menyediakan operasi lookup dan penghapusan dalam waktu konstan, sehingga lebih optimal dalam kasus ini.

3. Menurut saya, meskipun kita dapat mengimplementasikan Singleton pattern untuk mengelola akses ke data `Subscriber`, kesulitan yang muncul karena mutability dan persyaratan thread-safety di Rust membuat implementasi Singleton yang aman menjadi cukup kompleks. `DashMap` sudah menyediakan solusi yang thread-safe secara bawaan dan mudah digunakan, sehingga menurut saya menggunakan `DashMap` lebih sederhana dan efektif daripada mengembangkan Singleton pattern sendiri yang harus memenuhi banyak constraint dari Rust.

#### Reflection Publisher-2

1. Menurut saya, pemisahan `Service` dan `Repository` dari `Model` sangat penting agar setiap komponen dalam aplikasi dapat memiliki tanggung jawab yang spesifik dan tidak tercampur-campur. `Model` seharusnya hanya berperan sebagai representasi data, sementara `Repository` menangani operasi penyimpanan dan pengambilan data, dan `Service` bertanggung jawab atas logika bisnis yang lebih kompleks. Dengan cara ini, ketika terjadi perubahan pada logika bisnis atau cara penyimpanan data, kita tidak perlu mengubah keseluruhan `Model`, sehingga proses pemeliharaan dan pengembangan aplikasi menjadi lebih mudah dan terstruktur.

2. Jika hanya mengandalkan `Model` untuk menangani semua aspek, saya membayangkan bahwa kode di setiap `Model`, misalnya untuk `Product`, `Subscriber`, dan `Notification`, akan menjadi sangat kompleks karena harus meng-_handle_ penyimpanan data, logika bisnis, dan interaksi antar model secara langsung. Kompleksitas tersebut akan membuat setiap `Model` semakin sulit untuk dipahami dan dipelihara dan meningkatkan tingkat _coupling_ antar komponen. Hal ini tentu saja akan berdampak pada peningkatan potensi kesalahan dan kesulitan dalam proses debugging serta pengembangan fitur baru.

3. Dari informasi yang saya tahu, penggunaan `Postman` sangat membantu dalam proses testing `API`. Walaupun saya belum terlalu mengeksplorasi `Postman` secara mendalam, dari yang saya ketahui alat ini memungkinkan saya untuk dengan mudah mengirim berbagai jenis HTTP request dan langsung melihat response yang diterima. Hal ini memudahkan verifikasi bahwa setiap endpoint berfungsi sesuai harapan. Selain itu, fitur seperti pengaturan _custom headers_, _authorization_, dan _environment variables_ memungkinkan simulasi berbagai skenario pengujian dengan lebih fleksibel dan efisien. Kemampuan untuk menyimpan koleksi endpoint juga memudahkan integrasi testing dalam proyek kelompok maupun pengembangan software di masa depan, sehingga saya merasa Postman merupakan alat yang sangat berguna dalam workflow pengembangan aplikasi.

#### Reflection Publisher-3

1. Dalam tutorial ini, saya menggunakan variasi `Push` pada Observer Pattern. Setiap kali terjadi perubahan pada `Product` (misalnya create atau delete), `Publisher` langsung mengirimkan notifikasi ke setiap `Subscriber` yang relevan tanpa menunggu permintaan dari Subscriber.

2. Jika kita menggunakan variasi `Pull`, keuntungannya adalah `Publisher` tidak perlu langsung mengirim notifikasi ke setiap `Subscriber` setiap kali terjadi perubahan, sehingga beban proses notifikasi di sisi server bisa berkurang. `Subscriber` sendiri akan mengambil data sesuai kebutuhan dan kapan saja mereka inginkan data terbaru. Tetapi, kelemahannya adalah `Subscriber` tidak mendapatkan update secara real-time karena harus secara aktif meminta data baru, sehingga bisa terjadi keterlambatan dalam mendapatkan informasi terbaru. Apabila `Subscriber` melakukan polling secara sering, hal tersebut justru bisa meningkatkan beban server dalam jangka panjang.

3. Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, maka proses pengiriman notifikasi akan dilakukan secara berurutan. Hal ini berarti jika ada banyak `Subscriber` untuk suatu `product_type`, maka proses notifikasi akan menjadi sangat lambat dan memblokir jalannya proses lain pada aplikasi. Server akan sulit untuk meng-_handle_ request lain secara sekaligus, yang dapat mengurangi responsiveness dan performa aplikasi secara keseluruhan.