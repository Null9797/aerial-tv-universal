# Android TV 安装指南

本指南适用于支持 Android TV / Google TV 的电视或电视盒子。菜单因品牌、固件版本和地区而异；不支持 Android APK 的系统无法使用本项目。**如果电视没有明确提供 ADB 调试，不要尝试未知工程码、关闭系统安全服务或修改系统分区。**

## 1. 电视与电脑接入同一网络

在电视的「设置 → 网络」查看当前 IPv4 地址，例如 `192.168.1.25`。电视和电脑需在同一个家庭局域网；不要通过公网暴露电视的 ADB 端口。

## 2. 开启开发者选项和 ADB 调试

### Android TV / Google TV 通用入口

1. 打开「设置 → 系统」或「设备偏好设置 → 关于」。
2. 找到「Android TV OS build」「内部版本号」或「Build」，连续按遥控器确认键约 7 次，直到提示已开启开发者选项。
3. 返回系统设置 →「开发者选项」，开启「USB 调试」或「ADB 调试」。如果电视支持 Android 11+ 的「无线调试」，可以直接用配对码方式连接。
4. 连接电脑后，在电视授权本电脑的调试请求。

Google 的 ADB 文档说明 Android 11 及更新版本支持无线配对；电视厂商可隐藏或禁用该功能：[Android Developers：ADB over Wi‑Fi](https://developer.android.com/tools/adb#connect-to-a-device-over-wi-fi-android-11)。
TCL 通用 Android 设备的开发者选项步骤也见 [TCL 官方说明](https://www.tcl.com/global/en/support/faq/10877)；电视菜单仍可能因型号和软件版本不同而变化。

### 常见品牌路径

| 品牌 | 常见办法 | 说明 |
|---|---|---|
| Google TV / Android TV / Sony BRAVIA | 通用入口：关于 → OS build 连按约 7 次 → 开发者选项 → ADB 调试 | Sony 固件的设置层级和选项名称可能不同。 |
| TCL / 雷鸟 Google TV | 优先使用通用入口。设置 → 系统 → 关于 → Android TV OS build 连按约 7 次。 | 国行旧固件差异大；不要把某机型的隐藏菜单按键序列套用到其他机型。 |
| TCL 部分国行旧机型 | ADB 可能藏在厂商菜单中；若官方用户手册没有说明，请向厂商客服核实。 | 不要更改工程菜单中其他选项，不要执行恢复出厂设置。 |
| 小米 / Redmi Android TV | 关于中找到产品型号或 Android TV OS build，按机型提示开启开发者选项；部分国行机型在「账号与安全」里开启 ADB 调试。 | 入口会因系统版本不同而变化。 |
| Hisense / 海信 | Google TV / Android TV 机型使用通用入口。 | VIDAA 是另一套系统，不能通过 ADB 安装 Android APK。 |
| OPPO / 一加电视 | 关于本机开启开发者选项，再打开 USB/ADB 调试。 | 具体按键触发方式依机型而异。 |
| 海尔 / 模卡、部分创维等 | 以本机用户手册或厂商客服说明为准。 | 某些固件采用工程菜单，不建议盲目尝试网络上的序列。 |
| Amazon Fire TV / Fire TV Stick | Settings → My Fire TV / Device → Developer Options → ADB Debugging。IP 通常在 About → Network 查看。 | Amazon 官方提供 [ADB 连接说明](https://developer.amazon.com/docs/fire-tv/connecting-adb-to-device.html) 和 [APK 安装说明](https://developer.amazon.com/docs/fire-tv/installing-and-running-your-app.html)。本 APK 虽为 Android 应用，但桌面和屏保兼容性未经 Fire TV 验证。 |
| Samsung Tizen / LG webOS | 不支持本 APK 的 Android ADB 安装流程。 | 系统并非 Android TV。 |

该清单不是所有型号的完整列表。菜单不存在时，可能是厂商没有向该机型开放 ADB。

## 3. 安装 Android Platform Tools

下载 Google 官方的 [Android SDK Platform Tools](https://developer.android.com/tools/releases/platform-tools)，解压到电脑。打开终端或 PowerShell，切换到其中 `platform-tools` 目录，然后确认：

```shell
adb version
```

macOS 可将 `platform-tools` 目录加入 PATH，或在该目录运行 `./adb`。Windows PowerShell 在当前目录运行 `.\adb.exe`。下文假设命令已在 PATH 中。

## 4. 连接电视

### Android 11 及更新版本：无线调试配对

在电视开发者选项打开「无线调试」，选择「使用配对码配对设备」。电视会显示 IP、配对端口、一次性配对码；关闭配对弹窗前同时记下无线调试页显示的连接端口。

```shell
adb pair 192.168.1.25:37123
# 按提示输入电视显示的一次性配对码
adb connect 192.168.1.25:41267
adb devices -l
```

上面的 IP 和端口只是示例。必须使用电视当前屏幕显示的值；配对端口与连接端口可能不同，也可能在重启或断开网络后改变。

### 旧版 Android TV 或传统网络 ADB

某些电视开启 ADB 调试后会在本地网络开放 TCP 5555：

```shell
adb connect 192.168.1.25:5555
adb devices -l
```

如果电视屏幕显示了其它端口，使用屏幕上的端口。不要把 `5555` 当作所有电视的固定端口。

### 首次授权

`adb devices -l` 应显示电视序列号和 `device`。如果显示 `unauthorized`，看电视屏幕上的 RSA 授权对话框，确认这是自己的电脑后再允许。若没有弹窗，可在开发者选项中撤销 USB 调试授权后重新连接。

如果电脑同时连接了多台设备，后续命令加 `-s <设备序列号>`，避免把 APK 装到错误设备。

## 5. 安装 APK

先从 GitHub Releases 下载一个 APK。Standard 和 Guard 同包名，只安装一个。

macOS / Linux：

```shell
adb install -r "$HOME/Downloads/aerial-universal-1.49-standard.apk"
```

Windows PowerShell：

```powershell
adb install -r "$env:USERPROFILE\Downloads\aerial-universal-1.49-standard.apk"
```

选择 Guard 时，把文件名换成 `aerial-universal-1.49-guard.apk`。看到 `Success` 即表示 Android 安装器接受了安装。随后在电视的应用列表打开 Aerial。

可选：用 ADB 打开已安装的应用：

```shell
adb shell monkey -p app.orchard.tv 1
```

## 6. 选择桌面和屏保

- **Standard**：按 Home 键，在 Android 的桌面选择器中选 Aerial；确认要作为长期桌面后，再选择“始终”。也可从系统默认应用设置里更改主屏幕应用。
- **Guard**：此构建不注册常规 HOME 桌面入口。打开 Aerial，按其设置页提示配置守护桌面。某些设备会要求无障碍服务、使用情况访问或其他系统授权；只有理解授权用途并确认要使用守护功能后再逐项启用。
- 在 Aerial 设置里选择翻页时钟等屏保，并设定空闲进入时间。屏保后待机是否生效由电视固件决定。

## 7. 更新、变体切换与卸载

- 安装新版本时使用 `adb install -r <APK路径>`，这会尝试保留应用数据。
- Standard 与 Guard 共用包名，安装另一个变体会替换当前变体，不能并行使用。
- 卸载前先在电视中切回原桌面。随后可在「设置 → 应用 → Aerial → 卸载」操作；也可运行 `adb uninstall app.orchard.tv`。该命令只卸载 Aerial，不要将包名替换成系统应用。
- 安装完成后可在开发者选项关闭 ADB 调试；应用运行不要求电脑一直连接。

## 8. 让 AI 助手代为安装

请使用能够在本机执行命令的 AI 编程助手（例如 Codex、Claude Code 或其他本地代理）。纯网页聊天无法直接访问你的电脑或电视。将电视 IP、APK 文件路径和以下提示词交给助手：

```text
请通过我电脑上的 Android Platform Tools 用 ADB 安装这个 Aerial APK。
电视 IP：<电视 IP>
APK 文件路径：<APK 完整路径>

先运行 adb version 并确认 APK 文件存在。连接方式请按电视的开发者选项判断：
如果电视显示无线调试配对码，先 adb pair <IP:配对端口>，然后 adb connect <IP:连接端口>；
如果电视只开放传统网络 ADB，再尝试 adb connect <IP:5555>。
运行 adb devices -l 确认目标状态为 device 后，再运行 adb install -r "<APK路径>"。
若电视要求授权、配对码或安装确认，请暂停，让我在电视上亲自确认。
安装后告诉我如何打开 Aerial 和选择默认桌面。不要卸载其他应用、关闭系统验证、
停用系统安装器、改写系统设置或授予持续权限；这些步骤只有在我另行明确要求后才执行。
```

## 故障排查

| 现象 | 建议处理 |
|---|---|
| `failed to connect` | 核对 IP、端口、同一局域网和调试开关。Android 11+ 优先使用当前无线调试页显示的端口并重新配对。 |
| `unauthorized` | 在电视上确认 RSA 指纹授权；不要绕过授权。 |
| `no devices/emulators found` | 先检查 `adb devices -l`，并重新连接；多设备时用 `-s` 指定电视。 |
| `INSTALL_FAILED_USER_RESTRICTED` | 检查电视安装确认框和厂商“通过 ADB 安装”策略。本项目不建议关闭系统验证或停用系统安装器。 |
| `INSTALL_FAILED_VERSION_DOWNGRADE` | 已安装版本可能比此包版本新。先确认包来源和设置备份；不要盲目卸载。 |
| `INSTALL_FAILED_UPDATE_INCOMPATIBLE` | 现有应用的签名与此 APK 不同。先联系发布者确认，不要为了覆盖而清除数据或卸载。 |
| 安装成功但 Home 键仍回原桌面 | Standard 需要在系统里选为默认桌面；Guard 需完成应用内守护设置。厂商限制可能导致无法接管。 |
