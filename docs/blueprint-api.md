## 4. Chi tiết Các Điểm Cuối API (API Endpoints)

---

### 4.1. Auth Service (`auth-service`)
* **Cổng chạy độc lập**: `8081`
* **Tiền tố qua Gateway**: `/api/auth`

| Method | Endpoint | Mô tả | Yêu cầu bảo mật |
| :--- | :--- | :--- | :--- |
| `POST` | `/auth/login` | Đăng nhập hệ thống, trả về mã định danh JWT. | **Public** |
| `POST` | `/auth/register` | Đăng ký tài khoản người dùng mới (tùy chọn). | **Public** |

---

### 4.2. Course Service (`course-service`)
* **Cổng chạy độc lập**: `8082`
* **Tiền tố qua Gateway**: `/api/courses`

#### API Công Khai & Quản Trị (Lộ ra ngoài Gateway)

| Method | Endpoint | Mô tả | Yêu cầu bảo mật |
| :--- | :--- | :--- | :--- |
| `GET` | `/courses` | Lấy danh sách môn học, hỗ trợ tìm kiếm và phân trang. | **Public** |
| `GET` | `/courses/{id}` | Xem thông tin chi tiết của một môn học cụ thể. | **Public** |
| `POST` | `/courses` | Thêm mới một môn học vào hệ thống. | Quyền `ADMIN` |
| `PUT` | `/courses/{id}` | Cập nhật thông tin môn học hiện có. | Quyền `ADMIN` |
| `DELETE` | `/courses/{id}` | Xóa môn học khỏi hệ thống. | Quyền `ADMIN` |

#### API Nội Bộ (Chỉ gọi giữa các Service - KHÔNG lộ qua Gateway)
> 

| Method | Endpoint | Mô tả |
| :--- | :--- | :--- |
| `PATCH` | `/internal/courses/{id}/reserve-seat` | Kiểm tra số lượng chỗ và trừ bớt `soChoConLai` (Transactional). |
| `PATCH` | `/internal/courses/{id}/release-seat` | Hoàn trả lại 1 chỗ trống khi học viên hủy đăng ký. |

---

### 4.3. Registration Service (`registration-service`)
* **Cổng chạy độc lập**: `8083`
* **Tiền tố qua Gateway**: `/api/registrations`

| Method | Endpoint | Mô tả | Yêu cầu bảo mật |
| :--- | :--- | :--- | :--- |
| `POST` | `/registrations` | Đăng ký học phần (hệ thống sẽ gọi ngầm sang API `/reserve-seat` của `course-service`). | Quyền `STUDENT` |
| `GET` | `/registrations/my` | Lấy danh sách các môn học mà sinh viên hiện tại đã đăng ký thành công. | Quyền `STUDENT` |
| `DELETE` | `/registrations/{id}` | Hủy đăng ký học phần (hệ thống sẽ gọi ngầm API `/release-seat` để hoàn chỗ). | Quyền `STUDENT` / `ADMIN` |

