# Memory
## Description

There is a computer incident. We have collected the memory dump of the victim's computer. Can you guys help us to retrive some evidence?

**`author: pakkunandy `**

## Solution

Mình sử dụng công cụ `volatility` để phân tích file memory dump

Đầu tiên mình xem thử `clipboard` và `cmd` (lý do ở phần dưới) có gì

![image](https://user-images.githubusercontent.com/59532111/120072258-1e9eaf00-c0bd-11eb-98ba-7f9b90fccbca.png)

Aha, được 2 hint là flag tìm online, vậy có thể là 1 file online, hoặc là 1 cái gì đó liên quan đến việc kết nối ra mạng. Hint thứ 2 là part 1 của password


Sau đó, mình tiếp tục dò lại danh sách process (lần trước mình thấy cmd.exe và conhost.exe nên đã cmdscan)

![image](https://user-images.githubusercontent.com/59532111/120072359-bbf9e300-c0bd-11eb-9db2-8104ac56fdbd.png)

Và thầy có 1 process của notepad, nên mình dump ra để phân tích

![image](https://user-images.githubusercontent.com/59532111/120072481-59551700-c0be-11eb-8d20-7015e1fac82e.png)

Và in các chuỗi có chứa cụm "flag"

![image](https://user-images.githubusercontent.com/59532111/120072558-ad5ffb80-c0be-11eb-8981-c38dca9c7ae9.png)

Xuất hiện cụm `flag.txt.txt` nên mình tìm xem trong memory về file có tên đó

![image](https://user-images.githubusercontent.com/59532111/120072691-4abb2f80-c0bf-11eb-89b6-32a6316c0e7c.png)

Và đúng là có file đó trong memory, vậy mình dump file đó ra để đọc. (`0x000000001e903f20` là offset của file trong memory)

![image](https://user-images.githubusercontent.com/59532111/120072804-c1f0c380-c0bf-11eb-8fdc-4e1d1f8e9200.png)

Vậy tới đây mình đã có password hoàn chỉnh là `SuP3r_P@zzw0rD`. Quay lại với hint đầu tiên, là cần tìm file flag online. Và khi nãy mình list các process thì thấy có `chrome.exe`. Vậy mình sử dụng chromeplugin cho `volitality` xem history

![image](https://user-images.githubusercontent.com/59532111/120072912-417e9280-c0c0-11eb-8df5-6c562eea18c3.png)

Vậy mình tải file từ đường dẫn đó, sử dụng password đã tìm để unzip và được flag

![image](https://user-images.githubusercontent.com/59532111/120072943-6115bb00-c0c0-11eb-8b19-52d1fecce137.png)


## Flag

`HCMUS-CTF{simple_memory_forensics_stuff}`
