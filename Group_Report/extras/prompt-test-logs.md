# Prompt Test Logs

## Test 1
**Query:** Quy chế về học bổng như nào  
**Expected:** Hệ thống đi theo nhánh RAG, tìm thông tin từ tài liệu nội bộ và trả lời có nguồn.  
**Actual:** Chatbot trả lời rằng trong tài liệu hiện có chỉ tìm thấy một số thông tin liên quan đến học bổng, nhưng chưa có “quy chế học bổng” đầy đủ. Câu trả lời có trích nguồn từ tài liệu nội bộ.  
**Observation:** Kết quả hợp lý với trạng thái dữ liệu hiện tại. Hệ thống không bịa thông tin khi tài liệu chưa đầy đủ.  
**Assessment:** Pass

## Test 2
**Query:** lịch học của tôi như nào  
**Expected:** Router chọn agent tool lấy lịch học cá nhân, không yêu cầu nhập lại MSSV vì user đã đăng nhập bằng account sinh viên.  
**Actual:** Chatbot trả về lịch học theo từng ngày trong tuần, gồm giờ học, môn học, phòng học và giảng viên. Hệ thống không hỏi lại MSSV.  
**Observation:** Flow phân quyền hoạt động đúng. Khi user đã đăng nhập bằng account sinh viên, hệ thống dùng luôn MSSV gắn với account.  
**Assessment:** Pass

## Test 3
**Query:** Gợi ý cho tôi một vài địa điểm du lịch tại Hà Nội  
**Expected:** Vì đây là câu hỏi ngoài phạm vi student assistant, hệ thống nên fallback và thông báo giới hạn chức năng.  
**Actual:** Chatbot trả lời rằng nội dung này nằm ngoài phạm vi hỗ trợ, đồng thời nhắc lại các nhóm nội dung mà hệ thống hiện hỗ trợ như tài liệu nội bộ và thông tin cá nhân sinh viên.  
**Observation:** Fallback hoạt động đúng, giúp giữ phạm vi sản phẩm rõ ràng thay vì trả lời lan man ngoài domain.  
**Assessment:** Pass

## Test 4
**Query:** Chiến tranh Trung Đông hiện tại như thế nào?  
**Expected:** Vì đây là câu hỏi ngoài phạm vi student assistant, hệ thống nên từ chối ngay hoặc chuyển thẳng sang fallback rõ ràng, không nên gợi ý user đào sâu thêm.  
**Actual:** Chatbot ban đầu vẫn gợi ý user có thể hỏi tiếp về chiến tranh ở khu vực nào hoặc khía cạnh nào, nhưng nếu tiếp tục nhắn thì hệ thống vẫn từ chối trả lời vì ngoài phạm vi hỗ trợ.  
**Observation:** Đây là một hành vi chưa tối ưu. Việc gợi ý user hỏi sâu hơn trong khi hệ thống không hỗ trợ nội dung đó làm tăng số lượt trao đổi không cần thiết và gây tốn thêm chi phí model.  
**Assessment:** Needs improvement

**Suggested fix:** Với các truy vấn ngoài phạm vi rõ ràng như thời sự, chiến tranh, du lịch, giải trí hoặc tin tức, hệ thống nên fallback ngay từ lượt đầu, không gợi ý user tiếp tục đào sâu thêm.
