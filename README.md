# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [✔️] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✔️] Commit: `Create Notification model struct.`
    -   [✔️] Commit: `Create SubscriberRequest model struct.`
    -   [✔️] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [✔️] Commit: `Implement add function in Notification repository.`
    -   [✔️] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [✔️] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [✔️] Commit: `Create Notification service struct skeleton.`
    -   [✔️] Commit: `Implement subscribe function in Notification service.`
    -   [✔️] Commit: `Implement subscribe function in Notification controller.`
    -   [✔️] Commit: `Implement unsubscribe function in Notification service.`
    -   [✔️] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✔️] Commit: `Implement receive_notification function in Notification service.`
    -   [✔️] Commit: `Implement receive function in Notification controller.`
    -   [✔️] Commit: `Implement list_messages function in Notification service.`
    -   [✔️] Commit: `Implement list function in Notification controller.`
    -   [✔️] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. RwLock penting digunakan karena memungkinkan beberapa reader atau seorang penulis untuk access data yang digunakan secara bersamaan dengan tetap memastikan integritas data, memungkinkan multi thread untuk membaca data secara bersamaan yang juga menyediakan akses eksklusif untuk operasi menulis, dan mencegah data races.

Alasan tidak menggunakan mutex
- Tidak seperti RwLock, mutex hanya membolehkan satu thread untuk akses data dalam 1 waktu antara membaca atau menulis.
- mutex eksklusif untuk akses menulis, sehingga jika ada banyak akses membaca secara bersamaan akan diblock (konkurensi membaca data berkurang)
- RwLock memungkinkan beberapa reader untuk me-read lock secara bersamaan, yang cocok untuk skenario dimana operasi read lebih sering terjadi daripada operasi tulis.
2. Pada rust tidak boleh mengubah variabel static karena melanggar aturan Rust's ownership and borrowing, rust memastikan keamanan memori karena jika mengubah variabel static dapat menyebabkan isu keamanan memori seperti data races, dan rust memastikan keamanan thread karena jika mengubah variabel static dengan multi thread dapat menyebabkan isu concurrency

Pada java diperbolehkan mengubah variabel static karena :
- Pada Java aturan ownership tidak se ketat pada Rust, dan jaminan keamanannya tidak seperti Rust
- Memory dan concurrency primitive Java berbeda dengan Rust
- Java menggunakan mekanisme explicit synchronization seperti blok synchronized atau variabel volatile untuk memastikan keamanan thread, tetapi dapat membahayakan keamanan memori tidak seperti Rust

#### Reflection Subscriber-2
1. Saya belajar tentang penggunaan variable lazy static yaitu variable yang dinisialisasi secara lazy (hanya diinisialisasi saat pertama kali digunakan), saya belajar kita dapat membuat konfigurasi untuk aplikasi menggunakan file .env, error handling menggunakan Result<T, E> dan Custom<Json>.
2. Observer pattern dapat mempermudah untuk menambahkan jumlah subscribers karena tidak adanya tight couplling kepada subject (publisher), dengan observer pattern juga penerima atau di sini adalah subscriber dapat subscriber ke event atau notification dari aplikasi tanpa aplikasi harus tau instance penerima secara spesifik, dan tidak perlu khawatir seberapa banyak implementasi yang spesifik, dan dengan menyimpan list observer juga dapat menambahkan subscriber baru tanpa perlu mengubah struktur utama aplikasi.

untuk spawn lebih dari 1 instance dari main app perlu pertimbangan lebih khususnya ketika ingin berbagi notifikasi antar instance harus menyediakan media penyimpanan data bersama yang dapat diakses oleh semua instance karena setiap instance memiliki pola observer masing-masing.Atau dapat dibuat pola observer yang sejenis di mana setiap instance bertindak sebagai subject.Dan mungkin juga perlu menggunakan sistem arstitektur terdistribusi atau message brokers.
3. Saya belum membuat tes sendiri atau mencoba meningkatkan dokumentasi pada koleksi postman, tetapi menurut saya fitur tersebut akan sangat membantu dan berguna untuk tugas kelompok saya.

