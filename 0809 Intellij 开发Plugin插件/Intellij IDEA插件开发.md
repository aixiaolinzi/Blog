Think In Java 第一篇

##前言

前两天下载了Intellij IDEA工具，研究他的时候发现还能够开发插件呢！！！抱着惊奇的眼神，我决定一探究竟。

##任务

编写一个Plugin用于Android Studio的使用。

##创建插件工程
创建好项目之后，默认会在编辑器中打开plugin.xml，和你见过的其他配置文件一样，这个文件在插件安装时会提供给IDEA插件的各种信息，包括名称、插件信息、作者信息、挂载点、入口类、注册的代码模板和文件类型支持等。先不管别的，找到actions标签，在其中注册我们的插件，像这样：
```GROOVY
<action class="com.moxun.example.Plugin" id="moxun.plugin" text="Plugin Guide" description="A simple IDEA plugin." icon="/icons/icon.png">
<add-to-group  group-id="MainToolBar" anchor="last"/>
<keyboard-shortcut keymap="$default" first-keystroke="alt A"/>
</action>
```
解释一下各个参数的意思，`class`字段定义插件的入口类，`id`字段是插件在IDE内一个独有的id，`text`为当鼠标指到插件入口时浮出的文字提示，`description`为当鼠标指到插件入口时在状态栏上出现的提示，`icon`定义插件的图标。`add-to-group`标签中`group-id`指定要挂载插件的group，`anchor`字段指定插件在该group中的位置。`keyboard-shortcut`快捷键的设置。可以在IDE中搜索“PlatformActions.xml”查看作为当前SDK的IDE中提供了哪些group。