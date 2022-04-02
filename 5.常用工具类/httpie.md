
> https://httpie.io/docs#examples

## install

```bash
brew update
brew install httpie
brew upgrade httpie

apt install httpie

python -m pip install --upgrade pip wheel
python -m pip install httpie
python -m pip install --upgrade httpie
```

## 实例

methods (OPTIONS, GET, POST, HEAD, PUT, PATCH, DELETE)

```bash
https httpie.io/hello

# json
http PUT pie.dev/put X-API-Token:123 name=John
http PUT pie.dev/put name=John email=john@example.org

http https://api.github.com/search/repositories q==httpie per_page==1

http DELETE pie.dev/delete

http -f POST pie.dev/post hello=World
http --form POST pie.dev/post name='John Smith'

http -f POST pie.dev/post name='John Smith' cv@~/files/data.xml
http PUT http://127.0.0.1/webdav_test_inception/test.php --auth user:pass < test.php

http --offline pie.dev/post hello=offline

http pie.dev/post < files/data.json

http pie.dev/image/png > image.png

# wget
http --download pie.dev/image/png

# lfi
http --path-as-is -v example.org/./../../etc/password

http -a username:password pie.dev/basic-auth/username/password

http --proxy=http:http://user:pass@127.0.0.1:8080 example.org

http --verify=no https://pie.dev/get
```
