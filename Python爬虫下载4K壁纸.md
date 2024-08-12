>  🎁🎁创作不易，关注作者不迷路🎀🎀

**目录**

[🌸完整代码](#🌸完整代码)

[🌸分析](#🌸分析)

[ 🎁基本思路](# 🎁基本思路)

[🎁需要的库](#🎁需要的库)

[🎁提取图片的链接和标题](#🎁提取图片的链接和标题)

[👓寻找Cookie和User-Agent](#👓寻找Cookie和User-Agent)

[👓图片链接和标题](#👓图片链接和标题)

[🎁下载保存图片](#🎁下载保存图片)

[🎁获取目录页面图片和翻页提取](#🎁获取目录页面图片和翻页提取)

[👓目录页图片的提取](#👓目录页图片的提取)

[👓翻页规律寻找](#👓翻页规律寻找)

[🌸运行效果](#🌸运行效果)

[ 🌸文末彩蛋🎀](# 🌸文末彩蛋🎀)



![img](https://i-blog.csdnimg.cn/direct/78ea99e052ef478a9c3d9599448d5737.jpeg)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

> 我们经常想要寻找一些高清的壁纸，图片作为素材（为CSDN博客找一张吸引读者的封面🤣），然而一张一张的下载太慢了，因此为了提高工作效率， 我们可以采用爬虫的方式，快速下载图片。

## 🌸完整代码

```python
import os#导入操作系统的库
import requests  #导入HTTP库
from lxml import etree#导入lxml库，数据解析


global num
num=1
#请求头,伪装爬虫
header={
'user-agent':
'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36 Edg/127.0.0.0',
'cookie':
'zkhanecookieclassrecord=%2C66%2C70%2C'

}

#获取具体的图片的地址和名字信息
# url='https://pic.netbian.com/tupian/34694.html'
def get_pic(url,header):
    re=requests.get(url,headers=header)
    re.encoding=re.apparent_encoding#获取html文本时用网页原有的编码方式，防止乱码
    #print(re.apparent_encoding) #返回的编码
    html=etree.HTML(re.text)
    link=html.xpath('//div[@class="photo-pic"]/a/img/@src')[0]#获取图片链接
    link='https://pic.netbian.com'+link
    print(link)
    title=html.xpath('//div[@class="photo-pic"]/a/img/@title')[0]#获取图片名称
    print(title)
    return title,link

#如若下载其他页面,则修改文件夹名字即可
#下载保存图片
def download_pic(url,header):
    global num
    title,link=get_pic(url,header)
    if not os.path.exists(r"C:\Users\liu\Desktop\图片\4K壁纸"):#未找到文件夹则创建文件夹
        os.mkdir(r"C:\Users\liu\Desktop\图片\4K壁纸")
    content=requests.get(link,headers=header).content
    with open(rf"C:\Users\liu\Desktop\图片\4K壁纸\{str(num)}.jpg",'wb') as f:#以二进制编码写入文件
        f.write(content)
    num += 1

#目录翻页提取链接
def get_content_link(url,header):
    # url='https://pic.netbian.com/pingban/index.html'
    re=requests.get(url,headers=header)
    re.encoding=re.apparent_encoding
    # print(re.text)
    html=etree.HTML(re.text)
    links=html.xpath('//div[@class="slist"]//a/@href')
    for x in links:
        x='https://pic.netbian.com'+x
        download_pic(x,header)

#循环遍历网页，处理信息
for i in range(1,24):
    if i==1:
        url='https://pic.netbian.com/pingban/index.html'#修改目录页的地址可以提取其他页面的
    else :
        url=f'https://pic.netbian.com/pingban/index_{i}.html'#修改其他目录页地址
    get_content_link(url,header)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 🌸分析

###  🎁基本思路

-  找到图片页网页源代码
- 提取所有图片的链接和标题
- 下载保存图片
- 爬取目录页的网页源代码
- 下载目录页的图片
- 分析不同页面的地址变化，找出规律实现翻页下载

### 🎁需要的库

```python
import os
import requests
from lxml import etree
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

requests和lxml库是第三方库，需要自己安装

### 🎁提取图片的链接和标题

#### 👓寻找Cookie和User-Agent

首先打开页面，打开开发者工具，按Ctrl+R刷新页面，点击开发者工具的“网络”选项，点击第一份文件，查看请求地址，Cookie和User-Agent

![img](https://i-blog.csdnimg.cn/direct/f3606dd857fc49118eac5d682801885c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/7ccd6cc08bae4ffca38b47bac0dac60f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/3fdc42d403044749a5d971253502a84f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 将Cookie和User-Agent作为请求头

```python
header={
'user-agent':
'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36 Edg/127.0.0.0',
'cookie':
'zkhanecookieclassrecord=%2C66%2C70%2C'

}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 👓图片链接和标题

![img](https://i-blog.csdnimg.cn/direct/2ad03c738cf344a7ab5f608a6991066b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这里需要用到**lxml库以及xpath**的知识，看图说话，链接和地址存在<div class="photo-pic">下的a元素中img元素中的src属性和title属性

**图片链接**

```python
link=html.xpath('//div[@class="photo-pic"]/a/img/@src')[0]#获取图片链接
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**图片标题**

```python
title=html.xpath('//div[@class="photo-pic"]/a/img/@title')[0]#获取图片名称
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**写成函数方便调用**

```python
#获取具体的图片的地址和名字信息
# url='https://pic.netbian.com/tupian/34694.html'
def get_pic(url,header):
    re=requests.get(url,headers=header)
    re.encoding=re.apparent_encoding#获取html文本时用网页原有的编码方式，防止乱码
    #print(re.apparent_encoding) #返回的编码
    html=etree.HTML(re.text)
    link=html.xpath('//div[@class="photo-pic"]/a/img/@src')[0]#获取图片链接
    link='https://pic.netbian.com'+link
    print(link)
    title=html.xpath('//div[@class="photo-pic"]/a/img/@title')[0]#获取图片名称
    print(title)
    return title,link
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 🎁下载保存图片

存储到一个新的文件夹“4K壁纸”，如果文件夹不存在，需要创建，这里要用到os库

```python
#未找到文件夹则创建文件夹
if not os.path.exists(r"C:\Users\liu\Desktop\图片\4K壁纸"):        
    os.mkdir(r"C:\Users\liu\Desktop\图片\4K壁纸")
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

写入文件

```python
content=requests.get(link,headers=header).content
with open(rf"C:\Users\liu\Desktop\图片\4K壁纸\{str(num)}.jpg",'wb') as f:#以二进制编码写入文件
    f.write(content)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

写成函数方便调用

```python
#下载保存图片
def download_pic(url,header):
    global num
    title,link=get_pic(url,header)
    if not os.path.exists(r"C:\Users\liu\Desktop\图片\4K壁纸"):#未找到文件夹则创建文件夹
        os.mkdir(r"C:\Users\liu\Desktop\图片\4K壁纸")
    content=requests.get(link,headers=header).content
    with open(rf"C:\Users\liu\Desktop\图片\4K壁纸\{str(num)}.jpg",'wb') as f:#以二进制编码写入文件
        f.write(content)
    num += 1
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 🎁获取目录页面图片和翻页提取

上面我们实现一张图片的保存，写了十几行代码算是成功保存了🤣🤣🤣，一张图片干嘛这么麻烦捏😂，直接点击“图片另存为”不就行了吗，那如果是很多图片吗，那肯定是爬虫更快了呗

#### 👓目录页图片的提取

依然用到lxml库，利用xpath语法提取

![img](https://i-blog.csdnimg.cn/direct/806d80e1b40e425c985b7fc5edccaf1d.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

```python
#目录翻页提取链接
def get_content_link(url,header):
    # url='https://pic.netbian.com/pingban/index.html'
    re=requests.get(url,headers=header)
    re.encoding=re.apparent_encoding
    # print(re.text)
    html=etree.HTML(re.text)
    links=html.xpath('//div[@class="slist"]//a/@href')
    for x in links:
        x='https://pic.netbian.com'+x
        download_pic(x,header)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 👓翻页规律寻找

📕找到第一页目录页

```python
https://pic.netbian.com/pingban/index.html
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

📕找到第二页目录页

```python
https://pic.netbian.com/pingban/index_2.html
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

📕找到第三页目录页

```python
https://pic.netbian.com/pingban/index_3.html
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

发现规律:第一页单独列出来，其他页通过for循环改变index_{i}即可

```python
#循环遍历网页，处理信息
for i in range(1,24):
    if i==1:
        url='https://pic.netbian.com/pingban/index.html'
    else :
        url=f'https://pic.netbian.com/pingban/index_{i}.html'
    get_content_link(url,header)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

通过for循环遍历，最终可以实现所有图片的下载

## 🌸运行效果

成功下载4K壁纸，耗时两分半🐔，下载400多张图片，爬虫提取就是快，手动提取预估一坤时左右🐔

![img](https://i-blog.csdnimg.cn/direct/3cebfbecd1d840abb575016bf289891e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/8349dd1fc27242ceb05759df1431f1ed.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

##  🌸文末彩蛋🎀

![img](https://i-blog.csdnimg.cn/direct/6f88a26f863c438ebea14ae68fd77355.jpeg)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/e77652ad083c4915af09c48f2e7c36df.jpeg)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑 ![img](https://i-blog.csdnimg.cn/direct/2c02be81ca254f0588c882ba0acb2581.jpeg)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/278fde4b11fc4ffb870c8915a40b8631.jpeg)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑