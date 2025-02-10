#### 连接手机
```bash
adb connect localhost:5555
```
#### 列出手机上app的包名
```bash
shell pm list packages
```
#### 找出app的主Activity名
```bash
shell dumpsys package com.xxx.xxx | grep -A 1 "MAIN"
```
#### 启动app
```bash
shell am start -n com.xxx.xxx/.ui.MainActivity
```
#### 卸载app
```bash
uninstall com.xxx.xxx
```
