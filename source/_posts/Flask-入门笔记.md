---
title: Flask-入门笔记
tag: Python3
categories:
  - 后端
  - Python3
---

# Flask-入门笔记

[toc]

## 1. 案例1：创建1个flask应用程序

目录结构：

-   main.py

-   templates 目录

    -   index.html

    

**注意：** flask中的**模板**就是**html页面**

>   return  字符串

```python
from flask import Flask

app = Flask(__name__)


# 当浏览器的地址栏后为“ / ”时，调用相应处理的函数index()，
# 页面内容为return关键字后的内容
# 【这启示我们，只要将1个html页面放在return关键字后，就能实现页面的动态切换】

@app.route('/')
def index():
    return '你好，我是首页'

# 当浏览器的地址栏后为“ /pic ”时，调用相应处理的函数pic()
# 【即：页面显示return关键字后的内容】
@app.route('/pic')
def pic():
    return '你好，我是pic'



if __name__ == '__main__':
    app.run(debug=True)

```

>   return 页面

```python
from flask import Flask, render_template


app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/pic')
def pic():
    return 'pic'


if __name__ == '__main__':
    app.run(debug=True)

```

html页面：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    用户名：<input id="uname" type="text" > <br>
    密码：<input id="upsw" type="" ><br>
    <input id="btn" type="button" value="提交"><br>

</body>
</html>
```

>   后端 =传数据到=》前端

python:

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    arr = ['小a', '小b', '小c']
    
    # 页面放在templates目录下【该文件夹与当前python文件平级】
    return render_template('index.html',arr1=arr) # arr1传到html中


@app.route('/pic')
def pic():
    return '页面显示pic'


if __name__ == '__main__':
    app.run(debug=True) # 以调试模式,运行 网站app

```

html：

-   `{% python语句 %}` 
-   `{{ python的变量 }}`

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    
      {% for it in arr1 %}
          {{ it }} <br>
      {% endfor %}


</body>
</html>
```



>   前端表单 =》 后端

py:

```python
from flask import Flask, render_template, request

app = Flask(__name__)


@app.route('/')
def index():   

    return render_template('index.html')


@app.route('/login', methods=['POST']) # 注意：将方法改为post
def login():
    username = request.form.get('uname')  # 获取表单的 固定写法
    psw = request.form.get('upsw')
    if username == 'abc' and psw == '123':
        return '登录成功'
    else:
        #登录失败，则重新加载页面，并返回一句话，这句话在前端用{{ 变量名}} 来显示
        return render_template('index.html', msg='登录失败') 


@app.route('/pic')
def pic():
    return 'pic'


if __name__ == '__main__':
    app.run(debug=True)

```

html：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    
    <form action="/login" method="post">
        用户名：<input id="uname" name="uname" type="text"> <br><br>
        密码：<input id="upsw" name="upsw" type="text"><br><br>
        <input id="btn" type="submit" value="提交"><br><br>
        
        {{msg}}

    </form>

</body>
</html>
```

