# PID Studio 普通版下载

制作不易，请大家点个关注。小红书、B站、抖音搜索：**电机的主人**。

本仓库仅提供打包后的普通版软件、配套 MCP 及下载说明，不公开应用实现源码。普通版和配套 MCP 可免费使用，无需开发密码或企业授权。

## Windows 下载

当前版本：**V1.5.4**，Windows x64。

| 下载 | 用途 |
| --- | --- |
| [PID Studio 普通版软件](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.4/PIDStudio_Standard_App_V1.5.4_Windows.zip) | 手动 PID 调参、实时波形、分析和手动 AI 诊断 |
| [配套 MCP 程序](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.4/PIDStudio_Standard_MCP_V1.5.4_Windows.zip) | 供 Codex 等 MCP 客户端连接普通版使用 |
| [SHA256 校验值](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.4/SHA256SUMS.txt) | 核对下载文件完整性 |

全部下载见 [最新发布页](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/latest)。

## V1.5.4 更新

七种接收格式均提供内嵌、可复制的 C 示例与配置对照。Lxy 推荐用于文本调试，Yzx 推荐用于高速多通道波形。MCP 新增 `list_protocol_examples`、`get_protocol_example` 和 `pidstudio://protocols`，可读取同一份 C 示例及配套文件；普通版为 25 工具、5 资源。更新后重新加载 MCP。

## 使用

解压软件包，打开 `PIDStudioStandard/PIDStudioStandard.exe`。在“设备与协议”配置串口、接收格式、通道和参数，即可按设备实际情况连接使用。普通版提供“参数调试 / 分析 / AI 诊断”，不包含自动调参或电机特调模块。

只使用软件时无需下载 MCP。需要 MCP 时，将两个压缩包解压到**同一个父文件夹**，使 `PIDStudioStandard` 和 `PIDStudioStandardMCP` 并列。MCP 程序为 `PIDStudioStandardMCP/PIDStudioStandardMCP.exe`；启动参数中的 `--app` 指向软件完整路径，`--data-dir` 指向普通版的 `user_data_standard` 目录。详细说明随 MCP 包提供。

实验数据保存在软件旁的 `user_data_standard`，更新时保留此目录。AI 诊断需填写使用者自己的 API Key，并在界面同意发送所选区间摘要；发布包不包含 API Key、用户实验或企业授权。

## 使用许可

免费使用的授权范围见 [LICENSE.txt](LICENSE.txt)。未经作者书面许可，不得销售软件副本、换标收费分发或商业再分发本项目原创应用实现的衍生版本。第三方组件仍受各自许可证约束；协议接入示例可用于使用者自己的设备工程。
