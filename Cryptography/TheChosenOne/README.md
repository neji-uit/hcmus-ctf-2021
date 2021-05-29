# TheChosenOne
## Description

The cryptography technique can be good, but the implementation is bad. Do you know the weakness of [AES-ECB](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)? (Inspired from some old stuff with a little bit easier =D )

**`nc 61.28.237.24 30300`**  
**`author: pakkunandy`**

## Solution

Vì mode ECB sẽ mã hóa từng block 16-byte và việc mã hóa block này sẽ không ảnh hưởng để block kế tiếp.
Nên mình sẽ bruteforce bằng cách sử dụng phân tích mã hóa của 2 block 16-byte. Block đầu chứa chuỗi padding của mình, và kí tự cần đoán. Block thứ hai cũng chứa chuỗi padding và kí tự flag của chương trình (độ dài padding thay đổi theo số kí tự flag mình đã đoán được.

Đây là code 
```python
#!/usr/bin/python2

from pwn import *
import os.path

context.log_level = 'error'
paddingStr = 'RamjhzCXMKP4nIrRQVPvJ60QD5iObD0y'

def main():
    flagGuessed = ''
    flagGuessing = ''

    if (os.path.exists('AES_flag.txt')):
        with open('AES_flag.txt', 'r') as flag:
            flagGuessed = flag.read()
    
    round = 1

    for i in range(1, 33):
        for character in range(32, 127):
            flagGuessing = flagGuessed + chr(character)
            sendingText = paddingStr[:16 - i] + flagGuessing + paddingStr[:16 - i]  
            print ("Round {0:0>3} - sending {1}".format(round, sendingText))

            q = remote('61.28.237.24', 30300)
            q.recvuntil('=====================================\r\n\r\nYour input:')
            q.sendline(sendingText)
            q.recvuntil('The ciphertext:\r\n')
            receivedText = q.recv(128)
            q.close()

            print(receivedText)
            if (receivedText[:32] == receivedText[32:64]):
                flagGuessed += chr(character)
                break
            round += 1
        print("Round {0:0>3} - found {1}".format(round, flagGuessed))
        with open('AES_flag.txt', 'w') as flag:
            flag.write(flagGuessed)

if __name__ == "__main__":
    main()
```

## Flag

`HCMUS-CTF{You_Can_4ttack_A3S!?!}`
