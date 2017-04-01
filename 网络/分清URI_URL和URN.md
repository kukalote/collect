#分清URI,URL和URN

## URI

统一资源标识符 (**URI**::Uniform Resource Identifier),包括 **URL**(Uniform Resource Locators) 和 **URN**(Uniform Resource Names)。

按照 URI 标准，上面的第一个例子 —— `http://www.cisco.com/en/US/partners/index.html` —— 实际上是一个 URI，并且它由以下几个组成部分：

1. 方案名 (http)
2. 域名 (www.cisco.com)
3. 路径 (/en/US/partners/index.html)

来自许多不同协议的命名约定得到了统一。其中的一些例子有：

- mailto:mbox@domain
- ftp://host/file
- http://domain/path

## URL 和 URN

**URL**—Uniform Resource Location统一资源定位符URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。

URL一般由三部组成

1. 协议(或称为服务方式)
2. 存有该资源的主机IP地址(有时也包括端口号)
3. 主机资源的具体地址。如目录和文件名等

**URN**，uniform resource name，统一资源命名，是通过名字来标识资源，比如`mailto:java-net@java.sun.com`

URN是URL的一种更新形式，统一资源名称(URN, Uniform Resource Name)不依赖于位置，并且有可能减少失效连接的个数。但是其流行还需假以时日，因为它需要更精密软件的支持。

URN的语法用巴科斯范式来写是：

    <URN> ::= "urn:" <NID> ":" <NSS>

解析出来是：

    urn:<NID>:<NSS>