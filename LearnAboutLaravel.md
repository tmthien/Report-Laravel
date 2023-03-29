# LEARN ABOUT LARAVEL
---
### Getting Started On Windows
1. Create project "Learn-About-Laravel" with composer:
```
    composer create-project laravel/laravel Learn-About-Laravel
```
1. Run local server
```
    php artisan serve
```

1. Database Configuration
```
    DB_CONNECTION=mysql // Quản lý CSDL bằng MySQl
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel // Tên CSDL
    DB_USERNAME=root    // Username CSDL
    DB_PASSWORD=        // Password
```
### Request Lifecycle
- Tất cả yêu cầu sẽ được gửi đến public/index.php (Trong file này sẽ thực hiện 3 nhiệm vụ chính)
  - Kiểm tra xem ứng dụng có đang được bảo trì hay không (Check If The Application Is Under Maintenance)
  - Đăng ký cơ chế autoload (Register The Auto Loader)
  - Chạy ứng dụng (Run the application)
- Tiếp theo, request sẽ được gửi đến HTTP Kernel hoặc Console Kernel,tùy thuộc vào loại yêu cầu. (`app/Http/Kernel.php`)
  - HTTP Kernel kế thừa class `Illuminate\Foundation\Http\Kernel`, class này sẽ thực việc các công việc trước khi request được thực thi như cấu hình xử lý lỗi, cấu hình logger, xác định môi trường ứng dụng.
  - HTTP Kernel cũng thực hiện một số middleware mặc định của Laravel
- Tiếp theo, nhiệm vụ của HTTP Kernel đó chính là load các service provider trong file `config/app.php`.
  - Đăng ký service provider (Register service provider)
  - Khởi động service provider (Bootstrap service provider)
- Sau khi hoàn tất load service provider, các request sẽ được gửi đến `routes`
  - Nhiệm vụ chính của **route** chính là điều hướng request tới các `Controller/Action` tương ứng
    `Route -> Middleware -> Controller/Action` 
    `Route -> Controller/Action`
- Sau khi xử lý xong request thì sẽ trả về response qua *view* hoặc trả về trực tiếp phía client
### Dicrectory Structure
###### Ta sẽ làm việc chính với những thư mục sau:
- **`app`** 
  - *_`app/Http/Controllers`_* - thư mục này chứa các file xử lý request
  - *_`app/Http/Middleware`_* - thư mục này chứa các file Middleware do lập trình viên tạo ra cho hệ thống
  - *_`app/Http/Models`_* - chứa các file thao tác với DB
- **`database`**
  - *_`database/factories`_* - chứa các file có chức năng tạo dữ liệu ảo, thuận lợi cho việc phát triển và testing
  - *_`database/migrations`_* - chứa các file dùng để khởi tạo bảng trong DB
  - *_`database/seeders`_* - seeder sẽ hỗ trợ tạo dữ liệu 1 cách nhanh chóng 
- **`resource`**
  - Chứa các folder như CSS, JS, views,...
- **`routes`**
  - *_`routes/api.php`_* - định nghĩa router api
  - *`_routes/web.php`_* - định nghĩa các router cho project
- **`storage`**
  - đây là nơi chứa các file blade template
  - *_`storages/app/public`_* có thể dùng để lưu trữ các file do người dùng (user) đăng tải, chẳng hạn như ảnh
### Frontend
- Blade view có đuôi **`.blade.php`** - giúp ta viết cú pháp PHP trong view 1 cách ngắn gọn, logic. Trong blade thì ta có thể viết cả PHP thuần 
- Hiển thị dữ liệu trong Blade template
    ```laravel
        {{-- routes/web.php --}}
        Route::get('/', function () {
            return view('welcome', ['name' => "Trần Minh Thiện"]);
        })->name('home');

        {{-- resources/views/welcom.blade.php --}}
        <h1>Hello {{$name}} </h1>
    ```
- Hiển thị dữ liệu dạng json endcode trong Blade
  ```laravel
    @json($array, $flag)
    {{-- $array: mảng muốn encode, $flag là kiểu encode(có thể bỏ trống) --}}

    {{-- trong php thuần --}}
    <script>
        var app = <?php echo json_encode($array); ?>;
    </script>
  ```
- Laravel Breeze (Authentication Features)
  - Installation
    ```
        composer require laravel/breeze:1.9.2
        
        php artisan breeze:install
 
        npm install
        npm run dev
        php artisan migrate
    ```
