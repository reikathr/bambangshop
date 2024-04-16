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
<ol>
<li>In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?</li>
<p>Answer: In the Observer pattern, an observer can be defined as an interface/trait to allow for flexibility. If there are multiple observer objects with different behaviors, different observers can implement different interfaces/traits. In this case, BambangShop has one observer (Subscriber) so there's no need for it to be defined as a trait. So, a single Model struct for Subscriber is enough.</p>
<li>id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?</li>
<p>Answer: Technically using Vec is sufficient because the objects are still accessible, but using DashMap is much more efficient because it takes less time to access Program and Subscriber when they have a unique attribute (id and url respectively). Accessing something in a Vec would require O(n) time at the worst case for a Vec of size n, while accessing something in a DashMap would require O(1) in every case.</p>
<li>When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?</li>
<p>Answer: The Singleton Pattern ensures that only one instance of a class in a project, while the DashMap is a thread-safe version of HashMap which is important for concurrency in Rust. In this BambangShop case, the DashMap is ideal because it ensures that we can access the contents concurrently, while the Singleton Pattern may cause concurrency issues.</p>
</ol>

#### Reflection Publisher-2
<ol>
<li>In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?</li>
<p>Answer: As we've learned in PBP, it has to do with separation of concerns. Separating "service" and "repository" from models means models, service, and repository have their own roles. Similar to the Single Responsibility Principle, this makes the application more maintainable. In this case, a model would represent the data, the repository takes care of accessing that data, and the service is in charge of the business logic. Any error that might occur in development/maintenance would be easier to find and fix with those separated roles as opposed to actual MVC.</p>
<li>What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?</li>
<p>Answer: Only using model goes against separation of concerns, so it goes against SRP. This could cause the code to be harder to write, modify, and understand because it could cause different models to be heavily dependent on each other (coupling). For example, since models would have to cover data storage and business logic, a change in Subscriber might require a change in Notification as well. Without the separation, it's harder to find what needs to be changed.</p>
<li>Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.</li>
<p>Answer: I've used Postman before to test and simulate sending HTTP requests to API endpoints. Through Postman, I can send HTTP requests and view the details of the request and the response returned. For my group project, I'm interested in using send requests, add example, and environments.</p>
</ol>

#### Reflection Publisher-3
<ol>
<li>Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?</li>
<p>Answer: In BambangShop, the variation used is Push model where the publisher pushes data to subscribers. We can see this by looking at Product service and seeing that product is the one calling the notify method to notify observers who are subscribed when a product is added or deleted, so it's not the observers who are "looking" for a notification.</p>
<li>What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)</li>
<p>Answer: If we used pull, the advantages would be the fact that the observers (subscribers) get to call for a notification when they want to, so they're in control of it. That can decrease unnecessary/unwanted processes and reduce resources needed. However, because it's the subscribers calling for a notification, they wouldn't get instantaneous updates of when a product is released or deleted (which is the case for the push model), so it's not very useful for a notification feature.</p>
<li>Explain what will happen to the program if we decide to not use multi-threading in the notification process.</li>
<p>Answer: The lack of multi-threading means that notifications can't be processed and given in parallel (at the same time). If there are 1000 subscribers to give notifications to, they won't receive it at the exact same time and it'll take a long time to process them, making it unfair for the subscribers.</p>
</ol>