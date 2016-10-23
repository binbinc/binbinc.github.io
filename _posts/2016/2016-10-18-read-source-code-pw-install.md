---
layout: post
title:  PaperWork源码阅读＃2：mac安装篇
date:   2016-10-18
categories:
- coding
tags:
- 源码阅读
---


本文从零基础开始，学习**如何在MAC上安装PaperWork**。

##Step-1：确认是否安装了php

确认是否安装了php，在Terminal键入“php -v”，如下所示：

```php
$ php -v
PHP 5.5.30 (cli) (built: Oct 23 2015 17:21:45) 
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2015 Zend Technologies
```
如果还没安装 PHP，可以安装MAMP。

##Step-2：查看是否有安装sqlite

查看是否有安装sqlite，在Terminal键入“sqlite3 --version”，如下所示：

```php
$ sqlite3 --version
3.8.10.2 2015-05-20 18:17:19 2ef4f3a5b1d1d0c4338f8243d40a2452cc1f7fe4
```
很多类 Unix 系统都会自带 SQLite3。


##Step-3：安装Composer

安装Composer，在Terminal键入如下命令：

```php
php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

执行全局安装，将前面下载的 composer.phar 文件移动到 /usr/local/bin/ 目录下面：

```php
sudo mv composer.phar /usr/local/bin/composer
```

关于如何安装Composer，可以参考[这里][1]。


##Step-4：安装NPM

参考官网[Initial-installation][2]，执行下面的命令，

```php
//run composer
cd frontend
composer install

//install gulp cli and bower package manager globally, run the default task
sudo npm install -g gulp bower
npm install
bower install
gulp
```

##Step-5(optional)：安装Laravel

安装Laravel，通过 Laravel 安装器：

```php
composer global require "laravel/installer"
```

确保 ~/.composer/vendor/bin 在[系统路径][3]中，否则不能在任意路径调用 laravel 命令。
安装完成后，可以通过简单的 laravel new 命令即可在当前目录下创建一个新的 Laravel 应用:
```php
laravel new blog
```


##Step-6：安装PaperWork

安装PaperWork，移植数据库：

```php
//create a database named "paperwork"
DROP DATABASE IF EXISTS paperwork; CREATE DATABASE IF NOT EXISTS paperwork DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON paperwork.* TO 'paperwork'@'localhost' IDENTIFIED BY 'paperwork' WITH GRANT OPTION; FLUSH PRIVILEGES;

//run the migration jobs
php artisan migrate
```

出现一个错误，
```php
changbinMac:frontend changbin$ php artisan migrate
Mcrypt PHP extension required.
```

参考[这里][4]，发现是mamp的php路径没在系统路径中设置。
修改好后，又出现新的错误，

```php
changbinMac:frontend changbin$ php artisan migrate
**************************************
*     Application In Production!     *
**************************************

Do you really wish to run this command? y

  [PDOException]                                    
  SQLSTATE[HY000] [2002] No such file or directory  
                                                    
migrate [--bench[="..."]] [--database[="..."]] [--force] [--path[="..."]] [--package[="..."]] [--pretend] [--seed]

```

继续翻墙，发现已经有[答案][5]，需要在app/config/database.php中加上unix_socket这一行。

```php
'mysql' => array(
        'driver'    => 'mysql',
        'host'      => 'localhost',
        'unix_socket'   => '/Applications/MAMP/tmp/mysql/mysql.sock',
        'database'  => 'database',
        'username'  => 'root',
        'password'  => 'root',
        'charset'   => 'utf8',
        'collation' => 'utf8_unicode_ci',
        'prefix'    => '',
    ),
```

终于这一次迁移表成功了，

```php
changbinMac:frontend changbin$ php artisan migrate
**************************************
*     Application In Production!     *
**************************************

Do you really wish to run this command? y
Migration table created successfully.
Migrated: 2014_07_22_194050_initialize
Migrated: 2014_07_24_103915_create_password_reminders_table
Migrated: 2014_10_08_203732_add_visibility_to_tags_table
Migrated: 2015_01_21_034728_add_admin_to_users
Migrated: 2015_05_05_094021_modify_tag_user_relation
Migrated: 2015_05_22_220540_add_version_user_relation
Migrated: 2015_06_15_224221_add_tag_parent
Migrated: 2015_06_30_125536_add_sessions_table
Migrated: 2015_07_29_130508_alter_versions

```

安装成功。


注册用户后进入PaperWork工作界面，不能创建笔记，没有“File”和“Edit”等菜单。
按照Initial-installation的说明仔细检查，发现第一次安装没有安装bower，重新安装就好了。



  [1]: http://pkg.phpcomposer.com
  [2]: https://github.com/twostairs/paperwork/wiki/Initial-installation
  [3]: http://www.cyberciti.biz/faq/appleosx-bash-unix-change-set-path-environment-variable/
  [4]: http://stackoverflow.com/questions/16830405/laravel-requires-the-mcrypt-php-extension
  [5]: http://stackoverflow.com/questions/19475762/setting-up-laravel-on-a-mac-php-artisan-migrate-error-no-such-file-or-directory


