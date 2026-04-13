# Dabble Messenger

Dabble là một ứng dụng nhắn tin/giao tiếp toàn diện bao gồm Backend (Spring Boot) và Frontend Client (Next.js). Dự án tích hợp nhiều công nghệ như MySQL, Cassandra (cho tin nhắn), Redis, xác thực JWT, Google OAuth2, thanh toán PayPal và Cloudflare Turnstile.

## 🛠 Yêu cầu hệ thống (Prerequisites)

Trước khi chạy dự án, hãy đảm bảo máy của bạn đã cài đặt các công cụ sau:
* **Java JDK 11 hoặc 17+** (Dành cho Spring Boot)
* **Node.js 18+ & npm/yarn** (Dành cho Next.js Client & Admin)
* **MySQL 8.x** (Đang cấu hình chạy ở port `9307`)
* **Redis** (Đang cấu hình chạy ở port `9379`)
* **Apache Cassandra** (Dùng cho lưu trữ tin nhắn)

---

## 🚀 Hướng dẫn cài đặt Backend (Spring Boot)

Backend xử lý toàn bộ logic API, WebSockets và quản lý file upload. Server sẽ chạy ở port `3366`.

### Bước 1: Cấu hình Cơ sở dữ liệu
1. Khởi động **MySQL** trên port `9307`, tạo database có tên tương ứng (ví dụ: `yourdb`).
2. Khởi động **Redis** trên port `9379`.
3. Khởi động **Cassandra** và tạo keyspace tên là `dabble_messenger`.

### Bước 2: Cấu hình file `application.properties`
Đi tới thư mục `src/main/resources/application.properties`  và điền các thông tin bảo mật còn thiếu:

* **Cassandra:** `username`, `password`, `contact-points`, `port`, `local-datacenter`
* **MySQL:** `username`, `password`
* **Mail (SMTP):** Nhập `spring.mail.username` (email của bạn) và `spring.mail.password` (App Password của Gmail).
* **JWT:** Cung cấp `JWT_SecretKey` (chuỗi bí mật độ dài an toàn).
* **Cloudflare Turnstile:** Cung cấp `Turnstile_SecretKey`.
* **Google OAuth2:** Cung cấp `google.client-id`.
* **PayPal:** Cung cấp `paypal.client.id` và `paypal.client.secret`.

### Bước 3: Chạy Backend
Mở terminal tại thư mục gốc của Backend và chạy lệnh:

Đối với Maven:
```bash
./mvnw spring-boot:run

Sau khi chạy thành công, bạn có thể truy cập tài liệu API (Swagger UI) tại:
👉 http://localhost:3366/

--------------------------------------------------------------------------------------------------------------------------

💻 Hướng dẫn cài đặt Frontend (Client)
Ứng dụng Client được xây dựng bằng Next.js và sẽ chạy ở port 3000.

Bước 1: Cài đặt thư viện
Mở terminal tại thư mục của Client và chạy lệnh:

Bash
npm install
# hoặc
yarn install
Bước 2: Cấu hình biến môi trường
Tạo một file .env.local ở thư mục gốc của Client và sao chép cấu hình sau vào:

Code snippet
# Kết nối với Backend
NEXT_PUBLIC_BE_SERVER_API=http://localhost:3366/api
NEXT_PUBLIC_BE_SERVER_WS=http://localhost:3366/ws
NEXT_PUBLIC_IMAGE_URL=http://localhost:3366/images

# Client URLs
NEXT_PUBLIC_SERVER_API=http://localhost:3000/api

# Keys của bên thứ 3 (Cần điền đầy đủ)
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id_here
NEXT_PUBLIC_TURNSTILE_SITE_KEY=your_turnstile_site_key_here
Bước 3: Chạy Client
Khởi động server phát triển của Next.js:

Bash
npm run dev
# hoặc
yarn dev
Truy cập ứng dụng Client tại:
👉 http://localhost:3000/