---
title : "Giới thiệu"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về Gym Management System

+ Gym Management System là hệ thống quản lý phòng Gym được xây dựng nhằm hỗ trợ quản trị viên quản lý toàn bộ hoạt động của phòng tập trên một nền tảng tập trung. Hệ thống giúp quản lý hội viên, huấn luyện viên, các gói tập, lịch sử điểm danh, thanh toán và thống kê doanh thu một cách nhanh chóng và chính xác.
+ Ứng dụng được phát triển theo mô hình Client – Server với Frontend sử dụng **ReactJS** và **Vite**, Backend sử dụng **Node.js** và **ExpressJS**, kết hợp **Prisma ORM** để làm việc với cơ sở dữ liệu **MySQL**. Kiến trúc này giúp hệ thống dễ dàng mở rộng, bảo trì và triển khai trên môi trường Cloud.

#### Tổng quan về Workshop

Trong workshop này, bạn sẽ triển khai và xây dựng một hệ thống quản lý phòng Gym hoàn chỉnh theo mô hình Full Stack Web Application.

+ **"Gym Frontend"** được phát triển bằng **ReactJS** và **Vite**, cung cấp giao diện trực quan để quản lý hội viên, huấn luyện viên, gói tập, thanh toán, điểm danh và Dashboard thống kê.
+ **"Gym Backend"** được phát triển bằng **Node.js** và **ExpressJS**, cung cấp RESTful API để xử lý nghiệp vụ, xác thực người dùng bằng **JWT Authentication**, phân quyền quản trị viên và kết nối cơ sở dữ liệu thông qua **Prisma ORM**.

Trong quá trình thực hiện workshop, bạn sẽ lần lượt xây dựng các chức năng chính của hệ thống như:

+ Đăng nhập và xác thực người dùng.
+ Quản lý hội viên (Members).
+ Quản lý huấn luyện viên (Trainers).
+ Quản lý gói tập (Membership Packages).
+ Gán huấn luyện viên và gói tập cho hội viên.
+ Quản lý thanh toán (Payments).
+ Quản lý điểm danh (Attendance Check-in).
+ Xây dựng Dashboard thống kê tổng quan.

Sau khi hoàn thành workshop, bạn sẽ có một hệ thống quản lý phòng Gym hoàn chỉnh với đầy đủ các chức năng CRUD, xác thực người dùng, phân quyền quản trị và có thể triển khai lên các nền tảng Cloud hoặc môi trường thực tế.

![Kiến trúc hệ thống](/images/gym.drawio.png)