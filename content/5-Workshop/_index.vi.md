---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Cấu hình Kết nối Bảo mật từ Private Subnet tới Amazon S3 bằng VPC Endpoint
#### Tổng quan
Trong kiến trúc hệ thống của bạn (Website Gym / Hệ thống IoT thời tiết), các máy chủ Backend Node.js trên EC2 hoặc các tác vụ xử lý dữ liệu AWS Glue/Lambda nằm hoàn toàn trong Private Subnet và không có địa chỉ IP công khai (Public IP). Theo mặc định, để các dịch vụ này có thể upload/download hình ảnh học viên hoặc dữ liệu log lên Amazon S3, traffic sẽ phải đi qua Internet Gateway, điều này làm giảm tính bảo mật và tăng chi phí băng thông internet.

Bài Workshop này hướng dẫn bạn cách thiết lập VPC Endpoint (Gateway Endpoint) để tạo một đường truyền kết nối nội bộ, hoàn toàn tách biệt với Internet công khai giữa VPC và Amazon S3, kết hợp phân quyền thắt chặt với IAM Policy và S3 Bucket Policy.

#### Prerequisite (Điều kiện tiên quyết)
- Để thực hiện bài thực hành này, bạn cần chuẩn bị sẵn các tài nguyên cơ bản sau trên AWS:

- One VPC đã cấu hình sẵn ít nhất:

 1 Public Subnet (để đứng làm trung gian kết nối/bastion host nếu cần).

 1 Private Subnet (nơi đặt EC2 thử nghiệm, không có tuyến đường 0.0.0.0/0 đi ra Internet Gateway).

- One máy chủ Amazon EC2 chạy hệ điều hành Linux (Ubuntu/Amazon Linux 2) nằm trong Private Subnet, đã được cài đặt sẵn AWS CLI.

- One IAM Role gắn vào EC2 có quyền cơ bản để gọi dịch vụ S3 (ví dụ quyền: AmazonS3ReadOnlyAccess hoặc quyền tự cấu hình).

- One Amazon S3 Bucket đã được tạo 

#### Mô tả kiến trúc (Architecture Diagram & Description)
Khi chưa có VPC Endpoint, máy chủ EC2 trong Private Subnet muốn kết nối tới S3 phải đi vòng qua NAT Gateway ra Internet công cộng rồi mới tới được Endpoint công khai của S3.

Sau khi cấu hình VPC Gateway Endpoint cho S3, một thực thể logic sẽ được đính trực tiếp vào Route Table (Bảng định tuyến) của Private Subnet. Mọi traffic có đích đến là S3 sẽ tự động được định tuyến qua hạ tầng mạng nội bộ của AWS (AWS Backbone Network), đảm bảo dữ liệu không bao giờ rời khỏi tầm kiểm soát của hệ thống mạng private.