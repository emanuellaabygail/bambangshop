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
>In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Pada kasus Bambangshop ini, menurut saya tidak perlu menggunakan interface (trait)  karena observer-nya hanya satu, yaitu class Subscriber. Interface akan berguna jika ada banyak observer dengan jenis berbeda. Jika hanya ada satu observer, cukup gunakan satu Model struct saja.

>id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Jika id pada Program dan url pada Subscriber harus unik, maka struktur data yang tepat adalah yang memungkinkan pencarian cepat dan memastikan tidak ada duplikasi. Menggunakan Vec (list) kurang efisien karena pencarian memerlukan iterasi seluruh elemen (O(n)), dan memastikan keunikan membutuhkan pemeriksaan manual. Sebaliknya, menggunakan DashMap (atau struktur map lainnya) lebih cocok karena mendukung pencarian cepat (O(1)) dan secara alami memastikan keunikan key (dalam hal ini id atau url). Jadi, DashMap lebih tepat digunakan dalam kasus ini.

>When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Menggunakan DashMap dalam kasus List of Subscribers (SUBSCRIBERS) adalah pilihan tepat karena secara langsung menyediakan thread safety melalui fitur concurrent map, memungkinkan banyak thread untuk membaca dan menulis secara aman. Di sisi lain, menerapkan Singleton pattern hanya memastikan bahwa ada satu instance dari suatu objek secara global, tetapi tidak menjamin thread safety dalam pengaksesan data secara concurrent. Jadi, jika tujuan utamanya adalah thread safety dan akses cepat pada data bersama, DashMap tetap lebih sesuai dibandingkan dengan Singleton pattern.

#### Reflection Publisher-2
>In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Memisahkan Service dan Repository dari Model penting untuk menjaga separation of concerns dan membuat kode lebih terstruktur. Service bertanggung jawab atas logika bisnis dan aturan aplikasi. Dengan memisahkannya, kita bisa mengubah aturan bisnis tanpa memengaruhi data penyimpanan. Repository: bertanggung jawab mengelola akses data (misalnya, basis data). Dengan memisahkannya, kita bisa mengganti sumber data tanpa memengaruhi logika bisnis. Dengan memisahkan keduanya, kita mendapatkan kode yang lebih mudah diuji, diubah, dan dipelihara, sesuai prinsip Single Responsibility dan Dependency Inversion.

>What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika hanya menggunakan Model tanpa memisahkan Service dan Repository, semua logika bisnis dan akses data akan tercampur dalam satu tempat, sehingga kode menjadi kompleks dan tidak maintainable. Interaksi antar Model seperti Program, Subscriber, dan Notification akan saling terkait secara langsung, membuat perubahan pada satu Model dapat memengaruhi yang lain. Hal ini meningkatkan risiko bug dan kesulitan pengujian karena semua tanggung jawab berada dalam satu kelas. Selain itu, fleksibilitas dalam mengganti penyimpanan data atau memodifikasi logika bisnis akan sangat terbatas, membuat pengembangan lebih lambat dan rentan kesalahan.

>Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Ya, saya sudah mengeksplorasi Postman dan merasa alat ini sangat membantu dalam menguji API. Saya bisa melakukan request HTTP dengan mudah, mengelola collection untuk pengujian terstruktur, serta menggunakan Environment untuk menyimpan variabel seperti URL dan token. Fitur Automated Testing dan Mock Server juga sangat berguna untuk menguji respons API dan membuat API tiruan tanpa server asli, sehingga cocok untuk proyek kelompok dan pengembangan perangkat lunak di masa depan.

#### Reflection Publisher-3
>Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Variasi Observer Pattern yang digunakan pada tutorial ini adalah Push Model karena Publisher mengirim notifikasi kepada semua Subscriber-Nya saat terjadi CRUD pada product dengan method notify pada NotificationService

>What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Variasi pull pada Observer Pattern memiliki keuntungan dalam fleksibilitas data, karena observer dapat mengambil informasi yang diperlukan dari subject secara langsung, sehingga data yang tidak dibutuhkan tidak terkirim. Hal ini juga membuat penggunaan data lebih efisien jika hanya sebagian data yang berubah. Namun, ada kekurangannya, yaitu beban pemrosesan berpindah ke observer, karena observer harus tahu bagaimana mengambil data yang diperlukan dari subject, yang meningkatkan keterkaitan (coupling). Selain itu, ada risiko overhead komunikasi jika observer mengambil lebih banyak data dari yang sebenarnya diperlukan. Pada kasus tutorial ini, jika perubahan data tidak selalu relevan bagi semua observer, variasi pull bisa menjadi lebih rumit dan kurang efisien dibandingkan variasi push.

>Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika kita tidak menggunakan multi-threading dalam proses notifikasi, seluruh pengiriman notifikasi akan dilakukan secara berurutan pada main thread. Hal ini berarti jika ada banyak subscriber, program akan mengirim notifikasi satu per satu, sehingga proses menjadi lambat dan dapat menyebabkan blocking pada operasi lain. Akibatnya, responsivitas aplikasi menurun, terutama jika pengiriman notifikasi memerlukan waktu lama (misalnya, karena koneksi jaringan). Oleh karena itu, tanpa multi-threading, efisiensi dan performa program akan sangat terganggu, terutama pada skala besar dengan banyak subscriber.