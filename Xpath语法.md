### Xpath语法

```
XPath语法:XML Path Language是一门在XML和HTML文档中查找信息的语言，可以用来在XML和HTML文档中对元素和属性进行遍历
```

#### 语法

| nodename | 选取此结点的所有子节点                                   |
| :------- | -------------------------------------------------------- |
| /        | 从根节点选取                                             |
| //       | 从匹配选择的当前结点选择文档中的结点，而不考虑它们的位置 |
| .        | 选取当前结点                                             |
| ..       | 选取当前结点的父节点                                     |
| @        | 选取属性                                                 |

![image-20240801163844208](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20240801163844208.png)

```
谓语用来查找某个特定的结点或包含某个指定的值的结点。谓语被嵌在方括号中
```

![image-20240801164020337](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20240801164020337.png)

注意:Xpath语法中查找第一个元素是从下标1开始的



选取未知节点

![image-20240801165107747](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20240801165107747.png)

![image-20240801165225440](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20240801165225440.png)