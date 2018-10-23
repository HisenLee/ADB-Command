# ADB-Command

【001】生成签名文件：
【规则】“jdk bin目录” + “keytool -genkey -keystore” + "keystore的目录" + “-keyalg RSA -validity 10000 -alias” + "别名（尽量与keystore名字一致）"
C:\Program Files\Java\jdk1.8.0_45\bin>keytool -genkey -keystore C:\haixing\jks\86.keystore -keyalg RSA -validity 10000 -alias 86.keystore

【002】用签名文件给应用签名：
【规则】"jdk bin目录" + "jarsigner -verbose -keystore" + "keystore的目录" + “-signedjar” + "签名后生成的apk" + “签名前的apk” + “keystore的别名”
C:\Program Files\Java\jdk1.8.0_45\bin>jarsigner -verbose -keystore C:\haixing\jks\86.keystore -signedjar C:\haixing\testapks\86signed.apk 
	C:\haixing\testapks\x86.encrypted.apk 86.keystore
【注：如果是JDK7以上的话，签名需要加上-digestalg SHA1 -sigalg MD5withRSA】
C:\Program Files\Java\jdk1.8.0_45\bin>jarsigner -digestalg SHA1 -sigalg MD5withRSA -verbose -keystore C:\haixing\jks\86.keystore -signedjar C:\haixing\testapks\86signed.apk 
	C:\haixing\testapks\x86.encrypted.apk 86.keystore

【003】查看keystore签名的别名及MD5，Sha1等相关信息
【规则】"jdk bin目录" + “keytool -list -v -keystore” + "keystore的目录" + "-storepass" + "签名文件的密码"
C:\Program Files\Java\jdk1.8.0_45\bin>keytool -list -v -keystore C:\haixing\jks\86.keystore -storepass 123456


cls:清屏

1.查看所有包名
adb shell pm list package -f
adb logcat -c 先清除  ctrl+C可以退出adb

2.在手机上先打开某个apk，查看对应包名
adb shell dumpsys window w | findstr \/ | findstr name=

3.查看某应用的apk，在电脑上。
a.aapt dump badging ...（注：省略号代表apk在电脑上的路径）
C:\Users\haixingX>aapt dump xmltree C:\haixing\eclipse\workspace\Test\bin\Test.apk AndroidManifest.xml【查看xml文件】
b.aapt工具目录：C:\android-sdk-windows\build-tools\23.0.3
c.adb 启动 app : adb shell; am start -n pkgName/LaunchActivity


4.查看设备
adb devices

5.安装，卸载
adb install/uninstall ...
adb install --abi x86 *.apk[x86]
adb install --abi arm64-v8a *.apk[v8]
adb install --abi armeabi-v7a *.apk[v7]
adb install --abi armeabi *.apk[v5]

安装到sd卡
adb shell pm get-install-location 
adb shell pm set-install-location 2
如果提示Package android does not belong to 2000,就用super user来操作
adb shell;su;pm set-install-location 2.

6.生成Log日志
adb logcat -s pkgname *:E >C:\haixing\log.txt 【过滤包名以及Error以上级别】*:F是高于E的错误。一般Crash会直接是F级别的错误
【输出monkey测试日志】adb shell monkey -p com.bbk.recorder -v 10000 >d:\xxx.txt  
【输入优先级别大于Warning以上的Logcat信息】
adb logcat "*:W" > C:\haixing\log.txt
adb logcat -d -s xxx > xxx.log
adb logcat -d -s com.brianbaek.popstar > C:\haixing\log.txt

adb logcat >C:\haixing\log.txt 
adb logcat -c

【输出某包名的日志】adb logcat | grep "xxx" > C:\haixing\log.txt

7.重启、结束adb服务
adb start-server;  adb kill-server

8.指定设备执行adb
adb -s ...(注：省略号代表设备名)

9.卸载某设备上的应用
adb -s ... uninstall ...(注：第一个省略号是设备名，第二个省略号是包名)

10.启动某个应用程序，或者某个Activity
adb shell

am start -n .../...(注：第一个省略号是包名，第二个省略号是Activity在某包下的全名)
com.autonavi.amapauto/com.autonavi.auto.remote.fill.UsbFillActivity
11.获取管理员权限
adb root

12.显示系统中全部的AVD
android list avd

13.创建AVD
android create avd --name 名称 --target 平台编号

14.启动模拟器
emulator -avd 名称 -sdcard ~/名称.img (-skin 1280x800)

15.删除模拟器
android delete avd --name 名称

16.列出monkey进程
ps | grep monkey

17.杀死monkey进程
kill pid ...(省略号代表上边列出的monkey进程编号)

18.不同分辨率
adb shell wm size 查询设备分辨率
adb shell wm size [reset | WxH]  设定新的屏幕宽W和宽H，执行reset重置
adb shell wm size 1200x800
adb shell wm density [reset | DENSITY] 设定新的屏幕像素密度

19.关机： adb reboot



