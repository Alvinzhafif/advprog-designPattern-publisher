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
1. #### In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
    <br>
    I believe a single struct is enough, because currently we are only trying to implement one functionality for the subscribers. However, if somewhere on the future we are going to add different roles and functionality to the subscribers an interface or trait may be needed to ensure integrity between the roles and functionalities.
    <br>
    <br>
   
2. #### id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
   <br>
   
   I believe that using a `DashMap`  in the case that both ID and Url is intended to be unique. Additionally, `DashMap` is efficient in performance with `O(1)` Lookup and Insertion time for most operations and `DashMap` enforces uniqueness preventing a case of duplicates. This is a perfect fit, as we want both ID and Url to be unique in this case 

3. #### When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
    <br>
    
    I believe using `DashMap` is better. In addition to the previous advantages I had mentioned, `DashMap` also has an advantage in thread safety, concurrency, and dynamic growth. While singleton also can handle thread safety, it is a bit more complex compared to `DashMap` as it is needed to manually synchronize access to the singleton instance. While `DashMap` inherently does it, ensuring multiple threads can read and write without any explicit synchronization.
      
#### Reflection Publisher-2
1. #### In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
    <br>

    There are a few reasons why we need to separate `service` and `repository` from each other. Two of them adheres to the `SOLID` principle that has been covered before, which is **Single Responsibility Principle** or **SRP** and **Dependency Inversion Principle** or **DIP**. By separating both `service` and `repository` from model, we can ensure that each component focuses on a single responsibility and reduces coupling between each component
2. #### What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
    <br>

    If we were only to use the model, then most likely it will violate the previous `SOLID` principle. Violating the principle may not seem much at first however, when the code progressively gets bigger and bigger, it may become progressively harder to keep track of each of the codes responsibility or worse, the code can become bloated. Additionally, it might be challenging to understand the code and implement tests if this happens.

3. #### Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
    <br>

    I have experience using `Postman` ever since the previous course of Platform Based Programming or **PBP**. This app certainly helps me a lot of times, one of the features i liked is the way it can show datas in a neat and readable format. Especially in our group projects, I feel the need to read data clearly to make sure that there are no incomplete or miss inputted data. Hence, that is where `Postman comes`, it can show any data whether it is in `Json` or even `Xml` format. 
#### Reflection Publisher-3

1. #### Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
    <br>

    It adheres to the Push model, every function in `notification service` uses a push model as the `publisher` will push notifications to the `subscriber` without expecting anything in return from the subscriber. The `product service` is an implementation of it as the subscriber will be sent notification from the publisher for each time the product function operation is triggered.

2. #### What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
    <br>

    If it were to be a Pull model, then there a few disadvantages that might happen. Such as increased network traffic, this maybe triggered when there are a lot of subscribers and they want to pull from the publisher for updates. This can also lead to latency or delays due to subscribers need to wait until the next polling interval for them to receive the updates. 
    <br>
    <br>
    However there also advantages, as using a pull model can decrease the server load. In this case subscribers would only receive notification or updates when requested by them instead of it being pushed directly by the publisher. This is beneficial if the server load or space is pretty minimum. Additionally, the subscribers can maintain control of the notification load in cases where there will be a lot of notification sent by publisher. 

3. #### Explain what will happen to the program if we decide to not use multi-threading in the notification process.
    <br>
   
    If we decided to drop multi-threading, each and every notification process will be executed sequentially. It may not seem much, however when there's a certain delay to one of the notification process. In a nutshell, this will make the process behavior into synchronous. Additionally, it may also cause some performance issue if there were to be a lot of subscriber updates to process, causing a delay as the notification needs to be executed one by one whilst waiting for the previous one to finish.