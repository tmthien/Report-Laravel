# Problem 
### Cả 3 team BE, FE, Moblie đều chưa thống nhất được cách làm và dữ liệu trả về cho màn hình FAQ & Community Guidlines
***
##### Có 3 cách để thực hiện việc theo bên team Mobile đề xuất đó là :
1. Cả Mobile và FE đều lấy API từ backend, rồi tự render ra UI.
2. FE sẽ lấy API từ BE, sau đó thì làm luôn phần Reponsive cho cả màn hình Mobile, phía Mobile sẽ lấy link từ phía FE.
3. Set cứng text (Loại bỏ, vì dữ liệu được Admin có thể tùy chỉnh, nếu set cứng text thì phải tốn rất nhiều thời gian cho cả FE và Mobile khi dữ liệu có sự thay đổi)
---
#### Với cách 1, đây sẽ là dữ liệu mà bên phía BE sẽ trả về sau khi người dùng đã thêm và đã format
Vì phía BE sẽ build phần `Admin Pannel` và phần `FAQ & Community Guidlines` sẽ được Admin thêm vào và có thể tùy ý chỉnh sửa, nên phía BE sẽ sử dụng thư viện `CkEditor` để Admin có thể tự format nội dung nhập vào và sẽ lưu luôn phần format đó dưới dạng `HTML` vào DB. 
<br>
<br>
Dữ liệu được trả về theo định dạng HTML, với dữ liệu kiểu này thì cả bên FE và cả Mobile đều có thể xử lý
 ![Tux, the Linux mascot](/img/image.png)
#### Với cách 2, 
