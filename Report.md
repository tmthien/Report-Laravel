# Problem 
### Cả 3 team BE, FE, Moblie đều chưa thống nhất được cách làm và dữ liệu trả về cho màn hình FAQ & Community Guidlines
***
#### Có 3 cách để thực hiện việc theo bên team Mobile đề xuất đó là :
1. Cả **Mobile** và **FE** đều lấy API từ **BE**, rồi tự render ra UI. (Dự tính sẽ làm theo cách này khi clear requirement)
2. **FE** sẽ lấy API từ **BE**, sau đó thì làm luôn phần `Reponsive` cho cả màn hình Mobile, phía **Mobile** sẽ lấy link `webView` từ phía **FE**.
3. Set cứng text (Loại bỏ, vì dữ liệu được Admin có thể tùy chỉnh, nếu set cứng text thì phải tốn rất nhiều thời gian cho cả FE và Mobile khi dữ liệu có sự thay đổi)
---
#### Với cách 1
- Vì phía **BE** sẽ build phần `Admin Pannel` và phần `FAQ & Community Guidlines` sẽ được Admin thêm vào và có thể tùy ý chỉnh sửa, nên phía BE sẽ sử dụng thư viện `CkEditor` để `Admin` có thể tự format nội dung nhập vào và sẽ lưu luôn phần format đó dưới dạng `HTML` vào Database. 
- Phía **FE** sẽ chỉ cần lấy data từ `API` rồi render ra UI. 
- Phía bên **Mobile** thì sẽ phải lấy data từ `API`, sau đó sẽ phải xử lý các thẻ `HTML` trong data, rồi mới render ra UI của Mobile
##### Advantage
- **BE** dễ lưu trữ data, data trả về đa dạng
- Không cần phải build lại App và Web
- Cả bên **FE** và **Mobile** đều có thể dùng chung data từ API, khi có thay đổi data từ phía Admin thì data cũng sẽ được đồng bộ cả bên Web và Mobile devices.
##### Issue
- Bên phía **Mobile** cũng chưa từng làm chức năng này nên cần nhiều thời gian để research về cách convert các thẻ `html` sang `textfield` để render ra UI.
---
#### Với cách 2
Bên phía **BE** cũng sẽ làm như cách 1, và phía **FE** phải làm thêm cả phần `Responsive` cho cả Mobile devices.
Phía **Mobile** chỉ cần lấy link webView của bên FE.
##### Advantage
- Thuận tiện cho việc đồng bộ giữa Mobile devices và Web.
- Không cần phải build lại App và Web
##### Issue 
- Ảnh hưởng bởi link từ **FE**, nếu Web có gặp vấn đề thì bên **Mobile devices** cũng sẽ bị ảnh hưởng
- Nếu app có phát triển thêm tính năng mới như `Dark Mode - Light Mode` thì sẽ không dùng được link từ webView
- Ảnh hưởng tới thời gian làm việc của phía **FE** (tăng effort)
---
# Solution
Sau khi trao đổi thông tin và nhận được sự giúp đỡ của mentors, các team cũng đã thống nhất với nhau là sẽ làm theo cách 1, vì khi chọn 1 trong 2 cách ở trên thì **FE** và **Mobile** đều có vấn đề , do đó các team thống nhất sẽ theo cách làm như đã dự tính ban đầu.
