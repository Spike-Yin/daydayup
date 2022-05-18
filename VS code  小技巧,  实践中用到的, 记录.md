# Emmet语法/VS code  小技巧,  实践中用到的, 记录

* 就是在顶置的**文件地址input栏**里输入cmd,  打开该文件地址的cmd窗口,  输入**code .**
* 打开这个地址的vscode , 直接开始做新建项目

---

* 或者是直接从cmd里进入,   手动输入命令行进入想要去的文件地址, 然后输入**code .**
* 这个看起最装逼了
* cmd  --> d:  --> dir  -->  choose dir path  -->code .



----

* 建立前端文件
* 第一个就是总文件src  (源码文件)  src里有css文件夹, index.html, js文件夹, images文件夹



----

* 关闭代码缩略图,  设置里搜寻mini  点击是否显示代码缩略图

---





##  下面是有用的vs 快捷键

* 注释就是ctrl+/     取消注释, 就是在被注释的语句里面再写一次ctrl+/
* 移动代码行, 就是alt键加上下键
* 显示或隐藏左边显示栏  就是ctrl+b
* 复制代买就是shift+alt+上下键       (上下就是向上复制或向下复制)
* shift+ctrl+k   就是删除当前行
* 打开下方终端输入栏,    输入ctrl+`     **在数字1旁边那个键**
* ctrl+p   这是查找当前repo里的代码文件
* 格式化代码**(意思就是将写乱的代码规范化)**  shift+alt+f
* 新建一个窗口,    ctrl+alt+n
* **同步选中, 多行一起修改, 这个非常重要**        **ctrl+alt+上下键**      ----- 同步选中



## 下面是Emmet语法

* Emmet语法是可以提高开发效率的, 尤其是对前端开发,  这个很重要, 记得多学学



* 生成多个语法栏,   输入   **nav>ul>li**       或者多生成几个li  就这样输入  **nav>li*5**
*  **>**    大于号是从属关系,   **+**  加号就是平级关系了
* 比如输入  **div+p**
* 快捷输入class, 就是输入  **div.class>ul>li{item $}*10**
* 这个后面的**div.class**  的**.class就是div的class名, 只要加个点就好**
* **{}**大括号里就是li内部输出的内容, **$** 美刀号就是序号!
* ![image-20220511205804627](C:\Users\VLINi\AppData\Roaming\Typora\typora-user-images\image-20220511205804627.png)

* 就是上面那种



* 删除一行emmet语法,  只需要ctrl + x 就行



* 还有一种是添加id 的快捷语法  **div#id**
* div 是生成的语法栏,  id就是自己设置的id了