# 1 URL编码

http报文中，不能直接传递中文，需要采用URL编码

比如`name=张`

1. 对张三进行utf-8编码，返回16进制`E5 BC A0`
2. 在16进制数前加`%`，请求路径如下
3. 该规则适用于post请求

```http
GET /test2?name=%E5%BC%A0&age=20 HTTP/1.1
Host: localhost
```

# 2 POST请求

## 2.1 application/x-www-form-urlencoed

- form表单默认请求方式
- 参数由参数名和参数值组成。中间使用=分割，多参数使用&分割
- 特殊字符需要使用url编码
- `Content-Length`记录的是请求体长度

```http
POST /test2 HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 21

name=%E5%BC%A0&age=18
```

## 2.2 application/json

- 发送json的请求方式
- 不需要对特殊字符进行url编码
- `Content-Length`记录的是json格式长度。算`{}`

```http
POST /test3 HTTP/1.1
Host: localhost
Content-Type: application/json
Content-Length: 25

{"name":"zhang","age":18}
```

## 2.3 multipart/form-data

- 除了上传文件，该方式也可以上传数据
- boundary定义分隔符，结束分隔符要用`--分隔符--`
- `Content-Length`需要计算换行

```http
POST /test2 HTTP/1.1
Host: localhost
Content-Type: multipart/form-data; boundary=123
Content-Length: 125

--123
Content-Disposition: form-data; name="name"

lisi
--123
Content-Disposition: form-data; name="age"

30
--123--
```

