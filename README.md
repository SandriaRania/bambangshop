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
-   [✓] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✓] Commit: `Create Subscriber model struct.`
    -   [✓] Commit: `Create Notification model struct.`
    -   [✓] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✓] Commit: `Implement add function in Subscriber repository.`
    -   [✓] Commit: `Implement list_all function in Subscriber repository.`
    -   [✓] Commit: `Implement delete function in Subscriber repository.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✓] Commit: `Create Notification service struct skeleton.`
    -   [✓] Commit: `Implement subscribe function in Notification service.`
    -   [✓] Commit: `Implement subscribe function in Notification controller.`
    -   [✓] Commit: `Implement unsubscribe function in Notification service.`
    -   [✓] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [✓] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [✓] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [✓] Commit: `Implement publish function in Program service and Program controller.`
    -   [✓] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber
   is defined as an interface. Explain based on your understanding of Observer design patterns,
   do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model
   struct is enough?

Walaupun satu model struct saja memungkinkan, program akan jauh lebih baik jika Subscriber 
dibuat sebagai interfacenya sendiri. Hal ini menghindari terjadinya ketergantungan dari sisi 
Publisher dan juga mengabstraksikan isi dari Subscriber. Walaupun mungkin design pattern seperti ini
mungkin mengekspos Publisher ke Subscriber atau sebaliknya, tidak bisa dipungkiri bahwa design pattern
ini akan sangat membantu dari sisi reusability, efficiency, dan menerapkan konsep clean code.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your
   understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently
   use is necessary for this case?

Penggunaan Vec dan DashMap memiliki kelebihannya masing-masing, jadi hal ini tidak bisa langsung dibandingkan.
Dalam konteks ini, jika kita mengetahui id dan url yang diinginkan dan kita ingin merujuk terhadap
objek tersebut, maka tentunya DashMap akan lebih efisien dan hemat dibandingkan Vec. Tapi jika kita tidak tahu 
id dan url yang kita inginkan, namun kita dapat mengiterasi list id dan url untuk mencari kedua hal tersebut, 
maka Vec adalah tipe yang lebih sesuai digunakan. Sebagai kesimpulan, penggunaan Vec dan DashMap lebih bergantung
pada metode penggunaannya dibandingkan objek yang ingin disimpan di dalamnya, baik url, id, atau lainnya.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a
   thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we
   used the DashMap external library for thread safe HashMap. Explain based on your
   understanding of design patterns, do we still need DashMap or we can implement Singleton
   pattern instead?

Singleton adalah pattern yang digunakan di mana satu class dengan static variable digunakan dan direferensikan
secara global. Kelebihan singleton pattern adalah penggunaannya akan sangat menghemat memori, namun ia tidak terlalu
thread-safe. Sebagai contoh, jika dua thread merefer terhadap satu instance yang sama, maka mungkin saja bisa terjadi
race condition. Maka, jika program menggunakan beberapa thread, akan lebih aman untuk menggunakan DashMap dibandingkan
Singleton yang lebih rawan.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”.
   Model in MVC covers both data storage and business logic. Explain based on your
   understanding of design principles, why we need to separate “Service” and “Repository” from
   a Model?

Walaupun compound MVC pada umumnya tidak memiliki Service dan Repository, MVC di Springboot memiliki
kedua hal tersebut, sehingga terdapat lima role dalam MVC Springboot. Hal ini dikarenakan, walaupun Model
bisa saja mengurus data dan business logic dengan sendirinya, hal ini akan membuat Model menghandle terlalu banyak hal.
Jika kita membagi tugas ini kepada Service dan Repository, maka tidak hanya memenuhi Single Responsibility Principle,
hal ini juga akan memudahkan pengelolaan ke depannya.

2. What happens if we only use the Model? Explain your imagination on how the interactions
   between each model (Program, Subscriber, Notification) affect the code complexity for
   each model?

Jika kita tidak mengimplementasikan service dan repository dan hanya mengandalkan model, maka tentunya model-model itu
akan menjadi sangat panjang, kompleks, dan sulit dibaca. Andaikan jika ada suatu error dalam testing, kita akan kesulitan
memperbaikinya tanpa mempengaruhi fungsi lainnya di model-model tersebut. Hanya menggunakan model saja tentu bisa dilakukan,
namun hal ini hanya akan menyulitkan diri kita sendiri. Maka, akan jauh lebih baik jika kita dapat mengimplementasikan service
dan repository untuk membagi-bagi fungsi sesuai tugasnya masing-masing sehingga jauh lebih baik dari konteks keterbacaan, 
pengelolaan, dan efisiensi.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current
   work. You might want to also list which features in Postman you are interested in or feel like it
   is helpful to help your Group Project or any of your future software engineering projects.

Saya sudah sedikit-dikit mempelajari Postman. Menurut saya, fungsi Postman sebagai untuk mengetes HTTP
request secara local sangat menarik dan membantu saya untuk memperbaiki program saya sebelum dipush ke proyek akhir.
Saya rasa saya akan sering menggunakan Postman dalam projek saya ke depannya, tidak hanya untuk mengetes HTTP request,
namun mungkin untuk fungsi lain yang saya akan pelajari selama saya juga mengeksplorasi cara menggunakan Postman lebih lanjut.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and
   Pull model (subscribers pull data from publisher). In this tutorial case, which variation of
   Observer Pattern that we use?

Tutorial ini menggunakan push model, yaitu variasi observer pattern di mana publisher menge-push data ke subscriber. Hal ini
bisa dilihat di model Subscriber yang memiliki fungsi update dan Service Notification yang memiliki fungsi untuk memanggil update
dari setiap subscriber tersebut. Sehingga Subscriber bersifat pasif dan hanya perlu mengupdate data ketika diminta, sedangkan 
Notification yang bertugas memanggil dan meminta update setiap Subscriber yang ada.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern
   for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita menggunakan variasi Pull pattern, maka keuntungannya adalah setiap Subscriber memiliki kebebasan untuk memilih apa 
dan kapan data ingin ditarik. Kekurangannya adalah publisher mungkin harus dibuat lebih kompleks untuk mengakomodasi request 
dari Subscriber yang mungkin meminta informasi dengan kriteria berbeda-beda. Sebagai kesimpulan, sebenarnya kedua tipe observer
pattern bisa digunakan, namun dalam konteks banyak subscriber, mungkin push method akan lebih tepat digunakan.

3. Explain what will happen to the program if we decide to not use multi-threading in the
   notification process.
Jika kita tidak menggunakan multi-threading, maka kita harus memanggil update tiap Subscriber satu-satu. Tentunya hal ini
tidak efisien dan akan memakan waktu lama. Jika ada 100,000 subscriber, maka berapa lama waktu yang dibutuhkan untuk
mengirimkan notifikasi ke semuanya satu-satu? Maka, multi-threading sangat membantu dalam mempersingkat waktu dan meminimalisir
usaha yang perlu dilakukan.