---
title: "Python install cheatsheets"
date: 2018-12-20 13:00:00 -0400
---

Python Install Cheatsheets

## Reference
> [https://kimpaper.github.io/2016/02/12/install-python3/] [1]

```sh
# minimal 설치시
# yum install gcc wget -y # 추가 필요
yum install zlib-devel -y
yum install openssl openssl-devel -y


wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tar.xz
tar -xvf Python-3.6.6.tar.xz

cd Python-3.6.6
./configure --prefix=/usr/local --enable-shared LDFLAGS="-Wl,-rpath /usr/local/lib"
make && make altinstall

# pip설치 
curl -k -O https://bootstrap.pypa.io/get-pip.py
python3.6 get-pip.py
```

[1]: https://kimpaper.github.io/2016/02/12/install-python3/