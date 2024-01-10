第一步，安装Xshell和openvpn客户端

第二步，访问https://www.racknerd.com/，购买VPS，VPS使用CentOS 7系统

第三步，在Xshell中访问VPS并执行如下命令

```
yum -y update	//升级系统
yum -y install https://as-repository.openvpn.net/as-repo-centos7.rpm
yum -y install openvpn-as
//安装openvpn服务端
```

第四步，记录openvpn服务端安装时提供的连接和端口号及用户名密码

```
//上次记录的端口号和用户名及密码
端口号：943
用户名：openvpn
密码：Asd3366554785.
```

第五步，访问VPN管理界面：在浏览器输入https://IP:943/admin

第六步，打开openvpn客户端，输入如上信息









