自己CMD命令常用总结
##前言
CMD命令总是容易忘掉，那么用多少写多少吧。
##使用
####CMD命令进入某个目录
1. 开始->运行->CMD
2. 进入某个磁盘，直接盘符代号：如D：，不用CD 命令切换
3. 进入除根录以下的文件夹 cd 文件夹路径 例如我要进入 E:/Program Files/PHP 就
输入 E：回车
  注： 不 能在一打开CMD的时候运行CD E:/Program Files/PHP，需要先进入磁盘（若一打开CMD的时候运行CD E:/Program Files/PHP，目录不会切换，但在下次输入盘符的时候进入上一次希望进入的目录，如输入E：会直接进入E:/Program Files/PHP）
输入 CD "Program Files"/PHP 回车
  注：如果需要在dos下查看带有空格的文件夹（如Documents and settings，Program files等文件夹），可以有下面两种处理方法：
  >1、给文件夹加引号。 如C:/>cd c:/"documents and settings"
这样的好处是多长的文件名都可以全部显示出来。
2、由于一般情况下DOS系统只支持8.3格式的文件名，因此在DOS下遇到长文件名的文件夹时，取前面6位，然后在后面加上一个~号和数字1。 你可以输入C:/>cd c:/docume~1 进入Documents and settings文件夹。当截取前面的6个字母之后出现重复时，可以将1改为2，依此类推。
4. 进入特定的目录比如我要进入 E:/Program目录，但是不太清楚Program怎样拼写的，可以先输出cd E:/Pr然后点击Tab键，会自动补全。
5. 进入上一层目录 CD ../
6. 显示目录下的文件及了目录 dir
