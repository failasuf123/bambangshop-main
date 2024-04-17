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

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website

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
-   [ ] Clone  to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Dalam pola Observer, antarmuka atau trait digunakan   untuk memungkinkan objek-objek yang berbeda untuk berperan sebagai pengamat (observer) dan menerima pembaruan dari subjek (subject) tanpa bergantung pada implementasi konkret subjek. Dalam kasus BambangShop, meskipun Rust tidak memiliki konsep antarmuka secara langsung seperti bahasa lain, penggunaan trait untuk menggambarkan perilaku Subscriber masih dapat bermanfaat. Dengan menggunakan trait, saya dapat menentukan metode-metode yang diperlukan untuk pengamat (seperti notify) dan memastikan bahwa semua implementasi Subscriber mengikuti pola yang diharapkan. Namun, jika hanya ada satu jenis pengamat (Subscriber) yang tidak memerlukan variasi perilaku, menggunakan sebuah struct Model tunggal juga sudah cukup.
2. Penggunaan Vec (list) atau DashMap (map/dictionary) untuk menyimpan ID yang unik (seperti id in Program dan url in Subscriber) tergantung pada kebutuhan dan kompleksitas aplikasi. Jika kita  hanya memiliki beberapa elemen yang harus unik, Vec dapat menjadi pilihan yang cukup baik karena sederhana dan mudah digunakan. Namun, jika kita memerlukan kemampuan pencarian yang lebih efisien dan pengelolaan yang lebih kuat atas elemen-elemen yang unik, menggunakan DashMap atau struktur data lainnya yang lebih canggih mungkin lebih tepat.
3. Dalam konteks thread safety, Rust memang menekankan keamanan dan membatasi kemungkinan kesalahan yang terjadi karena operasi konkurensi. Penggunaan DashMap (atau HashMap yang aman dari segi thread) adalah pilihan yang baik untuk mengelola koleksi data yang dibagikan antara thread karena memastikan keselamatan akses ke data tersebut. Pola Singleton, sementara itu, dapat digunakan untuk memastikan bahwa hanya ada satu instance dari sebuah struktur yang dibuat, namun itu tidak menjamin thread safety secara otomatis. Jadi, penggunaan DashMap atau struktur data serupa untuk koleksi thread-safe adalah langkah yang tepat dalam konteks Rust.

#### Reflection Publisher-2
1. Dalam pola MVC tradisional, Model bertanggung jawab atas penyimpanan data dan logika bisnis. Namun, dengan memisahkan "Service" dan "Repository" dari Model, kita dapat mencapai beberapa manfaat:

  - Pemisahan Tanggung Jawab: "Service" bertanggung jawab atas logika bisnis yang lebih kompleks, seperti pengelolaan alur kerja, validasi, dan operasi bisnis lainnya. Di sisi lain, "Repository" bertanggung jawab atas operasi operasi CRUD (Create, Read, Update, Delete) terhadap penyimpanan data.
  - Fleksibilitas dan Penggantian: Dengan memisahkan "Service" dan "Repository", kita dapat dengan mudah mengganti implementasi "Repository" tanpa mengubah logika bisnis dalam "Service". Misalnya, kita dapat beralih dari database SQL ke NoSQL tanpa mempengaruhi layanan yang menggunakannya.
  - Pembacaan Kode yang Lebih Mudah: Pemisahan ini memungkinkan kode menjadi lebih terorganisir dan mudah dibaca, karena setiap komponen memiliki fokusnya sendiri.

2. Jika kita hanya menggunakan Model dalam pola MVC, maka semua logika bisnis, operasi CRUD, validasi, dan operasi lainnya akan dikumpulkan dalam satu tempat. Hal ini dapat menyebabkan beberapa masalah:

  - Overload dalam Model: Model menjadi terlalu berat karena harus menangani semua aspek aplikasi, membuatnya sulit dipahami dan dikelola.
  - Kode yang Sulit Didebug dan Diuji: Ketika segala sesuatu terkait dengan data dan logika bisnis dikemas dalam Model, debugging dan pengujian menjadi lebih sulit karena sulit untuk mengisolasi masalah dan menyusun pengujian yang terfokus.
  - Ketergantungan yang Tinggi: Implementasi terkait database dan operasi CRUD menjadi terikat erat dengan logika bisnis, sehingga sulit untuk mengganti atau memperbarui satu bagian tanpa memengaruhi yang lain.

#### Reflection Publisher-3
1. Dalam kasus tutorial ini, kita menggunakan variasi model Push dari Pola Observer. Hal ini terjadi karena pengirim (BambangShop) secara aktif mengirimkan notifikasi kepada pelanggan (Subscriber) ketika terjadi peristiwa yang relevan, seperti pembuatan, promosi, atau penghapusan produk.

2. Jika kita menggunakan variasi model Pull dari Pola Observer, di mana pelanggan menarik data dari pengirim saat diperlukan, berikut adalah kelebihan dan kekurangannya:

    Kelebihan:
      - Penyampaian Data yang Efisien: Dalam model Pull, pelanggan hanya akan meminta data ketika mereka membutuhkannya, mengurangi jumlah permintaan yang tidak perlu.
      - Kontrol Lebih Besar: Pelanggan memiliki lebih banyak kontrol atas data yang diterimanya, dapat meminta data spesifik yang mereka inginkan.

    Kekurangan:
      - Penundaan Respons: Jika terdapat banyak pelanggan yang menarik data secara bersamaan, dapat terjadi penundaan dalam respons karena pengirim harus menanggapi permintaan setiap pelanggan.
      - Kesulitan Sinkronisasi: Mengelola keadaan sinkronisasi antara pengirim dan pelanggan dalam model Pull dapat lebih rumit, terutama jika terdapat perubahan serentak yang harus disampaikan kepada banyak pelanggan.
