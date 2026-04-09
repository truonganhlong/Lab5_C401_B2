# Design Iterations - Vin AI Student Assistant

## Overview
Tài liệu này ghi lại quá trình phát triển và điều chỉnh thiết kế của dự án Student Assistant, đặc biệt tập trung vào những thay đổi hướng đi chính để giải quyết vấn đề bảo mật dữ liệu.

---

## Iteration 1: Initial Approach (ID-Based Access)

### Thiết kế ban đầu
- Sinh viên có thể tra cứu lịch học, lịch thi bằng cách **nhập mã ID sinh viên**
- Không yêu cầu xác thực hoặc đăng nhập
- System trả về dữ liệu dựa trên ID được nhập

### Vấn đề chính: Data Privacy Breach Risk
**Rủi ro lớn nhất**: Bất kỳ sinh viên nào cũng có thể nhập ID của sinh viên khác để xem lịch học, lịch thi, và các thông tin riêng tư khác của họ.

**Hậu quả**:
- Lộ lọt dữ liệu cá nhân (personal schedule information)
- Vi phạm quyền riêng tư sinh viên
- Tiềm ẩn nhiều vấn đề về pháp lý và tin cậy từ người dùng
- Không có cách để kiểm soát ai đang truy cập dữ liệu nào

---

## Iteration 2: Authentication-Based Access (Current Solution)

### Thiết kế mới
Sau cuộc họp nhóm bàn luận, chúng tôi quyết định thay đổi hoàn toàn chiến lược để **từ ID-based sang Account-based (Authentication-based)**.

#### 1. **Student Accounts** (Sinh viên bình thường)
- Mỗi sinh viên có **một account riêng** với thông tin đăng nhập
- Khi đăng nhập và tra cứu dữ liệu (lịch học, lịch thi, điểm, ...), hệ thống sử dụng **Account ID** để kiểm tra
- Sinh viên **chỉ có thể xem dữ liệu của chính mình**, không thể truy cập dữ liệu của sinh viên khác
- Mỗi yêu cầu được xác thực và phân quyền dựa trên account hiện tại

#### 2. **Admin Account** (Quản trị viên)
- Admin có **một tài khoản đặc biệt** với quyền cao hơn
- Quyền của Admin:
  - ✓ Xem dữ liệu của **tất cả sinh viên** (cho mục đích quản lý)
  - ✓ Upload tài liệu mới (bài giảng, tài nguyên học tập)
  - ✓ Xóa hoặc cập nhật tài liệu cũ
  - ✓ Quản lý tài khoản sinh viên (reset password, kích hoạt, vô hiệu hóa)

### Lợi ích chính

| Yếu tố | Trước | Sau |
|--------|--------|-------|
| **Bảo mật** | Không có xác thực | Yêu cầu đăng nhập |
| **Kiểm soát truy cập** | Bất cứ ai có thể xem bất kỳ dữ liệu nào | Mỗi user chỉ xem được dữ liệu của chính mình |
| **Audit trail** | Không thể theo dõi | Có thể theo dõi ai truy cập dữ liệu gì lúc nào |
| **Quản lý dữ liệu** | Không có cơ chế | Admin có toàn quyền quản lý |
| **Tuân thủ** | Vi phạm quyền riêng tư | Tuân thủ các tiêu chuẩn bảo mật |

---

## Architectural Changes

### Backend
- **Authentication Service**: Xác thực người dùng khi đăng nhập
- **Authorization Middleware**: Kiểm tra quyền truy cập dựa trên account ID
- **User Roles**: Phân biệt giữa role `student` và role `admin`
- **Audit Logging**: Ghi lại các hành động truy cập dữ liệu để theo dõi

### Frontend
- **Login Page**: Bắt buộc sinh viên phải đăng nhập
- **Session Management**: Duy trì session đăng nhập của người dùng
- **Personalized Dashboard**: Hiển thị dữ liệu riêng của sinh viên
- **Admin Panel**: Giao diện riêng cho admin với các tính năng quản lý

### Database
- **Users Table**: Lưu thông tin đăng nhập (username, hashed password, role)
- **User Profiles**: Liên kết dữ liệu sinh viên với account
- **Audit Logs**: Ghi lại các hoạt động truy cập


## Conclusion

Việc chuyển từ thiết kế ID-based sang Account-based là một bước tiến quan trọng trong việc bảo vệ dữ liệu riêng tư của sinh viên. Giải pháp này không chỉ giải quyết vấn đề bảo mật hiện tại mà còn tạo nền tảng vững chắc cho các tính năng quản lý tiên tiến trong tương lai.
