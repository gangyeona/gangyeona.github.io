---
layout:     post
title:      ubuntu系统vscode下的php调试xdebug的环境配置
subtitle:   php相关
date:       2019-11-01
author:     gangyeona
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - PHP
---

`phpstorm`找激活码太心累，开始换用`vs code`

所幸配置xdebug更加方便

1. 搜索并安装vs的php-xdebug插件，安装成功后重启vs.
2. 查看phpinfo是否安装xdebug
	```
    <?php
        echo phpinfo();
    ```
    输出的信息中搜索xdebug.
    如果没有則需要用命令安装

    ```
    apt-get install php-xdebug
    service php7.0-fpm reload //重启php-fpm服务，版本号修改为自己环境的php版本
    ```

    重启后再次确认下`phpinfo`中是否有xdebug扩展
3. 修改php.ini配置文件
    ```
    [XDebug]
    xdebug.remote_enable = 1
    xdebug.remote_autostart = 1
    ```
4. 打开vs，进入调试页面。选择添加调试，可以看到默认生成了两个配置文件，修改下配置文件里的端口即可。






