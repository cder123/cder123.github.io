

# Python爬虫-案例篇

[toc]





## 0、资料：

-   [视频教程](https://www.bilibili.com/video/BV1RJ411z7tH?p=9)





## 1、百度翻译

步骤：

-   打开页面，切换到移动端，找到`sug`报文
-   封装请求
-   获取数据
-   数据解析



```python
import requests


# 百度翻译

## 浏览器控制台切换成手机模式
## 找到XHR => sug报文
## 返回的数据示例：
### 	{'errno': 0, 'data': [{'k': '北京大学', 'v': '名. Peking University'}]}


url = "https://fanyi.baidu.com/sug"
headers = {'User-Agent': 'Mozilla/5.0'}
src_word = input("请输入要翻译的中文：")

data = {
    'kw': src_word
}

res = requests.post(url, data=data, headers=headers)

code = res.status_code

# 打印状态码
print(code)

# 如果请求成功，解析为JSON格式
if code == 200:
    print("请求成功！！")
    res.encoding = "utf-8"
    data_json = res.json()
    if data_json['errno'] == 0:
        # 打印返回的整个Json数据
        print("整体：")
        print(data_json)
        
        # 打印关键字
        print("键：",end=' ')
        print(data_json["data"][0]['k'])
        
        # 打印对应的值
        print("值：",end=' ')
        print(data_json["data"][0]['v'].split(';')[0])
    else:
        print("出错了！！")

```





## 2、爬取CSDN自己的收藏



携带 Cookie 爬取

```json

// 响应数据示例：

    {	'code': 200, 
        'message': '成功', 
        'data': {
            'result': [{
                'ID': 10063298, 
                'Name': 'Java-JVM',
                'FavoriteNum': 1,
                'FollowNum': 0, 
                'Favorites': None, 
                'Follows': None, 
                'Username': 'c13365_', 
                'Description': '',
                'IsPrivate': 0,
                'CreatedAt': '2021-07-13T16:40:23+08:00',
                'UpdatedAt': '2021-07-13T16:42:39+08:00'
             }],
        'total': 1
        }
    }

```





```python
import requests

url = "https://me.csdn.net/api/favorite/folderList"

# 伪装User-Agent、携带Cookie
headers = {
    'User-Agent': 'Mozilla/5.0',
    'Cookie': ' cookie值' # 浏览器抓包folderList响应，找到Cookie,复制到这里
}

res = requests.get(url,headers=headers)

code = res.status_code

print(code)

if code ==200:
    res.encoding="utf-8"    
    res_json = res.json() # 解析json数据
    print(res_json['data']['result'][0]['Name']) # 打印指定的数据（收藏文章的名字）
```





## 3、爬取网易云的歌曲评论

```python
import requests
import time


# url = url + "type=" + "search" + "&search_type=1&s=" + "28012031"

base_url = 'https://api.imjad.cn/cloudmusic/?'
headers = {'User-Agent': 'Mozilla/5.0'}

while True:
    song_id = input("请输入歌曲id【以便爬取评论】：")
    url = base_url + "type=" + "comments" + "&id=" + song_id

    time.sleep(3)
    res = requests.get(url, headers=headers)
    code = res.status_code
    print(code)

    if code == 200:
        res.encoding = "utf-8"
        res_json = res.json()
        print(url)
        print(res_json)

        if res_json["more"] != False:
            comments = res_json["comments"]
            with open("./comments.txt", "w+", encoding="utf-8") as f:
                for item in comments:
                    f.write(item["content"])
                    f.write('\n')
                f.close()

        else:
            print("无此歌曲的评论！！")
            break;

```





## 4、Xpath的使用：



第一种解析方式：【解析html字符串】

```python
html_str = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
        <ul>
            <li class="item-0"><a href="1.html">张三</a></li>
            <li class="item-1"><a href="2.html">李四</a></li>
            <li class="item-mid"><a href="3.html">成龙</a></li>
            <li class="item-1"><a href="4.html">王五</a></li>
            <li class="item-0"><a href="5.html">赵六</a></li>
        </ul>
</body>
</html>
"""

# 解析html
html_1 = etree.HTML(html_str)

# 提取数据：找出类名class="item-mid"的标签的文本n
data = html_1.xpath("/html/body/ul/li[@class='item-mid']/a/text()")

print(data)
```



---



第二种解析方式：【读取html文件并解析】

```python
# 第二种获取HTML的语法
html2 = etree.parse("./1.html",etree.HTMLParser())

# 找到 ul > li > a[href='5.html'] 的文本内容
res = html2.xpath("//ul/li/a[@href='5.html']/text()")

print(res) # ['赵六']
```







## 5、有道翻译



爬取有道翻译，记录到本地的`data.txt`中

```python
import requests

# 有道翻译
url = "https://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule"

headers = {
    'User-Agent': 'Mozilla/5.0'
}


while True:
    print()
    src_word = input("请输入要翻译的内容【按Q/q退出】：")

    if src_word.upper() == "Q":
        print("退出成功，欢迎下次使用！！")
        break;
    else:
        data = {
            'i': src_word,
            'doctype': 'json',
            'version': '2.1'
        }

        res = requests.post(url, data, headers=headers)

        if res.status_code == 200:
            restData = res.json()

            if restData["errorCode"] == 0:
                print("\t"+restData.get('translateResult')[0][0].get("src") + " ===> "+ restData.get('translateResult')[0][0].get("tgt"))

                with open("./data.txt", "a+", encoding="utf-8") as f:
                    f.write(restData.get('translateResult')[0][0].get("src") + " ===> "+ restData.get('translateResult')[0][0].get("tgt"))
                    f.write("\n")
                    f.close()

```

