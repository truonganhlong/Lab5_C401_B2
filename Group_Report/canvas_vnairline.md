# AI Product Canvas — template

Điền Canvas cho product AI của nhóm. Mỗi ô có câu hỏi guide — trả lời trực tiếp, xóa phần in nghiêng khi điền.

---

## Canvas

### Value
- User: Khách hàng phổ thông (không rành kỹ thuật/nghiệp vụ hàng không).

    + Pain Point: Chatbot không phản hồi trực tiếp thông tin khách hàng cần. Thay vì liệt kê danh sách chuyến bay theo yêu cầu, bot lại bắt người dùng phải cung cấp các dữ liệu "ngược" (mã đặt chỗ hoặc số hiệu) mà họ vốn đang đi tìm.
    + Augmentation (Tăng cường): AI đóng vai trò hỗ trợ người dùng tìm kiếm và lựa chọn chuyến bay phù hợp nhất trong danh sách, thay vì thay thế hoàn toàn việc đặt vé (vốn vẫn cần sự xác nhận và thanh toán của con người).
    + Tiết kiệm thời gian: Hiển thị ngay danh sách chuyến bay khả dụng chỉ sau 1 câu hỏi.

    + Tăng tỷ lệ chuyển đổi: Khách hàng thấy thông tin cần thiết ngay lập tức sẽ dễ dàng tiến tới bước đặt vé hơn.

    + Trải nghiệm mượt mà: Xóa bỏ rào cản về việc phải ghi nhớ mã sân bay hay quy trình tra cứu phức tạp
- User: Hành khách (đặt vé, check-in, đổi hành trình, hỏi về quy định hành lý).
    + Pain Point: Quá tải tổng đài: Cao điểm lễ tết khách phải chờ 20–30 phút mới gặp được tổng đài viên.
    + Auto (Automation): Tự động trả lời các câu hỏi thường gặp (FAQ), tự động làm thủ tục check-in qua chat, tra cứu tình trạng chuyến bay.
    + Value khi AI đúng: Phản hồi tức thì 24/7, không đợi chờ, cá nhân hóa hành trình.
    + Doanh nghiệp: Giảm chi phí vận hành (OpEx), tăng tỷ lệ tự phục vụ (Self-service rate), giảm áp lực cho nhân sự trực tiếp.

--- 

### Trust
- Vấn đề: Chatbot đang có Precision thấp về mặt nhận diện ý định (Intent Recognition). User muốn "Khám phá" (Discovery) nhưng hệ thống lại hiểu nhầm sang "Tra cứu trạng thái" (Status Check) hoặc Chatbot Neo không được thiết lập tra như vậy --> Hệ quả (Sai): Gây khó chịu cho khách hàng

- Khi sai:

    + Dấu hiệu nhận biết: Khi người dùng nhập "Tôi muốn xem các chuyển bay từ Hà Nội đi Đà Nẵng" 

    + Hành động: Chat bot trả lời "NEO xin đưa ra các phương án tra cứu sau.", sau đố có các lựa chọn "Tra cứu theo Mã đặt chỗ/Số vé",....

- Recovery:

    Chatbot cần thực hiện trả lời "Bạn có thể tra chuyến tại ...."  hoặc "vào db tra các chuyển bay từ A đến B"

---

### Feasibility
- Cost: 0.02$/session
- Latency: 
    + Intent recognition (LLM): ~0.5-1s
    + API call chuyến bay: ~0.8-1.5s
    + Response generation: ~0.5s-1s
    + Tổng (optimistic): ~2-3s
    + Tổng (worst case / peak load): ~5-7s
- Risk:
    + sai thông tin thì sẽ dẫn đến ảnh hưởng lớn --> ví dụ: khi chatbot của hãng trả lời sai thông tin chuyến bay sẽ dẫn đến người dùng đặt nhầm chuyến, ảnh hưởng đến thời gian, tiền bạc của người dùng.
- Dependency:
    + API chuyến bay
    + Dữ liệu nội bộ về các thủ tục của hãng
## Automation hay augmentation?

☐ Automation — AI làm thay, user không can thiệp
☐ Augmentation — AI gợi ý, user quyết định cuối cùng

chọn Augmentation

**Justify:** ___
- Chatbot không thực hiện hành động cuối (đặt vé) mà chỉ:
    + trả lời câu hỏi
    + hiển thị danh sách chuyến bay
- Người dùng vẫn phải:
    + tự chọn chuyến
    + chuyển sang hệ thống đặt vé chính thức
- Vì vậy đây là augmentation rõ ràng, không phải automation
- Nếu AI sai:
    + user vẫn có thể tự kiểm tra lại
    + không gây hậu quả trực tiếp như đặt nhầm vé --> mực độ rủi ro thấp hơn automation


---

## Learning signal

| # | Câu hỏi | Trả lời |
|---|---------|---------|
| 1 | User correction đi vào đâu? | - Khi user nhập lại câu hỏi (ví dụ: “tôi muốn xem chuyến bay chứ không phải tra cứu mã”) → lưu vào **intent correction dataset** <br> - Khi user click vào 1 chuyến trong danh sách → lưu vào **preference signal** <br> - Khi user bỏ qua toàn bộ gợi ý → đánh dấu **bot trả sai intent hoặc ranking kém** |
| 2 | Product thu signal gì để biết tốt lên hay tệ đi? | - Tỷ lệ phải hỏi lại (rephrase rate) <br> - Tỷ lệ fallback (“không hiểu”) <br> - CTR vào danh sách chuyến bay <br> - Thời gian tìm được chuyến phù hợp (time-to-find) <br> - Bounce rate (user thoát sau khi hỏi) |
| 3 | Data thuộc loại nào? | ☑ User-specific (hành vi click, query) <br> ☑ Domain-specific (chuyến bay, intent) <br> ☑ Real-time (giá, lịch bay) <br> ☑ Human-judgment (user chọn chuyến nào) |

**Có marginal value không?** (Model đã biết cái này chưa? Ai khác cũng thu được data này không?)
___
- Có, nhưng mức trung bình → cao (tùy cách khai thác) 

- Model chung:
  - đã hiểu cơ bản intent “tìm chuyến bay”  
  - nhưng **không biết user thực sự muốn chuyến nào**

- Data thu được:
  - hành vi click → giúp cải thiện **ranking chuyến bay**
  - query thực tế → giúp giảm lỗi **intent recognition**

- Hạn chế:
  - các hãng khác cũng có thể thu thập data tương tự  
  - → lợi thế cạnh tranh không hoàn toàn độc quyền  

- Tuy nhiên:
  - nếu kết hợp với lịch sử user và hành vi tìm kiếm  
  - → có thể tạo **cá nhân hóa (personalization)**  
- Kết luận: Có marginal value vì data mang tính hành vi thực tế (behavioral data), nhưng cần khai thác tốt để tạo lợi thế.
---

## Cách dùng

1. Điền Value trước — chưa rõ pain thì chưa điền Trust/Feasibility
2. Trust: trả lời 4 câu UX (đúng → sai → không chắc → user sửa)
3. Feasibility: ước lượng cost, không cần chính xác — order of magnitude đủ
4. Learning signal: nghĩ về vòng lặp dài hạn, không chỉ demo ngày mai
5. Đánh [?] cho chỗ chưa biết — Canvas là hypothesis, không phải đáp án

---

*AI Product Canvas — Ngày 5 — VinUni A20 — AI Thực Chiến · 2026*