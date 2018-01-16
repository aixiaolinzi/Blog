蓝牙BLE开发，第一篇
##问题：
1.怎样连接？又分为两种在扫描列表中和在已经绑定的设备中。是否还有已经连接的设备（a2dp和head的处理）。
2.mac地址是否在变化？
3.连接上以后怎样通信，我的BLE中的Service，Characteristic，Descripter都是使用UUID来作为唯一标识。所以我们在读写BLE蓝牙数据时，都要带上相应的UUID。那么UUID怎样定义的？？
4.注册的Notify或者Indicate机制(UUID是固定的)。则改服务上的该属性可以接受服务端对客户端的notify，前提是连接完成以后客户端需要设置改属性为true。


##前言
先来个声明，我也是半斤八两，第一次接触BLE。
好久没有更新文章了，最近在对接蓝牙的问题，公司想要做到BLE设备和手机通信。那么就我目前的所理解的写下面一片文章。
推荐一个辅助工具：：[nRF Master Control Panel (BLE)](http://www.shuame.com/app/no.nordicsemi.android.mcp)

##任务
手机连接上BLE，给BLE设备发消息。
##背景
**注意点：**可能很多人会误解蓝牙4.0就是低功耗蓝牙，但是完整的蓝牙4.0规范实际上是包含经典蓝牙和低功耗蓝牙两部分的。只是说在蓝牙4.0规范，蓝牙技术联盟才引入了BLE。
具体的蓝牙介绍请查看这篇文章：
[Android-BLE蓝牙原理](http://www.jianshu.com/p/f98e77c9ec65)

##开始
第一步：需要权限。
如果你想要你的app启动设备发现或操纵蓝牙设置，你必须也要申明 BLUETOOTH_ADMIN 权限。注意：如果你用了 BLUETOOTH_ADMIN 权限，则还必须有 BLUETOOTH 权限。
```groovy
    <uses-permission android:name="android.permission.BLUETOOTH"/>
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
```
除了蓝牙权限外，如果需要BLE feature则还需要声明uses-feature：
```groovy
    <uses-feature android:name="android.hardware.bluetooth_le" android:required="true"/>
```
按时required为true时，则应用只能在支持BLE的Android设备上安装运行；required为false时，Android设备均可正常安装运行，需要在代码运行时判断设备是否支持BLE feature：

```groovy
// Use this check to determine whether BLE is supported on the device. Then
// you can selectively disable BLE-related features.
if (!getPackageManager().hasSystemFeature(PackageManager.FEATURE_BLUETOOTH_LE)) {
    Toast.makeText(this, R.string.ble_not_supported, Toast.LENGTH_SHORT).show();
    finish();
}
```
开启蓝牙搜索的时候还需要下面的权限，**注意：**下面的权限任意一个都可以，当你搜索蓝牙设备的时候如果发现不了任何设备，就检查一下是否授予位置的权限。
```groovy
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```
第二步：开启扫描
现在我们来处理扫描，我们的APP已经有了权限，那么我们来找到设备。
