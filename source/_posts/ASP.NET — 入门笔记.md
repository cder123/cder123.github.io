---
title: ASP.NET —— 入门笔记
tag: C#
categories:
  - [后端,C#]
  - [后端,ASP.NET]


---

# ASP.NET-入门笔记

[toc]

## 1. 环境
### 1.1 工具
安装顺序：【IIS必须安装在VS前，不然需要在VS的安装路径下找到命令行，注册到 iis】
- SQL Server2012 
-  IIS
-  VS2012 (.NETv2.0) 


### 1.2 .NET注册IIS【当安装顺序有误时】
微软官网:`https://www.microsoft.com/en-us/download/`
[微软官网](https://www.microsoft.com/en-us/download/)

- asp.net4.0注册：

>打开程序-运行-cmd:输入一下命令重新注册IIS
`C:\WINDOWS\Microsoft.NET\Framework64\v4.0.30319\aspnet_regiis.exe -i`

- asp.net2.0注册：

> `C:\WINDOWS\Microsoft.NET\Framework64\v2.0.50727\aspnet_regiis.exe -i`

### 1.3 IIS的安装及配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208105135509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120810525914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208105342212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208105514125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208105612468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
一路 “下一步”
### 1.4 配置IIS
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208111241900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
自己建一个应用程序池
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208111513633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208111822740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208111920380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208112047663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208112233269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
默认文档的名字=》设为项目中要作为 首页的页面的文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208112317624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.5 使用VS 打开站点，进行编辑代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208112846738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208112923434.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208113058844.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208113141199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208113313339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 2. 添加母版页
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208113534864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208113623603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120811391559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 3. 添加主题、样式、Skin
一个站点可以有多个主题【但只能应用1个，主题相当于文件夹】，每个主题由 多个css文件和 skin文件构成
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208114303547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208114407482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
css文件用于调整 html 标签的样式
【一般使用**类名**调整**样式**，**ID**供 **后端代码**使用，**skinID**调整**控件样式**】
【控件要使用css样式，必须使用 CssClass="样式名"】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208114433403.png)
## 3. 应用主题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208115644546.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
删除原来的页面，添加使用 “主题” 的页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208115925372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208115947923.png)
## 3. 使用母版页创建页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120812022881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208120327266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208120423763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 4. 添加站点地图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120812055265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
叶子节点使用自闭合标签，非叶子节点使用双标签
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208121116863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 5. 网站Logo
打开 PS,设计Logo,保存为png格式，并放入网站的主题文件夹内  
-   宽：20cm  ;  
-  高：2cm  ;  
-  分辨率：72像素/英寸；  
-  颜色模式：RGB 8位；  
					(高度一般为页面的10%，宽度为高度的4~5倍)  
				拖入image 控件，设置图片的路径，显示图片

```csharp
<asp:Image ID="Image1" ImageURL="~/App_Themes/主题1/logo.png" runat="server" />	  
```
## 6. 数据库设置
 1. 准备好1个数据库 
 2. 【管理员方式打开数据库DBMS】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208122238288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208122309196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208122332430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208122443767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208122839385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208122920323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208123146273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120812321557.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
设置权限
![在这里插入图片描述](https://img-blog.csdnimg.cn/202012081233217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208123542685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 7. 网站连接数据库
- 随便在1个页面中 拖入 “数据源控件”
- 以可视化的方式连接数据库【目的是：在web.cofig中得到数据库的连接字符串】
- 删除数据源控件
>	-  左下角切换到设计视图  
>	-  控件右上角的箭头  
>	-  配置数据源  
>	- 展开+号，可以看到连接数据库的字符串，然后单击新建连接  
>	- sqlServer   
>	- 服务器名，输入你的数据库所在的实例名  
>	-  登录数据库验证方式，有你的数据库创建时的设定来决定  
>	-  下方，选择你的数据库，测试一下连接  
>	-  多了一个链接字符串  
>	- 打上√，这样连接字符串就自动写进了你的web.config中  
 >	- 【下一步，就建好了连接，并同时进入数据查询界面，
					为你的数据源控件准备一段查询语句】  


web.config就多了一段sql的相关代码 【Data Source=数据库所在电脑的主机名】 .

```csharp
 <connectionStrings>
    		 <add 	name="CompanySalesConnectionString" 
    				connectionString="Data Source=WIN-JM8KUC1NODF;
    				Initial Catalog=CompanySales;
    				Integrated Security=True"
        			providerName="System.Data.SqlClient" />
 </connectionStrings>
```


## 8. 数据库操作类
数据库访问类的编写步骤：

+   在任一页面放置 数据源控件 ，以可视化的方式创建  **连接字符串**
+   定义1个私有的静态 SqlConnection  属性，并 编写该属性的getter 访问器
+   编写 静态 获取数据集的方法【及其重载方法】
+   编写 静态 执行SQL语句的 方法【及其重载方法】
+   编写 静态 关闭 sqlConnection 的 方法

```csharp
using System;
using System.Collections.Generic;
using System.Web;


using System.Data;  //针对所有类型的数据库
using System.Data.SqlClient;  //针对SQL
using System.Configuration;  //针对web.config


/// <summary>
/// DBClass 的摘要说明
/// </summary>
public class DBClass
{
    //数据库连接
    private static SqlConnection connection;

    public static SqlConnection Connection
    {
        get
        {
            String connectionString = ConfigurationManager.ConnectionStrings["SchoolConnectionString"].ConnectionString;

            if (connection == null || connection.State == ConnectionState.Closed)
            {
                connection = new SqlConnection(connectionString);
                connection.Open();

            }
            return connection;
        }
    }


    //形成记录集

    public static DataTable GetDataSet(String sql)
    {
        SqlCommand command = new SqlCommand(sql, Connection); //静态类的方便之处，省去了new
        DataSet ds = new DataSet();//用DataSet()实例化(new)出1个ds,变量ds就有了内存
        SqlDataAdapter da = new SqlDataAdapter(command);
        /*
         * da负责 ：
                       1. 按照command指定的数据库连接Connection ；
                       2. 按照 sql给出的 SQL的脚本要求，
         *                  从数据库中 查询并且复制 1份数据Fill(注入到ds中)，
                            并自动解除对数据库的独占，等待程序结束。
                             比如：
                                    “确定”按钮的click 事件触发，da把所有更改 写回数据库
         */


        da.Fill(ds); //da把SQL中复制的数据注入到数据库
        return ds.Tables[0];//ds 类似于 select 后得到的表，所以有Tables属性，返回 ds.Tables[0]即第一个表
    }


    //方法重载 
    //c#【重载】的好处，避免了名目繁多

    //第一个方法：常用的就是看看数据，select然后GridView到web窗体上
    //第二个方法：增加了一个数组参数，比如你要删除数据，就拿这个数组来指示你删除哪个。
    //至于记录集的形成方式，两个一样，发挥关键作用的是那个SqlDataAdapter 类型的 da 对象
    /*
     * 真同名，连大小写都一样，
     *一种东西，两个含义:
     *     前一个，只有一个string类型的参数 sql，
     *     后一个有两个参数，除了string类型的参数 sql，
     *     还有一个SqlParameter[] 类型的参数 sqlparameter
           增加了这个数组类型的参数，我们就可以一个一个值读，
     *     如果遍历下去，就不是完成一件事，而是完成了同一操作的若干件事。
     *     
   */
    public static DataTable GetDataSet(String sql, SqlParameter[] sqlparameter)
    {
        SqlCommand command = new SqlCommand(sql, Connection);
        foreach (SqlParameter parameter in sqlparameter)
        {
            command.Parameters.Add(parameter); //sqlparameter是一个数组，用来替换command的某一个位置的参数
        }
        DataSet ds = new DataSet();
        SqlDataAdapter da = new SqlDataAdapter(command);

        da.Fill(ds);
        return ds.Tables[0];
    }

    public static int ExecuteSql(String sql)
    {
        SqlCommand command = new SqlCommand(sql, Connection);
        return command.ExecuteNonQuery();
    }

    public static int ExecuteSql(String sql, SqlParameter[] sqlparameter)
    {
        SqlCommand command = new SqlCommand(sql, Connection);
        foreach (SqlParameter parameter in sqlparameter)
        {
            command.Parameters.Add(parameter);
        }
        return command.ExecuteNonQuery();

    }

    public static int InsertSql(string sql)
    {

        SqlCommand sc = new SqlCommand(sql,Connection);
        
        return sc.ExecuteNonQuery();
        

    }
    public static int DeleteSql(string sql)
    {
        SqlCommand sc = new SqlCommand(sql, Connection);

        return sc.ExecuteNonQuery();
    }

    public static int UpdateSql(string sql)
    {
        SqlCommand sc = new SqlCommand(sql, Connection);

        return sc.ExecuteNonQuery();
    }
    public static bool CloseConn() {
        connection.Close();
        return true;
    }

}
									
```
## 10. ADO【数据库操作的驱动】
asp.net 数据对象【类似 jdbc】
- JDBC：导入对应的jar包；使用 maven
- C#：引入 DLL；使用 nuget

### 10.1 ADO 与 JDBC 的对比
[ADO 与 JDBC 的对比—链接](https://blog.csdn.net/qingming7573/article/details/53143180)

### 10.2 ADO 的几种对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208132618212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

