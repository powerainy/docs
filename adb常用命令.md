#### 连接手机
adb connect localhost:5555
#### 列出手机上app的包名
shell pm list packages
#### 找出app的主Activity名
shell dumpsys package com.xxx.xxx | grep -A 1 "MAIN"
#### 启动app
shell am start -n com.xxx.xxx/.ui.MainActivity
#### 卸载app
uninstall com.xxx.xxx
