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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
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
1. Jika menggunakan design pattern observer yang terdiri dari berbagai macam class karena terdiri dari berbagai jeni, maka diperlukan suatu observer yang menggunakan trait. Dalam case BambangShop, observer hanya terdiri dari satu class yaitu Subscriber sehingga untuk saat ini belum memerlukan pembuatan suatu trait

2. Penggunaan DashMap diperlukan agar dapat memetakan tiap jenis produk ke subscriber yang membutuhkan. Penggunaan Vector dapat diimplementasikan, tetapi akan memerlukan dua vector untuk masing-masing produk sehingga dapat mempersulit perubahan data.

3. Karena map SUBSCRIBER akan diakses melalui banyak thread, penggunaan DashMap dapat diimplementasikan ke dalam program karena sifatnya yang dapat mengakomodasi keperluan program yang multithreaded. Singleton sendiri digunakan untuk memastikan bahwa hanya ada satu instance dari program tersebut yang mana dalam konteks ini digunakan supaya pengembang dapat selalu memastikan bahwa produk tidak berada di struktur data lain yang berbeda.

#### Reflection Publisher-2
l. Untuk memisahkan fungsionalitas agar dapat mengimplementasikan single responsibility principle, service perlu dipisahkan dari Repository. Dengan adanya pemisahan service, modul-modul yang berfungsi untuk mendapatkan dan mengolah data didapatkan dari Repository sedangkan Layer Repository berfungsi sebagai layer yang berguna untuk mengakses basis data dan melakukan perubahan terhadap isi database, sehingga berperan dalam code maintainability

2. Dalam suatu skenario di mana hanya digunakan model tanpa adanya layer yang lain, maka program akan memiliki interdependensi tinggi terhadap modul-modul yang lain sehingga jika terdapat suatu perubahan di dalam kode, maka akan diperlukan banyak perubahan lain di dalam kode untuk menyesuaikan perubahan tersebut.

3. Postman merupakan sebuah tool yang sangat membantu dalam pengembangan aplikasi karena dapat membantu pengembang untuk menguji apakah aplikasi dapat berjalan dengan baik dengan melihat apakah aplikasi dapat mengembalikan respon yang sesuai berdasarkan request yang dibuat.

#### Reflection Publisher-3
1. Dalam tutorial ini, digunakan push model yang dapat terlihat dari modul objek seperti create, delete, update yang mana notification service akan melakukan iterasi ke seluruh list of subscribers untuk mendapatkan update terbaru

2. Dengan menggunakan metode Pull, maka terdapat kebebasan bagi observer dalam menentukan data apa saja yang diambil atau kapan diambil. Meskipun begitu, observer perlu mengetahui struktur dari data source dan pengiriman notification menjadi tidak real-time.

3. Jika program tidak menggunakan multithreading, maka program akan memiliki antrian yang panjang ketika pengiriman notifikasi perlu menunggu notifikasi yang lain untuk selesai terlebih dahulu sehingga menganggu performance aplikasi.