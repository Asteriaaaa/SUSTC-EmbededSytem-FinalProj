<center style="color:grey">
    <p> <u>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp嵌入式系统与微机原理实验报告&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</u>
    </p>
</center>
<center>
    <h2>
        期末项目：电子相册
    </h2>
    <br>
    <h5>11612803 程屹伟  11611722 钟兆玮  11612831 关浩 11611908 龚玥 </h5>
</center>



​    

* ### 实验器材

  * 硬件: ARM-STM32开发板，实验工具箱，J-Link， OV7670 摄像头， SanDisk SD 卡。
  * 软件：Win7/Win8/Win10, Keil uVision5.

* ### 实验要求

   * 实现用显示屏模块输出 SD 卡中储存的照片

   * 使用按键自由切换照片，删除图片，自动播放图片

   * 加入 OV7670 摄像头模块，实现摄像头实时输出到显示屏上

   * 实现照相功能，保存摄像头的图像到 SD 卡

* ### 问题分析

   * 图片输出与照片保存

     分析这应该与我们平时编程时遇到的二进制 I/O 问题相似，会有相关函数进行操作。

   * 摄像头实时画面显示

     实时地将摄像头画面传回 LCD 屏，应该也是通过一个类似图片显示的函数对屏幕进行扫描，解码输出。

* ### 实验思路

  * ##### 输出 SD 卡中的图片及播放

    根据相应文档，首先应该在 SD 卡中建立一个存放图片的文件夹，在代码中，各项初始化后先使用`f_opendir()`将该文件夹打开，其方式与 C 的传统方式相似，之后再使用`f_readdir()`读取路径下文件并判断类型，最后得出图片文件个数，之后用相似的方法读取图片并使用`piclib`显示，使用`dir_sdi()`改变当前的文件索引，循环遍历改变文件索引并显示图片即可实现幻灯片播放，查看前后图片可对该文件编号进行改变，暂停则可在循环中设置 Flag 变量，当其为 true 时不对文件索引进行改变。

  * ##### 摄像头及拍照

    摄像头实时记录主要就是根据文档使用 `LCD_WR_DATA`方法进行循环实时输出，至于拍照则可使用 `bmp_encode()` 方法保存至路径下，文件名按照统一格式，利用整数递增的方式进行区分。

  * ##### 整合

    主菜单:在最开始设置死循环，通过触摸屏幕不同区域进行功能选择，同时在这一步设置 Label ，在两个功能内部分别再设置一个区域用来返回该主菜单.

* ### 实验结果

  * #### 实际验证附图

    验证材料将以视频形式放在包中。

* ### 遇到的问题及解决

   首先遇到的问题是工程看起来十分复杂没有头绪，不过翻阅手册后发现了一些可用的函数，包括 SD 卡读取和摄像头的及思路，在边编写程序边做单元测试的时候偶尔发生调用库缺失或错误等编译或连接错误，解决方法是...缺啥补啥错啥改啥的治标战术，最后成功地让整个程序得以编译连接通过。

   之后便是功能上的问题，首先是删除照片的操作，根据我们本来的代码，在读完文件列表后才会进入一个 `while(1)` 的无限循环等待操作或是播放幻灯片，而删除操作明显应该放在这个循环中，问题就出现在删除某一文件后，程序可能仍然认为整个列表有原来那么多照片，从而导致被删除照片的前后照片被重复显示。解决方法是在删除的功能代码中使用 `goto` 回到读取文件列表的代码处，同时定义一个全局变量来记录被删除文件的位置，使删除文件后显示上不会出现异常。还有一个问题是我们使用按键进行照片前后查看的努力似乎失败了，在串口通过打印按键信息发现按键并没有起作用，多次尝试无果后决定采用触屏的方式来实现这一功能并获得了成功。

   硬件方面，由于似乎 OV7670 的线缆之间的相互干扰导致有时候使传回屏幕上的画面质量不佳，解决方法是将时钟线电源线与其它数据线分开，这样能有效减轻炸裂的情况。

* ### 分工情况

  程屹伟：工程搭建，播放图片部分代码，代码汇总验证。

  关浩：读取、播放图片部分代码。

  龚玥：摄像头显示相关代码。

  钟兆玮：摄像头显示部分代码，拍照代码。

<center style="color:grey">	<u>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</u>
<br>
    <p>
        姓名：程屹伟&nbsp&nbsp&nbsp学号：11612803&nbsp&nbsp&nbsp日期：2018/9/17
    </p>
</center>