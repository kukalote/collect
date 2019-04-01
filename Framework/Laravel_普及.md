# Laravel 普及

本人使用 `centos7 系统`,`nginx1.12.1`,`php7.2`,`php-fpm`

#### 1. 安装软件(安装laravel安装器)
```bash
composer global require "laravel/installer"
```
#### 2. 生成项目
```bash
# 通过 composer 创建项目
composer create-project --prefer-dist laravel/laravel blog "5.5.*"
# 或者用 laravel 创建项目 
~/.config/composer/vendor/bin/laravel new blog
```
#### 3. 项目初始化一此操作
**1. 生成 env 文件**
```bash
cp .env.example .env
```
**2. 生成项目密钥**
保证 session 等安全的必要
```bash
php artisan key:generate
```
**3. 配置目录权限**
先在 bootstrap/app.php 文件中手动指定storage目录
```php
$app->useStoragePath(base_path('../storage'));
```
```bash
cd project_dir
mkdir -p ../storage/logs
mkdir -p ../storage/framework/views
mkdir -p ../storage/framework/cache
# 此处目录以php-fpm使用的用户组为准 
chown -R apache:apache storage
chown -R apache:apache bootstrap/cache
```
#### 4. 搭建框架
**生成Model**
```bash
php artisan make:model Model/User
```
**生成实体**
```bash
php artisan make:model Entity/User
```
**生成Controller**
```bash
php artisan make:controller Auth/User
```
**生成 Exception**
```bash
php artisan make:exception  ParamException
```
**生成 Exception**
```bash
php artisan make:middleware  CheckLogin
```

**异常类型**

异常类型 | 关键字
----|---
接口异常 | ApiException
代码调用异常 | CodeException
操作异常 | CustomException
逻辑处理异常 | LogicalException
权限异常 | PowerException
参数异常 | RequestException

#### 5. 安装基础扩展
```bash
$ composer require predis/predis
```

#### 其他
**生成配置文件缓存**
// 将配置文件合并到一个文件，保存到 bootstrap/cache/config.php
```bash
php artisan config:cache
```

```bash
// 缓存路由
php artisan route:cache  // 缓存
```
** Artisan 命令可以通过 --env=testing 项，来调用 .env.testing 文件**
**开启维护状态**
```bash
php artisan down
php artisan down --message="Upgrading Database" --retry=60
php artisan down --allow=127.0.0.1 --allow=192.168.0.0/16
```
** 开启网站**
```
php artisan up
```
**定时任务执行配置**
`* * * * * /usr/local/php/bin/php /data/wwwroot/test/artisan schedule:run 1>> /dev/null 2>&1`
** 执行某定时任务**
执行 NcCron 类中某个方法
`/usr/bin/php /data/wwwroot/test/artisan cron:nc  myCronJob`