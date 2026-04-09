# Individual reflection — Đỗ Xuân Bằng

## 1. Role
Backend Engineer (Agent Tools). Phụ trách tạo Database ảo và đặc tả định dạng Tools API.

## 2. Đóng góp cụ thể
- Cấu trúc đặc tả Function Tooling JSON (Schema) cho 4 service: lịch, điểm học, lịch thi, tra học phí.
- Hiện thực Execution Engine dịch response JSON của AI thành lệnh code chạy hàm python thực tế.
- Khởi tạo Mock Data sinh viên (dictionaries), xử lý format tiền VNĐ, filter mảng kết quả.

## 3. SPEC mạnh/yếu
- Mạnh nhất: Thực thi Deterministic. Rút ngắn Hallucination 100% nhờ việc code Data python bóc tách sẵn rồi mới nhờ AI dịch lại cho trơn tru.
- Yếu nhất: Code đổ data trả về gộp hết lịch học 4 năm vào một Response cực dài ngốn Context Window lớn. 

## 4. Đóng góp khác
- Thiết kế object data point phù hợp chung với yêu cầu luồng Prompt.
- Filter, clean format `tool_used` thành object mảng tiêu chuẩn để nối sang LangGraph của Việt Anh.

## 5. Điều học được
Thiết kế JSON schema description càng cặn kẽ thì AI Tooling chạy càng ổn định. Phương diện lập trình tách lớp giữa Data Retrieval (code) và Synthesis (model) cho hiệu suất cao hơn nhồi AI sinh số ảo.

## 6. Nếu làm lại
Xóa toàn bộ list Mock Data và thay bằng SQLAlchemy Query móc vào Postgres. Dùng kiến trúc BaseTool của Langchain thay vì tự Build JSON arguments thô để có Validator (Pydantic) báo lỗi xịn hơn.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Render cả ngàn dòng dữ liệu Mock DB fake (điểm, môn, tiền tệ) sinh động chỉ sau một prompt. Viết sẵn khung catch error.
- **Sai/mislead:** Cục logic `tool_calls.arguments` OpenAI trả ra là stringified JSON nhưng AI quên không bọc `json.loads` vào code execute làm lỗi runtime crash server. Phải tự tay bắt try-catch parse string.
