# Individual reflection — Lã Thị Linh

## 1. Role

Phụ trách API Server, cấu hình hệ thống và tinh chỉnh câu lệnh cho AI.

## 2. Đóng góp cụ thể

- Setup toàn bộ khung API chạy bất đồng bộ bằng **FastAPI** (`/auth`, `/chat`, `/documents`).
- Thiết kế và phân vùng 4 cụm System Prompts lớn thành cấu trúc [Persona - Rule - Capabilites - Constraint ].
- Xây dựng các hàm dọn dẹp văn bản như clean_response_text trong backend/app/text_utils.py để làm sạch đầu ra của AI.

## 3. SPEC mạnh/yếu

- Mạnh nhất: Cấu trúc Prompt phân quyền siêu chặt. Việc tách rõ các Constraints (Đừng làm X Y) rất hiệu quả trong việc khoá mồm chatbot không chém gió bay bổng ngoài vùng an toàn.
- Yếu nhất: Luồng Authentication viết chay, xử lý I/O gọi AI theo Sync-call dễ bị treo API nếu đồng thời có vài chục request gõ vào Chatbot.

## 4. Đóng góp khác

- Review tune thông số `Temperature` linh hoạt giữa nhánh Router (chắc chắn, t=0) và Nhánh General (linh hoạt, t=0.7).
- Hỗ trợ dọn requirements rác và gắn session trả qua front-end của Long an toàn.

## 5. Điều học được

Kỹ nghệ Prompt (Prompt Engineering) không phải viết chuỗi văn xuôi dài dòng, mà là viết "Code bằng tiếng Anh". Liệt kê rõ ràng định dạng Array list giúp LLM nghe lời hơn cách chia đoạn văn thông thường.

## 6. Nếu làm lại

Sẽ setup Dependency Injections để chặn Route JWT cho API thay vì custom session yếu ớt. Bọc các luồng I/O OpenAI thành Task bất đồng bộ (Background Tasks) thay vì Async giả. Trích xuất prompts ra file DB/YAML ngoài để config nhanh hơn thay vì Hard-code vô Python.

## 7. AI giúp gì / AI sai gì

- **Giúp:** Khởi tạo API Router của FastAPI cực lẹ, setup CORS và viết class Pydantic BaseModel không hụt biến nào.
- **Sai/mislead:** Data train của AI thường là model cũ nên quên update các System Constraints về model có reasoning logic mới. Hậu quả là Bot phun trào thẻ suy nghĩ `<think>` lên giao diện. Mình phải cập nhật bằng tay lệnh cấm thẻ đặc biệt.
