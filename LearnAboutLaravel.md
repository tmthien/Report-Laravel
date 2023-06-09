# LEARN ABOUT LARAVEL
___
### Getting Started On Windows
- Create project "Learn-About-Laravel" with composer:
```
    composer create-project laravel/laravel Learn-About-Laravel
```
- Run local server
```
    php artisan serve
```

- Database Configuration
```
    DB_CONNECTION=mysql // Quản lý CSDL bằng MySQl
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel // Tên CSDL
    DB_USERNAME=root    // Username CSDL
    DB_PASSWORD=        // Password
```
***
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
***
### Dicrectory Structure
###### Ta sẽ làm việc chính với những thư mục sau:
- **`app`** 
  - *`app/Http/Controllers`* - thư mục này chứa các file xử lý request
  - *`app/Http/Middleware`* - thư mục này chứa các file Middleware do lập trình viên tạo ra cho hệ thống
  - *`app/Http/Models`* - chứa các file thao tác với DB
- **`database`**
  - *`database/factories`* - chứa các file có chức năng tạo dữ liệu ảo, thuận lợi cho việc phát triển và testing
  - *_`database/migrations`_* - chứa các file dùng để khởi tạo bảng trong DB
  - *_`database/seeders`_* - seeder sẽ hỗ trợ tạo dữ liệu 1 cách nhanh chóng 
- **`resource`**
  - Chứa các folder như CSS, JS, views,...
- **`routes`**
  - *`routes/api.php`* - định nghĩa router api
  - *`routes/web.php`* - định nghĩa các router cho project
- **`storage`**
  - đây là nơi chứa các file blade template
  - *`storages/app/public`* có thể dùng để lưu trữ các file do người dùng (user) đăng tải, chẳng hạn như ảnh
***
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
- [Laravel Breeze](https://laravel.com/docs/10.x/starter-kits) 
  - Installation
    ```
        composer require laravel/breeze:1.9.2
        
        php artisan breeze:install
 
        npm install
        npm run dev
        php artisan migrate
    ```
***
### Basic Laravel
- Route
  - Chức năng: điều hướng các request đến với URL path tương ứng với các Controller, Action, View, Command Line.
  - `Route::get($path, $callback)`: nhận request với phương thức GET - với $path là đường dẫn route, $callback là 1 hành động nào đó sẽ được trả về
    - Example: 
    ```
    use App\Http\Controllers\UserController;
 
    Route::get('/user', [UserController::class, 'index']);
    ```
  - `Route::post($path, $callback)`: nhận request với phương thức POST
    ```
    use App\Http\Controllers\UserController;
 
    Route::post('/edit-info/{id}', [UserController::class, 'edit']);
    ```
  - `Route::resource($path, $callback)`: Khi tạo Controller với lệnh `php artisan make:controller *NameController* --resource` thì trong Controller tạo ra 7 action mặc định: index(), create(), store(), show(), update(), edit(), delete(). Khi sử dụng `Route::resoure` thì tất cả các action sẽ được khai báo.
    - Example:
     ```
        php artisan make:controller ProductController --resource
        
        //Khai báo 
        Route::resource('products', ProductController::class);
        //đường dẫn của các action cũng sẽ được tạo tự động
     ```
     - Khi khai báo resource route hệ thống sẽ mặc định sẽ xử lý toàn bộ các action trong đó. Để chỉ dùng 1 vài action trong đó thì ta có thể sử dụng `->only([]);`
     ```
        Route::resource('photos', 'PhotoController')->only([
            'index', 'show'
        ]);
        //
        Route::resource('photos', 'PhotoController')->only([
            'index', 'create', 'delete'
        ]);
        
     ```
   - `Route::redirect($path, $redirectTo, $status)`: Nhận request sau đó sẽ chuyển hướng tới $redirectTo.
     - $status: ở đây sẽ là *Mã trạng thái HTTP* mặc định sẽ là 302, với *redirect* thì nó sẽ nằm trong khoảng 300-308 [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages)
     - Example:
     ```
        Route::redirect('google', 'https://www.google.com/');
     ```
   - `Route::view($path, $viewName, $data)`: Nhận request sau đó render ra view.
     - Example:
     ```
        Route::view('view', 'welcome');
     ```
   - `Route::group(['prefix' => '$path', 'middleware'=> '$middleware'], $callback)`: Nhóm các route với các prefix, middleware xác định
     - Example:
     ```
        Route::group(['prefix'=> 'admin','middleware' => 'auth'], function () {
            Route::resource('categories', CategoryController::class);
            Route::resource('products', ProductController::class);
        });
     ```    
   - Để kiểm tra các route đã được khai báo ta dùng `php artisan route:list`.
   <br>
- Controller
  - Có nhiệm vụ xử lý các request
  - Để tạo 1 controller mới
    ```
        php artisan make:controller *NameController*
    ```
    - Ex:
    ```
        <?php

        namespace App\Http\Controllers;

        use App\Models\Product;
        use Illuminate\Http\Request;

        class ProductController extends Controller
        {
            public function index()
            {
                $products = Product::latest()->paginate(5);// lấy bảng ghi mới nhất và phân trang

                return view('products.index',compact('products')) // trả về view
                    ->with('i', (request()->input('page', 1) - 1) * 5);
            }
        }
    ```
  <br>
- CSRF 
  - Cross-Site Request Forgery (CSRF) đại khái nghĩa là "giả mạo yêu cầu trên trang web". Đây là một loại tấn công sẽ thực hiện các request trái phép thông qua user đã được xác thực (tức là đã đăng nhập trên hệ thống).
  - CSRF Laravel: Laravel sẽ tự động tạo một "token" cho mỗi phiên hoạt động của người dùng, được quản lý bởi framework. Token dùng để xác minh rằng user đã đăng nhập là user thực sự thực hiện các request tới ứng dụng.
  - Để sử sử dụng chức năng này thì ta cần khai báo `@csrf` vào form để tạo `token`
    - Ex:
    ```
    <form method="POST" action="/profile">
        @csrf
        ...
    </form>
    ```
    <br>
- Models
  - Xử lý các thao tác với DB
  - Định nghĩa model
    ```
        php artisan make:model *NameModel*
    ```
    - Example:
    ```
        <?php

        namespace App\Models;

        use Illuminate\Database\Eloquent\Factories\HasFactory;
        use Illuminate\Database\Eloquent\Model;

        class Product extends Model
        {
            use HasFactory;
            protected $fillable = [
                'title', 'detail'
            ];
        }
        
        //Model Product 
    ```
    <br>
- Migration
    ```
        php artisan make:migration create_*NameTable*_table
    ```
  - Sau khi tạo xong 
    ```
        <?php

        use Illuminate\Database\Migrations\Migration;
        use Illuminate\Database\Schema\Blueprint;
        use Illuminate\Support\Facades\Schema;

        class CreateProductsTable extends Migration
        {
            /**
             * Run the migrations.
             *
             * @return void
             */
            public function up()
            {
                Schema::create('products', function (Blueprint $table) {
                    $table->id();
                    //ở đây sẽ thêm những trường và kiểu dữ liệu có trong table
                    $table->timestamps();
                });
            }

            /**
             * Reverse the migrations.
             *
             * @return void
             */
            public function down()
            {
                Schema::dropIfExists('products');
            }
        }
    ```
  - Factory, Seeder
        
    - Step 1:
    ```
        php artisan make:factory ProductFactory --model=Product
        // file ProductFactory sẽ được tạo trong thư mục *database/factory/UserFactory.php*
    ```    
    - Step 2:
    ```
        <?php
        namespace Database\Factories;

        use Illuminate\Database\Eloquent\Factories\Factory;

        class ProductFactory extends Factory
        {
            /**
             * Define the model's default state.
             *
             * @return array
             */
            public function definition()
            {
                return [
                    "name" => $this->faker->name(),
                    "detail" => $this->faker->sentence(),
                ];
            }
        }
     ```   
     - Step 3:
     ``` 
        php artisan tinker
     ```   
     - Step 4:
     ``` 
        App\Models\Product::factory()->count(10)->create(); //tạo dữ liệu với 10 record
     ```   
     - Hoặc có thể sử dụng Seeder
     `php artisan make:seed ProductTableSeeder`
     ```
        <?php

        namespace Database\Seeders;

        use App\Models\Product;
        use Illuminate\Database\Seeder;
        use Illuminate\Database\Console\Seeds\WithoutModelEvents;

        class ProductTableSeeder extends Seeder
        {
            /**
             * Run the database seeds.
             *
             * @return void
             */
            public function run()
            {
                Product::factory()->count(1000)->create();// Tạo dữ liệu với 1000 record
            }
        }
     ```
     - Sau đó chạy lệnh
     `php artisan db:seed --class=ProductTableSeeder`
    <br>
- Blade Templates
  - Code login với Blade Templates
    - `@if, @else, @elseif, @endif` tương tự với trong PHP thuần `if, else, elseif, endif`
    ```
        @if (count($records) === 1)
            I have one record!
        @elseif (count($records) > 1)
            I have multiple records!
        @else
            I don't have any records!
        @endif
    ```
    - `@unless` - Nếu logic trong `@unless` false thì code bên trong sẽ được thực hiện
    ```
        @unless (Auth::check())
            You are not signed in.
        @endunless
    ```
    - `@isset` - để kiểm tra 1 biến có tồn tại hay không.
    ```
        @isset($message)
            {{ $message }}
        @endisset
        ---
        @if(isset($massage))
            {{ $message }}
        @endif
    ```
    - `@empty` - để kiểm tra 1 biến có giá trị hay không.
    ```
        @empty($message)
            {{ $message }}
        @endempty
    ```
    
    - `@auth` - Kiểm tra xác thực. Nếu user đã login thì code bên trong sẽ đc thực thi
    ```
        @auth
            // The user is authenticated...
        @endauth
        ---
        Check guard admin
        @auth('admin')
            // The user is authenticated...
        @endauth
    ```
    - `@guest` - chức năng giống với `@auth`
  - Switch - Case
    - Tương tự với Switch-Case trong PHP thuần
    ```
        @switch($i)
            @case(1)
                First case...
                @break

            @case(2)
                Second case...
                @break

            @default
                Default case...
        @endswitch
    ```
  - Loop
    - Trong Laravel sẽ không hỗ trợ vòng lặp do-while
    - @continue, @break tương ứng với continue, break trong PHP
    ```
        @for ($i = 0; $i < 10; $i++)
            The current value is {{ $i }}
        @endfor
        ---
        @foreach ($users as $user)
            <p>This is user {{ $user->id }}</p>
        @endforeach
        ---
        //Trong vòng lặp này thì sẽ kiểm tra array, nếu array là 1 mảng rỗng thì sẽ thực hiện code trong @empty... @endforelse
        @forelse ($users as $user)
            <li>{{ $user->name }}</li>
        @empty
            <p>No users</p>
        @endforelse
        ---
        @while (true)
            <p>I'm looping forever.</p>
        @endwhile
    ```
  - Nhúng view vào Blade
    - @inlcude
