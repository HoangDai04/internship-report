---

title: "Introduction"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
----------------------

#### Introduction to the Gym Management System

* The **Gym Management System** is a web-based application designed to help administrators efficiently manage all aspects of a fitness center through a centralized platform. The system enables the management of members, trainers, membership packages, attendance records, payments, and revenue statistics in a fast and accurate manner.

* The application follows a **Client–Server architecture**, with the frontend developed using **ReactJS** and **Vite**, and the backend built with **Node.js** and **ExpressJS**. **Prisma ORM** is used to interact with a **MySQL** database. This architecture provides excellent scalability, maintainability, and suitability for deployment in cloud environments.

#### Workshop Overview

In this workshop, you will build and deploy a complete **Full-Stack Gym Management System**.

* The **Gym Frontend** is developed with **ReactJS** and **Vite**, providing an intuitive user interface for managing members, trainers, membership packages, payments, attendance records, and an administrative dashboard.

* The **Gym Backend** is developed using **Node.js** and **ExpressJS**, exposing RESTful APIs to handle business logic, user authentication through **JWT Authentication**, role-based authorization, and database access via **Prisma ORM**.

Throughout this workshop, you will implement the core features of the system, including:

* User login and authentication.
* Member management.
* Trainer management.
* Membership package management.
* Assigning trainers and membership packages to members.
* Payment management.
* Attendance check-in management.
* Building an administrative dashboard with statistical reports.

After completing this workshop, you will have a fully functional Gym Management System featuring complete CRUD operations, secure user authentication, role-based access control, and the ability to deploy the application to AWS or other cloud platforms as well as real-world production environments.

![System Architecture](/images/gym.drawio.png)
