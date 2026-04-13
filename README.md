Dabble Messenger
Dabble là một ứng dụng nhắn tin/giao tiếp toàn diện bao gồm Backend (Spring Boot) và Frontend Client (Next.js). Dự án tích hợp nhiều công nghệ như MySQL, Cassandra (cho tin nhắn), Redis, xác thực JWT, Google OAuth2, thanh toán PayPal và Cloudflare Turnstile.

🛠 Yêu cầu hệ thống (Prerequisites)
Trước khi chạy dự án, hãy đảm bảo máy của bạn đã cài đặt các công cụ sau:

Docker & Docker Compose (Bắt buộc để chạy các Database)

Java JDK 17+ (Dành cho Spring Boot)

Node.js 18+ & npm/yarn (Dành cho Next.js Client & Admin)

🐳 Bước 1: Khởi chạy Hạ tầng Database (Docker)
Thay vì cài đặt thủ công từng database, dự án sử dụng Docker để tự động hóa toàn bộ.

1. Khởi động các container
Mở terminal tại thư mục chứa file docker-compose.yml và chạy:

Bash
docker-compose up -d
(Hệ thống sẽ khởi tạo MySQL ở port 9307, Redis ở 9379 và Cassandra ở 9042).

🗄️ Bước 2: Khởi tạo Database MySQL (Migrations)
Dự án sử dụng Flyway để quản lý cấu trúc Database MySQL.

Mở IDE (IntelliJ, Eclipse...) hoặc dùng Terminal.

Đi tới folder/module app-migrations.

Chạy file Main.java.

Hệ thống sẽ tự động dọn dẹp và tạo toàn bộ các bảng cần thiết vào database dabble_huan_db trong MySQL.

🚀 Bước 3: Hướng dẫn cài đặt Backend (Spring Boot)
Backend xử lý toàn bộ logic API, WebSockets và quản lý file upload. Server sẽ chạy ở port 3366.

1. Cấu hình file application.properties
Đi tới thư mục src/main/resources/application.properties và điền các thông tin bảo mật còn thiếu (Các thông số cấu hình Database đã được thiết lập sẵn khớp với Docker):

Mail (SMTP): Nhập spring.mail.username (email của bạn) và spring.mail.password (App Password của Gmail).

JWT: Cung cấp JWT_SecretKey (chuỗi bí mật độ dài an toàn).

Cloudflare Turnstile: Cung cấp Turnstile_SecretKey.

Google OAuth2: Cung cấp google.client-id.

PayPal: Cung cấp paypal.client.id và paypal.client.secret.

2. Chạy Backend
Mở terminal tại thư mục gốc của Backend và chạy lệnh:

Bash
./mvnw spring-boot:run
Sau khi chạy thành công, bạn có thể truy cập tài liệu API (Swagger UI) tại:
👉 http://localhost:3366/

💻 Bước 4: Hướng dẫn cài đặt Frontend (Client)
Ứng dụng Client được xây dựng bằng Next.js và sẽ chạy ở port 3000.

1. Cài đặt thư viện
Mở terminal tại thư mục của Client và chạy lệnh:

Bash
npm install
# hoặc
yarn install
2. Cấu hình biến môi trường
Tạo một file .env.local ở thư mục gốc của Client và sao chép cấu hình sau vào:

Đoạn mã
# Kết nối với Backend
NEXT_PUBLIC_BE_SERVER_API=http://localhost:3366/api
NEXT_PUBLIC_BE_SERVER_WS=http://localhost:3366/ws
NEXT_PUBLIC_IMAGE_URL=http://localhost:3366/images

# Client URLs
NEXT_PUBLIC_SERVER_API=http://localhost:3000/api

# Keys của bên thứ 3 (Cần điền đầy đủ)
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id_here
NEXT_PUBLIC_TURNSTILE_SITE_KEY=your_turnstile_site_key_here
3. Chạy Client
Khởi động server phát triển của Next.js:

Bash
npm run dev
# hoặc
yarn dev
Truy cập ứng dụng Client tại:
👉 http://localhost:3000/