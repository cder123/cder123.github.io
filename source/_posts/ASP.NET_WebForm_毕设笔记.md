---
title: 2020-ASP.NET_WebForm_毕设笔记
tag: C#
categories:
  - [后端,C#]
  - [后端,ASP.NET]
---





# 2020-ASP.NET_WebForm_毕设笔记

[toc]



## 1. 整体概述





### 1.1 该电子商城的**页面**

+   首页
+   分类页【包含：一级分类/二级分类】
+   搜索页
+   商品详情页
+   注册页
+   登录页
+   忘记密码【即：修改密码页】
+   用户中心
    +   基本信息页
    +   地址管理
    +   收款码管理
    +   用户管理
    +   商品管理
+   订单中心
+   订单详情
+   订单录入
+   购物车







### 1.2 功能模块图



![1](https://z3.ax1x.com/2021/06/02/2QIm9S.png)





### 1.3 样式

+   该项目主要使用 【Bootstrap4 +外联css + 内联css + Jquery 样式】
+   由于需要使用 Bootstrap4 ，因此 在页面需要先导入Jquery ，再导入Bootstrap4 【因为Bootstrap4 有部分功能是基于Jquery开发的】  (本项目是在母版页导入)：
```js
<script src="/App_Themes/主题1/jquery.min.js"></script>
<script src="/App_Themes/主题1/bootstrap/js/bootstrap.bundle.min.js">		</script>    
<script src="/App_Themes/主题1/bootstrap/js/bootstrap.min.js"></script>
```

+   在页面标签显示小图标：【分别设置  aspx   和  webconfig 】

```js
<link rel="icon" href="/App_Themes/主题1/img/GM.png" type="image/x-icon" />
<link rel="shortcut icon" href="./App_Themes/主题1/img/GM.png" type="image/x-icon" />
```

```xml
//在system.webServer标签内部输入，且在IIS中注册.woff2格式
<staticContent>
   <mimeMap fileExtension=".woff2" mimeType="application/x-font-woff" />
</staticContent>
```



+   关联主题

    +   在web.config文件中的`<configuration> <configuration>`内部输入：

    ```xml
      <system.web>
        	<pages theme="主题1" />
        	<compilation debug="true" />
        	<sessionState mode="StateServer" cookieless="false" 					timeout="480"/>  
          	
      </system.web>
    ```

    +   在以上代码中 **sessionState** 标签是为了设置网站中的**session有效时间**，
        +    **sessionState**的几个属性的参数：
            +   mode**属性**：
                +   InProc：session保存在进程(IIS)内部，性能好，但容易丢失
                +   StateServer：保存在进程外，需要启动`asp.net State Service的服务`，session在有效期内除非重启电脑或关闭服务，否则一直保存直到失效时间自动失效
                +   SQLServer: 保存在数据库中，在标签中设置数据库的ip地址`stateConnectionString='tcpip=127.0.0.1:42424'`，部分数据类型需要序列化
            +   cookieless**属性**：
                +   true:在浏览器禁用cookie，使用url保存cookie,使用这种方法保存cookie时如果要跳转页面，不能直接覆盖地址栏的值，否则session会失效
                    +   示例：`http://localhost/ABC/(ulqsek45heu)/default.aspx`【小括号内的就是SessionID】
                +   false:【默认】，在浏览器启用cookie
            +   timeout**属性**：
                +   属性值为 **分钟数**，**默认 20 min**

    





### 1.4 收款码和商品图片

收款码和商品图片  保存  在项目文件夹下，数据库中 保存 图片路径





### 1.5 数据库访问类

数据库访问类的编写步骤：

+   在任一页面放置 数据源控件 ，以可视化的方式创建  **连接字符串**
+   定义1个私有的静态 SqlConnection  属性，并 编写该属性的getter 访问器
+   编写 获取数据集的方法
+   编写 执行SQL语句的 方法







#### 1.5.1 创建SQL连接

```c#
private static SqlConnection connection;    
    //连接的getter 访问器
public static SqlConnection Connection {
        get {
            string connectionString = ConfigurationManager.ConnectionStrings["OnlineMallConnectionString"].ConnectionString;
            if (connection == null || connection.State == ConnectionState.Closed) {
                connection = new SqlConnection(connectionString);
                connection.Open();
            }
            return connection;
        }
    }
```





#### 1.5.2 获取数据集的方法

```c#
    //获取数据集的方法
    public static DataTable GetDataSet(string sql) {
        SqlCommand cmd = new SqlCommand(sql, Connection);
        DataSet ds = new DataSet();
        SqlDataAdapter da = new SqlDataAdapter(cmd);
        da.Fill(ds);
        return ds.Tables[0];

    }
//------------------------------------------------------------
    //获取数据集的方法的重载
    public static DataTable GetDataSet(string sql, SqlParameter[] sqlparameters) {
        SqlCommand cmd = new SqlCommand(sql, Connection);
        foreach (SqlParameter arg in sqlparameters) {
            cmd.Parameters.Add(arg);
        }
        DataSet ds = new DataSet();
        SqlDataAdapter da = new SqlDataAdapter(cmd);
        da.Fill(ds);
        return ds.Tables[0];
    }
```







#### 1.5.3 执行SQL语句的方法

```c#
   //执行sql语句的方法
    public static int ExecuteSql(string sql)
    {
        SqlCommand command = new SqlCommand(sql, Connection);
        return command.ExecuteNonQuery();
    }


    //执行SQL语句 所需的方法的重载
    public static int ExecuteSql(String sql, SqlParameter[] sqlparameters)
    {
        SqlCommand command = new SqlCommand(sql, Connection);
        foreach (SqlParameter arg in sqlparameters)
        {
            command.Parameters.Add(arg);
        }
        return command.ExecuteNonQuery();

    }
```





#### 1.5.4 关闭连接

```c#
    public static bool CloseConn() {
        connection.Close();
        return true;
    }
```







### 1.6 数据库访问类的使用-示例

```c#
//在后端文件中 引入命名空间
using System.Data;
using System.Data.SqlClient;
using System.Collections;

//--------------------获取数据集---------------------------------
//第一种【获取数据集】
	//创建sql语句字符串【在方法/事件内】
	int Sid=10;
	string sql_1 = "select * from stuTable where SID="+Sid;

	//获取数据集【表】    
	DataTable StuDt =  DBClass.GetDataSet(sql_1);   

	//关闭连接
	DBClass.CloseConn();

	// 声明 ArrayList 保存数据
	ArrayList StuArr = new ArrayList();

	//遍历数据集以获取所需数据
    for(int i=0;i<StuDt.Rows.Count;i++)
    {
        //StuArr保存 第 i 行 的第1个数据
        //如果知道要取的值的列名，也可以根据列名取值
        StuArr.Add(StuDt.Rows[i][0]);         
       
    }

//第二种【获取数据集】
	int Sid = 10;
	string name = "小明";
	string sql = "select * from stuTb where SID=@Sid and Sname=@name";
	SqlParameter[] para = new SqlParameter[]
    {
        new SqlParameter("@Sid",Sid),
        new SqlParameter("@name",name)
    }
	DBClass.GetDataSet(sql,para)；

//-----------------------执行SQL----------------------------
//第一种【执行SQL】
    //执行sql语句(增/删/改) 
	string sql_2 = "insert into StuTable values(10,'小米','计算机系')";
	int impact_Rows_Num = DBClass.ExecuteSql(sql_2);

//第二种【执行SQL,类似获取数据集的第二种】


//-----------------------判断单元格是否为空------------------------
//判断单元格是否为空
	 if (dt1.Rows[0][0] is DBNull)
     {
         //代码。。。
     }


```







### 1.7 母版页

母板页只放置公共部分，母版页中会有1个占位符。所有根据母版页生成的页面，其与母版页不同的代码，实际上都是写在占位符中的。并且，占位符可以根据需要，自己拖出占位符控件

前端：

+   头部   ：个人中心，登录，注册，购物车等
+   底部   ：站点 相关信息

后端：

+   Page_Load事件 中 判断session中是否有用户名信息【即：是否登录】

    +   若 没登录【默认】：显示 - 【首页，登录，注册，购物车等几个按钮】
    +   若登录：清除头部nav 中的【登录，注册】按钮，替换上【个人中心按钮】

    ```c#
     this.top2.Controls.Remove(LoginPagebtn);
    //个人中心
    LinkButton myCenterPagebtn = new LinkButton();
    
    myCenterPagebtn.ID = "myCenterPagebtn";
    
    myCenterPagebtn.CssClass = "linkbtn";
    
    myCenterPagebtn.Text = Session["UserName"].ToString() + " 的个人中心";
    
    myCenterPagebtn.Click += new EventHandler(userbtn_Click);
    
    this.top2.Controls.Add(myCenterPagebtn);
    ```

**退出登录**按钮的事件：

```c#
//清除浏览器缓存    
	public void ClearClientPageCache()
    {
        
        Response.Buffer = true;
        Response.ExpiresAbsolute = DateTime.Now.AddDays(-1);
        Response.Cache.SetExpires(DateTime.Now.AddDays(-1));
        Response.Expires = 0;
        Response.CacheControl = "no-cache";
        Response.Cache.SetNoStore();

    }    
//退出登录
    protected void LogOutbtn_Click(object sender, EventArgs e)
    {
        Session["UserName"] = null;
        Session["UserPwd"] = null;
        ClearClientPageCache();
        Response.Redirect("~/Default.aspx");
       
    }

```







### 1.8 首页

+   轮播图左侧——分类导航

    +   设置好布局，子分类默认【display:none;】
    +   鼠标触发jquery 的**mouseenter**事件时， display:block;
    +   鼠标触发jquery 的**mouseleave**事件时， display:none;
    +   左侧主分类**前端**使用 ul+li , li 的 id包含索引，在jQuery事件中利用索引*偏移量实现 【id =“cate-0”】 。**后端**利用 `div.innerHtml = “ html字符串 ”; `渲染

    ```js
    $(".left-nav>li").mouseenter(function (e) {
                    var id = parseInt(e.target.id.split("-")[1]);
                    $(".subCategory").scrollTop(id * 100);
    });
    ```

+   轮播图使用 Bootstrap4 的轮播图组件

+   商品卡片：

    +   用 Hashtable  存放 单个商品的相关信息
    +   用 ArrayList  存放 页面中的所有商品【Hashtable  】

    

### 1.9 搜索页

搜索框，数据库中返回数据，排序。











## 2. 常用控件的使用技巧

**注意：**

+   所有 **asp.net控件** 最终都会**编译**成  **html标签** 呈现在浏览器

    +   Label  ->  span
    +   TextBox  -> input: textbox
    +   Button   -> input: submit

+   如果**控件是动态生成**的，需要在**事件中访问当前操作的控件**时，可以在new控件时将控件ID按自己的需求更改，将要传的参数通过分割符以字符串的形式与ID拼接，要在事件中使用时，先强制转化为控件，再进行字符串的分割等操作

    示例：

```c#
//生成控件
protected void Page_Load(object sender, EventArgs e)
{
    Button btn_1 = new BUtton();
    btn_1.ID = "btn-1";    
    btn_1.Text = "点我";
    btn_1.Click += new EventHandler(btn_1_Click);
}

protected void btn_1_Click(object sender, EventArgs e)
{
    //例如：该按钮的id格式为：btn-商品编号
    //字符串的Split('分隔符');方法最后生成的是一个数组
    Button  btn1 = (Button)sender;
    string pid = btn1.ID.Split('-')[1];
}
```

+   控件的操作

```c#
//假设前端已经有了控件 Pannerl_1
//后端

	Button btn_1  = new Button();

//添加控件
	Pannerl_1。Controls.Add(控件名);
//删除控件
	Pannerl_1。Controls.Remove(控件名);
//清空控件
	Pannerl_1。Controls.Clear();

```

+   表单控件的相关信息会传到  **Request** 的 **formdata** 中







### 2.1 事件：

+   控件的事件名规则一般为`控件ID_事件`

```c#
//前端
    <asp:Button ID="btn_1" Text="按钮1" OnClick="btn_1_OnClick">
    </asp:Button>

//后端
    protected void btn_1_OnClick(object sender, EventArgs e)  
    {
        //相应的代码。。。
    }
```

+   动态生成控件-绑定事件

```c#
//生成控件
protected void Page_Load(object sender, EventArgs e)
{
    Button btn_1 = new BUtton();    
    btn_1.Text = "点我";
    //绑定事件
    btn_1.Click += new EventHandler(btn_1_Click);
}
//事件
protected void btn_1_Click(object sender, EventArgs e)
{
   //事件代码。。。
}
```







### 2.2 只需加载一次的代码

```c#
protected void Page_Load(object sender, EventArgs e)
{
    if(!isPostBack){
        //第一次加载页面时，执行的代码
    } 
    //每次页面加载都执行的代码
    
}
```









### 2.3 Session的使用

```c#
protected void Page_Load(object sender, EventArgs e)
{
    //存session
   		Session["username"] = this.txtbox_1.Text.Trim();
    
    //取session【由于session是1个对象，因此使用时一般需要强制转化】
    	string username = Session["username"].toString();
    
    //判断Session是否为空
    	if(!(Session["username"] == null))
        {
            //代码            
        }
}
```











### 2.4 html标签 与 asp.net 控件

+   后端控件  中加上1句 `CssClass="css类名"`来设置相应的 css样式

+   html标签 中加上1句 `runat="server"` 就能在后端用 ID访问到

如：

```
//前端
	<div id="a1" runat="server">你好！<div>
	
	<asp:TextBox ID="txtbox_a" CssClass="txtbox_1" runat="server" >	
    </asp:TextBox>
	
	
//后端
	a1.innerHtml="hello world!";
	
	txtbox_a.Text = "你好";
```









### 2.5 地址栏传参

+   当使用地址栏传参数时，如果参数中包含中文，必须先编码，然后再在后端获取参数后解码

```c#
//传参页-后端
protected void imgbtn_Click(object sender, ImageClickEventArgs e)
{
    if (this.searchInput.Text != "") {
        //编码
        string KW1 =Server.UrlEncode(this.txtbox_1.Text.Trim());
        Response.Redirect("/Pages/Search/Search.aspx?kw="+KW1);
    }
}

//接受参数页-后端
protected void go_Click(object sender, EventArgs e)
{
    string tmp = Request["kw"].toString();
    //解码
    string KW2 = Server.UrlEncode(tmp);
    this.textbox_2.Text = KW2;
    
}
```









### 2.6 ImageButton控件

```c#
//前端 
	<asp:ImageButton CssClass="imgBtn" ID="imgbtn" runat="server" ImageUrl="~/App_Themes/主题1/img/img.png" OnClick="imgBtn_Click" />

//后端
    protected void imgBtn_Click(object sender, ImageClickEventArgs e)
    {
        search_Container.InnerHtml = "";
        Response.Redirect("./Search.aspx?kw=" + 		Server.UrlEncode(this.txtbox_1.Value.Trim()));
        ShowSearchItems(this.search_MaxBar_input.Value.Trim());
    }     
```









### 2.7 DropDownList控件

+   DropDownList控件必须设置属性`AutoPostBack="True"`以保证每次**更改选项**都能自动**传到后端**

+   DropDownList控件中的选项可以是固定的，也可以是从数据库中查询得到的

    +   **固定**的选项：

    ```c#
    //前端
    	<asp:DropDownList ID="dp_1" class="dp_1" runat="server" AutoPostBack="True"  
            OnSelectedIndexChanged="dp_1_SelectedIndexChanged">
                    <asp:ListItem Selected="True">按价格排序</asp:ListItem>
                    <asp:ListItem>降序</asp:ListItem>
                    <asp:ListItem>升序</asp:ListItem>
    	</asp:DropDownList>
            
    //后端
        protected void dp_1_SelectedIndexChanged(object sender, EventArgs e)
        {   
            //ID为dp_1的DropDownList控件在选项更改时触发的事件。。。
        
        }
    ```

    +   **不固定**【从数据库中获得】

```c#
// 利用数据库操作类中的获取数据集方法-->提取数据并存到数组或arraylist中 
// DropDownList的DataSource属性绑定数据集
// 执行数据绑定方法

 protected void Page_Load(object sender, EventArgs e)
 {
     DataTable dt1 = DBClass.GetDataSet("select sid from stuTable");
     
     ArrayList arr1 = new ArrayList();
     
     for(int i=0;i<dt1.Rows.Count;i++)
     {
         arr1.Add(dt1.Rows[i][0]);         
     }
     
     this.dp_1.DataSource = arr1;
     this.dp_1.DataBind();     
     
 }


```









### 2.8 GridView控件

**基本使用步骤：**

1.  拖出控件
2.  禁用自动生成列
3.  手动添加 **DataField**,修改字段的headerText，并为每个DataField绑定数据库中的列
4.  绑定数据源，并使用 GridView1.DataBind();
5.  【必要时，可将**DataField**转化为**模板字符串**】
6.  【添加命令字段（更新，删除，取消）】
7.  【编写事件】
8.  保存



**编辑时的TextBox大小：**调整`<asp:BoundField></asp:BoundField>`的`ControlStyle-Width`属性



**获取当前操作的单元格的值：**

+   普通：`string str = e.Row.Cells[2].Text`
+   编辑时：

```c#
string str = ((TextBox)(productManageGridView.Rows[e.RowIndex].Cells[6].Controls[0])).Text.Trim();
```









#### 2.8.1   GridView 的 RowEditing事件

```c#
//编辑
	protected void GridView1_RowEditing(object sender, GridViewEditEventArgs e)
    {	
        GridView1.EditIndex = e.NewEditIndex;
        GridView1.DataBind();
    }
```







#### 2.8.2  GridView 的 RowCancelingEdit事件

```c#
//取消
protected void GridView1_RowCancelingEdit(object sender, GridViewCancelEditEventArgs e)
{
        GridView1.EditIndex = -1;
        GridView1.DataBind();
}
```







#### 2.8.3  GridView 的 RowDeleting事件

```c#
//删除
protected void GridView1_RowDeleting(object sender, GridViewDeleteEventArgs e)
{
    //执行删除
        string delSql = "  delete addTb where pid = 1 and name='abc'  " ;
        DBClass.ExecuteSql(delSql);
		
	//执行完后退出编辑模式
        GridView1.EditIndex = -1;

    //重新从数据库中获取数据集，并与GridView绑定
        GridView1.DataBind();        
}
```







#### 2.8.4  GridView 的 RowUpdating事件

```c#
protected void GridView1_RowUpdating(object sender, GridViewUpdateEventArgs e)
{
    TextBox  currTxtBox = (TextBox)GridView1.Rows[e.RowIndex].Cells[1].Controls[0];
    string addrStr = currTxtBox.Text;

    string upSql = "update addTb set address='"+addrStr+"' where sid=35" ;
    DBClass.ExecuteSql(upSql);
    GridView1.EditIndex = -1;
    
    //重新获取数据集，绑定数据源，绑定
    GridView1.DataBind();
    
}
```





#### 2.8.5  GridView 的 RowDataBound事件【表格前面显示  行号】

```c#
//前端：在gridview的columns标签内添加
		<asp:BoundField HeaderText="编号" DataFormatString="{0:Container.DataItemIndex}" />

//后端
    	protected void GridView1_RowDataBound(object sender, GridViewRowEventArgs e)
    	{
            if (e.Row.RowIndex >= 0)
            {
                e.Row.Cells[0].Text = Convert.ToString(e.Row.DataItemIndex + 1);
            }
    	}
```







#### 2.8.6  GridView 删除1行时弹出提示信息

```c#
//数据绑定
    protected void productManageGridView_RowDataBound(object sender, GridViewRowEventArgs e)
    {
        //如果是绑定数据行
        if (e.Row.RowType == DataControlRowType.DataRow)
        {
            if (e.Row.RowState == DataControlRowState.Normal || e.Row.RowState == DataControlRowState.Alternate)
            {
               //cell[17]就是删除按钮，cell[2]是商品名
                ((LinkButton)e.Row.Cells[17].Controls[0]).Attributes.Add("onclick", "javascript:return confirm('你确认要删除：\"" + e.Row.Cells[2].Text + "\"吗?')");
            }
        }
        //编号
        if (e.Row.RowIndex >= 0)
        {
            e.Row.Cells[0].Text = Convert.ToString(e.Row.DataItemIndex + 1);
        }
    }
```







#### 2.8.7  GridView 更改分页

```c#
//前端：
//GridView属性设置：
//	允许分页，分页大小
//	【即：AllowPaging="True" PageSize="5"】


//后端
		
	//更改分页
		protected void GridView_PageIndexChanging(object sender, GridViewPageEventArgs e)
    	{
            this.productManageGridView.PageIndex = e.NewPageIndex;
            this.productManageGridView.DataBind();
    	}

```













### 2.9 FileUpload 控件

+   FileUpload 控件一般结合Button使用

```c#
//前端
<asp:FileUpload ID="ImgFileUpload" runat="server" />
<asp:Button ID="okBtn" runat="server" Text="确定" OnClick="okBtn_Click" />  
    
//后端
    
//更新收款码
protected void updateRecvCodeImgButton_Click(object sender, EventArgs e)
{
    bool fileIsValid = false;
        if (this.updateRecvCodeImgFileUpload.HasFile)
        {
            String fileExtension = System.IO.Path.GetExtension(this.ImgFileUpload.FileName).ToLower();
            String restrictExtension = ".png";
            //判断拓展名
            if (fileExtension == restrictExtension)
            {
                fileIsValid = true;
            }
            //判断文件是否有效
            if (fileIsValid == true) 
            {
                try
                {
                    //保存图片
                    string imgUrl_Pre = "/PayCodeImg/";
                    string imgUrl = imgUrl_Pre + Session["UserName"].ToString() + ".png";
                    this.ImgFileUpload.SaveAs(Server.MapPath(imgUrl));
                    this.Label1.Text = "ok";

                    //更新数据库
                    string updateRecvCodeImg = "update uTb set recvimg='" + Session["UserName"].ToString() + ".png' where name = '" + Session["UserName"].ToString() + "'";
                    DBClass.ExecuteSql(updateRecvCodeImg);
                    DBClass.CloseConn();
                }
                catch {
                    this.Label1.Text = "文件上传不成功！";
                }
            }
        } 
}   
```

