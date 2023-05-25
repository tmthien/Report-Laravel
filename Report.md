# Problem 
### Cả 3 team BE, FE, Moblie đều chưa thống nhất được cách làm và dữ liệu trả về cho màn hình FAQ & Community Guidlines
***
##### Có 3 cách để thực hiện việc theo bên team Mobile đề xuất đó là :
1. Lấy API từ backend và render lên UI
2. Sử dụng webView và lấy link từ FE
3. App xử lý và set cứng text
---
#### Với cách 1, đây sẽ là dữ liệu mà bên phía BE sẽ trả về sau khi người dùng đã thêm và đã format
Vì phía BE sẽ build phần `Admin Pannel` và phần `FAQ & Community Guidlines` sẽ được Admin thêm vào và có thể tùy ý chỉnh sửa, nên phía BE sẽ sử dụng thư viện `CkEditor` để Admin có thể tự format nội dung nhập vào, phía BE sẽ lưu luôn phần format đó dưới dạng `HTML`.
Dữ liệu được trả về theo định dạng HTML, với dữ liệu kiểu này thì cả bên FE và cả Mobile đều có thể xử lý
 ![Tux, the Linux mascot](/img/image.png)

