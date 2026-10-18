# PID Studio 普通版下载

**为爱发电，有条件的可以支持一下**

| 支付宝 | 微信支付 |
| :---: | :---: |
| <a href="assets/support/alipay.jpg"><img src="assets/support/alipay.jpg" width="280" alt="支付宝收款码"></a> | <a href="assets/support/wechat.png"><img src="assets/support/wechat.png" width="280" alt="微信收款码"></a> |

制作不易，请大家点个关注。小红书、B站、抖音搜索：**电机的主人**。

本仓库提供普通版软件、配套 MCP 及下载说明，不公开应用实现源码。软件和 MCP 可免费使用，无需开发密码或企业授权。

## Windows 下载

当前版本：**V1.5.8**，Windows x64。

| 下载 | 用途 |
| --- | --- |
| [PID Studio 普通版软件](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.8/PIDStudio_Standard_App_V1.5.8_Windows.zip) | PID 参数调试、实时波形、分析和 AI 波形诊断 |
| [配套 MCP 程序](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.8/PIDStudio_Standard_MCP_V1.5.8_Windows.zip) | 供 Codex 等 MCP 客户端连接普通版，包含连续 AI 调参接口 |
| [SHA256 校验值](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.8/SHA256SUMS.txt) | 核对下载文件完整性 |

全部版本见 [发布页](https://github.com/YANG985-CMD/PID-Studio-Standard/releases)。

## V1.5.8 更新

- 打开串口成功后自动开始采集，关闭时停止并保存实验。
- 参数和 AI 权限统一设置，支持 P / PI / PID 及逐项选择读取参数；读取值原样显示。
- 新增蓝色“一键配置”：先打开串口，再读取勾选参数，自动同步参数、诊断权限和 MCP 权限页中的可计算项。
- 默认调节步长按初始值绝对值的 1% 对齐设备精度；滑条范围为初始值 ± 步长 × 100，AI 单次变化上限为步长 × 10，范围保留设备及人工限制。
- 诊断目标提供七个常用选项，也可自定义。点击“诊断选定波形”发送本次选定摘要与参数，诊断报告精简显示。
- 配套 MCP 更新为 **29 个工具 / 5 个资源**，支持人工确认范围后的连续 AI 调参。

## 使用

1. 解压软件包，打开 `PIDStudioStandard/PIDStudioStandard.exe`。
2. 在“串口设置”和“通信协议”中配置设备，点击“打开串口”即可采集波形。接收格式提供原始接收、Lxy 和 Yzx。
3. 打开“PID 参数设置”，在参数页勾选需要读取的 P、I、D、目标值，并核对板端地址、类型、范围和最小有效步长。PI 设备可取消 D，无需导入 JSON。
4. 在“一键配置”页点击“确定配置”，读取成功后同步后面三个设置页。空白项自行填写。
5. AI 波形诊断先配置自己的 API，在“波形诊断权限”页保存目标、建议权限和参数限制，再选择波形区间并点击诊断。API Key 存入 Windows 凭据存储。

普通版提供“参数调试 / 分析 / AI 诊断”，连续调参由外部 MCP 客户端在你确认的范围内执行。普通版不包含企业版自动优化算法和电机特调模块。

## 配套 MCP

只使用软件时无需下载 MCP。需要 MCP 时，将两个压缩包解压到**同一个父文件夹**，使 `PIDStudioStandard` 和 `PIDStudioStandardMCP` 并列。

MCP 程序为 `PIDStudioStandardMCP/PIDStudioStandardMCP.exe`。启动参数中的 `--app` 指向软件完整路径，`--data-dir` 指向普通版的 `user_data_standard` 目录。软件与 MCP 使用同一版本；更新后重新加载客户端 MCP。详细注册和工具清单见 MCP 包内的 `docs/CODEX_CONTROL.md`。

连续 AI 调参前，在红色“AI 权限设置”的 MCP 页填写目标值安全范围，选择允许调整的参数、观察窗口、轮数和时限，再从主页面启动并确认本次会话。之后 AI 可逐轮观察、下发和独立读回，无需逐轮确认；设备须明确支持运行中修改参数，软件遵守板端限制。

实验数据保存在软件旁的 `user_data_standard`，更新时保留此目录。发布包不包含用户实验、API Key 或企业授权。

## 使用许可

免费使用的授权范围见 [LICENSE.txt](LICENSE.txt)。未经作者书面许可，不得销售软件副本、换标收费分发或商业再分发本项目原创应用实现的衍生版本。第三方组件仍受各自许可证约束；协议接入示例可用于使用者自己的设备工程。
