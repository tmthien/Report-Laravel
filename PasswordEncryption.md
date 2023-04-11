# Report - Password Encryption
---
### Cách "Password Encryption" hoạt động?
  - Có 4 phương pháp mã hoá thông dụng:
    - **Symmetric Key** - *Mã hoá đối xứng* : là một lớp các thuật toán mã hóa, trong đó các khóa dùng cho việc mật mã hóa/giải mã có quan hệ rõ ràng với nhau (có thể dễ dàng tìm được một khóa nếu biết khóa kia). Mã khóa loại này không công khai.
      - Example: Người A sẽ sử dụng 1 thuật toán mã hoá + KEY để mã hoá dữ liệu -> Người A sẽ giao cho người B 1 KEY (giống với KEY để mã hoá dữ liệu) -> Người B sẽ dùng KEY để giải mã và đọc dữ liệu.
    - **Public key** - *Mã hoá bất đối xứng*: Hai khóa đóng vai trò thay đổi mật khẩu. **public key** , có sẵn cho bất kỳ ai sử dụng. Cái còn lại, **private key** chỉ dành cho một số ít người được chọn. Sử dụng một để mã hóa tin nhắn và người nhận cần người kia hiểu ý nghĩa của nó.
      - Example: A là người gửi và B là người nhận. Người B sẽ tạo ra 1 cặp key public và private, B sẽ giữ private key và gửi public key cho A, A sẽ sử dụng public key để mã hoá dữ liệu, sau đó gửi file đã mã hoá cho B. B sẽ sử dụng private key để giải mã dữ liệu đó.
    - **Hashing** - *Mã hoá 1 chiều*: Là một thuật toán sẽ biến đổi mật khẩu thành 1 chuỗi kí tự ngẫu nhiên. Phương pháp này dùng để mã hoá những thứ không cần dịch lại dữ liệu ban đầu. *hash function* tạo ra kết quả không thể đảo ngược và duy nhất. 2 khối dữ liệu khác nhau sẽ cho ra 2 mã băm khám nhau.
      - **Salting** là quá trình thêm 1 chuỗi kí tự ngẫu nhiên và duy nhất vào dữ liệu trước khi dữ liệu đó được băm
      - **Stretching** là quá trình lặp đi lặp lại quá trình băm. 
      - Example: Khi dùng phương pháp này để mã hoá mật khẩu, mật khẩu trong CSDL sẽ được mã hoá thành 1 chuỗi kí tự ngẫu nhiên. Khi đăng nhập, *hash function* sẽ so sánh mật khẩu người dùng nhập với mã trong CSDL, nếu trùng thì sẽ cho phép đăng nhập.
    - **Classic Encryption** - *Mã hoá cổ điển*: Mã hoá cổ điển là phương pháp mã hóa đơn giản nhất, tồn lại lâu nhất trên thế giới và không cần khóa bảo mật, chỉ cần người gửi và người nhận cùng biết về thuật toán này là được.
      - Example: Thuật toán mã hoá của người gửi là hoán đổi kí tự trong 1 chuỗi với kí tự liền kề của nó trong bảng chữ cái. Khi người nhận biết được thuật toán đó thì có thể giải mã dữ liệu đó. 
### Các định dạng mã hoá phổ biến
  - **MD5**: là 1 *hash function* .Mã MD5 dài 128-bit và thường biểu diễn bằng một số hệ thập lục phân 32 ký tự. Vì là mã hoá 1 chiều nên MD5 không thể giải mã. Nhưng cũng có trường hợp có thể giải mã MD5.
    - Cơ chế hoạt động của các website giải mã MD5 đó là sưu tập các mã hóa MD5 người dùng nhập vào, để tạo thành cơ sở dữ liệu thật lớn. Khi dữ liệu đủ lớn, xác xuất giải mã MD5 cũng cao lên. Cơ chế hoạt động của nó như thế này:
      - Người dùng mã hóa ký tự bằng các hàm mã hóa MD5.
      - Hệ thống lưu lại cả ký tự và hàm băm đã được mã hóa.
      - Khi có người tìm cách giải mã MD5 của chuỗi mã hoá, nó sẽ tra trong CSDL và trả về kết quả
  - **Bcrypt**: là một hàm mã hóa mật khẩu được thiết kế bởi Niels Provos và David Mazières dựa trên các thuật toán mã hóa Blowfish. Nó là sự kết hợp giữa *hashing*, *streching* và *salting*. 
      - Example: 
      ` $[algorithm]$[cost]$[22 characters salt][31 characters hash] `
  - Trong Laravel sẽ sử dụng dạng mã hoá **Bcrypt**
  
