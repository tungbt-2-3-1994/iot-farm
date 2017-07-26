## Base link
- User: link sẽ là link mặc 
- admin: sẽ thêm /admin
- Đối tác: sẽ thêm /partner
- Cảm biến: sẽ thêm /sensor


**Ví dụ:** Domain của server là iot-farm.vn thì trang xem trạng thái đăng nhập sẽ là
- admin: http://iot-farm.vn/admin/session
- user: http://iot-farm.vn/session
- partner: http://iot-farm.vn/partner/session
- sensor: http://iot-farm.vn/sensor/session

## Login
### 1. Xem thông tin đăng nhập

**Path:** /session

**Method:** GET

**Response:**

- Khi đã đăng nhập

200

```
{
    status: 'logined',
    user: {
        name: 'xyz',
        email: 'abc@gmail.com',
        phone_number: '1234'
    },
    auth_token: 'token_dung_de_dang_nhap_socket',
    token: 'token_dung_de_gui_post_request'
}
```

- Khi chưa đăng nhập

401

```
{
    status: 'not_login',
    message: 'User not logged in!',
    token: 'token_dung_de_gui_post_request'
}
```

###  2. Đăng nhập

**Path:** /session/login

**Method:** POST

**Response:**

- Login success:

200

```
{
    status: 'logined',
    user: {
        name: 'xyz',
        email: 'abc@gmail.com',
        phone_number: '1234'
    },
    auth_token: 'token_dung_de_dang_nhap_socket',
    token: 'token_dung_de_gui_post_request'
}
```

- Login fail:

401

```
{
    status: 'not_login',
    message: 'These credentials do not match our records.',
    token: 'token_dung_de_gui_post_request'
}
```

###  3. Logout

**Path:** /session/logout

**Method:** POST

**Response:**

- logout thành công:

401

```
{
    status: 'not_login',
    message: 'User not logged in!',
    token: 'token_dung_de_gui_post_request'
}
```

- Chưa đăng nhập mà gửi logout request: sẽ gửi thông báo lỗi không được tiếp tục truy nhập.

400

```
{
    status: 'error',
    message: 'Can not continue executing request!'
}
```

## Đăng nhập socket server

- Để đăng nhập vào socket server thì lắng nghe sự kiện `connect` để emit sự kiện `authenticate` với data là `{token: 'auth_token_nhan_duoc'}`.

```
socket.on('connect', function () {
    socket.emit('authenticate', {token: result.auth_token});
});
```

## Thông báo lỗi
### Link không tồn tại (not found)
```
{
    status: 'warning',
    message: 'Not found!'
}
```

## Quá trình gửi request
### Đăng nhập

- Gửi request get xem thông tin đăng nhập
- Nhận được `token`
- Sử dụng token này để gửi request post đăng nhập.

*Chú ý:* POST request phải có một trường `_token` = token bên trên. Hoặc phải được set header:  `X-CSRF-TOKEN` = 'token'. Hoặc đưa token này lên url: http://farm.vn/?_token=token. Xem https://laravel.com/docs/5.4/csrf#csrf-x-csrf-token để biết thêm chi tiết.

- Sau khi đăng nhập thì sẽ nhận được `auth_token`. Dùng `auth_token` để đăng nhập socket server. Socket server sẽ lắng nghe ở cổng 3000 và cùng tên miền với php server (cái này có thể thay đổi).


Nếu biết một chút code web có thể xem ví dụ ở file `/static/auth.html`

###  2. 

**Path:**

**Method:**

**Response:**
