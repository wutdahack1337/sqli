# sqli
SQL Injection

Top 10 rủi ro bảo mật website năm 2017 và năm 2023 (theo đánh giá của [OWASP](https://owasp.org/www-project-top-ten/)):  
<img width="100%" heigth="600px" src="https://owasp.org/www-project-top-ten/assets/images/mapping.png">

Ta có thể thấy, Injection vẫn chiếm một vị trí rất cao trong bảng xếp hạng này, mặc dù sau 6 năm các công nghệ ngăn chặn lỗi này vẫn liên tục được cập nhật và phát triển. Chứng tỏ, đây vẫn là một lỗi nghiêm trọng và vẫn còn nhiều kẻ hở mà các công ty, lập trình viên hay là các nhà bảo mật chưa khai thác được triệt để nhằm mục đích ngăn chặn các cuộc tấn công của hacker.

Do mức độ nghiêm trọng của Injection, thế nên t quyết định sẽ tìm hiểu về một dạng của nó trong nhiều dạng khác, là **SQL Injection**.  

**Lưu ý**: Bài viết này chỉ mang tính chất giáo dục, học hỏi ~~. Chỉ vọc phá khi được chủ website cho phép hoặc trên labs ảo 

# 1. SQL Injection
## 1.1 Những thứ cần biết
[SQL](https://en.wikipedia.org/wiki/SQL) (hay Structured Query Language) là một ngôn ngữ được sử dụng để truy vấn hoặc thay đổi dữ liệu được lưu trữ trong databases. Được phát triển bởi IBM trong những năm 1970. Hiện nay, SQL đang được sử dụng trong hầu như tất cả hệ thống quản lý cơ sở dữ liệu ([DBMS](https://en.wikipedia.org/wiki/Database)), có thể kể tới là MariaDB (MySQL), Oracle, PostgreSQL,...  

Phía sau hầu hết tất cả các website là databases. Databases có thể được dùng cho các mục đích xác thực người dùng, thương mại điện tử, lưu trữ dữ liệu khách hàng và hàng tá những thông tin nhạy cảm khác. Nếu hackers bằng một cách nào đó chèn và gửi SQL commands thông qua website để tới databases, thì hacker có thể thực thi những commands để thay đổi, xóa thậm chí là trích xuất dữ liệu. Đó cũng chính là mục đích của SQL Injection.

Trước khi thực hành SQLi thì ta phải biết và hiểu một chút về SQL. Một truy vấn SQL cơ bản sẽ có dạng như thế này:  
```
  SELECT <columns>
  FORM <table>
  WHERE <conditions>
```
Những truy vấn đơn giản này có thể dùng để xác thực dữ liệu người dùng. Hãy tưởng tượng một database có tên là "users" lưu trữ username và password của tất cả người dùng. Trên giao diện web, nếu người dùng muốn đăng nhập thì phải nhập username và password vào một form đăng nhập, và sau đó check trong database xem có người dùng nào có username và password trùng khớp. Truy vấn sẽ nhìn như thế này khi người dùng nhập thông tin vào form:
![image](https://github.com/wutdahack1337/sqli/assets/153523415/56b2d22a-6cb5-46dc-8ec5-13795ad57132)
```
  SELECT username, password
  FORM users
  WHERE username = 'wutdahack1337' AND password = 'hahahaha'
```
Nếu trong lệnh WHERE trả về TRUE thì người dùng đó có tồn tại và sẽ được thông báo xác thực thành công, còn nếu FALSE thì đăng nhập không thành công.

Và một thứ quan trọng cần phải biết đó chính là "--", nếu "--" xuất hiện trong câu lệnh, nó sẽ không quan tâm tới mọi thứ phía sau nó (giống "//" trong C và "#" trong Python)  
Hãy thử đoán xem chuyện gì sẽ xảy ra nếu t nhấn nút "Log in" khi nhập thông tin này?
![image](https://github.com/wutdahack1337/sqli/assets/153523415/9f857e78-db26-4d37-ba5a-9865be73d528)

## 1.2 Vào vấn đề chính
Bây giờ, khi thông tin được gửi vào database, lệnh truy vấn SQL sẽ nhìn như sau:
```
  SELECT username, password
  FORM users
  WHERE username = 'wutdahack1337' OR 1 = 1 -- AND password = 'hahahaha'
```
Như t đã nói trước đó, "--" sẽ lờ đi tất cả những gì đằng sau nó, vậy truy vấn trở thành:
```
  SELECT username, password
  FORM users
  WHERE username = 'wutdahack1337' OR 1 = 1
```
Do lệnh WHERE luôn trả về TRUE nên chúng sẽ đăng nhập được mà không cần tới mật khẩu!
Tới đây bạn đã có thể giải được lab [này](https://portswigger.net/web-security/sql-injection/lab-login-bypass).

## 2. sqlmap Tool (coming soon)
