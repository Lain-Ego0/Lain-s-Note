# 速通

1：打开首选项设置，搜索encoding，然后打上对钩

2：在vscode中的扩展中搜索：GBKtoUTF8，然后安装

工程关闭重新加载即可

#### 文章目录

- [（一）中文乱码出现的原因](https://link.csdn.net/?target=%23_1%3Flogin%3Dfrom_csdn)
- - [1.关于编码](https://link.csdn.net/?target=%231_2%3Flogin%3Dfrom_csdn)
  - [2.乱码](https://link.csdn.net/?target=%232_4%3Flogin%3Dfrom_csdn)
  - [3.keil工程导入vscode出现乱码](https://link.csdn.net/?target=%233keilvscode_6%3Flogin%3Dfrom_csdn)
- [（二）解决方法](https://link.csdn.net/?target=%23_13%3Flogin%3Dfrom_csdn)

## （一）中文乱码出现的原因

### 1.关于编码

世界上语言文字、符号文字丰富多彩；为了存放不同的文字符号，就有了不同的编码格式。常使用的有GB、GBK以及Unicode等等。

### 2.乱码

每种编码格式能容纳的字符是有一定的数量限制的，当输入一个字符不属于改编码格式的范畴之内，便会使用一种特定的符号来表示这个字符，也就是人们常说的乱码。

### 3.keil工程导入vscode出现乱码

以我个人的KEIL为例说明

![keil设置界面](https://i-blog.csdnimg.cn/blog_migrate/5a242a2146dcc2a2e8737014b73fd0c8.png#pic_center)
在我的keil设置界面中**Encoding**选项中，编码格式是ChineseGB2312，当我把这个工程文件导入到vscode当中时，就出现了❓❓的乱码，因为vscode的默认编码格式为UTF-8。

![vscode的默认编码格式](https://i-blog.csdnimg.cn/blog_migrate/85e6c4863a0611abcae8adcc5aa5db06.png#pic_center)
但是有小伙伴再将编码格式修改为GB2312时，乱码不仅没有解决，反而❓❓变成了一大堆“锟斤铐”的乱码。![尝试修改vscode的编码格式](https://i-blog.csdnimg.cn/blog_migrate/0714bed003c2b78c9bb96fc91a9a6cfc.png#pic_center)
关于为啥会出现这个问题，这里放一下b站up主**柴知道**的视频，他做出更有原理性的解释。[点击这里就能跳转观看](https://link.csdn.net/?target=https%3A%2F%2Fwww.bilibili.com%2Fvideo%2FBV1cB4y177QR%2F%3Fspm_id_from%3D333.999.0.0%26vd_source%3Df862f9173ff2c068de1453e2ef71699e%26login%3Dfrom_csdn)。

## （二）解决方法

明白了原理，解决就有了思路。就是修改vscode工程的默认打开编码格式，要与keil工程中的一致就行了。
在vscode的设置中输入**encoding**进入修改编码格式界面，![修改编码格式](https://i-blog.csdnimg.cn/blog_migrate/caaf84e6ba9baa124dd56b861c08540e.png#pic_center)
其中第一项 `Files:Auto Guess Encoding`的意思是让vscode在导入工程时自动识别工程的编码格式，并与调整为与之一致。
第二项 `Files:Encoding`的意思就是修改vscode的默认编码格式，我这里修改为了**GB2312**。
当然并不是说两项都必须这样设置，选择哪一种方式可以按照自己的喜好来进行设置。
