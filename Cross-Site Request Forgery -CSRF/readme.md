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

<a name="2"></a>
## Cách phòng tránh
- **đối với người dùng**:Nên thoát khỏi các website quan trọng: Tài khoản ngân hàng, thanh toán trực tuyến, các mạng xã hội, gmail, yahoo… khi đã thực hiện xong giao dịch hay các công việc cần làm

- Không nên click vào các đường dẫn mà bạn nhận được qua email, qua facebook … Khi bạn đưa chuột qua 1 đường dẫn, phía dưới bên trái của trình duyệt thường có địa chỉ website đích, bạn nên lưu ý để đến đúng trang mình muốn.

- Không lưu các thông tin về mật khẩu tại trình duyệt của mình (không nên chọn các phương thức “đăng nhập lần sau”, “lưu mật khẩu” …
Trong quá trình thực hiện giao dịch hay vào các website quan trọng không nên vào các website khác, có thể chứa các mã khai thác của kẻ tấn công.
