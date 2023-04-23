## 面试题

#### 1. 什么是 Servlet

用来处理客户端请求并产生动态网页内容的 Java 类，主要是用来处理或存储 HTML 表单提交的数据，产生动态内容，在无状态的 HTTP 协议下管理状态信息



#### 2. Servlet 体系结构

Servlet 必须实现核心接口 javax.servlet.Servlet，或继承 javax.servlet.GenericServlet / javax.servlet.http.HTTPServlet

Servlet 使用多线程可以并行的为多个请求服务



#### 3. GenericServlet 和 HttpServlet

- GenericServlet：通用的，**协议无关**的 Servlet，实现了 Servlet 和 ServletConfig 接口。继承自 GenericServlet 的 Servlet 应该要覆盖 service() 方法。
- HttpServlet：在网页上，服务于使用 **HTTP 协议**请求的 Servlet 必须要继承自 HttpServlet



#### 4. Servlet 生命周期

- 客户端发起请求
- Servlet 引擎载入，调用 init 初始化方法创建 Servlet
- Servlet 对象为每个请求调用 service 方法
- 最后，调用 Servlet 的 destroy 方法删除对象



#### 5. doGet 和 doPost

- doGet：GET 方法把名值对追加在请求的 URL 后面。URL 对字符数目有限制，限制了客户端请求参数值的数目。并且请求参数值可见，不能传递敏感信息
- doPOST：POST 方法通过把请求参数值放在请求体中来打破 GET 方法的限制，发送的参数数目是没有限制的。请求传递的敏感信息对外部客户端不可见



#### 6. Web 应用程序

对 Web 或应用服务器的动态扩展，Web 应用可以看成一组安装在服务器 URL 名称空间的特定子集下面的 Servlet 集合



#### 7. 服务端包含 SSI

仅用在 Web 上的简单解释型服务端脚本语言

场景：把一个或多个文件包含到 Web 服务器的一个 Web 页面中，当浏览器访问页面时，服务器用对应 servlet 产生的文本替换页面中的 servlet 标签



#### 8. Servlet Chaining

Servlet 链：把一个 Servlet 的输出发送给另一个 Servlet 的方法。链条上最后一个 Servlet 负责把响应发送给客户端



#### 9. 获取客户端的信息

ServletRequest 类可以找出客户端机器的 IP 地址或主机名

- getRemoteAddr() 获取客户端主机的 IP 地址
- getRemoteHost() 获取主机名



#### 10. Http 响应结构

- 状态码 Status Code：描述响应的状态。用来检查是否成功的完成请求。请求失败的情况下，状态码用来找出失败的原因。如果 Servlet 没有返回状态码，默认返回成功的状态码 HttpServletResponse.SC_OK
- 响应头 Http Header：包含更多关于响应的信息。如响应过期的日期或给用户安全传输实体内容的编码格式
- 响应体 Body：包含相应的内容，如 HTML 代码，图片等



#### 11. Cookie 和 Session

Cookie：Web 服务器发送给浏览器的信息，浏览器会在本地文件中给每一个 Web 服务器存储 Cookie。以后浏览器在给特定的 Web 服务器发请求的时候，同时会发送所有为该服务器存储的 Cookie

Session：服务器给每个客户端分配不同的 “身份标识”

- session 能够存储任意的 Java 对象，cookie 只能存储 String 类型的对象



#### 12. Http 隧道

利用 HTTP 或 HTTPS 把多种网络协议封装起来进行通信的技术

HTTP 协议扮演了打通用于通信的网络协议的管道包装器的角色，**把其他协议的请求掩盖成 HTTP 的请求**



#### 13. sendRedirect 和 forward

sendRedirect()：创建一个新的请求，之前请求作用域范围以内的对象就会失效

forward()：把请求转发到一个新的目标上，之前请求作用域范围以内的对象还是能访问的

sendRedirect() 比 forward() 慢



#### 14. URL 编码和解码

编码：把 URL 里面的空格和其他的特殊字符替换成对应的十六进制表示



#### 15. JSP 请求的处理过程

- 首先，浏览器请求一个以 .jsp 扩展名结尾的页面，发起 JSP 请求

- Web 服务器读取请求，使用 JSP 编译器把 JSP 页面转化成一个 Servlet 类

  只有当第一次请求页面或 JSP 文件发生改变的时候 JSP 文件才会被编译

- 服务器调用 Servlet 类，处理浏览器的请求

- 处理结束，Servlet 把响应发送给客户端























