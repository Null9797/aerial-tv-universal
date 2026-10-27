# Halo TV 1.58（Standard + Guard）

**Halo TV 是基于 [pengchujin/aerial-tv](https://github.com/pengchujin/aerial-tv) 二次开发的非官方版本，不是 Aerial 原作或原作者发行版。** 上游 MIT 许可和版权声明保留在本仓库。上游作者未参与、未背书本版本。

## 下载

每台电视只安装一个变体；Standard 与 Guard 使用相同包名，不能并存。APK 文件仍使用 `aerial-universal-` 前缀，以便已发布的 1.57 更新器找到匹配变体。

- `aerial-universal-1.58-standard.apk` — Android TV / Google TV 常规桌面入口。
- `aerial-universal-1.58-guard.apk` — 仅供固件拦截桌面类应用时使用，需要按应用内指引完成 Guard 设置。

## 1.58 更新

- 应用名称、启动图标、Android TV 横幅、控制中心版本标识和手机设置页标题统一为 Halo TV。
- 更新页面增加醒目的来源说明：Halo TV 基于 Aerial 开源项目二次开发，非官方版本。
- 保持包名 `app.orchard.tv`、原签名身份和更新器可识别的 APK 命名前缀，以支持 1.57 覆盖更新并保留设置与数据。
- 继续包含自适应电视桌面、五应用 Dock、14 款内置电视图标、天气日期、壁纸和视频壁纸、翻页时钟屏保及刷新率选项。
- 两个 APK 的版本号相同，`versionCode=81`；新包不会静默安装，更新仍须在电视系统安装器中确认。

其他适配、功能及已知限制见 [README](README.md)、[安装指南](INSTALL.md) 和 [FAQ](FAQ.md)。

## 兼容性说明

TCL Q10G Pro 是当前实测设备。其他 Android TV / Google TV 机型尚未逐台回归；布局、安全区域、屏保调度、刷新率与待机行为可能因固件不同。非 Android TV 系统（如 Tizen、webOS、VIDAA）不能安装 Android APK。
