Chào các bạn, hôm nay mình xin chia sẻ với các bạn cách chỉnh sửa file hosts trên một số hệ điều hành thông dụng.

## Linux

1.  Mở cửa sổ terminal
2.  Gõ lệnh **sudo vim /etc/hosts** để mở file hosts bằng chương trình text editor **vim**
3.  Nhập password admin của bạn
4.  Bấm phín **Insert** hoặc phím **i** để bắt đầu chỉnh sửa file
5.  Tiến hành chỉnh sửa nội dung file hosts
6.  Sau khi chỉnh sửa xong bấm **Esc**, sau đó gõ **:wq!** để thoát khỏi màn hình **vim**, như thế là đã xong

## Windows 10 và 8

1.  Nhấn phím **Windows**
2.  Trong khung tìm kiếm gõ chữ **notepad**
3.  Trong kết quả trả về, bấm chuột phải vào **Notepad** và chọn **Run as administrator**
4.  Khi chương trình **Notepad** mở ra, mở file hosts tại đường dẫn sau **C:\Windows\System32\drivers\etc\hosts**
5.  Bắt đầu chỉnh sửa nội dung file
6.  Sau đó chọn **File > Save** để lưu lại những thay đổi

## Mac OS X

1.  **Applications > Utilities > Terminal**
2.  Mở file hosts bằng cách gõ lệnh sau **sudo nano /private/etc/hosts**
3.  Nhập password của bạn
4.  Chỉnh sửa file hosts
5.  Lưu lại thay đổi bằng cách nhấm tổ hợp phím **Ctrl X** sau đó nhấn **y**
6.  Để những thay đổi có hiệu lực, gõ lệnh sau để flush DNS cache **dscacheutil -flushcache**
