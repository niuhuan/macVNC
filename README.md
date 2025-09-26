macVNC
======

我修改了原版macVNC，只有在接入控制时录屏。

### 1. 先安装依赖库

可以自行github搜索该仓库手动make install

```shell
brew install libvncserver
```

(https://github.com/LibVNC/libvncserver.git)

### 2. 克隆本仓库 dev-mine 分支

```shell
git clone -b dev-mine https://github.com/niuhuan/macVNC.git
cd macVNC
mkdir build
cmake ..
make
```

### 3. 将取出二进制文件，并配置权限

```shell
cp macVNC.app/Contents/MacOS/macVNC /Library/
```

并且在设置中增加“控制当前电脑”、“录屏和录音”的权限

### 4. 配置服务

`~/Library/LaunchAgents/macVNC.plist`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>macVNC</string>
    <key>ProgramArguments</key>
    <array>
    <string>/Library/macVNC</string>
    <string>-rfbportv6</string>
    <string>5903</string>
    <string>-rfbport</string>
    <string>5903</string>
    <string>-listen</string>
    <string>0.0.0.0</string>
    <string>-passwd</string>
    <string>password</string>
    </array>
    <key>QueueDirectories</key>
    <array>
    <string>/Library</string>
    </array>
    <key>KeepAlive</key>
    <true/>
    <key>EnableGlobbing</key>
    <true/>
    <key>StandardOutPath</key>
    <string>/tmp/macVNC.std.log</string>
    <key>StandardErrorPath</key>
    <string>/tmp/macVNC.err.log</string>
</dict>
</plist>
```

### 5. 启动服务

```shell
# 加载
launchctl load -w ~/Library/LaunchAgents/macVNC.plist
# 查看状态
launchctl list | grep VNC
launchctl print gui/$(id -u)/macVNC
```
