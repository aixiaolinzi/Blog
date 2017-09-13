npm的使用

##前言

忽然之间发现好多地方使用了npm。但是npm是什么？

##任务

简单了解npm，使用npm。

##开始npm的认识之旅
npm的使用就是安装node.js工具。
安装成功就可以调用：
```groovy
node -v 和 npm -v
得到版本号。
```
npm可以为npm自己升级：
```groovy
npm install npm --global
```


可以全局范围安装包。
```groovy
npm install 工具包名 --global
```
全局范围卸载包：
```groovy
npm uninstall 工具包名 --global
```


本地项目安装包：
```groovy
npm install 工具包名
```
> npm list或者npm ls
> 查看项目安装的包

特殊需求安装特定的工具包：
先查看工具包名称：
```groovy
npm info 工具包名
```
使用特定的版本号:
```groovy
npm install 工具名称@版本号
```
删除本地项目安装包：
```groovy
npm uninstall 工具包名
```


package.json有点像是配置文件，可以描述项目的问题。
创建文件：可以是手工，也可以是代码创建
```groovy
npm init
```
>里面的项目依赖在哪里
>dependencies:项目依赖的
>devDependencies:开发项目的时候的依赖
>

保存在dependencies里面：
```groovy
npm install 工具包名 --save
```
保存在devDependencies里面：
```groovy
npm install 工具包名 --save-dev
```
分享项目的时候，会删除node_modules工具包，我们可以根据package.json来恢复相应的工具包。
```groovy
查看没有安装的包：
npm list
直接安装没有安装的包：
npm install
```
删除安装的包并且在package.json中删除:
```groovy
npm uninstall 工具包名 --save
```
查看一下包的列表
```groovy
npm list|grep 工具包名
就没有相应的值了
```

更新本地安装的包：
检查更新：
```groovy
npm outdated
```
> 注意：
> ```groovy
> 	"devDependencies": {
		"babel-jest": "^20.0.3",
		"jest": "^20.0.4"
	},
问题来了：
版本号前面有^就说明只能更新第二位数字就是中间那个。
版本号前面有~就说明只能更新第三位数字就是最后面的那个。
版本号被*替代就说明能更新到最新的版本号。
^的使用更新是最安全的。
> ```


安装源的问题：就是解决下载的速度：
在没有设置之前`npm install`使用的是默认的源。有些时候这些源是比较慢的。我们就要切换源。
使用nrm的指令去替换npm的安装源。
先安装nrm：
```groovy
npm install nrm --global
```
查看可以使用的源：
```groovy
nrm ls
```
测试电脑连接源的速度：
```groovy
nrm test
```
切换源：
```groovy
nrm use 源名字
```