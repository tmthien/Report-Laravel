# Report - Password Encryption
---
### Cách "Password Encryption" hoạt động?
  - Có 4 phương pháp mã hoá thông dụng:
    - **Symmetric Key** - *Mã hoá đối xứng* : là một lớp các thuật toán mã hóa, trong đó các khóa dùng cho việc mật mã hóa/giải mã có quan hệ rõ ràng với nhau (có thể dễ dàng tìm được một khóa nếu biết khóa kia). Mã khóa loại này không công khai.
      - Example: Người A sẽ sử dụng 1 thuật toán mã hoá + KEY để mã hoá dữ liệu -> Người A sẽ giao cho người B 1 KEY (giống với KEY để mã hoá dữ liệu) -> Người B sẽ dùng KEY để giải mã và đọc dữ liệu.
    - **Public key**: Hai khóa đóng vai trò thay đổi mật khẩu. **public key** , có sẵn cho bất kỳ ai sử dụng. Cái còn lại, **private key** chỉ dành cho một số ít người được chọn. Sử dụng một để mã hóa tin nhắn và người nhận cần người kia hiểu ý nghĩa của nó.
      - Example: A là người gửi và B là người nhận. Người B sẽ tạo ra 1 cặp key public và private, B sẽ giữ private key và gửi public key cho A, A sẽ sử dụng public key để mã hoá dữ liệu, sau đó gửi file đã mã hoá cho B. B sẽ sử dụng private key để giải mã dữ liệu đó.
    - **Hashed**: Một thuật toán sẽ biến đổi mật khẩu thành 1 chuỗi kí tự ngẫu nhiên
    - **Salted**: Một vài số hoặc chữ cái ngẫu nhiên được thêm vào đầu hoặc cuối mật khẩu của bạn trước khi nó chuyển qua **Hashed**
### Các định dạng mã hoá phổ biến
  - **SHA**: Secure Hashing Algorithm - là một thuật toán nhằm biến đổi bất kỳ dữ liệu nào thành một định dạng đơn giản. Đây đc biết tới là "mã hoá một chiều". Mã hóa hàm băm SHA tạo ra những kết quả băm không thể đảo ngược và là duy nhất. Không thể đảo ngược nghĩa là dù cho có được kết quả băm cũng không thể tìm ra dữ liệu ban đầu được băm, do đó đảm bảo tính bảo mật tuyệt đối của dữ liệu. Tính duy nhất có nghĩa là hai khối dữ liệu khác nhau sẽ không bao giờ tạo ra kết quả băm giống hệt nhau.
  -  
      
