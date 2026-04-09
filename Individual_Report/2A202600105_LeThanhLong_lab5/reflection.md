# Individual reflection — Long

## 1. Role
Fullstack UI Developer. Xây dựng giao diện web, tích hợp Client-Server và lưu trữ Document.

## 2. Đóng góp cụ thể
- Lên code màn hình Chatbot UI tương tác hai chiều, render bảng biểu, Markdown động.
- Viết luồng Upload ngầm và tích hợp xử lý PDF parser để lấy Byte text từ file thô.
- Setup Admin dashboard và tính năng lưu vết Cites (trích dẫn) bên dưới nội dung chat.

## 3. SPEC mạnh/yếu
- Mạnh nhất: Chăm chút được UI hiển thị rõ trích dẫn (Citation) từ các bộ luật lấy ra, giúp trải nghiệm tương tác với AI được minh bạch 100%.
- Yếu nhất: Chưa setup Streaming Data (Server-Sent Events), sinh viên phải thao thức đợi vòng xoay mạng loading 1 cục dài. File bóc chữ PDF bị thiếu dấu ngắt khoảng trắng.

## 4. Đóng góp khác
- Review logic luồng Backend API để gắn mapping param json trên Frontend gọi về.
- Xử lý mảng State hiển thị trạng thái đang gọi Tools ra View nhằm xoa dịu người dùng trong quá trình chờ lấy DB.

## 5. Điều học được
Việc format Markdown kèm parse dữ liệu array ngay trên luồng react là thử thách thú vị. Học được tư duy hiển thị AI không chỉ là output text, mà User-Experience còn nằm ở các yếu tố nhỏ nhặt như show Thought process/Tool calls.

## 6. Nếu làm lại
Sẽ dẹp cơ chế call REST thông thường và implement **WebSockets / SSE Streaming** để chữ nhả ra từ từ rải rác như ChatGPT xịn. Cải thiện lại parser PDF bằng OCR xịn sò thay vì bóc text thô làm mất dấu cách.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Setup CSS Tailwind, build layout box chat qua React một cách cực kỳ chuẩn xác và có thẩm mỹ nhanh chóng.
- **Sai/mislead:** Khuyên dùng các package render Markdown lỗi thời như bản react-markdown cũ không hỗ trợ tốt plugin bảng (`remark-gfm`). Code sinh ra file PDF bóc text hay bị dính 2 cột chữ liền nhau (whitespace issue).
