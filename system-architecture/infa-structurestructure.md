---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Infa structurestructure

Sơ đồ kiến trúc hệ thống :

<figure><img src="../.gitbook/assets/Screenshot from 2025-09-18 16-29-15.png" alt=""><figcaption></figcaption></figure>

**Mô tả thành phần:**

* ECR (Elastic Container Registry): Code từ máy tính của developer được đóng gói vào docker image và được tải lên và lưu trữ trên dịch vụ này
* ECS (Elastic Container Service): Lấy docker image được lưu trữ từ ECR, build và quản lý các docker container được chạy trên EC2
* EC2 (Elastic Compute Cloud): Nền tảng để chạy các docker container
  * Host và xử lý các page admin và superadmin
  * Build các page user
  * Xử lý các tính toán khác
* RDS (Relational Database Service): Lưu trữ và quản lý dữ liệu. Sử dụng engine PostgreSQL
* CloudWatch: Lư​u trữ và quản lý application log, giám sát status của EC2 và RDS
* SES (Simple Email Service): Gửi các email chứa nội dung được điền trong contact form đến các địa chỉ được cài đặt
* S3 (Simple Storage Service):
  * Host các page user
  * Lưu trữ và quản lý các file tĩnh và ảnh của client
* SQS (Simple Queue Service): Hàng đợi các background job để build các page user
* Lambda:
  * Basic authentication cho trang user
  * Redirect trang user theo cài đặt trên trang admin
  * Reverse proxy: thêm hoặc bớt www vào tên miền nếu cần; định hướng URL để tìm đến trang user đúng trong S3
* CloudFront: Dịch vụ phân phối nội dung (CDN), caching để tăng tốc độ, giảm tải cho các page user
* EventBridge: Đặt lịch tự động restart các docker container hàng ngày

**Các dịch vụ bên ngoài:**

* DNS service: Cung cấp và kết nối tên miền đến CloudFront Distribution
* Google Analytics: Theo dõi và thống kê lượt truy cập và hành vi người dùng
* Google Search Console: Theo dõi và thống kê hành vi search trên trang Google của người dùng, từ đó đánh giá hiệu suất của công tác SEO
