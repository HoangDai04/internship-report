---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# KIẾN TRÚC & TRIỂN KHAI HỆ THỐNG WEBSITE GYM 


### 1. Tóm tắt điều hành  
Dự án tập trung vào việc xây dựng và triển khai một nền tảng website fullstack hiện đại dành cho việc quản lý vận hành phòng gym và tối ưu trải nghiệm của học viên. Hệ thống được thiết kế theo kiến trúc điện toán đám mây Amazon Web Services (AWS) kết hợp giữa các dịch vụ Serverless và hạ tầng có khả năng tự động mở rộng (Auto Scaling). Mục tiêu cốt lõi của giải pháp là đảm bảo tính sẵn sàng cao (High Availability), bảo mật dữ liệu tuyệt đối cho học viên, và tối ưu hóa chi phí vận hành dựa trên lưu lượng truy cập thực tế (FinOps).
### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Các hệ thống phòng gym truyền thống thường gặp khó khăn khi lượng học viên truy cập trang web để check-in hoặc đặt lịch tập với Huấn luyện viên cá nhân (PT) tăng đột biến vào các khung giờ cao điểm (6h-8h sáng và 17h-20h tối), dẫn đến hiện tượng nghẽn mạng hoặc sập máy chủ. Bên cạnh đó, việc lưu trữ hình ảnh, hóa đơn và nhật ký quẹt thẻ check-in của học viên ngày một lớn, đòi hỏi một hạ tầng lưu trữ và database đủ mạnh, an toàn nhưng không quá tốn kém chi phí cố định khi ở khung giờ thấp điểm (đêm muộn). 

*Giải pháp*  
Triển khai hệ thống Website Gym phân lớp (Layering) trên AWS: sử dụng S3 và CloudFront để tối ưu tốc độ tải giao diện ứng dụng (React.js); bảo vệ toàn diện bằng AWS WAF ở vòng ngoài; xử lý logic nghiệp vụ bằng cụm Node.js + Express trên Amazon EC2 đặt trong cấu hình tự động co giãn Auto Scaling Group xuyên suốt 2 vùng độc lập (Multi-AZ); lưu trữ phân tách giữa dữ liệu quan hệ (Amazon RDS MySQL), dữ liệu check-in log (DynamoDB), và bộ nhớ đệm cache (ElastiCache Redis). 

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Về mặt kỹ thuật: Hệ thống loại bỏ hoàn toàn rủi ro sập web giờ cao điểm, thời gian phản hồi API giảm xuống dưới 100ms nhờ bộ nhớ cache Redis, dữ liệu được backup tự động.
Về mặt kinh doanh: Giảm thiểu tối đa chi phí phần cứng vật lý cố định. Nhờ cơ chế tự động co giãn và Free Tier từ AWS, hệ thống tối ưu được đến 60% chi phí vận hành so với việc thuê máy chủ truyền thống. Tăng tỷ lệ giữ chân học viên nhờ trải nghiệm ứng dụng mượt mà, không gián đoạn.  

### 3. Kiến trúc giải pháp  
Nền tảng áp dụng kiến trúc AWS Serverless để quản lý dữ liệu từ 5 trạm dựa trên Raspberry Pi, có thể mở rộng lên 15 trạm. Dữ liệu được tiếp nhận qua AWS IoT Core, lưu trữ trong S3 data lake và xử lý bởi AWS Glue Crawlers và ETL jobs để chuyển đổi và tải vào một S3 bucket khác cho mục đích phân tích. Lambda và API Gateway xử lý bổ sung, trong khi Amplify với Next.js cung cấp bảng điều khiển được bảo mật bởi Cognito.  



![kien truc](/images/gym.drawio.png)

*Dịch vụ AWS sử dụng*  
- *Amazon Route 53*: Dịch vụ DNS quản lý tên miền và điều hướng request từ internet toàn cầu vào hệ thống.  
- *AWS WAF (Web Application Firewall)*: Tường lửa ngăn chặn mã độc, SQL Injection, và chống tấn công từ chối dịch vụ (DDoS).  
- *Amazon CloudFront (CDN)*: Mạng lưới phân phối nội dung giúp tăng tốc tải trang Frontend gần vị trí học viên nhất.  
- *Amazon S3*: Kho lưu trữ đối tượng (Object Storage) dùng để chứa file tĩnh Frontend và hình ảnh/avatar/tài liệu của học viên và PT.  
- *Application Load Balancer (ALB)*: Bộ cân bằng tải phân phối traffic thông minh từ Public Subnet vào Private Subnet.  
- *Amazon EC2 (Node.js + Express, Nginx + PM2)*: Cụm máy chủ ảo chịu trách nhiệm xử lý toàn bộ logic nghiệp vụ cốt lõi.  
- *Amazon RDS (MySQL)*: Hệ quản trị cơ sở dữ liệu quan hệ cấu hình Multi-AZ, lưu thông tin cốt lõi (User, Gói tập, Hóa đơn). 
- *Amazon DynamoDB*: Cơ sở dữ liệu NoSQL tốc độ cao chịu trách nhiệm lưu log quẹt thẻ check-in và metadata hệ thống.
- *Amazon ElastiCache (Redis)*: Bộ nhớ đệm lưu session đăng nhập và cache lịch học/lịch PT để giảm tải cho database chính.
- *Amazon CloudWatch & CloudWatch Alarms*: Hệ thống giám sát tài nguyên và tự động kích hoạt trạng thái cảnh báo khi gặp sự cố.
- *Amazon SNS (Simple Notification Service)*: Dịch vụ gửi thông báo tự động (Email/SMS) cho đội ngũ kỹ thuật khi có báo động lỗi.
- *AWS IAM*: Quản lý và phân rã quyền truy cập bảo mật giữa các tài nguyên bên trong hệ thống đám mây.
*Thiết kế thành phần*  
- Tầng Biên & Bảo mật (Edge Layer): Khách hàng (User) tương tác thông qua Route 53 và AWS WAF để đảm bảo request sạch trước khi tới CloudFront CDN.
- Tầng Giao diện (Frontend Layer): Toàn bộ giao diện React.js được đóng gói tĩnh trong S3 và phân phối siêu tốc qua CloudFront.
- Tầng Mạng & Điều hướng (Network & Routing Layer): Thiết lập một mạng ảo VPC. ALB nằm tại Public Subnet để làm cổng đón request động (API), trong khi toàn bộ máy chủ tính toán và database được giấu hoàn toàn trong các Private Subnet độc lập để ngăn chặn hacker tiếp cận trực tiếp.
- Tầng Tính toán (Compute Layer): Gồm 2 máy chủ EC2 chạy Node.js đặt tại 2 Availability Zone khác nhau, bọc ngoài bởi Auto Scaling Group để tự động nhân bản máy chủ khi CPU quá tải.
- Tầng Dữ liệu (Data Layer): Phân bóc rạch ròi 3 nhóm: RDS MySQL (Dữ liệu quan hệ vững chắc) - DynamoDB (Log ghi liên tục) - Redis (Bộ nhớ đệm siêu tốc trên RAM).
- Tầng Vận hành (Ops Layer): CloudWatch liên tục hứng metrics của EC2/RDS, nếu có lỗi sẽ kích hoạt Alarm chuyển tiếp qua SNS để bắn Email/SMS trực tiếp cho người quản trị.

### 4. Triển khai kỹ thuật  
- *Frontend*: Triển khai mã nguồn React.js lên Amazon S3 dưới dạng Static Website Hosting, cấu hình Invalidations trên CloudFront để cập nhật giao diện ngay lập tức khi có phiên bản mới.
- *Backend*: Sử dụng AWS CDK hoặc Console để khởi tạo môi trường VPC 2 AZs. Cài đặt các EC2 instance với Ubuntu Image, cấu hình Nginx làm Reverse Proxy hướng traffic về cổng chạy của Node.js, sử dụng PM2 để quản lý tiến trình (Process) chạy ngầm và tự động khởi động lại ứng dụng nếu xảy ra lỗi Crash.
- *Bảo mật mạng*: Thiết lập Security Group cấu hình khắt khe: ALB chỉ nhận cổng 80/443 từ CloudFront; EC2 Security Group chỉ nhận traffic duy nhất từ ALB; RDS/Redis Security Group chỉ nhận kết nối nội bộ từ cụm EC2 Security Group.
### 5. Lộ trình & Mốc triển khai  
- *Tuần 1* - Lập kế hoạch & Thiết kế: Khởi tạo tài liệu cấu trúc dữ liệu, chuẩn bị thiết kế các bảng cơ sở dữ liệu (MySQL Schema), hoàn thiện sơ đồ Draw.io.
- *Tuần 2* - Triển khai Giao diện & Hạ tầng mạng: Cấu hình mạng VPC, thiết lập Public/Private Subnets. Upload Frontend lên S3 + CloudFront và trỏ domain qua Route 53.
- *Tuần 3* - Phát triển API & Đóng gói Backend: Cài đặt Node.js, Nginx, PM2 trên EC2. Khởi tạo cơ sở dữ liệu RDS MySQL, DynamoDB và cấu hình bộ nhớ đệm Redis. Kết nối các luồng tương tác dữ liệu (Mũi tên 5).
- *Tuần 4* - Tối ưu, Giám sát & Kiểm thử: Thiết lập CloudWatch Alarms và Amazon SNS. Thực hiện kiểm thử tải (Load Testing) giả lập kịch bản học viên check-in giờ cao điểm để kiểm tra tính năng co giãn của Auto Scaling Group. Bàn giao hệ thống lên Production.

### 6. Ước tính ngân sách  
- Dựa trên công cụ tính toán giá AWS Pricing Calculator áp dụng cho quy mô dự án vừa và tận dụng gói AWS Free Tier 12 tháng đầu tiên:
- Amazon EC2 (Backend): ~0.00 USD (Nếu sử dụng 1 instance t2.micro/t3.micro nằm trong gói Free Tier) hoặc ~15.00 USD/tháng cho cụm Auto Scaling khi co giãn thực tế.
- Amazon RDS (MySQL): ~0.00 USD (Sử dụng db.t2.micro/db.t3.micro miễn phí 750 giờ/tháng thuộc Free Tier).
- Amazon S3 & CloudFront: ~0.50 USD/tháng (Cho khoảng 10-20 GB hình ảnh học viên và băng thông truyền tải dữ liệu).
- Amazon DynamoDB & ElastiCache: ~1.00 USD/tháng (Tận dụng mức dung lượng 25GB miễn phí vĩnh viễn của DynamoDB).
- AWS WAF & Route 53: ~1.50 USD/tháng (Chi phí quản lý Hosted Zone và số lượng request quét cơ bản).
- CloudWatch & SNS: ~0.00 USD (Nằm hoàn toàn trong hạn mức Free Tier mặc định).
- Tổng chi phí ước tính: Dao động từ 2.00 USD - 18.00 USD / tháng tùy thuộc vào lượng học viên thực tế truy cập và sử dụng dịch vụ.
### 7. Đánh giá rủi ro  
*Rủi ro 1*: Tấn công DDoS hoặc brute-force vào hệ thống đăng nhập của học viên.
Chiến lược giảm thiểu: Đã thiết lập AWS WAF đứng chặn đầu luồng để giới hạn số lượng request từ một IP (Rate-limiting) và chặn đứng botnet nguy hiểm.

*Rủi ro 2*: Sự cố sập một vùng trung tâm dữ liệu (Data Center Outage) của AWS.
Chiến lược giảm thiểu: Hạ tầng mạng thiết kế dạng Multi-AZ (2 AZs trở lên), nếu Availability Zone 1 gặp sự cố, ALB sẽ tự động ngắt và chuyển toàn bộ học viên sang cụm EC2 và RDS Standby ở Availability Zone 2 ngay lập tức mà không gây gián đoạn dịch vụ.

*Rủi ro 3*: Chi phí tăng vọt ngoài tầm kiểm soát khi bị spam hệ thống.
Chiến lược giảm thiểu: Cấu hình CloudWatch Budget Alarms gửi mail cảnh báo ngay khi chi phí chạm ngưỡng vượt mức 20 USD.

### 8. Kết quả kỳ vọng  
- Hệ thống website phòng gym vận hành trơn tru 24/7 với độ ổn định (Uptime) đạt 99.99%.

- Trang web có khả năng đáp ứng đồng thời từ hàng trăm đến hàng ngàn lượt check-in và đặt lịch PT cùng một thời điểm vào giờ cao điểm mà không gặp hiện tượng giật lag hay phản hồi chậm.

- Bảo mật thông tin cá nhân và dữ liệu thanh toán của học viên phòng tập theo tiêu chuẩn đám mây an toàn, tạo dựng uy tín và sự chuyên nghiệp tuyệt đối cho thương hiệu phòng gym của bạn.