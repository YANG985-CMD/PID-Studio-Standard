# PID Studio｜软件与 MCP 使用说明

**为爱发电，有条件的可以支持一下回回血，感谢**

| 支付宝 | 微信支付 |
| :---: | :---: |
| <a href="assets/support/alipay.jpg"><img src="assets/support/alipay.jpg" width="280" alt="支付宝收款码"></a> | <a href="assets/support/wechat.png"><img src="assets/support/wechat.png" width="280" alt="微信收款码"></a> |

小红书、B站、抖音搜索：**电机的主人**。

本仓库提供软件、配套 MCP 及下载说明，不公开应用实现源码。软件和 MCP 可免费使用，无需开发密码或企业授权。

**给 AI 阅读：[完整 Markdown 原文](https://raw.githubusercontent.com/YANG985-CMD/PID-Studio-Standard/main/README.md)。** 本文包含安装配置、软件操作、全部 MCP 工具和 AI 调参流程；可将此链接直接交给支持联网阅读的 AI。实际控制本机软件，还需要在该 AI 客户端连接下面介绍的 MCP。

## 软件和 MCP 分别做什么

PID Studio 是 Windows 串口波形与 PID 参数调试软件，适用于已实现配套通信协议的设备。它显示实时波形、记录实验、读取参数，并在用户确认后下发参数。电脑端选择 P / PI / PID 只改变软件使用哪些参数，不改变设备固件中的控制算法。

配套 MCP 是软件与 AI 客户端之间的连接程序。AI 通过结构化工具读取设备定义、参数、波形和实验记录，提交设备配置供用户审阅、连接已保存串口，进行诊断、提出参数建议，或在用户授权的连续会话内逐轮调参。MCP 程序已打包，使用时无需安装 Python。

| 使用方式 | 谁进行分析 | 需要什么 |
| --- | --- | --- |
| 手动调参、查看波形、记录实验 | 用户操作软件 | 只需软件，设备实现对应协议 |
| 软件内“诊断选定波形” | 软件调用用户配置的 AI API | 在软件的“API 设置”中配置服务和模型 |
| 外部 AI 通过 MCP 查看、诊断或连续调参 | 当前 MCP 客户端里的 AI | 软件、配套 MCP、支持 MCP 的 AI 客户端；无需在软件里额外配置模型 API |

软件提供原始接收、Lxy 文本协议和 Yzx 批量二进制协议。**接收到波形不等于设备支持参数读写**：参数 GET / SET、地址、数值类型和应答必须与设备实现一致。原始接收主要用于通信检查；Lxy / Yzx 接入示例可通过 MCP 或软件的“通信协议”窗口查看。

软件提供“参数调试 / AI 诊断”。连续调参由外部 AI 在用户确认的范围内决定每轮参数。

## Windows 下载

当前版本：**V1.5.12**，Windows x64。

| 下载 | 用途 |
| --- | --- |
| [PID Studio ](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.12/PIDStudio_Standard_App_V1.5.12_Windows.zip) | PID 参数调试、实时波形和 AI 波形诊断 |
| [配套 MCP 程序](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.12/PIDStudio_Standard_MCP_V1.5.12_Windows.zip) | 供 Codex 等 MCP 客户端连接 PID Studio，包含连续 AI 调参接口 |
| [SHA256 校验值](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.5.12/SHA256SUMS.txt) | 核对下载文件完整性 |

全部版本见 [发布页](https://github.com/YANG985-CMD/PID-Studio-Standard/releases)。

## V1.5.12 更新

- 新增 21 个 MCP 工具，覆盖串口扫描与连接错误日志、当前页一键读取配置、波形显示控制、PID 备份管理、离线 CSV / 实验 / 设备管理，以及软件内 AI 连接设置、测试和诊断。现在共 54 个工具、5 个资源。
- API 密钥由用户在软件密码框输入，只存系统凭据库；不要把密钥交给聊天或工具参数。备份恢复只暂存候选；设备删除、一键配置和 AI 连接改动先审阅确认。

- 新增完整配置读取、配置校验、人工审阅保存配置与连接已保存设备 4 个 MCP 工具。Codex 可提交通道关联、设备能力、安全范围和两套 AI 权限，确认保存后继续连接、采集和读取参数；配置保存不授予硬件写入或连续会话。

- 设置窗口保留“参数设置 / 波形诊断权限 / MCP 连续调参权限”三个页面，各页底部金色按钮**只读取并配置当前页**，保留另一用途设置和人工限制。
- 统一使用“当前参数值”，指设备此刻的 P、I、D（按控制类型读取）及目标值；未读到的项需自行填写。控制类型位于页签左侧，带下拉箭头，并与本页增益勾选双向同步；目标值可独立选择。
- 参数调试分为“滑动调参 / 输入调参”，共用读取与 PID 备份；“写入选中参数”仅在输入页显示，MCP 单次候选也在此审阅。
- “选择分析区间”移到诊断按钮左侧，未开启浅蓝、开启深蓝。拖图只停止自动跟随，数据继续刷新；“暂停视图”才冻结显示，“自动缩放”恢复跟随。
- 波形诊断可在选区内定位目标阶跃；明确通道关联保留，通道身份有歧义时需核对。历史回放可对当前连接设备审阅和确认参数，历史数据与实时采集分开。
- “控制与安全”位于 AI 诊断的“测试连接”右侧。右键删除已保存实验无需二次确认，正在采集的实验保留。
- 软件与配套 MCP 同步为 **V1.5.12**，提供 **54 个工具 / 5 个资源**；工具说明、能力信息和随包指南同步当前操作。

## 软件快速上手

1. 解压软件包，打开 `PIDStudioStandard/PIDStudioStandard.exe`。
2. 在“串口设置”“通道设置”和“通信协议”中配置端口、波特率、通道接收位置与设备协议。通道标识、换算系数和单位须与实际数据一致。
3. 在 AI 诊断页“测试连接”右侧的“控制与安全”里核对当前任务的目标值、实际值、控制输出通道对应关系及保护阈值。例如 SP / PV / OUT 只是常用标识，实际以自己的配置为准。
4. 点击“打开串口”，连接成功后软件自动采集；关闭串口时停止采集并保存实验。
5. 打开“PID 参数设置”，核对板端地址、数值类型、设备范围与步长。默认地址 P=1、I=2、D=3、目标值=4 仅在设备按同一约定实现时适用，不能当成任意板子的真实地址。
6. 三个设置页底部都有“一键读取当前参数值并配置上方参数”。参数页点击后选择 P / PI / PID，再读取并配置本页；两个 AI 权限页按已保存模式读取，仅配置本用途的范围与单次上限。读取不到的项自行设置，已有人工限制保留；AI 权限勾选和目标值安全上下限仍需填写、保存。
7. 在“滑动调参”里修改步长或用滑块、箭头调节；步长与参数设置同步，实时下发须先确认本次会话。在“输入调参”里编辑待写入值，点击“写入选中参数”并确认。两页共用“读取设备当前参数值”“备份PID参数”“恢复备份PID参数”“导出PID参数”；表格中的数字不代表已写入。
8. 使用软件内 AI 波形诊断时，先配置自己的 API，在“波形诊断权限”页保存目标、建议权限和参数限制，再点击诊断按钮左侧的“选择分析区间”，框选包含阶跃和响应的波形并点击“诊断选定波形”。API Key 存入 Windows 凭据存储。

当前参数值指**设备此刻的 P、I、D（按所选控制类型读取）及目标值**。参数页的一键读取使用本次成功读值计算默认步长：当前参数值绝对值的 1%，小数默认按 0.000001 或已有更细刻度取整，零值退回该步长；之后仍可手动修改。调节范围以本轮读取的当前参数值为基准，按基准 ± 步长 × 100 计算，并与设备上下限取交集。诊断与 MCP 的单次变化上限默认各为步长 × 10，可分别修改。

读取失败时保留旧数值和旧步长，不能把表格中的旧数字当成本次已读成功；**读回的 0 也可能是有效数值**。是否成功要看读取状态。读取数值不按写入范围或步长裁剪，实际写入仍检查范围、刻度、权限和单次变化上限。

## 安装并连接 MCP

只使用软件时无需下载 MCP。需要 MCP 时，将两个压缩包解压到**同一个父文件夹**，使 `PIDStudioStandard` 和 `PIDStudioStandardMCP` 并列。

例如解压到 `D:/PIDStudio/` 后，目录应为：

```text
D:/PIDStudio/
├─ PIDStudioStandard/
│  ├─ PIDStudioStandard.exe
│  ├─ _internal/
│  └─ user_data_standard/       首次运行后生成，更新时保留
└─ PIDStudioStandardMCP/
   ├─ PIDStudioStandardMCP.exe
   └─ _internal/
```

不要只复制两个 exe，须保留各自整个文件夹。MCP 使用 **STDIO**，由 AI 客户端启动，无需自行填写 HTTP 地址。`--app` 指向软件 exe，`--data-dir` 指向软件实际使用的实验目录；两个程序须为同一版本。

### Codex 配置示例

在 Codex 的 MCP 设置中配置，或将以下内容合并到自己的 `config.toml`。**先把示例中的 `D:/PIDStudio` 换成实际解压路径，保留已有的其他设置。** 格式参考 [官方 MCP 配置说明](https://learn.chatgpt.com/docs/extend/mcp?surface=cli)。

```toml
[mcp_servers.pid_studio_standard]
command = "D:/PIDStudio/PIDStudioStandardMCP/PIDStudioStandardMCP.exe"
args = ["--app", "D:/PIDStudio/PIDStudioStandard/PIDStudioStandard.exe", "--data-dir", "D:/PIDStudio/PIDStudioStandard/user_data_standard"]
```

其他采用 `mcpServers` JSON 配置结构的客户端可参考：

```json
{
  "mcpServers": {
    "pid_studio_standard": {
      "command": "D:/PIDStudio/PIDStudioStandardMCP/PIDStudioStandardMCP.exe",
      "args": [
        "--app", "D:/PIDStudio/PIDStudioStandard/PIDStudioStandard.exe",
        "--data-dir", "D:/PIDStudio/PIDStudioStandard/user_data_standard"
      ]
    }
  }
}
```

保存后重新加载 MCP 或重启 AI 客户端。V1.5.12 正常应显示 **54 个工具、5 个资源**。仅修改配置文件不会替换当前会话里已经启动的旧 MCP 进程，须以实际返回的版本为准。

实验数据保存在软件旁的 `user_data_standard`，更新时保留此目录。发布包不包含用户实验、API Key 或企业授权。

## 给 AI 助手的操作说明

以下是 MCP 工具名，实际名称前缀以客户端展示为准。请使用已连接的工具，按用户当前请求操作；阅读本文本身不代表用户授权参数写入。

### 第一次连接：固定顺序

```text
open_studio({})
→ get_studio_status({})
→ get_device_definition({})
→ get_studio_capabilities({})
```

上述为调用顺序示意，`{}` 表示该工具不需要输入参数，不是需要发送到串口的命令。

1. `open_studio` 打开或连接软件，**不会打开串口**。需要连接时，先读取并核对完整配置，再用 `connect_device` 打开已保存串口。需要改配置或权限时按下一节提交人工审阅；也可用 `read_and_configure_parameters` 指定页面，在软件确认后执行一键读取配置。
2. 从 `get_studio_status` 核对 `connected`、`acquiring`、`replaying`、`jobs`，区分实时采集和历史回放。不要在已有任务仍运行时重复提交同一操作。
3. 从 `get_device_definition` 读取实际参数 ID、`task.algorithm`、`task.pid_parameters`、`task.parameter_reads`、`task.roles`、协议字段、读取状态和两套 AI 限制。P / I / D / 目标值是角色，**不保证真实 ID 就叫 Kp / Ki / Kd / setpoint**；也不能把参数地址当成参数 ID。
4. 从 `get_studio_capabilities` 核对 `app_version`、`companion_version`、`versions_match`、产品标识和能力。版本不匹配时先同步更新两个程序并重新加载 MCP。

`get_studio_capabilities` 还会返回 `parameter_setup_scope="current_page"`、`manual_parameter_modes=["slider","input"]` 和当前读取按钮名称，便于 AI 对照界面指导操作。

`task.roles` 中的目标值、实际值、控制输出若未关联，或协议缩放 / 单位明显不符，请先让用户核对配置。设备上下限、AI 允许范围、步长、单次变化限制及目标安全范围以当前返回结果为准，不照搬示例。

### 通过 MCP 配置和连接设备

1. 调用 `get_device_configuration`，取 `result.configuration` 的完整配置副本及 `result.configuration_revision`。它包含串口、全部通道与参数、控制关联、设备能力、安全限制，以及诊断和连续调参两套 AI 权限，不返回密钥。
2. 依据实际固件和用户请求修改副本，保留其余字段。将完整副本和读取时的版本传给 `validate_device_configuration(configuration, expected_revision)`，先检查全部变更。通道位置、参数地址、量程和运行中改参能力须有真实依据。
3. 断开设备、结束采集 / 回放 / 后台任务并关闭其他设置窗口后，调用 `configure_device(configuration, expected_revision, reason)`。软件弹出原值与拟保存值对照，返回 `needs_confirmation` 和操作 `id`。**请用户在软件点击“确认保存配置”，AI 不替用户确认。**
4. 调用 `get_operation(request_id=id)` 查询原操作；只有 `state=completed` 且 `result.saved=true` 表示保存成功。取消或失败应报告实际原因。若无变更，返回 `unchanged=true, saved=false`；配置版本过期须重新读取后合并，不重复提交旧草稿。
5. 保存后重新调用 `get_device_configuration`，用新的版本与当前设备 ID 调用 `connect_device(device_id, expected_revision)`。此工具只打开已保存串口；`started=true` 表示连接开始，用 `get_studio_status` 确认连接与采集结果，再用 `read_parameters` 读实际参数。

配置保存不写板端参数、不启动电机、不授权拖动或连续调参。配置参考值不能当作读回，单次参数下发及连续会话仍按下文确认。`online_tuning.runtime_writes` 必须由实际固件能力支持，不能为了绕过报错而设为 true。新接收格式只允许 raw / lxy / yzx；原始命令模板、插件及凭据不经配置工具修改。审阅期间发生配置 / 设备切换或停止操作，旧提案失效。

### 工作台操作：新增 21 个工具

- **串口诊断**：先用 `list_serial_ports` 枚举端口，再用 `get_connection_diagnostics` 查看当前阶段、最近尝试 / 错误和统计。`get_communication_log(level="ERROR", limit=100)` 获取详细错误，最多 500 条。扫描不打开端口；设备没插入时端口缺失属于正常情况，连接回执 started=true 不表示已连上。
- **当前页一键配置**：`read_and_configure_parameters(scope, control_type)`，scope 为 parameters / diagnosis / continuous。参数页必须明确提供 P / PI / PID，两个 AI 页用已保存模式。已连接空闲时，在软件确认后读取并只同步此页；保留人工限制，未读到的项明确返回，不发 SET、不授予会话。回执需查询到 completed。
- **波形显示**：`get_waveform_view` 查询；`set_waveform_view` 可暂停 / 恢复、跟随、自动缩放、设置相对时间范围或返回实时。暂停显示不停止采集。`set_channel_display` 调整颜色（#RRGGBB）及显示隐藏，首批数据前也可设置，不改通道映射。
- **PID 备份**：`list_parameter_backups(session_id)` 列备份，`restore_parameter_backup(snapshot_id, parameter_ids)` 按当前 AI 权限和范围校验后只暂存到输入调参，真实下发仍需用户确认；`export_parameter_backup` 导出备份或当前显示值到 exports，不读取设备，也不覆盖原文件。
- **数据与设备**：`import_waveform_csv(path, expected_revision, replay)` 导入用户明确指定的本地绝对 CSV 路径（最多 32 MiB / 200 万通道样本）为离线实验，须断开空闲且配置版本匹配，失败撤销本次新实验；离线数据不用于连续实时调参。`delete_experiment` 按明确实验 ID 和完整名称删除，无二次弹窗，禁止删除当前采集或拿用户数据做测试。`create_device` / `copy_device` 建立定义但不切换和连接；`set_default_device` 只设下次默认；`export_device_configuration` 导出定义；`delete_device` 经用户确认删除定义并保留历史实验。
- **软件内 AI**：`configure_ai_connection` 提交服务 / 模型 / 地址等设置供用户审阅，request_api_key=true 显示软件密码框，密钥不经聊天、工具参数或回执。`test_ai_connection` 发送无设备数据的最小测试，`diagnose_selected_waveform` 调用软件内 AI 诊断当前选区，二者会使用 API 配额，仅在用户要求时调用。`get_ai_diagnosis` 仅读已有诊断，不发请求，不自动应用建议。

导出工具只接受文件名，使用返回 path；所有 started / running / needs_confirmation 回执都要按原操作 id 查询实际结果。新增工具没有开放任意串口命令、固件烧录、电机启停或代替用户授权。

### 场景一：查看波形、只做诊断

1. 核对连接、采集状态和通道关联。需要读取设备参数时，用 `read_parameters`；只查询软件状态不会刷新设备读回值。
2. 用 `get_waveform` 获取波形预览；用户要求特定区间时，再调用 `set_analysis_interval`。区间单位是相对于当前显示波形起点的秒。
3. 调用 `analyze_waveform` 计算本地指标。它分析当前选区，没有选区时使用显示缓冲区；独立“分析”界面虽已移除，这个 MCP 工具仍保留。
4. 数据不足、指标无效、缺少通道或参数未确认时，直接说明缺什么，不能据此编造已收敛或调参成功的结论。
5. 只诊断不调用写入工具。给用户一句结论、最多三条具体建议；需要保存波形时使用 `export_waveform`，完整样本以导出文件为准。

常用只读输入示例：

```json
{"tool": "get_waveform", "arguments": {"max_points": 200}}
```

```json
{"tool": "read_parameters", "arguments": {}}
```

这些 JSON 用于说明工具名和参数结构，由 AI 客户端调用工具，不是程序的独立 HTTP 接口。省略 `ids` 时仅读取参数设置中勾选的项；显式 `ids: []` 表示不读取任何项，不会退回全部读取。成功读值保留原数值，不能把读取理解为写入授权。

### 场景二：提出一次参数修改

1. 用户明确要求调参后，读取实际值和 `ai_parameter_constraints`，基于有效波形计算候选。使用真实参数 ID，逐项检查允许状态、当前值、设备 / AI 范围、写入刻度和单次变化限制。
2. 有当前实验时，可先用 `save_parameter_snapshot` 保存参数备份。
3. 调用 `write_parameters`，输入为 `values` 对象，键是参数 ID，值是候选数值。
4. 真实设备返回 `needs_confirmation` 时，表示建议已填入“输入调参”的参数表；请用户在软件中选择并确认写入。AI 不能代替确认，也不能报告“已写入”。
5. 用户执行后，重新核对参数状态和设备读回结果，再说明哪些参数已验证、哪些仍未确认。不要仅凭参数表中出现数字判断写入成功。

软件内“波形诊断权限”控制这种 AI 建议 / 单次写入用途；它不等于“MCP 连续调参权限”。单次写入的完整数值示例应由 AI 根据当前设备定义生成，不能套用其他设备的 P / I / D 数值。

### 场景三：MCP 连续 AI 调参

1. 先确认用户希望进行连续调参，并确认设备实现方支持运行中改参。只有波形上传或停机改参能力时，不能据此声明支持连续调参，也不能由 AI 擅自修改能力声明来绕过限制。
2. 调用 `prepare_continuous_tuning({})` 显示状态与操作提示。可通过上述配置工具提交目标值安全上下限、允许项、各项范围、观察窗口、轮数和总时长，经用户审阅保存；也可请用户在红色“AI 权限设置”的“MCP 连续调参权限”页填写。之后仍须用户从主页面点击 MCP 启动按钮并确认本次会话。**prepare 只准备，不启动或授权。**
3. 调用 `get_continuous_tuning_state({})`。只有 `active=true`、`state="active"` 且 `observation` 非空时，才基于该观察和 `constraints` 决定下一轮参数；否则按 `wait_seconds` 或观察窗口等待再查询。历史选区不能代替本轮实时观察。
4. 调用 `apply_continuous_tuning_step`，传入当前连续会话的 `session_id`、本轮唯一 `step_id`、最新 `observation.id` 作为 `observation_id`、受限候选 `values` 和具体依据 `reason`。`values` 只接受数值，只能包含本次授权参数。
5. 等待这一轮完成，只有 `readback_verified=true` 才表示本轮写入经应答及独立 GET 核对。随后必须等**新观察窗口**再决定下一轮，不能连续使用旧观察下发。
6. 达到目标、证据不足或需要结束时，调用 `stop_continuous_tuning`。失败或部分写入后先核对当前状态，不自动回滚或重复发送未知结果的请求。新一轮会话须由用户重新确认。

连续会话确认后，合法步骤不再逐轮弹确认；仍受设备边界、两套用途权限、本次安全范围、时间 / 轮数上限、遥测质量、写入应答和读回检查。软件的“停止写入”只停止后续命令，**不能代替设备的物理急停**。

### 返回状态和 ID：不要混用

工具回执通常包含 `id`、`state`、`result`；具体结构以工具实际返回为准。

| 返回内容 | AI 应如何处理 |
| --- | --- |
| `state="queued"` 或 `state="running"` | 操作仍在执行，用回执 `id` 调用 `get_operation`；不要重新提交写入 |
| `state="needs_confirmation"` | 配置或参数候选等待用户在软件确认，不能当成已保存或已写入 |
| `state="completed"` | 该请求结束；仍需检查 `result`，它不保证硬件写入已成功 |
| `state="error"` 或工具报错 | 按错误说明核对配置 / 连接，报告实际结果，不假装完成 |
| `result.readback_verified=true` | 连续步骤全部写入已通过读回核对 |
| `result.observation=null` | 尚无完整新观察，继续等待，不下发参数 |

连续调参的 `session_id` 要取自 `get_continuous_tuning_state`，不是状态页里的实验 ID。`observation_id` 取当前 `observation.id`。每轮 `step_id` 必须唯一；同一 ID 只能对应完全相同的请求。结果不确定时查询原操作，不更换 ID 重发。

## MCP 工具速查：54 个

`—` 表示无输入参数。下表是 V1.5.12 的工具索引；调用时以客户端提供的实际工具 schema 为准。

| 工具 | 输入 | 用途 |
| --- | --- | --- |
| `open_studio` | — | 打开 / 连接软件，不打开串口 |
| `get_studio_status` | — | 连接、采集、回放、任务、当前参数及后台操作状态 |
| `get_device_definition` | — | 参数 ID、角色、协议、范围、读取选择及 AI 限制 |
| `get_device_configuration` | — | 完整设备配置、版本摘要和当前能否编辑，不含密钥 |
| `validate_device_configuration` | `configuration`、`expected_revision` | 校验完整配置并列出全部变更，不保存 |
| `configure_device` | `configuration`、`expected_revision`、`reason` | 显示逐项变更，须用户在软件确认后保存 |
| `connect_device` | `device_id`、`expected_revision` | 打开当前设备已保存串口，成功后正常采集，不授予写入 |
| `get_studio_capabilities` | — | 软件 / MCP 版本、工具数量和支持能力 |
| `list_devices` | — | 列出已保存设备 |
| `select_device` | `device_id` | 断开且空闲时切换已有设备 |
| `list_protocol_examples` | — | 原始接收、Lxy、Yzx 三种接收格式说明 |
| `get_protocol_example` | `protocol`；可选 `include_support_files` | 读取内置 C 接入示例；protocol 为 raw、lxy 或 yzx |
| `get_ai_configuration` | — | 模型选项和密钥是否已配置，不返回密钥 |
| `select_ai_model` | `provider`、`model` | 选择内置提供者已有模型；provider 为 deepseek 或 openai，不调用 API |
| `import_project_parameters` | `device_id`、`source_folder`、`sources`、非空 `parameters` | 基于 AI 已阅读的真实工程导入定义或暂存建议；须断开且空闲，不写设备、不授予 AI 权限 |
| `read_parameters` | 可选 `ids` | 读取指定 ID，省略时读参数页勾选项 |
| `write_parameters` | `values` | 校验并暂存单次候选，真实设备仍须用户确认 |
| `get_waveform` | 可选 `max_points`，默认 200，范围 10–2000 | 获取均匀抽样的波形预览及质量信息 |
| `set_analysis_interval` | 可选 `start_seconds`、`end_seconds` | 设置相对时间选区；两项都省略则清除 |
| `analyze_waveform` | — | 计算当前选区 / 显示缓冲区的本地指标 |
| `start_capture` | — | 已连接时开始记录；已在记录时返回当前实验 |
| `stop_capture` | — | 停止并保存记录，串口保持连接 |
| `disconnect_device` | — | 停止采集并断开设备 |
| `save_parameter_snapshot` | — | 保存当前实验的参数备份 |
| `export_waveform` | 可选 `name`，默认 codex-waveform.csv | 导出保存的样本，有选区时导出该区间；仅传 CSV 文件名，使用返回路径 |
| `list_experiments` | — | 列出历史实验 |
| `replay_experiment` | `session_id` | 加载历史实验；已连接时须属于同一设备和任务，等待回放加载完成 |
| `get_operation` | `request_id` | 查询排队 / 运行中的原操作；值取回执 id |
| `emergency_stop` | — | 取消排队操作、停止后续参数下发，不是物理急停 |
| `prepare_continuous_tuning` | — | 显示连续调参提示，等待用户设置和确认 |
| `get_continuous_tuning_state` | — | 连续授权、计划、约束、实际值与新观察 |
| `apply_continuous_tuning_step` | `session_id`、`step_id`、`observation_id`、`values`、`reason` | 执行用户已授权会话内的一轮有界写入并独立读回 |
| `stop_continuous_tuning` | `session_id`；可选 `reason` | 结束本次连续会话，不自动恢复旧参数 |
| `list_serial_ports` | — | 扫描端口、说明和硬件标识，不打开串口 |
| `get_connection_diagnostics` | — | 连接阶段、最近尝试 / 错误、统计及阻塞原因 |
| `get_communication_log` | 可选 `level`、`limit` | 最近日志，最多 500 条，隐藏密钥 |
| `read_and_configure_parameters` | `scope`；参数页需 `control_type` | 人工确认后只读取并配置指定页面 |
| `get_waveform_view` | — | 显示状态、范围、通道颜色与显示隐藏 |
| `set_waveform_view` | 可选 `paused`、`following`、`autoscale`、两个时间边界、`return_to_live` | 调整显示，采集继续 |
| `set_channel_display` | `channel_id`；可选 `visible`、`color` | 通道显示与 #RRGGBB 颜色 |
| `list_parameter_backups` | 可选 `session_id` | 列实验备份 ID、名称、时间与数值 |
| `restore_parameter_backup` | `snapshot_id`；可选 `parameter_ids` | 按 AI 校验暂存备份候选，不直接写设备 |
| `export_parameter_backup` | 可选 `snapshot_id`、`name` | 导出备份 / 当前显示的 PID 值 |
| `import_waveform_csv` | `path`、`expected_revision`；可选 `replay` | 导入离线 CSV，失败回滚本次新实验 |
| `delete_experiment` | `session_id`、`expected_name` | 明确指定后直接删除，禁止当前采集 |
| `create_device` | `name` | 建立空定义，不选择或连接 |
| `copy_device` | `device_id`；可选 `name` | 复制定义到新 ID，读回状态重置 |
| `delete_device` | `device_id`、`expected_name` | 用户在软件确认删除，历史实验保留 |
| `export_device_configuration` | 可选 `device_id`、`name` | 导出设备 JSON 到 exports，不含 API 密钥 |
| `set_default_device` | `device_id` | 设置下次启动默认，不立即切换 |
| `configure_ai_connection` | `provider`、`model`、`reason`；可选连接字段及 `request_api_key` | 审阅保存 AI 设置，密钥仅软件密码框输入 |
| `test_ai_connection` | — | 发出无设备数据的 API 连接测试 |
| `diagnose_selected_waveform` | — | 通过已保存 API 诊断选区，不下发参数 |
| `get_ai_diagnosis` | — | 读取已有结构化诊断和可应用状态 |

工程导入的 `source_folder` 须为实际存在的工程绝对路径，`sources` 形如 `[{"file":"src/control.c","line":42}]`，行号可省略；`parameters` 须提供实际核实的定义，不能只给来源路径而不提供任何参数。来源文件由 AI 在用户授权范围内阅读，MCP 不执行工程源码。

## MCP 资源：5 个

| 资源 URI | 内容 |
| --- | --- |
| `pidstudio://status` | 软件当前状态 |
| `pidstudio://device` | 当前设备定义 |
| `pidstudio://capabilities` | 版本与能力 |
| `pidstudio://ai` | 模型选项与凭据配置状态，不含密钥 |
| `pidstudio://protocols` | 三种接收格式及接入示例目录 |

除公开协议资料外，工作台工具 / 资源应在首次连接流程之后读取。`list_protocol_examples`、`get_protocol_example` 和 `pidstudio://protocols` 可在软件未启动时阅读，不会连接设备或应用配置。

## 可以直接给 AI 的示例任务

只诊断：

```text
请先阅读 https://raw.githubusercontent.com/YANG985-CMD/PID-Studio-Standard/main/README.md，
通过本机 pid_studio_standard MCP 分析我当前选定的波形。
先按文档核对版本、设备定义、通道和采集状态；需要我打开串口或补配置时告诉我。
这次只做诊断，不写参数。结论简洁，并说明依据和缺少的数据。
```

连续调参：

```text
请先阅读上述 README，通过 pid_studio_standard 帮我准备连续 AI 调参。
先核对设备是否支持运行中改参，再提示我设置 MCP 权限和本次安全范围。
等我在软件里确认会话后，按新的观察窗口逐轮分析、执行受限步骤并核对读回；
达到我设定的目标或证据不足时停止，并报告实际验证的结果。
```

## 常见问题

| 现象 | 处理 |
| --- | --- |
| AI 没有 PID Studio 工具 | 核对实际 exe 路径、完整解压目录和 STDIO 配置，然后重新加载 MCP |
| `versions_match=false` | 同时更新软件与 MCP，并重载客户端；不要只看已经改过的配置路径 |
| 有波形，参数却读取失败 / 未知参数 | 核对板端 GET 应答、参数地址、数值类型、协议及所选 P / PI / PID；上传波形本身不提供参数读写 |
| P、I 步长仍是 0.01 | 可能是本次未读成功而保留的旧设置；读取成功后才重算，也可直接手动修改 |
| 诊断数据不足或数值明显不对 | 核对 SP / PV / OUT 角色、接收位置、缩放、单位、有效样本和选区 |
| 连续状态的 observation 为空 | 按返回等待时间收集完整的新鲜同步窗口，不用旧波形凑数 |
| 连续会话不能启动 | 核对该用途权限、目标安全范围、运行中写入能力、遥测、保护阈值和软件返回原因 |
| 请求超时、返回 running | 用原回执 id 查询 get_operation，不重复发送参数修改 |

更详细说明随压缩包提供：`docs/CODEX_CONTROL.md`、`docs/MCP_DEVICE_CONFIGURATION.md`、`docs/MCP_WORKBENCH_TOOLS.md`、`docs/AI_PARAMETER_LIMITS.md`、`docs/MCP_CONTINUOUS_TUNING.md`。

## 使用许可

免费使用的授权范围见 [LICENSE.txt](LICENSE.txt)。未经作者书面许可，不得销售软件副本、换标收费分发或商业再分发本项目原创应用实现的衍生版本。第三方组件仍受各自许可证约束；协议接入示例可用于使用者自己的设备工程。
