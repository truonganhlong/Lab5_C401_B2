# Phản hồi đánh giá dự án (Cross-team Feedback)

| Nhóm đánh giá | TC1 | TC2 | TC3 | Điểm mạnh / Nhận xét tích cực | Điểm yếu / Góp ý cải thiện |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `VinUni_B1` | 3 | 5 | 2 | Trình bày rõ mục tiêu ý tưởng. | Ý tưởng chưa được thiết thực, điểm đau chưa đúng (thời gian đọc tin nhắn thông báo chỉ tốn 1-2 phút và việc tóm tắt này giáo viên chỉ mất 10 phút tóm tắt lại từ thông báo sang tin nhắn cho phụ huynh), phần phân loại tin nhắn cao/trung/thấp chỉ để đặt lịch thông báo trong khi giáo viên chỉ cần nhắn 1 câu vào trong Group là xong. |
| `VinUni_A1` | 3 | 5 | 3 | Chatbot không trả lời dữ liệu không có trong data, khi trả lời dữ liệu đã có thì chính xác. | Load lâu ~30s, ngang với thời gian không cần chatbot mà truy vấn database bình thường còn nhanh hơn vì data hiện tại test không quá nhiều. |
| `VinUni_X100` | 5 | 5 | 4 | Bài toán rất thực tế và cần thiết cho sinh viên. | Chưa xử lý fallback tốt. |
| `VinUni_A2` | 5 | 5 | 5 | Ý tưởng hay nếu làm tốt sẽ giải quyết được vấn đề của sinh viên hiện tại. Demo tốt AI không bị Hallucination (đề tài tốt). | Câu hỏi chung chung của user "hàm sigmoid" liên quan đến khóa học nhưng không search trên web (tool search web). |

---

## Chú thích Tiêu chí chấm điểm
- **TC1: Problem-solution fit:** Bài toán có thật, giải pháp hợp lý.
- **TC2: AI product thinking:** Auto/Augmentation phân định rõ, failure modes có cân nhắc, eval metrics có ý nghĩa.
- **TC3: Demo quality:** Dàn source code/flow chạy được, trình bày rõ ràng, trả lời câu hỏi phản biện tốt.

## Tổng hợp nhận xét chung

### 🌟 Điểm mạnh:
- **Tính thực tiễn (TC1):** Nhóm X100, A2 đánh giá cao bài toán, ghi nhận dự án giải quyết được những vấn đề cấp thiết, thực tế cho sinh viên.
- **Tính an toàn của hệ thống (TC2, TC3):** Hệ thống demo cho thấy có kiểm soát Hallucination tốt. AI nhận diện và từ chối trả lời những gì không có trong cơ sở dữ liệu thay vì "bịa" đáp án, đồng thời trả lời rất chính xác nội dung sẵn có.

### 🚧 Điểm cần khắc phục & Cải thiện:
1. **Mô hình hóa điểm đau (Pain points - TC1):** Phần thuyết phục về nỗi đau người dùng cần sắc bén hơn. Cần tìm các quy trình mà chatbot thật sự tiết kiệm một lượng lớn thời gian (như trong feedback của B1 chỉ ra rằng cách làm thông báo thực tế hiện nay không chênh lệch quá nhiều công sức).
2. **Xử lý hụt luồng (Fallback - TC2):** 
   - Thiếu tính linh hoạt với các truy vấn chung chung nhưng có ích. Ví dụ câu hỏi "hàm sigmoid" là các khái niệm ngoài phạm vi nội bộ nhưng cần thiết trong học tập, nhóm có thể kết hợp thêm **Web Search Tool** chứ không nhất thiết phải chặn đứng và từ chối.
3. **Tốc độ phản hồi (Performance - TC3):** API hoặc pipeline RAG đang chạy khá chậm (~30s). Cần tối ưu hóa vì ở bài toán demo data ít, người dùng thà tra cứu DB tay có khi lại nhanh hơn dùng Chatbot.
