# Buổi 1

## Nội dung bài học hôm nay:  
  
`1.`Khái niệm Vue  
`2.`Các cách sử dụng Vue  
`3.`So sánh Vue, React và Angular  
`4.`Xây dựng todo list bằng js thuần  
`5.`Xây dựng todo list bằng Vue  
  
## Bài tập: 
  
  _thêm action delete, undo cho các item của list_   
  
## Deadline: thứ 6 ngày 1/7  
  
Đề bài cho ta một trang upload file.

Thử điền 1 username bất kỳ

![172.104.49.143_1574_ (1).png](https://images.viblo.asia/81f21e49-0ad2-4ee4-b3d7-00c744d39167.png)

Ồ ta có một form upload file, thử tải lên file nào đó xem.

![172.104.49.143_1574_upload.png](https://images.viblo.asia/1e1c2011-4883-4ab3-af12-7c8d18d4e369.png)

Quay lại đường dẫn trang chủ `/` ta thấy các file đã upload được lưu ở đây. Đường dẫn lưu file là `website/uploads/<username>/<filename>`

![Screenshot from 2022-10-13 21-43-20.jpg](https://images.viblo.asia/759b0c12-bf5f-493b-8d90-4859b0a7a512.jpg)

Thử update một vài file shell nhưng không thể

![172.104.49.143_1574_upload (1).png](https://images.viblo.asia/8b7fbe4c-dd6a-449d-bd9a-bdbd6124c40f.png)

Hãy thử xem source code có tìm được điều gì không và tìm thấy `static/getflag.js`

![image.png](https://images.viblo.asia/238e8e34-fc15-4faf-a7d4-24ad5fa722f0.png)

Truy cập thử đường dẫn `/getflag`

![image.png](https://images.viblo.asia/a0f97493-f92b-4e5e-8835-94267ce224df.png)

Ta thấy mình không được chấp nhận truy cập gì đó, có phải xác thực hay phân quyền gì chăng?

Mở DevTools thì ta thấy hệ thống xác thực bằng token.

```
Cookie: token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImlzcyI6Imh0dHA6Ly9sb2NhbGhvc3Q6NTAwMC9zdGF0aWMva2V5cy5wdWIifQ.eyJ1c2VybmFtZSI6Im5hbXB0Lm1lIiwiYXBwcm92ZSI6ZmFsc2V9.jqBe7KtFn8Oj3gS_eiVNBBmHTuFYvfCiYBxleb7jnpu-IeGA9VfXbaDbcfPPnFGWZ73qSlD1UNsCMZ5_8ZG9TI53P9wlE0t0KURJ5Z6ad9SpmsG7U33-3GGppDhUfM4o7pw_KQTmqPXpjmGEw-LZ6LI2vPcmj7yVg-ZnIScQQXnbIys-48Fxh_EwJ7wsrS_aLqkeSgcy0AzBwVg0d9b2DN9e5R6YJJRXISAJLPsh3VkYGzQ83JcF7htJQX1JJkFoGT_LTpwTFD0SE5X0PvvowYcMLQ_r2yNKlRGtA43Ly6smz3Vlt3MUXCP6LyxGAc0zWBaJvorJ7qRFcnUxEcIZqMsc7z9A812MJpG9T78-i98xNL3cOrENEhT7FvXLLzvtpETp-I2nY9cF90Ya4riNkEIEjnxKnBGC9HkNvqwExAqqCKOVJsfoD-pWhoiXvWW2GSpteoCR-OzM9-pdGEDg2gMNaP9dAQ3FLAk7dCTNHpJFi0-CQ6Jxx6gOV4ljcwdy
```

Decode Token này bằng JWT Tool (https://token.dev/). Cho bạn muốn tìm hiểu thêm vào JWT (https://viblo.asia/p/tim-hieu-ve-json-web-token-jwt-7rVRqp73v4bP)

![image.png](https://images.viblo.asia/4b953299-057e-4d35-b3ac-5940c6d4151b.png)

Chúng ta cần thay đổi giá trị `"approve": true` nhưng vẫn cần đảm bảo token tuân thủ theo RS256

Ta thấy một giá trị trên header là `"iss": "http://localhost:5000/static/keys.pub"` truy cập theo đường dẫn `/static/keys.pub` được file chứa public key  của RS256. 
Vậy ta có thể  khai thác bằng cách dùng một cặp public key và private key khác để gen ra JWT token mà ta mong muốn `"approve": true` đồng thời đổi giá trị `"iss": "http://localhost:5000/static/keys.pub"` thành đường dẫn đến file `keypublic.txt` chứa public key của chúng ta (file đó sẽ được upload lên hệ thống bằng form upload file)

![Screenshot from 2022-10-13 22-10-36.jpg](https://images.viblo.asia/d6d4c531-816c-4cd3-bd5d-ed440d557ead.jpg)

![image.png](https://images.viblo.asia/6cdd52da-e7a3-49d2-830b-738ae6d20311.png)

Sau khi có token mới rồi thì chỉnh sửa cookie và truy cập thử `/getflag`

![image (1).jpg](https://images.viblo.asia/ceeec0f0-2366-48be-988e-0ff20b422d3d.jpg)

> Đáp án: Flag{JWT_token_is_the_best_token_ever}
