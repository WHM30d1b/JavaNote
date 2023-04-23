## 面试题

#### 1. Nginx 是什么

 用于 HTTP、HTTPS、SMTP、POP3 和 IMAP 协议的 Web 服务器和反向代理服务器



#### 2. 如何处理 HTTP 请求

**反应器模式**：主事件循环等待操作系统发出准备事件的信号，数据可以从 Socket 套接字读取到缓冲区并进行处理。单个线程可 以提供数万个并发连接。



#### 3. 使用未定义的服务器名称阻止处理请求

```json
Server {listen 80 ;server_name "" ;return 444;}
```

服务器名被保留为一个空字符串，在没有主机头字段的情况下匹配请求，返回 Nginx 非标准代码 444，从而终止连接。



#### 4. 使用反向代理服务器的优点

隐藏源服务器的存在和特征，充当云和 web 服务器的中间层，更加安全。



#### 5. Master 进程和 Worker 进程

Master 进程：主进程，读取判断配置文件，管理 Worker 进程

Worker 进程：处理请求



#### 6. C10K 问题

无法同时处理大量客户端 ( 10,000 ) 的网络套接字



#### 7. 杂谈

- 换端口：Like server { listen 81; }

- 将 Nginx 错误替换为 502 或 503 错误

  502：错误网关；503：服务器超载

  - fastcgi_intercept_errors on
  - error_page 502 503 error_page.html

- URL 双斜线：merge_slashes [on / **off**]

- ngx_http_upstream_module：将多个服务器定义为服务器组，通过 fastcgi 传递、proxy 传递、uwsgi 传递、memcached 传递和 scgi 传递指令

- stub_status：Nginx 当前状态，活动链接和读写等待链接的总数

  sub_filter：过滤响应中的内容

- 获得当前时间：SSI 模块中的 $date_gmt 和 $date_local 的变量。















