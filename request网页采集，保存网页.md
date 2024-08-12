##### ==request网页采集== 

需要在pycharm中安装requests模块

```python
#request网页采集
import requests
keyword=input("请输入搜索关键词:")

#指定url
url=f'https://www.baidu.com/s?tn=15007414_15_dg&ie=utf-8&wd={keyword}'
#header要在自己电脑浏览器中找，不是用别人的代码
header={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36'}

#网络请求,获得响应
reponse=requests.get(url=url,headers=header)
print(reponse.text)
#写入文件中,在本地找到然后打开就是网页
with open(keyword+'.html','w',encoding='utf-8') as f:
    f.write(reponse.text)
    print(f'已下载...{keyword}')
```

![image-20240722110745920](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20240722110745920.png)

打开保存的文件，在浏览器中打开,如图:

![image-20240722110848495](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20240722110848495.png)