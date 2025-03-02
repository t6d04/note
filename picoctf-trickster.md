---
title: 'picoCTF - Trickster'
date: '08-02-2025'
category: 'ctf'
---

# Đề bài
Bài medium đầu tiên trên picoCTF
![alt text](/picoctf-trickster/image-3.png)
Ta có một đề bài có vẻ là liên quan đến file.
# First Look
Đúng như dự đoán thì ta có một bài liên quan đến dạng `File Upload`.
![alt text](/picoctf-trickster/image-4.png)
Ta thử xem file `robots.txt` xem có không
![alt text](/picoctf-trickster/image-9.png)
Với `/instructions.txt`:
![alt text](/picoctf-trickster/image-10.png)
Như vây ta có thể biết rằng ta phải thêm magic byte của file png và phải có đuôi `.png` để có thể submit được.
Với '/uploads':
![alt text](/picoctf-trickster/image-11.png)
Dự đoán rằng các file được upload nằm sau directort này
# Thử nghiệm
Ta sẽ mò sẽ thử upload lên 1 vài loại file xem sao, trước tiên là đúng file PNG:
![alt text](/picoctf-trickster/image-5.png)
Thử với file PNG nặng hơn tí xem sao:
![alt text](/picoctf-trickster/image-6.png)
Từ cái exception nhỏ này ta có được vài thông tin nhỏ nhỏ như sau:
> - Web app viết bằng PHP
> - Document Root nằm ở `/var/www/html`
> - File được xử lý bằng `file_get_content`
Ta tiếp tục với file PHP, ta sẽ file `test.php` có nội dung như sau:
```php
<?php echo "HelloWorld" ?>
```
Kết quả sau khi upload là:
![alt text](/picoctf-trickster/image-7.png)
Không được, ta thử thêm vào extension `.png` như exception thành `test.png.php` xem sao:
![alt text](/picoctf-trickster/image-8.png)
Có vẻ là ta phải biến nó thành file PNG thật.

# Tạo payload
Ta sử dụng một script nhỏ để làm cho nhanh:
```bash
# The magic bytes for PNG
echo '89 50 4E 47 0D 0A 1A 0A' | xxd -p -r >> reverse.png.php
echo "<?php echo 'HelloWorld' ?>" >> reverse.png.php
```
> Trong đó cái `xxd` 1 tool đọc hex hoặc là dịch ngược lại
> - Với `-p` là chỉ in ra dạng hexdump style, tức là chỉ có số kiểu như: ![alt text](/picoctf-trickster/image-13.png)
> - Với `-r` là lật dạng hex thành dạng bin

![alt text](/picoctf-trickster/image-12.png)
Một cách nữa là chuyển thẳng thành dạng UTF-8 rồi copy vào. 
- Ta dùng CyberChef để chuyển:
![alt text](/picoctf-trickster/image-14.png)
- Rồi paste vào BurpSuite kèm đoàn payload ở sau:
![alt text](/picoctf-trickster/image-15.png)

# Kiếm flag
Việc bây giờ là lục lọi nốt:
![alt text](/picoctf-trickster/image-16.png)
![alt text](/picoctf-trickster/image-17.png)
Ta thấy có một file có tên lạ, ta sẽ `cat` nó lên là ta đã có flag rồi!