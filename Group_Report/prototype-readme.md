# Prototype — AI Student Assistant (Trợ lý Sinh viên AI)

## Mô tả

Chatbot thông minh hỗ trợ sinh viên đại học tra cứu 2 nhóm thông tin cơ bản: (1) Dữ liệu cá nhân (lịch học, bảng điểm, lịch thi, học phí) và (2) Thông tin học vụ chung (quy chế đào tạo, thông báo). Hệ thống kết hợp cấu trúc RAG và Agent Tools thông qua kiến trúc LangGraph. Router LLM tự động nhận định luồng câu hỏi. Truy vấn cá nhân được xác thực chặt chẽ qua session theo Role (tự động lấy `student_id` nếu là Sinh viên.).

## Level: Working Prototype

- Giao diện Frontend UI Next.js + React.js + Tailwind CSS.
- FastAPI backend tích hợp đồ thị LangGraph orchestration sinh workflow tự động.
- Router LLM sử dụng OpenAI-compatible API cấu hình linh hoạt để phân luồng.
- Trình truy vấn tài liệu RAG kết nối cơ sở dữ liệu Vector FAISS + Jina Embeddings.
- Truy vấn vào database cá nhân (lịch học, bảng điểm, lịch thi, học phí) và thông tin học vụ chung (quy chế đào tạo, thông báo).

---

## Links

| Mục | Link |
|-----|------|
| **Frontend source** | `frontend/` |
| **Backend source** | `backend/app/main.py` |
| **Graph Logic Agent** | `backend/app/assistant_graph.py` |
| **Spec-final** | spec-final.md trong repo nhóm |


---

## Tools & API đã dùng

| Thành phần | Tool / API |
|---|---|
| LLM & Router | OpenRouter API / GPT-5.4-Mini (`OPENAI_MODEL="gpt-5.4-mini"` trong `.env`) |
| Orchestration | LangGraph + LangChain framework |
| RAG Embeddings | Jina AI (`jina-embeddings-v5-text-small`), Parameters: `chunk_size=500 chars`, `overlap=100`, retrieval `top_k=5 (threshold 0.5)` |
| Vector Database | FAISS IndexFlatIP (Lưu trữ dữ liệu nhúng mapping tại ổ cứng cục bộ) |
| Backend API | FastAPI + Uvicorn + Pydantic |
| Giao diện UI | Next.js 14 + React 18 + Tailwind CSS |
| Data cơ sở | Giả lập tĩnh Mock JSON / Dictionary (`mock_data/`) |

---

## Phân công chi tiết

| Thành viên | Phần phân công thực thi | Danh sách File Code đảm nhận |
|---|---|---|
| **Đỗ Việt Anh** | Xây dựng kiến trúc LangGraph, Router LLM & Fallback | `backend/app/assistant_graph.py`<br>`backend/app/router.py`<br>`backend/app/general/`<br>`backend/app/fallback/` |
| **Trương Anh Long** | Xây dựng hệ thống RAG | `backend/app/rag/`<br>`backend/app/mock_data/documents.py` |
| **Đỗ Xuân Bằng** | Viết các hàm thực thi Agent Tools & Mock DB | `backend/app/agents/tools.py`<br>`backend/app/agents/executor.py`<br>`backend/app/mock_data/students.py` |
| **Lã Thị Linh** | Xây dựng API Server, Backend Settings & System Prompts | `backend/app/main.py`<br>`backend/app/config.py`<br>`backend/app/system_prompts.py`<br>`backend/app/text_utils.py` |
| **Lê Thành Long** | Phát triển toàn bộ Hệ thống web UI, Auth và Documents | `frontend/`<br>`backend/app/documents/`<br>`backend/app/auth/` |

---
