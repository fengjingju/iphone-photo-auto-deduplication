@[TOC]
# 一、前提

> 用苹果手机照相，有不使用默认的4:3拍照的习惯。
>  如果只使用默认4:3比例拍照，后面的内容均可跳过。

# 二、问题描述
我们在将iphone照片拷贝到电脑的过程中（USB接入电脑拷贝的方式），如果喜欢使用16:9来拍照或者出现过不是默认的4:3拍照的情形，你会发现拷贝出来的照片 总是有一模一样的两张，只不过一张是4:3的一张是16:9的（假设开的是16:9），因为iphone的其他比例方式是通过在4:3的比例上剪裁出来的16:9。

我们暂且管4:3叫原片，4:3的比例为A，16:9的比例为B，那么实际的情况是：

 1. A为原片，B是在A的基础上剪裁的
 2. 真正通过USB方式拷贝的，会同时存在A与B
 3. A与B文件名几乎相同，唯一的区别是B的名称中间加了个E，例如：A-->IMG_3854.HEIC，B-->IMG_E3854.HEIC
 4. 只有HEIC的苹果相机照片会有这个问题，png等不存在
 5. 我们需要B存在时留下B，否则留下A
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a54986bbfdfe4dfca78f4593a705ea30.png =300x)
# 三、原始处理方式
最消耗时间的方式肯定是用肉眼去对比，一张一张的删，只有对比过才知道有多么麻烦
# 四、程序处理
于是想到用程序来解决，具体代码就不讲了，流程无非就是：
 
 1. 将所有iphone拷贝出来的照片文件夹放到一个文件夹路径下
 2. 提供这个文件夹的路径，可以是文件夹套着文件夹
 3. 根据这个路径，去遍历该文件夹以及该文件的所有子文件夹下每一个HEIC扩展名 照片，若一个照片名字同时存在中间带E和不带E的，则删除不带E的，否则跳过

## 4.1 java程序如何打包exe
### 4.1.1 首先打包jar
  File--->Project Structure--->Artifacts，点加号
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/6562f313c60c4bbe809ea10f94ce1396.png =550x)
 选择需要生成jar的Module。
 <font color='red'>注意：此处一定要把只与本次相关的程序单独放在一个工程或者一个Module，否则生成的jar会包含所有的无关的类。</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e371b526e5bc4a5a874c0f59b08ba377.png =400x)
单击OK，然后Build-->Build Arrifacts-->Build，即可在刚刚设置的路径下生成jar包
![在这里插入图片描述](https://img-blog.csdnimg.cn/10f6a5697db1431387e39cec9c00b8f7.png =200x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ce977e3456d4e7e83a4ea83b87a0e5d.png =500x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/40a8a363f068402695b0edbaaa2c66f8.png =100x)
如果在生成的过程中遇到如下报错，是因为META-INF已经存在了，删了重新生成即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/329ad30a3d1748fcacb23082a478928d.png =500x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/13fb56c363d94f859070b0e54aea9cd0.png =250x)
### 4.1.2 开始生成exe
<font color='red'>[ 注意：此种方式生成的exe不能在没有java环境的电脑上运行，怎么解决后面说 ]</font>
通过jar生成exe，我们选择launch4j来生成
launch4j官网：[https://launch4j.sourceforge.net/](https://launch4j.sourceforge.net/)

安装后打开，有几个必填项：
1、Basic
![在这里插入图片描述](https://img-blog.csdnimg.cn/11e7a5f493cf4f158f17926abbe90a5c.png =600x)
2、Header
默认是GUI，就是一个干净的窗口。如果选择Console，打开exe时还会附带一个cmd窗口 用于控制台输出
![在这里插入图片描述](https://img-blog.csdnimg.cn/449160547a7149b5ab0aa4b70e771340.png =600x)
console就是这个效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/9fc4bf390824477a9fe90437cbd11747.png =800x)
3、其他的classpath、JRE什么的，用默认的就行了 不用管
<font color='red'>[ 注意：如果需要在没有java环境的电脑上运行，此处JRE需要配一下，怎么配跳转至4.3 ]</font>

4、然后直接点上方的小齿轮就能生成了
![在这里插入图片描述](https://img-blog.csdnimg.cn/f10ac1cdc7b14cf2b88fdaedb5f8682d.png =600x)
随便输入个保存xml，回头删了即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ded8bb979324f00bb601c9e1a9937e6.png =600x)
运行效果如下：
![](https://img-blog.csdnimg.cn/12fbb472738e4273b6a6ba61e4e58698.png =700x)
没有java环境会报这个
![在这里插入图片描述](https://img-blog.csdnimg.cn/76741790c40d48e1a922d05d069d6404.png =300x)

### 4.1.3 软件使用方式

 1. 输入需要清理的照片路径，该路径随便填，支持递归。例如：E:\新建文件夹，则可以清理该文件夹下的内容以及其所有子文件夹内容
 2. 单击 [开始清理] 按钮，程序会自动获取E:\新建文件夹下所有文件夹内的照片，逐个清理重复的照片
 3. [清空输出文本] 按钮，可清除所有绿色文字
![在这里插入图片描述](https://img-blog.csdnimg.cn/99b029626aa3449395a68669b24ca003.png =500x)
## 4.2 更换图标
默认的图标如果嫌丑的话，可以更换ico图标，首先去网上下载或者自己制作一个ico图标
### 4.2.1 更换swing的打包jar图标
这个图标，可以使用png、jpg。ico不行
```java
		// 设置左上角图标
        ImageIcon imageIcon = new ImageIcon("C:\\Users\\xxx\\Desktop\\Backpack.png");
        jFrame.setIconImage(imageIcon.getImage());
```
运行效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/5e938c9e61234de485211c8ff69e3a9a.png =500x)
### 4.2.2 更换exe图标
Launch4j的Basic添加Icon路径，然后点齿轮生成exe
![在这里插入图片描述](https://img-blog.csdnimg.cn/2bd581954e1f4926800141ab53ae3222.png =600x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/da45384a9ea04750b6b9fa8547beba41.png =100x)
## 4.3 如何使生成的exe在没有java环境的电脑上运行
首先需要把jdk下面的jre文件夹完整拷贝出来，我的路径是：D:\Java\jdk1.8.0_162\jre，和最终生成的exe放到一个目录下，然后把Launch4j的JRE路径改成.\jre，说明运行的jre环境是同一目录的这个，生成exe
![在这里插入图片描述](https://img-blog.csdnimg.cn/d4dd548b3bbb4a3c94b24e94bccd6465.png =600x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/da65840a892f4fdc8a830803353bd793.png =400x)
然后有两个方案：
方案一：把jre文件夹和exe程序打成压缩包，别人在使用时，不能移动任何一个文件夹的位置，否则会出现问题
方案二：直接打包成Setup文件，使用时先安装，后使用。
### 4.3.1 Inno Setup打包
我们来说方案二
首先去下载Inno Setup，是一个安装制作软件，使用其可以将多个文件/文件夹打包成安装包
官网：[https://jrsoftware.org/isinfo.php](https://jrsoftware.org/isinfo.php)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1cd911783ed346a3a99bdfb6b0126114.png =400x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b92a12710c7a40be9509a49c7093f755.png =500x)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1853ac65a9f04338a463e434b0a36743.png =350x) 
![在这里插入图片描述](https://img-blog.csdnimg.cn/5e1fd0645a4a496f88806495f0378028.png =450x)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2ecf301124bf45ec905bc7ce55a3c273.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d0521c5ba3364e5e9e92d11ad3dc5419.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ccdbde5e81f5486fb56308f7aadc2e2a.png =450x)


![在这里插入图片描述](https://img-blog.csdnimg.cn/4258966709fe4f1a9ff00f7845770c1c.png =350x)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ae00542fce2443dda2781705129682aa.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bf8954a39fd54a748ca944ebf0e68717.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a66d4b9774de47f0b0a9b34ec40eb656.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/119619917fee4523879ac05007a8914a.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3205920d89764884b7943b1d04a4fc5d.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/cf8715ac325a4d1687487ab22e6b0835.png =450x)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d937a5a225c24308bcc129c3fa6fb5f5.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/131ef01c6e9e46aaab1a0d0c4e82ae0c.png =450x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2eca59692fb44e0baab6a7d5f3c0f604.png =350x)
如果在生成的过程中因为什么被打断了，可以通过如下按钮重新生成
![在这里插入图片描述](https://img-blog.csdnimg.cn/72efd0538093402eb293d04fb64f101f.png =450x)
最终生成了一个Output文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/70a30fcbe02f4d5f9031bde9e18ba976.png =350x)
打开之后就是安装包
![在这里插入图片描述](https://img-blog.csdnimg.cn/421e00858afc41ad9f59eacfca7c816a.png =350x)
双击安装包试一下，大功告成！
![在这里插入图片描述](https://img-blog.csdnimg.cn/a63159180ae14e60a94b541c5eaa6b3e.png =450x)

## 4.4 附件下载
需要java环境的exe见文章头部
不需要java环境的安装包：[https://download.csdn.net/download/qq_26012495/88210285](https://download.csdn.net/download/qq_26012495/88210285)
