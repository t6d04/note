---
title: 'Session Management'
date: '07-02-2025'
category: 'learning'
---

# Definition
![alt text](/session-management/image.png)
- Trên đây là hình ảnh lifesycle của session, từ đó ta có một số khái niệm như sau:
## Session Creation
- Sesson được khởi tạo khi bạn đã xác thực danh tính (hay đơn giản là login).
- Tuy nhiên ở một số web app thì trước khi login, tức là khi truy cập vào web thì bạn đã được gán cho 1 session để admin có thể theo dõi các hành động của bạn trước đăng nhập.
- Trong vấn đề an toàn, ta quan tâm tới việc tạo, sử dụng và lưu trữ các session.
## Session Tracking
- Khi đã đã được nhận một session value thì mỗi lần ta request thì cái value này đều được gán vào. 
- Mục đích của việc này là đê theo dõi xem việc ta đang làm gì và có quyền làm việc ta đang làm hay không.
- Đương nhiên nếu vấn đề này có lỗi thì có thể dẫn đến việc hijack hoặc giả mạo.
## Session Expiry
- Đương nhiên với việc HTTP là stateless nên ta sẽ không có hình thức nào để thông báo cho web app biết khi người dùng tắt cả tab hay browser.
- Session sẽ kết thúc khi đến hạn và không thể dùng lại session cũ cho lần tiếp.
## Session Terminatation
- Khi người dùng logout ra thì đương nhiên ta phải hủy ngay phiên dù cho nó còn hạn
- Nếu việc này ko xảy ra thì sẽ dẫn đễn duy trì sử dụng.
# Autthentication vs Authorisation
- 2 khái niệm này rất dễ nhầm vì tên gọi giống nhau và cùng nằm trong IAAA model. Ta sẽ xem để biết được từng phần làm gì:
![alt text](/session-management/image-1.png)
## Identification
- Phần này đệ nhận dạng xem ai đang truy cập.
- Có nhiều cách để định danh như sử dụng email, username, số điện thoại,...
## Authentication
- Phần này đảm bảo người dùng truy cập đúng với định danh ở phần Identification.
- Ta có thể sử dụng mật khẩu, mã OTP, mã Pin, ....
- Nếu vượt qua phần này ta sẽ có session creation.
## Authorization
- Phần này đảm bảo ta truy cập và sử dụng đúng quyền hạn của mình.
- Session tracking giữ vai trò quan trọng trong phần này.
## Accountability
- Đây là quá trình tạo các bản log các hành động của người dùng.
- Phần này là phần tối quan trọng trong xử lý các sự cố.
# Cookies vs Token
- Có 2 cách trong Session Management là sử dụng Cookie hoặc Token.
## Cookie-Based Session Management
- Khi người dùng đăng nhập thành công, server sẽ gửi lại phía browser session ID để user có thể sử dụng để truy cập. Response sẽ có dạng: `Set-Cookie: <cookie-name>=<cookie-value>`
- User sẽ sử dụng session ID này cho các lần request yêu cầu tới cookie.
- Phía server sẽ kiểm tra xem session ID này còn hợp lệ không, nếu ko ta bị kick.
![alt text](/session-management/image-2.png)
- Có một số điều đáng chú ý trong phần cookie như:
    - `Secure`: Chỉ ra rằng cookie chỉ được sử dụng trong HTTPS, nếu ở HTTP, hay certificate lỗi thì cookie sẽ ko được sử dụng
    - `HTTPOnly`: Chỉ ra rằng cookie sẽ không được đọc bởi client-side JS.
    - `SameSite`: Chỉ ra khi nào cookie được truyền qua cross-site để chống CSRF.
## Token-Based Session Management
- Sau khi Authentication, người dùng sẽ được server cung cấp cho một token để sử dụng và browser sẽ lưu nó lại ở localStorage.