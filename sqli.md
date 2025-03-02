---
title: 'SQL Injection'
category: 'Learning'
date: '01-02-2025'
---

# Tips and Tricks
- Với SQL Injection (sqli) thì điều cần nhớ là phải kiểm tra lại DBMS và version của nó.
- Nếu có chuỗi + `''` thì nó vẫn là chuỗi, ví dụ:

    - `select version() ''` ***tương đương*** `select version()`
- Sử dụng group_concat nếu cần gom data thành 1 cột.
- Tham khảo [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection) nếu thấy mình ngu quá...

# Quy trình
1, Enumeration: Thử các trường hợp dấu đặc biệt như `' " -- # ; /* */ %0b (null byte)`, các loại keyword `and or not select from where from` để xem cái nào bị cấm, cái nào dùng được
2, Từ việc enum, lựa chọn 1 trong 2 các inject: Chèn cuối và chèn giữa

- Chèn cuối dùng trong trường hợp có thể dùng comment, có thể đóng chuỗi (đóng dấu `' "`) hoặc kết thúc câu bằng `;`.
- Chèn giữa nếu ko thể làm các trường hợp trên.

3, Kiểm tra loại và version DMBS: (Nhớ kiểm tra đủ loại và đừng quên loại trừ việc target dùng DBMS lạ)
- OracleSQL: 
```sql
SELECT banner FROM v$version
SELECT version FROM
v$instance
```

- MSSQL: 
```sql
SELECT banner FROM v$version
SELECT version FROM
v$instance
```

- MSSQL: 
```sql
SELECT @@version
```

- MySQL:
```sql
SELECT @@version
```

- PostgreSQL:
```sql
SELECT version()
```

- SQLite:
```sql
select sqlite_version();
```

- Vài loại nữa có thể xem trên PayloadsAllTheThings

4, Việc còn lại là dựa trên sự khôn ngu của bạn...