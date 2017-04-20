# Cross-Site Request Forgery (CSRF) (10/04/2017)

- **[Kỹ thuật tấn công CSRF là gì?](#1)**
- **[Cách phòng tránh](#2)**

<a name="1"></a>
## Kỹ thuật tấn công CSRF là gì?

CSRF là một cuộc tấn công lừa các nạn nhân vào trình một yêu cầu độc hại. Nó kế thừa bản sắc và đặc quyền của nạn nhân để thực hiện một chức năng không mong muốn thay cho nạn nhân, như kiểu ném đá dấu tay :troll: .Về cơ bản, kẻ tấn công sẽ sử dụng CSRF để lừa nạn nhân truy cập trang web hoặc nhấp vào liên kết URL có chứa các yêu cầu nguy hiểm hoặc trái phép, như mua hàng , xóa account , gửi link jav ..., kết hợp xss vs csrf để tạo nên khác biệt( tham khảo juno_okyo)



-  Đầu tiên, người dùng phải đăng nhập vào trang mình cần (Tạm gọi là trang A). 

-  Để dụ dỗ người dùng, hacker sẽ tạo ra một trang web độc. (dưới url khác)

-  Khi người dùng truy cập vào web độc này, một request sẽ được gửi đến trang A mà 
hacker muốn tấn công (thông qua form, img, …). (thường lấy form của web a , rồi chỉnh sửa code )

-  Do trong request này có đính kèm cookie của người dùng, trang web A đích sẽ nhầm 
rằng đây là request do người dùng thực hiện. 

-  Hacker có thể mạo danh người dùng để làm các hành động như đổi mật khẩu, chuyển 
tiền, gửi mess,
---------trích bảo mật nhập môn---------

- đầu tiên là coi source 1 coppy đoạn code đổi thông tin , pass... đem về web mình chỉnh sửa thông tin cần thiết qua thuộc tính value , rồi cho nó là thuộc tính ẩn , chèn nó vào trong 1 đoạn hình ảnh , hay đại loại cái gì đó , chỉ cần victim click vào thì đoạn code se chạy kết nối với trang web bị tấn công dưới quyền của người dùng 

<a name="2"></a>
## Cách phòng tránh
- **đối với người dùng**:Nên thoát khỏi các website quan trọng: Tài khoản ngân hàng, thanh toán trực tuyến, các mạng xã hội, gmail, yahoo… khi đã thực hiện xong giao dịch hay các công việc cần làm

- Không nên click vào các đường dẫn mà bạn nhận được qua email, qua facebook … Khi bạn đưa chuột qua 1 đường dẫn, phía dưới bên trái của trình duyệt thường có địa chỉ website đích, bạn nên lưu ý để đến đúng trang mình muốn.

- Không lưu các thông tin về mật khẩu tại trình duyệt của mình (không nên chọn các phương thức “đăng nhập lần sau”, “lưu mật khẩu” …
Trong quá trình thực hiện giao dịch hay vào các website quan trọng không nên vào các website khác, có thể chứa các mã khai thác của kẻ tấn công.

### Đối với các website
- **Hạn chế thời gian hiệu lực của SESSION**

Tùy theo ngôn ngữ hoặc web server dc sử dụng mà cách thực hiện có thể rất khác nhau. Với PHP, thông số “session.gc_maxlifetime” trong file php.ini qui định thời gian hiệu lực của session.
Nếu lập trình viên sử dụng ngôn ngữ k hỗ trợ chức năng này và họ phải quản lý session bằng CSDL (ví dụ lưu bảng session…) hay bằng các tập tin tạm thời (ví dụ /tmp/session/) thì họ sẽ viết các chương trình hỗ trợ việc xóa các session này (cron job,scheduler…)

- **Lựa chọn việc sử dụng GET VÀ POST**

Phương thức GET dc dùng để truy vấn dữ liệu, đối với các thao tác tạo ra sự thay đổi hệ thống thì các phương thức khác như POST hay PUT sẽ dc sử dụng (theo khuyến cáo của W3C tổ chức tạo ra chuẩn http)
- **Sử dụng captcha, các thông báo xác nhận**

Captcha được sử dụng để nhận biết đối tượng đang thao tác với hệ thống là con người hay không? Các thao tác quan trọng như “đăng nhập” hay là “chuyển khoản” ,”thanh toán” thường là hay sử dụng captcha. Tuy nhiên, việc sử dụng captcha có thể gây khó khăn cho một vài đối tượng người dùng và làm họ khó chịu.
Các thông báo xác nhận cũng thường dc sử dụng, ví dụ như việc hiển thị một thông báo xác nhận “bạn có muốn xóa hay k” cũng làm hạn chế các kĩ thuật
Cả hai cách trên vẫn có thể bị vượt qua nếu kẻ tấn công có một kịch bản hoàn hảo và kết hợp với lỗi XSS.

- **Sử dụng token**

Tạo ra một token tương ứng với mỗi form, token này sẽ là duy nhất đối với mỗi form và thường thì hàm tạo ra token này sẽ nhận đối số là”SESSION” hoặc được lưu thông tin trong SESSION. Khi nhận lệnh HTTP POST về, hệ thống sẽ thực hiên so khớp giá trị token này để quyết định có thực hiện hay không.


### LAB : coppy đoạn forrm đổi pass , rồi gán giá trị value để thay đổi , lưu dưới trang khác ( lap đang hỏng nên chưa up được T.T)
