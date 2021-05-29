# Maquerade
## Description

You can find all you need in the network communication =D 
Nothing is encrypted? Hmmm...
UPDATED: The alternative file that you can find is now: ***edited*** (alternative file I included in this challenge folder - neji-uit)

**`author: pakkunandy`**"

## Solution

Đây là một file pcapng nên mình dùng `wireshark` để phân tích. Mở ra và follow theo TCP stream thì mình thấy được stream "trò chuyện" này :))

![image](https://user-images.githubusercontent.com/59532111/120073667-86f08f00-c0c3-11eb-9c5e-5e6151d5636e.png)

Và tiếp sau đó là các HTTP stream về tải file 

### File `secret.zip`
![image](https://user-images.githubusercontent.com/59532111/120073680-996ac880-c0c3-11eb-8f6f-eacc22ad4ee5.png)

### File `OTP.mp3`
![image](https://user-images.githubusercontent.com/59532111/120073686-a25b9a00-c0c3-11eb-9e54-9eeb07569bd7.png)

### File `CheckPass.class`
![image](https://user-images.githubusercontent.com/59532111/120073697-a982a800-c0c3-11eb-9809-479537ae76bb.png)

Dùng tính năng `export object` của `wireshark` để lưu riêng các file 
![image](https://user-images.githubusercontent.com/59532111/120073712-ba331e00-c0c3-11eb-9980-2d152473fe1d.png)

File `secret.zip` bị lỗi nên BTC đã cung cấp file mới (mình có upload trong folder này luôn). Sử dụng chuỗi nghe được từ `OTP.mp3` là `pwd785$` để mở file `secret.zip` thì được part 1 của flag: `HCMUS-CTF{Just_Network_Stuff_`



## Flag
