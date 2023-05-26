# Problem 
### Cả 3 team BE, FE, Moblie đều chưa thống nhất được cách làm và dữ liệu trả về cho màn hình FAQ & Community Guidlines
***
#### Có 3 cách để thực hiện việc theo bên team Mobile đề xuất đó là :
1. Cả **Mobile** và **FE** đều lấy API từ **BE**, rồi tự render ra UI.
2. **FE** sẽ lấy API từ **BE**, sau đó thì làm luôn phần `Reponsive` cho cả màn hình Mobile, phía **Mobile** sẽ lấy link `webView` từ phía **FE**.
3. Set cứng text (Loại bỏ, vì dữ liệu được Admin có thể tùy chỉnh, nếu set cứng text thì phải tốn rất nhiều thời gian cho cả FE và Mobile khi dữ liệu có sự thay đổi)
---
##### Với cách 1
Vì phía **BE** sẽ build phần `Admin Pannel` và phần `FAQ & Community Guidlines` sẽ được Admin thêm vào và có thể tùy ý chỉnh sửa, nên phía BE sẽ sử dụng thư viện `CkEditor` để `Admin` có thể tự format nội dung nhập vào và sẽ lưu luôn phần format đó dưới dạng `HTML` vào Database. 
<br>
Phía **FE** sẽ chỉ cần lấy data từ `API` rồi render ra UI. 
<br>
Phía bên **Mobile** thì sẽ phải lấy data từ `API`, sau đó sẽ phải xử lý các thẻ `HTML` trong data, rồi mới render ra UI của Mobile
 <br>
 Issue của cách này đó chính nếu có thẻ `style` thì không thể render data lên webview, lúc này thì sẽ viết `function` để xử lý mấy thẻ trong `body` rồi render vào layout của mobile
##### Với cách 2
Bên phía BE cũng sẽ làm như cách 1, và phía FE phải làm thêm cả phần `Responsive` cho cả Mobile devices.
Phía Mobile chỉ cần lấy link webView của bên FE.
Issue của cách 2 là bên phía **FE** chưa từng làm `Response` bằng `React` và vẫn còn nhiều task core chưa thực hiện xong.
