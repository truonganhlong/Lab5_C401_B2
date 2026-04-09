# Research Notes

## 1. Problem Framing

Khi xây dựng project `student_assistant`, tôi xác định hệ thống cần xử lý hai nhóm truy vấn khác nhau:

- Truy vấn kiến thức chung của trường như quy chế, hướng dẫn, thủ tục, tài liệu học vụ
- Truy vấn dữ liệu cá nhân như lịch học, bảng điểm, lịch thi, học phí

Tôi không chọn cách để một prompt duy nhất xử lý toàn bộ, vì hai nhóm dữ liệu này có bản chất khác nhau:

- Tài liệu học vụ là dữ liệu phi cấu trúc, phù hợp với RAG
- Dữ liệu cá nhân là dữ liệu có cấu trúc, nên tôi cho đi qua tool để hạn chế hallucination

## 2. Routing Strategy

Sau khi tìm hiểu, tôi chọn hướng dùng LLM router để chia câu hỏi thành ba nhánh:

- `RAG path`: cho câu hỏi cần tra cứu tài liệu
- `agent tools path`: cho câu hỏi cần đọc dữ liệu sinh viên
- `general/fallback path`: cho câu hỏi xã giao hoặc ngoài phạm vi

Trong thiết kế này:

- Nếu câu hỏi liên quan quy định, thủ tục, học vụ chung thì router ưu tiên `search_documents`
- Nếu câu hỏi liên quan lịch học, điểm, lịch thi, học phí thì router chọn tool tương ứng
- Nếu không có tool phù hợp thì hệ thống trả về general chat hoặc fallback

Tôi chọn cách này vì nó tách rõ trách nhiệm của từng nhánh, giúp hệ thống dễ kiểm soát, dễ mở rộng và dễ debug hơn.

## 3. RAG Design Notes

Phần RAG hiện tôi tách thành ba phần chính:

- `ingestion.py`
- `retrieval.py`
- `generator.py`

Luồng tôi đang triển khai là:

1. Admin upload PDF
2. Hệ thống trích xuất text từ PDF
3. Text được chia thành nhiều chunk
4. Mỗi chunk được embedding bằng model Jina
5. Vector được lưu vào FAISS cùng metadata
6. Khi user đặt câu hỏi, query cũng được embedding và search top-k trong FAISS
7. Các chunk phù hợp được ghép thành context để model sinh câu trả lời grounded

Khi rà lại code hiện tại, tôi ghi nhận:

- Chunking đang dùng `chunk_size=500` và `overlap=100`
- Logic đang đo bằng `len(...)`, nên thực tế hiện tại gần với `500 ký tự` và `100 ký tự overlap`, chưa phải 500 từ
- Embedding model đang dùng là `jina-embeddings-v5-text-small`
- Vector store đang dùng là `FAISS IndexFlatIP`
- Retrieval hiện lấy `top_k = 5` và lọc theo `relevance threshold = 0.5`

Tôi chọn cách làm này vì nó đơn giản, phù hợp để demo, và đủ rõ ràng để kiểm soát được chất lượng retrieval ở giai đoạn hiện tại.

## 4. Agent Tool Design Notes

Với các truy vấn dữ liệu cá nhân, tôi không cho đi qua RAG mà cho đi qua tool, ví dụ:

- `get_schedule`
- `get_grades`
- `get_exam`
- `get_tuition`

Lý do là các dữ liệu này có cấu trúc rõ ràng và cần độ chính xác cao. Nếu để model tự suy luận bằng prompt thì rủi ro trả sai thông tin sẽ lớn hơn.

Trong graph hiện có bước thu thập `student_id`, nhưng khi đối chiếu lại toàn bộ flow backend, tôi xác định logic đúng phải là:

- Nếu đăng nhập bằng account `student`, backend sẽ tự lấy `student_id` từ account đăng nhập
- Trong trường hợp này, user không nên phải nhập lại MSSV khi hỏi điểm, lịch học, lịch thi, học phí
- Nếu đăng nhập bằng account `admin`, admin mới là người có thể nhập MSSV để tra cứu sinh viên bất kỳ

Nói cách khác, trong thiết kế hiện tại của tôi:

- `student login` -> tra cứu dữ liệu cá nhân của chính mình
- `admin login` -> có thể truyền hoặc nhập MSSV để tra cứu dữ liệu của sinh viên khác

Tôi xem đây là một điểm quan trọng về phân quyền. Backend không nên tin `student_id` do client tự gửi lên nếu user là sinh viên.

## 5. Observations From Current Flow

Sau khi rà lại các file flow chính, tôi rút ra một số nhận xét:

- Kiến trúc hiện tại đã tách khá rõ giữa tri thức chung và dữ liệu cá nhân
- `assistant_graph` giúp tôi quản lý luồng xử lý rõ ràng hơn so với việc nhét toàn bộ logic vào một hàm lớn
- Với role `student`, `student_id` thực tế được bind theo session đăng nhập chứ không theo input tự do từ client
- Cách này giúp tránh việc một sinh viên nhập MSSV của người khác để xem dữ liệu ngoài quyền hạn
- RAG hiện mới trả source ở mức tài liệu, chưa xuống được tới page hoặc đoạn cụ thể trong PDF

## 6. Current Limitations

Trong bản hiện tại, tôi thấy còn một số hạn chế:

- Chunking đang dựa trên character count nên chưa thật sự tối ưu theo ngữ nghĩa
- PDF parser hiện flatten text nên có thể làm mất cấu trúc heading, bảng, hoặc format gốc
- Citation hiện mới ở mức document-level
- Router có thể chọn sai khi câu hỏi pha trộn giữa quy chế và dữ liệu cá nhân
- Tôi chưa có bộ evaluation rõ ràng để benchmark chất lượng router và retrieval

## 7. Possible Improvements

Nếu phát triển tiếp, tôi muốn cải thiện theo các hướng sau:

- Chuyển chunking sang token-based hoặc word-based để ổn định hơn
- Bổ sung page-level citation hoặc span-level citation
- Tạo bộ test queries cho router
- Log các truy vấn thất bại để cải thiện prompt và retrieval
- Thêm tính năng `like/dislike` cho mỗi câu trả lời của chatbot
- Với phản hồi `dislike`, hệ thống sẽ lưu lại câu hỏi và câu trả lời tương ứng để gom thành dữ liệu phục vụ việc phân tích lỗi, điều chỉnh prompt, cải thiện retrieval và nâng chất lượng phản hồi về sau
- Tách UX rõ hơn giữa flow của `student` và `admin` khi cần MSSV

## 8. Conclusion

Qua quá trình thiết kế và triển khai, tôi thấy kiến trúc hiện tại phù hợp với bài toán trợ lý sinh viên:

- Dùng RAG cho tri thức chung
- Dùng tool cho dữ liệu cá nhân
- Dùng session và role để kiểm soát `student_id`

Điểm tôi đánh giá quan trọng nhất là không để sinh viên tự do truyền MSSV khi tra cứu dữ liệu cá nhân. Trong flow hiện tại, phần nhập MSSV nên được hiểu là use case dành cho admin hoặc trường hợp đặc biệt, không phải flow mặc định của sinh viên sau khi đã đăng nhập.
