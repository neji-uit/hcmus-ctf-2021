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

### Phân tích `CheckPass.class`
Đây là một file class trên ngôn ngữ `java` đã được build từ .java nên mình sử dụng `JD-GUI` để decompile ra code java như dưới
```java
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Scanner;

public class CheckPass {
  public static void main(String[] paramArrayOfString) {
    Scanner scanner = new Scanner(System.in);
    System.out.println("Please enter the password!!");
    String str1 = scanner.nextLine();
    if (str1.length() != 8) {
      System.out.println("Oops!!");
      return;
    } 
    if (!str1.substring(0, 6).matches("[0-9]+")) {
      System.out.println("Oops!!");
      return;
    } 
    if (!str1.substring(0, 1).equals(str1.substring(5, 6))) {
      System.out.println("Oops!!");
      return;
    } 
    if (!str1.endsWith("}")) {
      System.out.println("Oops!!");
      return;
    } 
    String str2 = "(?![@',&])\\p{Punct}";
    if (!str1.substring(6, 7).matches(str2)) {
      System.out.println("Oops!!");
      return;
    } 
    if (!getMd5(str1).equals("53e443c9f65cd5f816452ae66ec65834")) {
      System.out.println("Oops!!");
      return;
    } 
    System.out.println("You get the second part!!!");
  }
  
  public static String getMd5(String paramString) {
    try {
      MessageDigest messageDigest = MessageDigest.getInstance("MD5");
      byte[] arrayOfByte = messageDigest.digest(paramString.getBytes());
      BigInteger bigInteger = new BigInteger(1, arrayOfByte);
      String str = bigInteger.toString(16);
      while (str.length() < 32)
        str = "0" + str; 
      return str;
    } catch (NoSuchAlgorithmException noSuchAlgorithmException) {
      throw new RuntimeException(noSuchAlgorithmException);
    } 
  }
}
```

Từ đoạn code trên, mình kết luận được chuỗi pass mà file này kiểm tra có 8 kí tự, 6 kí tự đầu là số (số thứ 1 và thứ 6 giống nhau), kí tự cuối cùng là `}`, và kí tự thứ 7 là một trong các kí tự `[!"@'#$%&()*+\-.,/:;<=>?[\]^_``{|}~]` ngoại trừ `@',&`.
**Và một điều kiện nữa là hash md5 của chuỗi pass phải là `53e443c9f65cd5f816452ae66ec65834`.**
Nên mình viết đoạn code để bruteforce ra kết quả

```python
import hashlib

ANSWER = '53e443c9f65cd5f816452ae66ec65834'
PUNCHS = '[!"#$%()*+\-./:;<=>?[\]^_`{|}~]'

TEMPLATE = '{0}{1}{2}{3}{4}{5}{6}}}'

for first_char in range(0, 10):
    for second_char in range(0, 10):
        for third_char in range(0, 10):
            for fourth_char in range(0, 10):
                for fifth_char in range(0, 10):
                    for sixth_char in PUNCHS:
                        now = TEMPLATE.format(first_char, second_char, third_char, fourth_char, fifth_char, first_char, sixth_char)
                        hashed = (hashlib.md5(now.encode('ascii'))).hexdigest()
                        print("String: {0}".format(now))
                        print("Hashed: {0}".format(hashed))
                        if (hashed == ANSWER):
                            exit(0)
```

## Flag
