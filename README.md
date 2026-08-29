# Creality K2 Fluidd Dashboard

> 面向创想三维 Creality K2 / Fluidd / Klipper 的单文件本地 Web 控制台。  
> 集成原厂 WebRTC 摄像头、打印状态、温控、风扇、LED、打印速度、CFS/耗材管理、XYZ 控制、本地文件、历史记录与延时摄影。


## 项目简介

`Creality K2 Fluidd Dashboard` 是一个直接部署到 K2 打印机 Fluidd Web 目录中的单文件控制页面。

项目最初用于恢复 / 增强 K2 原厂摄像头在局域网中的访问能力，随后扩展为一个完整的设备控制台。页面无需额外 Web 服务、npm、Node.js、Python 包或数据库，部署后由 K2 现有的 nginx / Fluidd 服务直接提供。

默认部署地址：

```text
http://打印机IP:4408/camera.html
```

页面会自动使用当前访问地址中的打印机 IP，无需另外填写主机地址。

---

## 主要功能

### 原厂摄像头

- 使用 K2 原厂 H.264 WebRTC 视频流。
- 支持通过 `9999` WebSocket 获取视频 Token 的新固件。
- 保留旧版 `8000` WebRTC 信令接口作为兼容回退。
- 自动重连。
- 全屏显示。
- 内置调试信息窗口。
- 无需占用 `/dev/video0` / `/dev/video1`。
- 不需要额外安装 go2rtc、crowsnest 或 MJPEG 服务。

### 当前打印状态

实时显示：

- 当前文件名。
- 打印状态。
- 打印进度。
- 当前层 / 总层数。
- 已用时间 / 剩余时间。
- 预计完成时间。
- 喷嘴温度。
- 热床温度。
- 腔体温度。
- 当前打印速度。
- 模型风扇。
- 机箱风扇。
- 辅助风扇。

支持：

- 暂停打印。
- 继续打印。
- 停止打印。

### 打印设置

支持直接控制：

- 喷嘴目标温度。
- 热床目标温度。
- 腔体目标温度。
- LED 灯开关。
- 模型风扇。
- 机箱风扇。
- 辅助风扇。
- 打印速度模式。

打印速度提供：

- 静音。
- 平稳 `50%`。
- 标准 `100%`。
- 极速 `125%`。

### CFS / 耗材设置

![Material Editor](docs/material-editor.png)

支持读取并显示 CFS / 外置料架状态。

点击料槽后可以编辑：

- 品牌。
- 类型。
- 名称。
- 颜色。

其中：

- **品牌 / 类型 / 名称**使用下拉选择。
- **名称**会优先读取打印机 `reqMaterials` 返回的本机耗材配置。
- 本项目同时内置 Creality K2 0.4 mm 的官方默认耗材预设作为补充。
- **喷嘴温度只读**。
- **压力提前（Pressure Advance）只读**。
- 选择不同耗材预设时，喷嘴温度和压力提前会自动跟随预设显示。
- 保存时通过 K2 `modifyMaterial` 更新对应 CFS / 外置料架料槽。

### XYZ 控制

![XYZ Control](docs/xyz-control.png)

支持：

- `1 mm`
- `10 mm`
- `100 mm`

三档移动步进。

支持：

- X+
- X-
- Y+
- Y-
- Z+
- Z-
- XY 归零。
- Z 归零。
- 全部归零 `G28`。
- 实时显示 X / Y / Z 当前位置。

> 移动前请确认打印头、平台、模型和工具之间没有碰撞风险。

### 本地文件

![Local Files](docs/local-files.png)

支持读取本地 G-code 文件，并以双列卡片形式显示：

- 文件名。
- 文件大小。
- 层高。
- 创建时间。
- 耗材长度。
- 文件选择 / 全选。

页面会优先尝试 Moonraker 文件接口；无法读取时会回退到 K2 WebSocket 文件接口。

### 历史记录

支持读取最近打印任务记录。

页面会优先尝试 Moonraker 历史接口；失败时自动回退到 K2 WebSocket `reqHistory`。

### 延时摄影

支持：

- 开启 / 关闭 K2 原厂延时摄影。
- 读取延时摄影文件列表。
- 刷新视频列表。

---

## 内置 K2 官方耗材预设

当前版本内置 16 个 Creality K2 0.4 mm 默认耗材预设。

| 品牌 | 耗材 | 喷嘴温度 | Pressure Advance |
| --- | --- | ---: | ---: |
| Generic | PLA-CF | 190–240 °C | 0.05 |
| Generic | PLA | 190–240 °C | 0.04 |
| Generic | PETG | 220–270 °C | 0.04 |
| Generic | PLA-Silk | 190–240 °C | 0.04 |
| Creality | Hyper PETG | 220–270 °C | 0.05 |
| Creality | CR-Silk | 190–240 °C | 0.04 |
| Creality | ENDER FAST PLA | 190–240 °C | 0.04 |
| Creality | Hyper PLA-CF | 190–240 °C | 0.04 |
| Creality | Hyper PLA | 190–240 °C | 0.04 |
| Creality | Hyper ABS | 230–260 °C | 0.03 |
| Creality | HP-TPU | 200–220 °C | 0.4 |
| Creality | CR-PLA | 190–240 °C | 0.04 |
| Creality | CR-PETG | 220–270 °C | 0.07 |
| Creality | CR-ABS | 230–260 °C | 0.04 |
| Generic | ASA | 240–280 °C | 0.04 |
| Generic | ABS | 240–280 °C | 0.06 |

耗材配置的优先级为：

```text
打印机 reqMaterials 本机配置
        ↓
内置 Creality K2 官方预设
        ↓
当前 CFS / 外置料架数据
```

如果打印机中的同名耗材缺少喷嘴温度或压力提前参数，页面会尝试使用内置官方预设进行补齐。

> 内置预设针对 K2 0.4 mm 配置。不同喷嘴、固件或 Creality Print 版本的预设可能不同，请以实际切片配置为准。

---

## 兼容环境

本项目主要面向使用 Fluidd / Klipper 的 Creality K2 系列固件。

已知目标环境通常包含：

```text
4408  Fluidd / nginx
9999  Creality WebSocket / web-server
8000  webrtc_local
```

可以通过 SSH 检查：

```bash
netstat -tulnp | grep -E '8000|9999|4408'
```

典型输出：

```text
0.0.0.0:4408   LISTEN   nginx
0.0.0.0:8000   LISTEN   webrtc_local
0.0.0.0:9999   LISTEN   web-server
```

摄像头通常还可以看到：

```bash
ls -l /dev/video*
```

例如：

```text
/dev/video0
/dev/video1
```

本项目不会直接抢占这些 V4L2 设备，而是使用打印机原厂 WebRTC 服务。

---

## 使用要求

- Creality K2 系列打印机。
- Fluidd 可以通过 `4408` 端口访问。
- 打印机允许 root / SSH 文件访问。
- 电脑或手机与打印机处于可互访的局域网。
- 浏览器支持 WebRTC 和 H.264。
- 推荐 Chrome / Edge / Firefox / Safari 的较新版本。

页面必须通过打印机 Web 服务访问。

正确：

```text
http://192.168.1.33:4408/camera.html
```

不要直接双击本地 HTML 文件：

```text
file:///C:/camera.html
```

---

# 安装

## 方法一：Windows + WinSCP

这是最推荐的方法，尤其适合 K2 系统没有 `wget` / `curl` 的情况。

### 1. 下载文件

从 GitHub Releases 或仓库下载：

```text
camera.html
```

### 2. 使用 WinSCP 登录打印机

协议：

```text
SCP
```

或打印机当前支持的 SSH/SFTP 方式。

主机填写 K2 IP，例如：

```text
192.168.1.33
```

用户：

```text
root
```

### 3. 备份旧文件

如果已经存在：

```text
/usr/share/fluidd/camera.html
```

建议先在 SSH 终端执行：

```bash
cp /usr/share/fluidd/camera.html /usr/share/fluidd/camera.html.bak
```

### 4. 上传

将项目中的：

```text
camera.html
```

上传到：

```text
/usr/share/fluidd/camera.html
```

### 5. 设置权限

SSH 执行：

```bash
chmod 644 /usr/share/fluidd/camera.html
```

无需重新启动 nginx。

### 6. 打开

浏览器访问：

```text
http://打印机IP:4408/camera.html
```

例如：

```text
http://192.168.1.33:4408/camera.html
```

第一次替换文件后建议：

```text
Ctrl + F5
```

强制刷新浏览器缓存。

---

## 方法二：SCP

Windows PowerShell：

```powershell
$PrinterIp = "192.168.1.33"
scp .\camera.html "root@${PrinterIp}:/usr/share/fluidd/camera.html"
```

Linux / macOS：

```bash
scp ./camera.html root@192.168.1.33:/usr/share/fluidd/camera.html
```

然后：

```bash
ssh root@192.168.1.33
chmod 644 /usr/share/fluidd/camera.html
```

---

# 在 Fluidd 中作为 HTTP Page 使用

如果你的 Fluidd 版本支持 `HTTP Page` 摄像头类型，可以进入：

```text
Settings
→ Cameras
→ Add Camera
```

建议配置：

```text
Name:
K2 Camera

Camera Type / Service:
HTTP Page

URL:
/camera.html
```

然后保存。

也可以不嵌入 Fluidd，直接使用完整控制台：

```text
http://打印机IP:4408/camera.html
```

由于当前版本已经扩展为完整 Dashboard，直接单独打开页面通常拥有更好的显示空间。

---

# 页面使用说明

## 摄像头

页面加载后会自动连接。

顶部按钮：

- `↻`：重新连接摄像头。
- `⛶`：全屏。
- `⌁`：打开调试信息。

连接正常时调试信息通常类似：

```text
正在连接 192.168.1.33...
控制通道已连接，正在请求视频令牌...
正在使用令牌保护 WebRTC 协议。
已处理 1 个 WebRTC 候选地址。
正在请求摄像头视频流...
ICE 状态：检查中。
WebRTC 协商完成，正在等待视频画面...
连接状态：连接中。
ICE 状态：已连接。
连接状态：已连接。
实时视频：1280×720。
```

---

## 温度设置

目标温度通过打印机 G-code 控制：

```text
喷嘴   M104
热床   M140
腔体   M141
```

页面会根据打印机返回的最大温度参数限制输入范围。

---

## 风扇设置

风扇使用：

```text
M106 P<风扇编号> S<0-255>
```

页面将 `0–100%` 自动转换为 `0–255`。

---

## 打印速度

标准模式使用：

```text
M220 S50
M220 S100
M220 S125
```

静音模式使用 K2 固件提供的：

```text
Qmode
```

离开静音模式时发送：

```text
Qmode_exit
```

---

## XYZ 移动

移动操作使用相对运动：

```text
G91
G1 ...
G90
```

XY 与 Z 使用不同移动速度。

全部归零：

```text
G28
```

---

# 工作原理

## 摄像头连接

页面打开后：

1. 从 `location.hostname` 获取打印机 IP。
2. 建立：

```text
ws://打印机IP:9999
```

3. 请求视频 Token。
4. 浏览器创建 H.264 WebRTC Offer。
5. 将 Offer 与 Token 发送到打印机 WebRTC 信令接口。
6. 打印机返回 WebRTC Answer。
7. 浏览器直接播放视频轨道。

新固件通常使用 Token 保护接口。

旧版固件会尝试：

```text
http://打印机IP:8000/call/webrtc_local
```

作为兼容路径。

视频本身不是通过 `9999` WebSocket 传输；`9999` 主要用于控制、状态数据和视频授权。

---

## 控制通道

打印状态、CFS 信息、LED、延时摄影、部分文件/历史功能均依赖打印机：

```text
ws://打印机IP:9999
```

项目使用的是 Creality 固件中的本地接口。

这些接口并不是稳定公开的 Web API，因此未来固件可能发生改变。

---

## Moonraker

部分功能会优先尝试标准 Moonraker 接口，例如：

- 本地文件。
- 历史记录。

如果 Moonraker 接口无法提供数据，则自动回退到 K2 WebSocket。

---

# 更新

更新项目只需要重新覆盖：

```text
/usr/share/fluidd/camera.html
```

例如：

```bash
cp /usr/share/fluidd/camera.html /usr/share/fluidd/camera.html.old
```

上传新版本后：

```bash
chmod 644 /usr/share/fluidd/camera.html
```

然后浏览器：

```text
Ctrl + F5
```

无需重启 Klipper、Moonraker 或 nginx。

---

# 卸载

如果之前做了备份：

```bash
rm /usr/share/fluidd/camera.html
mv /usr/share/fluidd/camera.html.bak /usr/share/fluidd/camera.html
```

如果原本没有 `camera.html`：

```bash
rm /usr/share/fluidd/camera.html
```

即可。

---

# 故障排查

## 打开 `/camera.html` 后还是 Fluidd 首页

通常表示 nginx 没有找到该静态文件。

检查：

```bash
ls -lh /usr/share/fluidd/camera.html
```

权限建议：

```text
-rw-r--r--
```

如果权限不正确：

```bash
chmod 644 /usr/share/fluidd/camera.html
```

然后强制刷新浏览器。

---

## 摄像头一直黑屏 / 加载中

检查服务：

```bash
netstat -tulnp | grep -E '8000|9999|4408'
```

确认至少存在：

```text
4408
9999
8000
```

然后打开页面中的：

```text
调试信息
```

重点查看：

- 是否成功连接 `9999`。
- 是否成功取得视频 Token。
- ICE 是否到达 `connected`。
- WebRTC connection state 是否为 `connected`。
- 浏览器是否支持 H.264。

---

## 显示“不支持 H.264”

升级浏览器。

推荐测试：

- Chrome
- Edge
- Firefox
- Safari

---

## 摄像头正常，但控制按钮无效

摄像头与控制通道并不是完全相同的连接。

检查调试信息中是否出现：

```text
控制通道已连接
```

并检查：

```text
9999
```

端口是否正常监听。

---

## LED 状态不正确

LED 使用打印机状态字段：

```text
lightSw
```

以及对应设置命令。

如果固件更改字段定义，可能需要更新页面代码。

---

## CFS / 耗材配置为空

点击：

```text
刷新耗材
```

页面会请求：

```text
boxsInfo
reqMaterials
```

如果你的固件返回结构与当前版本不同，可以打开调试信息并提交 Issue。

提交 Issue 时建议附带脱敏后的 WebSocket 返回结构。

---

## 喷嘴温度 / 压力提前显示 `--`

当前版本会按以下顺序查找参数：

```text
打印机本机耗材配置
→ K2 官方内置预设
→ 当前料槽数据
```

如果仍然显示 `--`，通常表示：

- 当前耗材名称无法匹配任何官方预设。
- 固件返回字段名称发生变化。
- 使用的是自定义耗材。

可以在 Issue 中提供：

- 品牌。
- 类型。
- 名称。
- `reqMaterials` 返回的对应数据。

请不要提交设备序列号、账号 Token 或其他敏感信息。

---

## 本地文件为空

页面优先读取 Moonraker。

如果失败，会尝试 K2：

```text
reqGcodeFile
```

不同固件可能返回不同结构，因此文件元数据完整程度可能存在差异。

---

## 历史记录为空

页面会先读取 Moonraker 历史接口。

失败后使用：

```text
reqHistory
```

---

## 延时摄影列表为空

点击：

```text
刷新视频
```

页面会请求：

```text
reqElapseVideoList
```

如果打印机没有生成过延时摄影视频，列表为空属于正常情况。

---

# 已知限制

- 主要针对 Creality K2 系列测试。
- 不保证其他 Creality 打印机兼容。
- Creality 本地 WebSocket 接口并非稳定公开 API。
- 固件更新后部分字段或命令可能改变。
- 固件升级 / 恢复出厂设置可能删除 `/usr/share/fluidd/camera.html`。
- 内置耗材预设针对 K2 0.4 mm。
- 自定义耗材的温度和 Pressure Advance 取决于打印机返回的数据。
- 本项目不能代替切片软件中的打印参数配置。
- XYZ 手动移动具有机械碰撞风险，请确认打印机状态后操作。

---

# 安全说明

本项目设计用于可信局域网。

**不要直接把以下端口通过路由器端口转发暴露到公网：**

```text
80
4408
8000
9999
```

需要远程访问时，推荐使用可靠配置的 VPN。

启用 K2 root / SSH 权限意味着可以完全控制打印机。请只运行你理解的命令，并做好配置备份。

---

# 项目结构

```text
Creality-K2-Fluidd-Dashboard/
├── camera.html
├── README.md
└── docs/
    ├── dashboard.png
    ├── local-files.png
    ├── material-editor.png
    └── xyz-control.png
```

整个运行功能都包含在：

```text
camera.html
```

中。

`docs/` 只用于 GitHub README 截图，并不是运行所必需。

---

# Credits / 致谢

本项目的摄像头 WebRTC 实现与早期页面工作参考 / 演进自：

- `geckotdf/Creality-K2-Camera-Fix`
- https://github.com/geckotdf/Creality-K2-Camera-Fix

K2 官方耗材预设信息参考：

- `CrealityOfficial/CrealityPrint`
- https://github.com/CrealityOfficial/CrealityPrint

同时感谢：

- Klipper
- Moonraker
- Fluidd
- Creality K2 社区

---

# Licensing / 发布前请注意

这个项目包含或演进自多个来源，发布到 GitHub 前请认真确认上游许可条件。

`CrealityPrint` 官方仓库声明使用 **GNU AGPL v3**。

另外，如果你的代码仍包含从其他仓库直接复制或修改的代码，而该仓库没有明确许可证，则不能仅因为“代码公开可见”就默认拥有重新授权或再分发的权利。

因此：

1. 不建议在未确认所有上游授权前直接给整个项目加 MIT / Apache-2.0 等宽松许可证。
2. 保留清晰的 Credits / 来源说明。
3. 对于没有明确许可证的上游代码，建议先向作者确认再分发授权，或者将相关部分独立重新实现。
4. 如果最终项目构成满足 AGPL 派生作品条件，应遵循对应的 AGPL 义务。

这不是法律意见；正式公开发布前请自行确认适用于代码的许可证要求。

---

# Disclaimer / 免责声明

这是一个非官方社区项目，与 Creality 官方不存在隶属、授权或背书关系。

项目依赖打印机固件中的本地接口，这些接口可能随固件更新发生变化。

使用本项目可能涉及：

- root 权限。
- 温度控制。
- 风扇控制。
- 运动控制。
- 打印任务控制。
- CFS 配置修改。

请在理解相关风险后使用。

**使用本项目造成的打印失败、设备配置变化、机械碰撞或其他损失，由使用者自行承担。**

---

## Issue 建议格式

如果遇到问题，提交 Issue 时建议包含：

```text
打印机型号：
固件版本：
Fluidd 地址端口：
浏览器：
问题功能：
复现步骤：
调试信息：
```

请删除：

- 设备序列号。
- 云账号信息。
- Token。
- Wi-Fi 密码。
- 其他个人敏感信息。

---

## Star

如果这个项目对你的 K2 有帮助，欢迎点一个 Star，也欢迎提交 Issue / Pull Request 改善不同固件版本的兼容性。
