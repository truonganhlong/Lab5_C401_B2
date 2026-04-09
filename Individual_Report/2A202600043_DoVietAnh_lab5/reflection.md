# Individual reflection — Đỗ Việt Anh

## 1. Role
AI Core Architect & Spec Reviewer. Phụ trách xây dựng kiến trúc State Machine Chatbot bằng LangGraph, thiết lập LLM Router, định hình Eval Metrics và đánh giá chéo.

## 2. Đóng góp cụ thể
- Dựng State Machine (LangGraph) gồm 5 node xử lý chính: route, RAG, verify_auth (xác thực Session/Role), agent tools, final response (`backend/app/assistant_graph.py`).
- Dùng OpenAI Tool Calling làm bộ định tuyến (Router) và xử lý ngắt luồng (interrupt) chỉ áp dụng cho quyền Admin tra cứu chéo, thay vì bắt ép mọi sinh viên tự gõ MSSV.
- Code luồng xử lý riêng cho General Chat và Fallback giúp tiết kiệm token, tránh ảo giác và đảm bảo an toàn.
- Chịu trách nhiệm thiết kế bộ tiêu chuẩn đo lường Eval Metric cho file `spec-final.md`.
- Đi tham khảo, chấm chéo và viết bảng tổng hợp đánh giá `feedback.md` cho các nhóm khác.

## 3. SPEC mạnh/yếu
- **Mạnh nhất:** Chia rẽ các luồng RAG và Agent Tools cụ thể rõ ràng; Node xác thực (Auth) tự động lấy mã từ Session giúp bảo mật tốt dữ liệu cá nhân, giữ interrupt cho đúng user case (ví dụ Admin).
- **Yếu nhất:** Router hiện tại hoàn toàn lệ thuộc LLM (Zero-shot) dẫn đến dễ bị dội độ trễ (latency); quy trình bắt ID từ chuỗi đôi khi bằng regex nội bộ của Admin còn hơi cứng nhắc.

## 4. Đóng góp khác
- Trực tiếp review và kết nối code RAG + Tool của các module rải rác lại với nhau bằng Node trong Graph.
- Xử lý State gom mảng kết quả, lọc duplicate source trả về từ nhiều Tools nhằm mang lại output sạch và gọn nhất.

## 5. Điều học được
Chuyển đổi hoàn toàn tư duy từ "viết prompt thuần túy" sang thiết kế Kiến trúc AI Workflow. Việc sử dụng LangGraph cho phép chia nhỏ bài toán phức tạp thành nhiều state riêng biệt, thay vì ép LLM phải "gồng gánh" xử lý mọi thứ trên một cửa sổ Context duy nhất. Qua việc đi chấm điểm chéo, tôi nhận ra Metric không chỉ là thông số dev, nó là thứ định hình trải nghiệm người dùng thực.

## 6. Nếu làm lại
Sẽ thay thế `InMemorySaver` bằng Persistent Database như SQLite để tự động bảo lưu lịch sử hội thoại ngay cả khi crash/restart server. Đồng thời bổ sung thêm ví dụ Few-shot cho system prompt để Router phán đoán đa ý định (Mix-intent) chuẩn xác và nhanh hơn.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Hỗ trợ xây dựng nhanh khung kiến trúc LangGraph (skeleton code), giúp rút ngắn thời gian khởi tạo hệ thống.. Hỗ trợ drafting khung template liệt kê Eval Metrics.
- **Sai/mislead:** Cung cấp code LangGraph sử dụng syntax phiên bản cũ, đặc biệt là cách dùng TypedDict không còn phù hợp với version mới → gây lỗi khi chạy, khiến quá trình phát triển bị gián đoạn do phải tự đối chiếu lại tài liệu chính thức (docs) để sửa và cập nhật. Có xu hướng over-engineering, đề xuất hệ thống gồm nhiều Agent Node phức tạp, gọi chéo nhau không cần thiết, dễ dẫn đến scope creep, làm dự án vượt khỏi phạm vi MVP ban đầu.
