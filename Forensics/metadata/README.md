# Metadata
## Description

A Docker image is a file used to execute code in a Docker container. Docker images act as a set of instructions to build a Docker container, like a template.

Image: vinhph2/hcmus-ctf-2021

**author: `vinhph2`**

## Solution

Dựa vào description thì đây là 1 docker image. Nên pull về máy bằng lệnh `docker pull`. Mình đoán flag sẽ nằm trong các layer (history) của image (vì mỗi lần lưu trạng thái mới thì sẽ ghi chồng lên layer cũ) 
![image](https://user-images.githubusercontent.com/59532111/120071844-2a897180-c0bb-11eb-9dea-a0fb439773fb.png)

Và như vậy, mình đã tìm ra flag, tuy nhiên nó đã bị truncate. Vậy thì cho nó in hết ra và tìm chuỗi có dạng HCMUS-CTF{...}

![image](https://user-images.githubusercontent.com/59532111/120071956-9c61bb00-c0bb-11eb-8f2c-2a1146f55320.png)




## Flag

`HCMUS-CTF{d0ck6r_1mag6_1nsp6ct}`
