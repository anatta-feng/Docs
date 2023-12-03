安卓设备版本在4.0以上，而且打开了View Server（这不一定根据手机，有的手机不需要打开就能通过Hierarchy Viewer可以看到app的UI结构），如果不能看到，执行打开命令：


adb shell service call window 1 i32 4939 
然后通过执行如下命令判断是否开启View Server： 
adb shell service call window 3 
若返回值是：Result: Parcel(00000000 00000001 '........') 说明View Server处于开启状态 
若返回值是：Result: Parcel(00000000 00000000 '........') 说明View Server处于关闭状态 
如果想关闭View Server执行如下命令： 
adb shell service call window 2 i32 4939
如果能成功通过uiautomator查看app的页面元素，说明你可以进行如下命令：


adb shell uiautomator dump --compressed  /data/local/tmp/uidump.xml
这个命令是把设备UI信息存入在设备文件uidump.xml：

/data/local/tmp/uidump.xml

通过命令，把设备上的命令拉取到PC桌面上，下面是问我执行的命令实例：

adb pull "/data/local/tmp/uidump.xml "    "C:\Users\e.wang\Desktop"


获取的设备UI信息文件uidump.xml中的信息内容如下：

```xml
  <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 

<hierarchy rotation="0">

<node index="0" text="" resource-id="" class="android.widget.FrameLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[0,0][720,1280]">

<node index="0" text="" resource-id="com.meizu.flyme.launcher:id/workspace" class="android.view.View" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="true" long-clickable="true" password="false" selected="false" bounds="[0,0][720,1280]">

<node index="13" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[32,64][196,256]">
<node index="0" text="钱包" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[44,194][184,256]" /> 
</node>

<node index="14" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[196,64][360,256]">
<node index="0" text="国际漫游设置" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[208,194][348,256]" /> 
</node>

<node index="15" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[360,64][524,256]">
<node index="0" text="KingRoot" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[372,194][512,256]" /> 
</node>

<node index="16" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[524,64][688,256]">
<node index="0" text="BusyBox Pro" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[536,194][676,256]" /> 
</node>

<node index="17" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[32,256][196,448]">
<node index="0" text="喜马拉雅FM" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[44,386][184,448]" /> 
</node>

<node index="18" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[196,256][360,448]">
<node index="0" text="Bookmarks" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[208,386][348,448]" /> 
</node>

<node index="19" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[360,256][524,448]">
<node index="0" text="AirDroid" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[372,386][512,448]" /> 
</node>

<node index="20" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[524,256][688,448]">
<node index="0" text="网易云音乐" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[536,386][676,448]" /> 
</node>

<node index="21" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[32,448][196,640]">
<node index="0" text="追书神器" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[44,578][184,640]" /> 
</node>

<node index="22" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[196,448][360,640]">
<node index="0" text="Packet Capture" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[208,578][348,640]" /> 
</node>
<node index="23" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[360,448][524,640]">
<node index="0" text="计步器" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[372,578][512,640]" /> 
</node>
<node index="24" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[524,448][688,640]">
<node index="0" text="小密圈" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[536,578][676,640]" /> 
</node>
<node index="25" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[32,640][196,832]">
<node index="0" text="微信" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[44,770][184,832]" /> 
</node>
<node index="26" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[196,640][360,832]">
<node index="0" text="QQ浏览器" resource-id="com.meizu.flyme.launcher:id/app_name" class="android.widget.TextView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[208,770][348,832]" /> 
</node>
</node>

- <node index="2" text="" resource-id="com.meizu.flyme.launcher:id/icon_page_indicator" class="android.widget.HorizontalScrollView" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[308,1030][412,1078]">
  <node NAF="true" index="0" text="" resource-id="" class="android.widget.FrameLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="false" bounds="[308,1030][360,1078]" /> 
  <node NAF="true" index="1" text="" resource-id="" class="android.widget.FrameLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="false" password="false" selected="true" bounds="[360,1030][412,1078]" /> 
  </node>
  

<node index="3" text="" resource-id="com.meizu.flyme.launcher:id/hotseat" class="android.widget.FrameLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[0,1084][720,1280]">
  <node NAF="true" index="0" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[48,1084][198,1226]" /> 
  <node NAF="true" index="1" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[202,1084][358,1226]" /> 
  <node NAF="true" index="2" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[362,1084][518,1226]" /> 
  <node NAF="true" index="3" text="" resource-id="" class="android.widget.LinearLayout" package="com.meizu.flyme.launcher" content-desc="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" scrollable="false" long-clickable="true" password="false" selected="false" bounds="[522,1084][678,1226]" /> 
  </node>
  </node>
  </hierarchy>
```