# Danh Sách Microservices

| Service | Cổng | Database | Trách nhiệm chính |
| :--- | :--- | :--- | :--- |
| **api-gateway** | 8080 | *(Không có DB)* | Điểm vào duy nhất, định tuyến, xác thực sơ bộ, CORS |
| **auth-service** | 8081 | auth_db | Quản lý User, Student, đăng nhập, sinh/xác thực JWT |
| **course-service** | 8082 | course_db | Quản lý Course, tìm kiếm, phân trang, quản lý số chỗ |
| **registration-service** | 8083 | registration_db | Quản lý Registration, gọi sang course-service để đăng ký |

# Bảng định tuyến GateWay

| Route | Forward tới | Cơ chế bảo mật & Ghi chú |
| :--- | :--- | :--- |
| `/api/auth/**` | `http://localhost:8081` | Đường dẫn login là **Public**. Các tài nguyên khác bên trong yêu cầu mã **JWT**. |
| `/api/courses/**` | `http://localhost:8082` | Phương thức `GET` là **Public**. Các thao tác `POST / PUT / DELETE` yêu cầu quyền `ADMIN`. |
| `/api/registrations/**` | `http://localhost:8083` | Yêu cầu mã **JWT** hợp lệ với quyền truy cập là `STUDENT` hoặc `ADMIN`. |
| `/api/public/courses` | `http://localhost:8082` | Sử dụng phương thức xác thực bằng **API Key**, dành riêng cho các đối tác tích hợp bên ngoài. |