### 一、基于windows平台的MySQL项目场景

01、小型购物网站
02、小型论坛
03、中小型门户网站
04、博客系统
05、IDC/云平台/VPS/虚拟主机



### 二、mysql与操作系统兼容性查询

https://www.mysql.com/support/supportedplatforms/database.html

| Operating  System                 | Architecture | 8.0  | 5.7  |
| :-------------------------------- | ------------ | ---- | ---- |
| Microsoft  Windows 2019 Server    | x86_64       | √    | ` `  |
| Microsoft  Windows 2016 Server    | x86_64       | √    | √    |
| Microsoft  Windows 2012 Server R2 | x86_64       | √    | √    |



### 二、系统安装

1、选择企业版完全安装
2、设置密码是p@ssw0rd
3、可选安装vmware tools，方面放大看



### 三、系统设置

1.设置IP地址
2.设置主机名
3.更改hosts文件，去掉127.0.0.1和::1的#，增加ip host，可用cmd测试一下
4.关闭防火墙
5.打开远程桌面
6.磁盘初始化
7.磁盘或者文件夹开启共享
8.安装vc++2013（X32和X64），2015（X64）再安装.net4.5.2，高版本的mysql还需要安装vc2019
#2012的操作系统需要安装.net4.5.2，vc++2013 X64即可
9.完成之后重启系统

MySQL 8.0 Server requires the Microsoft Visual C++ 2019 Redistributable Package to run on Windows platforms. 

MySQL debug binaries require Visual Studio 2019 to be installed.
