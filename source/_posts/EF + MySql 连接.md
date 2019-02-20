# EF + MySql 连接

标签（空格分隔）： mysql ef 数据库操作

---
## MySql 数据库连接

### Question1：配置问题</h3>

某人的建议：**坑**
>当前配置
1. Nuget包
2. EF 6.0.0
3. Mysql.Data 6.9.12
4. Mysql.Data.Entity 6.9.12

>电脑配置
 安装mysql-connector-net-6.9.12
 *这个版本最好要低于，Vs2017安装的mysql.data.dll版本，这个文件的目录位于
 `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\PrivateAssemblies`*

警告： 谁说的，不要在乎这个问题，胡说八道，折磨死我了


> 成功配置如下
> 安装的MySql版本为5.7
> 安装mysql connect net 6.10.8
> Nuget添加包，有Mysql的data与data.Entity 6.10.8
版本号一致，即可处理


出现的各种问题：
“MySql.Data.MySqlClient.MySqlConnection.get_Settings()”失败。”
我目前出现的问题，配置失败占百分之八十
另外SSL问题
找到Web.config文件，修改connectionStrings，添加SslMode=none即可

PS：都是坑啊
