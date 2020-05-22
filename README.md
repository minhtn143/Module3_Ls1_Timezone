## [Thực hành] Ứng dụng xem giờ hiện tại của các thành phố trên thế giới
### Mục tiêu
Luyện tập sử dụng Route trong Laravel

### Mô tả
Trong phần này, chúng ta sẽ phát triển một ứng dụng cho phép hiển thị thời gian hiện tại của các thành phố lớn trên thế giới.

Ứng dụng hiển thị một danh sách các thành phố cho sẵn trong Combobox, cho phép người dùng chọn lựa một thành phố và sau đó hiển thị thời gian của thành phố được lựa chọn.

### Hướng dẫn
#### Bước 1: Tạo ứng dụng Laravel có tên “timezone”

Vào thư mục chúng ta muốn lưu trữ mã nguồn của dự án rồi gõ lệnh:

composer  create-project laravel/laravel timezone --prefer-dist

#### Bước 2: vào trong thư mục code và mở toàn bộ tên trong thư mục bằng trình soạn thảo yêu thích

cd timezone/
#### Bước 3: Tạo 1 giao diện combo box bằng html:

Tạo file “index.blade.php” trong folder “resources/views/”

    <div class="main-content">
    <h1>Ứng dụng xem giờ hiện tại của các thành phố trên thế giới</h1>
    <label for="select-city"></label>
    <select id="select-city">
        <option>Chọn thành phố</option>
        <option value="America-Chihuahua">Chihuahua, Mexico</option>
        <option value="America-Costa_Rica">Costa Rica</option>
        <option value="America-Havana">La Habana, Cuba</option>
        <option value="Asia-Hong_Kong">Hồng Kông</option>
        <option value="Asia-Ho_Chi_Minh">Hồ Chí Minh, Việt Nam</option>
    </select>
    </div>
Sau khi tạo xong chúng ta thay đổi function Route mặc định nằm trong file “routes/web.php” thành:

Route::get('/', function () {
    return view('index');
});
#### Bước 4: Chạy ứng dụng và truy cập URL greeting để xem kết quả.

Gõ lệnh: php artisan serve

Kiểm tra giao diện đã tạo bằng cách truy cập “http://127.0.0.1:8000/”.

#### Bước 5: Viết javascipt chuyển đổi url ứng với từng giá trị “value” trong “select tag”

    <script>
    document.getElementById("select-city").onchange = function () {
        ChooseCity()
    };

    function ChooseCity() {
        var time_zone = document.getElementById("select-city");
        window.location.href = time_zone.value;
    }
    </script>
Với đoạn lệnh script này, mỗi khi ta thay đổi option của select thì url của chúng ta sẽ được thay đổi thành các đường dẫn như sau:

“http://127.0.0.1:8000/America-Chihuahua”

“http://127.0.0.1:8000/America-Costa_Rica”

…

#### Bước 6: Xử lý dữ liệu múi giờ

Thay đổi nội dung file “routes/web.php” thành:

    Route::get('/{timezone?}', function ($timezone = null) {
    if (!empty($timezone)) {

        // Khởi tạo giá trị giờ theo múi giờ UTC
        $time = new DateTime(date('Y-m-d H:i:s'), new DateTimeZone('UTC'));

        // Thay đổi về múi giờ được chọn
        $time->setTimezone(new DateTimeZone(str_replace('-', '/', $timezone)));

        // Hiển thị giờ theo định dạng mong muốn
        echo 'Múi giờ bạn chọn ' . $timezone . ' hiện tại đang là: ' . $time->format('d-m-Y H:i:s');
    }
    return view('index');
    });
#### Bước 7: Kiểm tra trên trình duyệt web ta sẽ thấy kết quả:

