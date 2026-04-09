# Individual reflection — Trương Anh Long

## 1. Role
RAG Engineer. Đảm nhận luồng Ingestion (chế biến tài liệu) và Retrieval (truy xuất) PDF.

## 2. Đóng góp cụ thể
- Setup luồng embedding với FAISS vector store In-memory.
- Chunking dữ liệu PDF theo window size và liên kết chặt với metadata (doc_id, title).
- Viết prompt tổng hợp nội dung RAG chỉ trả lời từ Context đã bóc tách.

## 3. SPEC mạnh/yếu
- Mạnh nhất: Gắn chặt metadata ở DB giúp chatbot luôn trích xuất ngược lại Citation minh bạch, tăng độ Trust.
- Yếu nhất: Chunking text cố định có thể vô tình chia cắt giữa câu. Chưa dùng Persistent DB nên sập nguồn là sập cả database.

## 4. Đóng góp khác
- Code backend nhận diện PDF upload trực tiếp đẩy vào FAISS index (Hỗ trợ Linh).
- Mockups array Text JSON ban đầu để test truy xuất trước khi hoàn thiện tính năng bóc text máy.

## 5. Điều học được
Việc chuẩn bị tài liệu (Retrieval - tìm cái gì) quan trọng không kém việc Generation (sinh văn bản). Chọn Top K và Threshold distance ảnh hưởng đến 80% chất lượng của bộ RAG. 

## 6. Nếu làm lại
Sử dụng ChromaDB hoặc Qdrant thay mảng FAISS In-memory để quản lý dữ liệu dai dẳng. Đổi chunking tay bằng Semantic Chunking để phân tách nội dung hiểu theo cụm nghĩa.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Cung cấp cấu trúc class model và logic toán tính Distance cho Vector cực trơn tru.
- **Sai/mislead:** AI làm lệch logic metadata FAISS trong code xóa Document (ID vector và ID metadata bị bất đồng bộ). Phải tự trace bug để rebuild lại mảng index.
