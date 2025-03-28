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
> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Saat ini, satu struct model sudah mencukupi karena BambangShop hanya memiliki satu tipe pelanggan dengan perilaku yang seragam. Oleh karena itu, penggunaan trait untuk mendukung polimorfisme tidak diperlukan. Namun, jika terdapat berbagai jenis pelanggan dengan perilaku berbeda, penggunaan trait dapat memberikan antarmuka bersama serta fleksibilitas dalam implementasi.

> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Menggunakan DashMap lebih tepat dibanding Vec dalam kasus ini. Pertama, pengecekan keunikan id atau url pada Vec memerlukan  O(n), sedangkan DashMap menggunakan hashing untuk akses langsung sehingga kompeksitasnya O(1), yang jauh lebih efisien, terutama saat data bertambah besar. Kedua, DashMap secara langsung mencegah duplikasi data karena menggunakan url sebagai key sedangkan Vec tidak memiliki mekanisme otomatis untuk mencegah entri duplikat.

> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?


DashMap sangat dioptimalkan untuk operasi yang bersifat konkuren, sehingga menjadi pilihan ideal untuk mengelola variabel thread-safe seperti . Meski pola desain Singleton dapat memastikan hanya ada satu instance global, pola tersebut tidak secara inheren memberikan thread-safety tanpa mekanisme sinkronisasi tambahan yang dapat menyebabkan bottleneck kinerja. Dalam kasus ini, DashMap menyederhanakan implementasi dengan menggabungkan mekanisme locking yang efisien dan kinerja yang unggul dalam kondisi konkuren tinggi, sehingga tidak diperlukan setup Singleton manual yang thread-safe. Jadi, DashMap adalah pilihan yang aman dan efisien.

#### Reflection Publisher-2

>In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Memisahkan Service dan Repository dari Model membuat kode lebih terorganisir dan mudah dikelola. Service bertanggung jawab atas logika bisnis, sementara Repository menangani interaksi dengan database. Hal ini mengikuti prinsip Single Responsibility, membuat kode lebih mudah dipelihara, diuji, dan diperluas. Jika semua logika ditempatkan di dalam Model, kode akan menjadi lebih kompleks, sulit diuji, dan perubahan kecil dapat berdampak besar pada seluruh sistem.

> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika semua logika dimasukkan ke dalam Model, kode akan berantakan dan sulit dikembangkan. Setiap Model harus mengurus penyimpanan data, validasi, dan logika bisnis sekaligus. Akibatnya, kode jadi panjang, sering terduplikasi, dan sulit diubah. 

> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Penggunaan Postman sangat membantu dalam pengujian API karena memungkinkan saya untuk mengirim request HTTP dengan mudah tanpa harus menulis skrip tambahan.

#### Reflection Publisher-3

> Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Pada tutorial ini, kita menggunakan model Push dalam pola Observer. Implementasi ini terlihat jelas pada kode `NotificationService::notify()` yang secara aktif mengirimkan data perubahan produk ke semua subscriber melalui HTTP POST request setiap kali terjadi modifikasi produk. Subscriber tidak perlu meminta data - mereka langsung menerimanya secara otomatis ketika ada perubahan produk.

> What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika menggunakan Pull Model, terdapat beberapa keuntungan seperti, subscriber memiliki kontrol lebih besar atas kapan dan bagaimana mereka menerima update, mengurangi beban jaringan karena tidak ada notifikasi yang tidak diperlukan, lebih efisien untuk data yang besar atau kompleks. Namun terdapat juga kekurangan seperti, update tidak terjadi secara real-time, dapat menyebabkan beban berlebih pada server jika terlalu banyak request polling, memerlukan implementasi endpoint khusus untuk pengecekan update

> Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Tanpa multi-threading, proses notifikasi akan berjalan secara sinkron, yang berarti setiap subscriber harus menunggu sampai notifikasi sebelumnya selesai dikirim. Hal ini akan menyebabkan keterlambatan yang signifikan, terutama jika ada banyak subscriber. Dengan menerapkan multi-threading, notifikasi dapat dikirim secara bersamaan, sehingga meningkatkan efisiensi dan mempercepat respons sistem.