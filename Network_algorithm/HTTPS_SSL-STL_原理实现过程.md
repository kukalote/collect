# HTTPS (SSL/STL)原理

来源 : http://www.cnblogs.com/digdeep/p/4832885.html
来源 : http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html
来源 : http://www.guokr.com/post/114121/
来源 : http://www.cnblogs.com/digdeep/p/4833856.html

### 1. 基本的运行过程

HTTPS 的基本运行过程：

1. 利用对称加密算法来加密网页内容，那么如何保证对称加密算法的秘钥的安全呢？

2. 使用非对称加密算法来获得对称加密算法的秘钥，从而保证了对称加密算法的秘钥的安全，也就保证了对称加密算法的安全。

这里这样安排使用的原理是，利用了对称加密算法和非对称加密算法优点，而避免了它们的缺点。利用了对称加密算法速度快，而非对称加密算法安全的优点；同时巧妙的避免了对称加密算法的不安全性，以及需要同步密钥的缺点，也避免了非对称加密算法的速度慢的缺点。实在是巧妙了。

这里有两个问题：

> （1）如何保证非对称加密算法公钥不被篡改？

> **解决方法**：将公钥放在数字证书中。只要证书是可信的，公钥就是可信的。

> （2）公钥加密计算量太大，如何减少耗用的时间？

> **解决方法**：每一次对话（session），客户端和服务器端都生成一个"对话密钥"（session key），用它来加密信息。由于"对话密钥"是对称加密算法，所以运算速度非常快，而服务器公钥只用于加密"对话密钥"本身，这样就减少了加密运算的消耗时间。（也就是网页内容的加密使用的是对称加密算法）

因此，SSL/TLS协议的基本过程是这样的：

    1. 客户端向服务器端索要并验证非对称加密算法的公钥。

    2. 双方协商生成对称加密算法的"对话密钥"。

    3. 双方采用对称加密算法和它的"对话密钥"进行加密通信。

上面过程的前两步，又称为"握手阶段"（handshake）。

### 2. 握手阶段的详细过程

![握手阶段的详细过程](./images/ssl_progress1.png)

"握手阶段"涉及四次通信，我们一个个来看。需要注意的是，"握手阶段"的所有通信都是明文的。

1. 客户端发出请求（ClientHello）
首先，客户端（通常是浏览器）先向服务器发出加密通信的请求，这被叫做ClientHello请求。
在这一步，客户端主要向服务器提供以下信息。

		1. 浏览器支持的SSL/TLS协议版本，比如TLS 1.0版。

		2. 一个浏览器客户端生成的随机数，稍后用于生成对称加密算法的"对话密钥"。

		3. 浏览器支持的各种加密方法，对称的，非对称的，HASH算法。比如RSA非对称加密算法，DES对称加密算法，SHA-1 hash算法。

		4. 浏览器支持的压缩方法。

	这里需要注意的是，客户端发送的信息之中不包括服务器的域名。也就是说，理论上服务器只能包含一个网站，否则会分不清应该向客户端提供哪一个网站的数字证书。这就是为什么通常一台服务器只能有一张数字证书的原因。

	对于虚拟主机的用户来说，这当然很不方便。2006年，TLS协议加入了一个Server Name Indication扩展，允许客户端向服务器提供它所请求的域名。

2. 服务器回应（SeverHello）

	服务器收到客户端请求后，向客户端发出回应，这叫做SeverHello。服务器的回应包含以下内容。

		1. 确认使用的加密通信协议版本，比如TLS 1.0版本。如果浏览器与服务器支持的版本不一致，服务器关闭加密通信。

		2. 一个服务器生成的随机数，稍后用于生成对称加密算法的"对话密钥"。

		3. 确认使用的各种加密方法，比如确认算法使用：RSA非对称加密算法，DES对称加密算法，SHA-1 hash算法

		4. 服务器证书。

	除了上面这些信息，如果服务器需要确认客户端的身份，就会再包含一项请求，要求客户端提供"客户端证书"。比如，金融机构往往只允许认证客户连入自己的网络，就会向正式客户提供USB密钥，里面就包含了一张客户端证书。

3. 客户端回应

	客户端收到服务器回应以后，首先验证服务器证书。如果证书不是可信机构颁布、或者证书中的域名与实际域名不一致、或者证书已经过期，就会向访问者显示一个警告，由其选择是否还要继续通信。

	如果证书没有问题，客户端就会从证书中取出服务器的非对称加密算法的公钥。然后，向服务器发送下面三项信息。

		1. 一个随机数。该随机数用服务器发来的公钥进行的使用非对称加密算法加密，防止被窃听。

		2. 编码改变通知，表示随后的信息都将用双方商定的加密方法和密钥发送(比如确认使用：RSA非对称，DES对称，SHA-1 hash算法)。

		3. 客户端握手结束通知，表示客户端的握手阶段已经结束。这一项同时也是前面发送的所有内容的hash值，用来供服务器校验。

	上面第一项的随机数，是整个握手阶段出现的第三个随机数，又称"pre-master key"。有了它以后，客户端和服务器就同时有了三个随机数，接着双方就用事先商定的对称加密算法，各自生成本次会话所用的同一把"会话密钥"。也就是说浏览器和服务器各自使用同一个对称加密算法，对三个相同的随机数进行加密，获得了，用来加密网页内容的 对称加密算法的秘钥。(注意：这里浏览器的三个随机数都是明文的，但是服务端获得的"pre-master key"是密文的，所以服务器需要使用非对称加密算法的私钥，来先解密获得"pre-master key"的明文，在来生成对称加密算法的秘钥。这样的目的是为了防止："pre-master key"被窃听，因为发送明文会被窃听，但是发生的是非对称加密算法的加密过后的密文，因为窃听者不知道私钥，所以即使窃听了，也无法解密出其对应的明文。从而保证了最后生成的：用于加密网页内容的对称加密算法的秘钥的安全性！！！)

	至于为什么一定要用三个随机数，来生成"会话密钥"，dog250解释得很好：

    > "不管是客户端还是服务器，都需要随机数，这样生成的密钥才不会每次都一样。由于SSL协议中证书是静态的，因此十分有必要引入一种随机因素来保证协商出来的密钥的随机性。

    > 对于RSA密钥交换算法来说，pre-master-key本身就是一个随机数，再加上hello消息中的随机，三个随机数通过一个密钥导出器最终导出一个对称密钥。

    > pre master的存在在于SSL协议不信任每个主机都能产生完全随机的随机数，如果随机数不随机，那么pre master secret就有可能被猜出来，那么仅适用pre master secret作为密钥就不合适了，因此必须引入新的随机因素，那么客户端和服务器加上pre master secret三个随机数一同生成的密钥就不容易被猜出了，一个伪随机可能完全不随机，可是是三个伪随机就十分接近随机了，每增加一个自由度，随机性增加的 可不是一。"

	这里：其实如果 pre-master-key 和 浏览器生成的随机数都可能被猜出来，那么最后生成的对称加密算法的秘钥就是不安全的。因为三个随机数都可能被窃听到了。

	此外，如果前一步，服务器要求客户端证书，客户端会在这一步发送证书及相关信息。

4. 服务器的最后回应

	服务器收到客户端的第三个随机数pre-master key之后，计算生成本次会话所用的对称加密算法的"会话密钥"。然后，向客户端最后发送下面信息。

		1.编码改变通知，表示随后的信息都将用双方商定的对称加密算法和密钥进行加密。

		2.服务器握手结束通知，表示服务器的握手阶段已经结束。这一项同时也是前面发送的所有内容的hash值，用来供客户端校验。

	至此，整个握手阶段全部结束。接下来，客户端与服务器进入加密通信，就完全是使用普通的HTTP协议，只不过用"会话密钥"加密内容。

![握手阶段的详细过程](./images/ssl_progress2.png)