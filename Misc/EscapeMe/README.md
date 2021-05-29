# EscapeMe
## Description

Can you escape me!

`ssh ctf@61.28.237.24 -p30402`
password: `hcmus-ctf`

**`author: pakkunandy`**

## Solution

Mình cũng không nhớ rõ nữa khi viết write-up bài này. Mà server cũng đóng mất tiêu
Khi sử dụng `ls -la` để xem các file trong `~` thì thấy file `flag.txt` nhưng quyền đọc chỉ có `root` trong khi mình ssh đến bằng user `ctf`.
Nên mình sử dụng `sudo -l` để xem lệnh mà có thể sử dụng `sudo` không cần password.
Kết quả là lệnh python.
Vậy nên sử dụng `sudo python` và lúc này mình đã `privilege escalation` thành công. 
Mình có thể sử dụng dòng lệnh python để đọc file `flag.txt` mà không cần password root

## Flag
