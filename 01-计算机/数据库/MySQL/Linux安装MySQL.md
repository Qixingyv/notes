# Linux上安装MySQL

## 通过rpm安装

**MySQL版本：8.0.26**

**CentOS版本：7.6**

1. 使用XFTP上传MySQL安装包至Linux服务器

   - 在甲骨文官网需要寻找MySQL community server
   - Operating System选Red Hat Enterprise Linux/ Oracle Linux
   - OS Version 选用 Red Hat Enterprise Linux 7

2. 使用解压命令解压

3. 按照如下图片安装依赖，遇到错误百度

   ![MySQL安装顺序](C:\WorkSpace\笔记\数据库笔记\MySQL\assets\MySQL安装顺序.png)

4. 开启MySQL服务

   ```shell
   systemctl start mysqld	//开启MySQL服务
   systemctl restart mysqld //重启MySQL服务
   systemctl stop mysqld	//停止MySQL服务
   ```

5. 找到rpm自动生成的MySQL初始登录密码

   ```sh
   vim /var/log/mysqld.log
   ```

6. 登录后，修改MySQL登录密码，并更改设备限制

   ```mysql
   --修改root密码
   alter user 'root'@'localhost' identified by 'xxx';
   
   --修改root设备限制
   use mysql;
   update user set host='%' where user='root';
   flush privileges;	--刷新
   ```

   MySQL密码规则:https://dev.mysql.com/doc/	搜索validate_password

7. 解决日志下大量反解析问题

   因为mysql默认会反向解析DNS，对于访问者Mysql不会判断是hosts还是ip都会进行dns反向解析，频繁地查询数据库和权限检查，这大大增加了数据库的压力，导致数据库连接缓慢，严重的时候甚至死机，出现“连接数据库时出错”等字样。

   日志提示如下

   ```
   MySQL [Warning]: IP address 'xxxx' could not be resolved: Name or service not known
   ```

   解决：禁用DNS反查

   打开etc目录下的my.cnf（MySQL配置文件）配置文件，在[mysqld]添加如下参数

   ```
   skip_host_cache
   skip-name-resolve=1
   ```

   


## 使用宝塔面板安装

环境:CentOS 7.6

使用Xshell连接服务器

安装宝塔面板https://www.bt.cn/new/download.html

在服务器防火墙界面放行宝塔界面端口

进入对应的宝塔后台界面（本次为http://120.26.81.186:8888/），设置面板登录名和密码

安装MySQL软件（本教程以MySQL8.0为例）

在宝塔后台页面修改MySQL密码

在MySQL服务器上输入如下代码，赋予远程访问权限

```mysql
CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
#新建用户，密码自定义
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
#赋予权限
FLUSH PRIVILEGES;
#刷新权限
```

使用远程登录手段登录

