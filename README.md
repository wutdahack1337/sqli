# sqli
SQL Injection

Top 10 rủi ro bảo mật website năm 2017 và năm 2023 (theo đánh giá của [OWASP](https://owasp.org/www-project-top-ten/)):  
<img width="100%" heigth="600px" src="https://owasp.org/www-project-top-ten/assets/images/mapping.png">

Có thể nhận thấy Injection vẫn chiếm một vị trí cao trong bảng xếp hạng này, dù sau 6 năm các công nghệ ngăn chặn lỗi này vẫn liên tục được cập nhật và phát triển. Điều này cho thấy, Injection vẫn là một lỗi nghiêm trọng và còn nhiều kẻ hở chưa được khai thác triệt để nhằm ngăn chặn hacker tấn công.

Injection có thể được chia thành nhiều loại, nhưng hôm nay t sẽ tập trung vào loại phổ biến nhất đó là **SQL Injection**

**Lưu ý**: Bài viết này chỉ mang tính chất giáo dục và học tập, chỉ vọc phá khi có sự cho phép của chủ website hoặc trong các bài labs ảo ~~

# I. SQL Injection
## 1. Những thứ cần biết
[SQL](https://en.wikipedia.org/wiki/SQL) (hay Structured Query Language) là một ngôn ngữ lập trình được sử dụng để truy vấn hoặc quản lý, tương tác với dữ liệu được lưu trữ trong databases. Nó được phát triển bởi IBM vào những năm 1970 và hiện nay đang được sử dụng rộng rãi trong hệ thống quản lý cơ sở dữ liệu ([DBMS](https://en.wikipedia.org/wiki/Database)), như là MariaDB (MySQL), Oracle, PostgreSQL, ...  

Databases có thể được sử dụng cho các mục đích xác thực người dùng, thương mại điện tử, lưu trữ dữ liệu khách hàng và hàng tá những thông tin nhạy cảm khác. Nếu hackers chèn và gửi các lệnh SQL độc hại vào forms hoặc tham số URL, hacker có thể thực thi những commands để thay đổi, xóa thậm chí là trích xuất dữ liệu từ databases. Đó cũng chính là mục tiêu của SQL Injection.

Trước khi thực hành SQLi, ta cần phải biết và hiểu một chút về SQL. Một truy vấn SQL cơ bản sẽ có dạng như sau:  
```
  SELECT <columns>
  FORM <table>
  WHERE <conditions>
```
Những truy vấn đơn giản này có thể dùng để xác thực dữ liệu người dùng. Ví dụ có một table database tên là "users" lưu trữ username và password của tất cả người dùng. Trên giao diện web, nếu người dùng muốn đăng nhập, thì họ sẽ phải nhập username và password vào một form đăng nhập, và sau đó hệ thống sẽ kiểm tra xem có bất kỳ username và password nào trùng khớp hay không.

![image](https://github.com/wutdahack1337/sqli/assets/153523415/56b2d22a-6cb5-46dc-8ec5-13795ad57132)  
Truy vấn sẽ nhìn như thế này:  
```
  SELECT username, password
  FORM users
  WHERE username = 'wutdahack1337' AND password = 'hahahaha'
```
Nếu điều kiện trong WHERE trả về TRUE, người dùng sẽ được xác thực thành công, người lại thì đăng nhập không thành công.

Hai thứ quan trọng cần phải biết để có thể tấn công SQLi là "--"/"#" và lệnh "UNION". Nếu "--" xuất hiện trong một câu lệnh, nó sẽ bỏ qua tất cả mọi thứ phía sau nó (giống "//" trong C). Còn lệnh "UNION" được sử dụng để kết hợp nhiều kết quả của nhiều truy vấn lại với nhau, để UNION hoạt động, số lượng và kiểu dữ liệu của các cột phải khớp với nhau.

Hãy thử đoán xem chuyện gì sẽ xảy ra nếu t nhấn nút "Log in" khi nhập thông tin sau:
![image](https://github.com/wutdahack1337/sqli/assets/153523415/9f857e78-db26-4d37-ba5a-9865be73d528)

## 2. Bypass Xác Thực Người Dùng
Khi thông tin được gửi vào database, lệnh truy vấn SQL sẽ nhìn như sau:
```
  SELECT username, password
  FORM users
  WHERE username = 'wutdahack1337' OR 1 = 1 --' AND password = 'hahahaha'
```
Như t đã nói trước đó, "--" sẽ lờ đi tất cả những gì đằng sau nó, vậy truy vấn trở thành:
```
  SELECT username, password
  FORM users
  WHERE username = 'wutdahack1337' OR 1 = 1
```
Vì điều kiện trong lệnh WHERE luôn trả về TRUE nên t sẽ đăng nhập được mà không cần tới mật khẩu!!!  

Sau khi đã nắm được ý tưởng chính của SQLi, t đã có thể giải được lab [này](https://portswigger.net/web-security/sql-injection/lab-login-bypass) và lab [này](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data).

## 3. Trích Xuất Dữ Liệu

# II. sqlmap Tool (coming soon)
