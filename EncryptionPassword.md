# Report - Encryption Password
---
### Cách "Encryption Password" hoạt động?
  - Có 4 loại mã hoá chính:
    - **Symmetric Key**: là một lớp các thuật toán mã hóa, trong đó các khóa dùng cho việc mật mã hóa/giải mã có quan hệ rõ ràng với nhau (có thể dễ dàng tìm được một khóa nếu biết khóa kia). Mã khóa loại này không công khai.
    - **Public key**: Hai khóa đóng vai trò thay đổi mật khẩu. **public key** , có sẵn cho bất kỳ ai sử dụng. Cái còn lại, **private key** chỉ dành cho một số ít người được chọn. Sử dụng một để mã hóa tin nhắn và người nhận cần người kia hiểu ý nghĩa của nó.
    - **Hashed**: Một thuật toán sẽ biến đổi mật khẩu thành 1 chuỗi kí tự ngẫu nhiên
    - **Salted**: Một vài số hoặc chữ cái ngẫu nhiên được thêm vào đầu hoặc cuối mật khẩu của bạn trước khi nó chuyển qua **Hashed**
### Các định dạng mã hoá phổ biến
  - **SHA**: Secure Hashing Algorithm - là một thuật toán nhằm biến đổi bất kỳ dữ liệu nào thành một định dạng đơn giản.
      - Example: 
