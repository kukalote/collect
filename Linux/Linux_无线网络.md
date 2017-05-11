# 配置无线网络

环境 : ubuntu 15.04
网卡名称 : wlan0

## 1. 查看无线网络

我们可以使用 iwlist 命令，可以显示周围所有的无线网络，

```bash
# 显示全部无线网卡
$ sudo iwconfig
# 如果网卡名称为wlan0 可用的wifi
$ sudo iwlist wlan0 scanning
```

如下图所示，ESSID即为无线网络名:

![image](./images/wlan0.png)

## 2. 生成访问密钥

把下面文件中的 ssid 和 passwd 换成无线网络的 ssid 和密码。
```bash
$ wpa_passphrase washionton25 “password”
# 输出如下 :
# network={
#	ssid="washionton25"
#	#psk="password"
#	psk=7d78c395cb32c2e249bfef026fd15af250cb514a5706b3c66e50ee09e16c510c
# }

```
这里至少我们要用到 psk 的值。

## 2. 配置无线网络

这里有两种配置方法,完成配置文件的填充：

```bash
$ sudo vim /etc/network/interfaces
# 1. 当前文件配置
auto wlan0
iface wlan0 inet dhcp
wpa-ssid "washionton25"
wpa-key-mgmt WPA-PSK
wpa-psk 54e8ebcbb60d3d60ba24f442a3829c3c8068e0ec1c989b018d22e677a7652fc5

# 2. 放到配置文件
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

## 方法2,接如下操作 :
$ sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
# 将如下内容加入文件
# network={
#	ssid="washionton25"
#	#psk="password"
#	psk=7d78c395cb32c2e249bfef026fd15af250cb514a5706b3c66e50ee09e16c510c
# }
```

<!--![image](./images/wpa_supplicant.png) -->

添加完之后，保存退出，wifi网络即可添加完成。
接下来是重启网络 或者 网卡:

```bash
# 重启网络
$ sudo systemctl restart networking.service

# 重启网卡
$ sudo ifup wlan0
```
