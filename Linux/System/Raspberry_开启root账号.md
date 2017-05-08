# 开启root账号


树莓派使用的Linux是debian系统，所以树莓派启用root和debian是相同的。

debian里**root账户默认没有密码，但账户锁定**。

**当需要root权限时，由默认账户经由sudo执行**，Raspberry pi 系统中的Raspbian **默认用户是pi 密码为raspberry**.

### 1. 重新开启root账号，可由pi用户登录后，在命令行下执行

```bash
sudo passwd root  
```

### 2. 执行此命令后系统会提示输入两遍的root密码，输入你想设的密码即可，然后在执行

```bash
sudo passwd --unlock root  
su root #再输入设置好的密码即可
```

这样就可以解锁root账户了。
