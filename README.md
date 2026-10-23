# PID Studio｜软件与 MCP 使用说明

本仓库提供软件、配套 MCP 及下载说明，不公开应用实现源码。软件和 MCP 可免费使用，无需开发密码或企业授权。

**给 AI 阅读：[完整 Markdown 原文](https://raw.githubusercontent.com/YANG985-CMD/PID-Studio-Standard/main/README.md)。** 本文包含安装配置、软件操作、全部 MCP 工具和 AI 调参流程；可将此链接直接交给支持联网阅读的 AI。实际控制本机软件，还需要在该 AI 客户端连接下面介绍的 MCP。

## 软件和 MCP 分别做什么

PID Studio 是 Windows 串口波形与 PID 参数调试软件，适用于已实现配套通信协议的设备。它显示实时波形、记录实验、读取参数，并在用户确认后下发参数。电脑端选择 P / PI / PID 只改变软件使用哪些参数，不改变设备固件中的控制算法。

配套 MCP 是软件与 AI 客户端之间的连接程序。AI 通过结构化工具读取设备定义、参数、波形和实验记录，提交设备配置供用户审阅、连接已保存串口，进行诊断、提出参数建议，或在用户授权的连续会话内逐轮调参。MCP 程序已打包，使用时无需安装 Python。

| 使用方式 | 谁进行分析 | 需要什么 |
| --- | --- | --- |
| 手动调参、查看波形、记录实验 | 用户操作软件 | 只需软件，设备实现对应协议 |
| 软件内“诊断选定波形” | 软件调用用户配置的 AI API | 在软件的“API 设置”中配置服务和模型 |
| 连续调参选择 API | 软件内已配置的模型 | 使用对应 API 账户的 Token；先保存连续权限并确认会话 |
| 外部 AI 通过 MCP 查看、诊断或连续调参 | 当前 MCP 客户端里的 AI | 软件、配套 MCP、支持 MCP 的 AI 客户端；无需在软件里额外配置模型 API |

软件提供原始接收、Lxy 文本协议和 Yzx 批量二进制协议。**接收到波形不等于设备支持参数读写**：参数 GET / SET、地址、数值类型和应答必须与设备实现一致。原始接收主要用于通信检查；Lxy / Yzx 接入示例可通过 MCP 或软件的“通信协议”窗口查看。

软件提供“参数调试 / AI 诊断”。金色“PID 线性前馈设计”和“启动连续调参”横向并排；连续调参可选择 API / MCP，两种来源共用权限、试验、写入保护和独立读回。PID 当前参数下方提供“AI 分析对话框 / 连续调参对话框”两个切换按钮，可同时查看实时波形、PID 读回值和调参状态。

## Windows 下载

当前版本：**V1.8.16**，Windows x64。升级前正常退出软件，保留原 `user_data_standard` 文件夹；同时更新 GUI 与 MCP，更新后重载 MCP 客户端。

| 下载 | 用途 |
| --- | --- |
| [PID Studio 软件包](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.8.16/PIDStudio_Standard_App_V1.8.16_Windows.zip) | PID 参数调试、实时波形、AI 诊断与 API 连续调参 |
| [配套 MCP 程序](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.8.16/PIDStudio_Standard_MCP_V1.8.16_Windows.zip) | 供 Codex 等 MCP 客户端连接 PID Studio，包含固定试验与连续 AI 调参接口 |
| [SHA256 校验值](https://github.com/YANG985-CMD/PID-Studio-Standard/releases/download/v1.8.16/SHA256SUMS.txt) | 核对下载文件完整性 |

全部版本见 [发布页](https://github.com/YANG985-CMD/PID-Studio-Standard/releases)。

## 支持作者

**为爱发电，有条件的可以支持一下回回血，感谢**

| 支付宝 |
| :---: |
| <a href="assets/support/alipay.jpg"><img src="assets/support/alipay.jpg" width="280" alt="支付宝收款码"></a> |

小红书、B站、抖音搜索：**电机的主人**。

## V1.8.15 更新

- 自动管理模式取消 P/I/D 单次变化上限，旧人工上限、手动滑条步长乘十及自动参考幅度均不拦截合法 PID 候选。API/MCP 共用此行为；固定 PID 模式、目标值阶跃和前馈参数仍执行各自数值上限，设备边界、自动搜索范围、配置写入刻度和运行保护继续有效。
- 连续状态嵌入 AI 页，在“AI 分析对话框 / 连续调参对话框”间切换；切换保留原诊断与连续会话，可同时看波形和 PID 实际读回值。四张紧凑卡片横排显示运行来源、调参进度、目标安全范围与评分，较小字号保留详细说明；详细过程可滚动，停止按钮常驻。
- 改善持续采集后的密集曲线绘制、鼠标读数和重复界面排版。仅更新有新数据的通道，鼠标复用显示快照，保留峰值显示、完整实验保存和分析样本。
- “启动连续调参”与左侧前馈设计按钮统一为金色；结果页按钮名称更明确。
- GUI / MCP 均为 **V1.8.15**，**57 个工具 / 5 个资源**。V1.8.13 完整离线回归 872 项通过，后续卡片、启动与切换检查通过；独立程序、MCP 版本、包内容和安装文件核对一致。本次汇总 V1.8.12～V1.8.15 的更新。

## V1.8.16 更新

- 新增“静态稳定（固定当前工况）”：保持目标值不变，目标值和实际值连续稳定后才交给 AI 调 PID。
- 新增“使用者调节工况（稳定后调 PID）”：使用者手动切换目标值，每个工况稳定后 AI 根据该工况数据调整 PID，AI 不修改目标值。
- 波形诊断增加“自动识别 / 静态稳定 / 使用者调节工况”数据方式，明确稳定段和目标变化段的证据边界。
- 依据 Nakamoto（2003）面向稳态过程的迭代反馈整定，以及 Hjalmarsson 等（1998）闭环迭代调参思路，详见软件包内 `docs/FIXES_V1.8.16.md`。
- GUI / MCP 均为 **V1.8.16**，**57 个工具 / 5 个资源**；设备边界、权限、运行保护、ACK 和独立读回继续有效。

## V1.8.11 更新

- 连续 API 兼容多余说明字段、完整 JSON 代码块和明确的动作别名。回复为空、截断或暂时无法解析时，本轮建议不应用，保持当前参数，等待后以新波形继续；连续格式异常按 5、10、20、40、60 秒延长等待，正常回复后恢复正常节奏。原有请求预算和设备保护继续有效。
- API / MCP 连续小数候选自动对齐配置写入刻度，回执区分原建议、实际提交和设备读回。对齐后无有效变化时保持观察，不增加成功调参轮次；原始越界或超过单次上限的建议仍拒绝。
- PID 局部搜索范围与幅度按实测自动管理，取消连续页重复的 PID AI 上下限。设备已保存可写边界仍在“AI 权限设置 → 参数设置 → 高级设置”中，自动搜索不会扩大这些设备边界；已有人工单次上限保留。
- 前馈总开关和必要授权入口加大、加粗并显示状态底色。新前馈参数“诊断 / 连续”默认勾选为可编辑草稿，已有明确保存的选择保留；总开关及本次会话仍由用户确认。
- 常用连续权限页和启动窗口精简默认展示，运行保护折叠但保留当前阈值摘要；状态窗口支持自由滚动和可选跟随最新状态。
- GUI / MCP 均为 **V1.8.11**，**57 个工具 / 5 个资源**；**844 项完整离线回归通过**，独立程序、版本和打包内容核对通过。实际效果以设备完整复测为依据。

## V1.8.6 更新

- 连续调参默认“自动调整幅度”：按设备实际读回量级起步，同条件改参复测持续改善时扩大，表现变差或输出余量不足时缩小，改善较小时细调。API 与 MCP 共用同一套规则，手动滑条刻度独立。
- 手动单次上限收进“AI 权限设置 → 连续调参权限 → 幅度上限（高级）”。已有人工上限继续有效，增益项清空后恢复自动幅度；也可选择“固定幅度（高级）”。目标值的阶跃上限独立保留。
- 只有参数修改后的完整复测、并通过末尾读回核对，才累计幅度更新依据；查询状态、保持参数、过期建议或失败读回不会扩幅。跟随实际工况保留保守幅度，避免直接比较不同工况。
- “方法与论文依据”按“借鉴哪些部分、软件怎样运用”展示，补充局部搜索幅度管理的论文来源；状态显示幅度更新原因，接近范围边界时提示具体设置入口。
- 界面默认使用“设备使能”“设备启停”，观察预设按响应快慢说明，适用于不同类型设备；已有自定义通道名称保留。
- GUI / MCP 均为 **V1.8.6**，**57 个工具 / 5 个资源**；**756 项完整离线回归通过**，独立程序、版本、打包内容与安装文件核对通过。效果以设备实际工况复测为依据。

## V1.8.5 更新

- 连续调参默认不限总时间和轮数，由用户点击停止。稳定容差和连续稳定观察只用于评价，完整响应尚未稳定时仍交给 AI 继续调整。
- 启动窗口直接选择每段观察时长：2 秒快速、4 秒通用（推荐）、8 秒充分、15 秒慢响应，并保留 2～60 秒自定义。每档显示适用情况和整组采集耗时，成功启动后记住选择与评价设置。
- 目标值取消重复的 AI 上下限，统一使用同页“目标值安全下限 / 上限”；旧目标 AI 子范围不再额外拦截。P/I/D 范围独立保留，设备边界和目标单次变化上限继续生效。
- 状态窗口支持自由滚动与可选“跟随最新状态”，刷新不再强制回滚；启动预填已有授权范围内的合法 A/B 目标，仍需用户核对确认。
- 改善遥测时效、浮点读回、小数目标、固定 PID/前馈核对、历史长窗口、CSV 参数来源、重复备份及历史定位。MCP 新增 `observe_continuous_tuning`，可保持参数并重新收集完整观察；终态保留最后已验证评价。
- GUI / MCP 均为 **V1.8.5**，**57 个工具 / 5 个资源**；**738 项完整离线回归通过**，冻结 GUI/MCP 与安装文件核对通过。验证使用隔离数据、假设备与模拟 AI，未把离线通过当作真实设备调参效果证明。

## V1.8.0 更新

- 连续调参可选择 API 或 MCP，参数权限与执行保护相同，模型和用量来源不同。主页面入口用于 PID，前馈窗口底部入口支持 PID 与前馈。
- 新增自动阶跃（默认）、多目标综合、跟随实际工况和前馈多点整定；默认选择“平衡性能”。目标轨迹由用户确认，自动模式每完整批次评价后才分析，调整后使用相同轨迹复测，末轮候选也需复测。
- 前馈多点模式固定 PID，仅调整 Kff/Bff。状态显示本地评分、基准与最佳已验证结果；高级设置可填写验收阈值，实测逐段核对。未设置阈值时只报告改善，AI 建议完成不等于量化达标。
- 启动窗口按所选方式显示“AI 本次可调整”的参数；默认折叠高级设置和“方法与论文依据”，每种方式写明步骤、指标、参考论文与采用范围。
- 包含 V1.7 系列的启动提示、运行中改参入口、前馈交互和诊断范围修复。运行能力开关位于“AI 权限设置 → 连续调参权限 → 设备支持运行中修改参数（PID / Kff / Bff）”，新建配置默认打开，已有值保持；它只是本地能力标记，不能替代固件实际支持。Kff/Bff 的“连续”权限仍在“PID 线性前馈设计 → 参数与判据”。
- GUI / MCP 均为 **V1.8.0**，**56 个工具 / 5 个资源**。**645 项完整离线回归通过**，冻结程序和实际 MCP 只读握手通过；未连接真实设备或调用真实 AI，效果须按实际设备验证。

## 连续调参的方式与论文依据

### 自动管理范围与幅度：借鉴部分与软件做法

借鉴 Eriksson 等 NeurIPS 2019 论文 [Scalable Global Optimization via Local Bayesian Optimization](https://proceedings.neurips.cc/paper/2019/file/6c990b7aca7bc7058f5e98ea909e924b-Paper.pdf)第 2 节中，根据连续试验结果扩大或收缩局部搜索区域的思路。软件由 AI 提出参数候选，再根据参数修改后的同条件实测评分管理 PID 局部搜索区间与调节幅度。

| 实际响应 | 软件如何调整 |
| --- | --- |
| 连续两次改参后的完整复测均明显改善，且输出余量充足 | 逐渐扩大搜索幅度 |
| 改善较小，适合进一步细调 | 缩小幅度进行精细调整 |
| 评分、超调或波动明显变差，或输出余量不足 | 缩小幅度，并提示在当前限制内向已验证较好参数回调 |

PID 局部搜索区间在完整同条件改参复测、末尾读回核对通过后扩大、收缩并移动到本次确认参数附近，已保存设备可写边界、写入刻度和运行保护继续执行。自动 PID 的参考幅度仅用于决策，不作为单次写入上限；已有人工上限保留供固定模式使用。目标值阶跃和前馈幅度独立保留；跟随实际工况不会因不同工况误差直接比较而扩大或移动搜索区间。具体工程比例和次数见解压包内 `docs/ADAPTIVE_ADJUSTMENT.md`；状态中的 `adjustment` 解释本轮范围与幅度，`constraints` 给出每次发送时执行的实际限制。

第一次使用建议保留“自动阶跃＋平衡性能”，核对预填的目标 A、B，并选择每段观察时长。软件读取当前参数，完成 A、B、A 整组响应后再请求建议；初始波形尚未稳定也可以调参。目标值只设置一套安全范围，单次变化量和设备保护仍有效；默认不限总时间和轮数，由用户停止。AI 不能修改已确认的测试轨迹或评分规则，主页面仍可查看实时波形。

| 方式 | 怎么做 | 参考论文与采用范围 |
| --- | --- | --- |
| 自动阶跃（推荐） | 读取当前参数后完成 A→B→A，完整响应后评价超调、误差、稳定时间与波动，调整后重复 | Hjalmarsson 等，1998，[Iterative feedback tuning: theory and applications](https://doi.org/10.1109/37.710876)：借鉴闭环试验、固定性能准则和受限迭代，用相同测试比较调整前后效果 |
| 多目标综合（高级） | 按用户填写的 3～8 个不同目标测试并回到首点，同一组参数兼顾平均和最差表现 | Neumann-Brosig 等，2020，[Data-Efficient Autotuning With Bayesian Optimization: An Industrial Control Study](https://doi.org/10.1109/TCST.2018.2886159)：借鉴试验成本、历史评价与数据效率，将多工况实测表现汇总为统一指标 |
| 跟随实际工况（高级） | 软件不改目标，依据用户或板端给定工况的新鲜波形小幅调参 | Hjalmarsson 等，1998，[IFT 论文](https://doi.org/10.1109/37.710876)：借鉴利用闭环运行数据迭代改善的思路，结合当前工况波形提出受限参数建议 |
| 前馈多点整定 | 从前馈入口启动，固定 PID，多点稳定采集、拟合与评价，仅修正 Kff/Bff 后复测 | van der Meulen 等，2008，[Fixed Structure Feedforward Controller Design Exploiting Iterative Trials](https://doi.org/10.1115/1.2957626)：借鉴固定结构前馈与重复试验，用于静态 Kff×SP+Bff 的多点拟合、误差评价与复测 |

每种自动模式使用固定工程权重评价跟踪误差、超调、稳定时间、末段波动及输出饱和；全组兼顾平均与最差段，评分越低越好。高级设置中的验收阈值只由实测结果核对。数据不足时等待新的完整观察；完整响应尚未稳定时仍可继续调整，明确显示“未稳定”，不伪造零秒稳定时间。跟随模式不使用自动阶跃验收阈值。

保护设计参考 Berkenkamp 等，ICRA 2016，[Safe Controller Optimization for Quadrotors with Gaussian Processes](https://doi.org/10.1109/ICRA.2016.7487170) 从已知可用参数出发、在运行约束内逐步探索的思路，结合权限、设备范围、调节幅度、新鲜遥测、ACK 与独立读回管理每次调整。

解压后的 `docs/CONTINUOUS_TUNING_METHODS.md` 提供完整操作与评分规则，`docs/CONTINUOUS_TUNING_LITERATURE.md` 提供文献核对记录。停止仅撤销后续软件写入，不自动恢复最佳参数或保证设备物理停机。

### 每段观察时间怎么选

| 预设 | 适用情况 | A、B、A 三段采集约需 |
| --- | --- | --- |
| 快速观察 · 2 秒 | 响应快、扰动小的小幅阶跃 | 6 秒 |
| 通用观察 · 4 秒（推荐） | 响应在数秒内基本展开，兼顾响应过程、末段波动与每轮等待 | 12 秒 |
| 充分观察 · 8 秒 | 波动明显、负载较重，观察振荡衰减或缓慢回落 | 24 秒 |
| 慢响应 · 15 秒 | 惯量较大、恢复较慢、低频波动明显 | 45 秒 |
| 自定义 · 2～60 秒 | 按实际响应选择其他时长 | 每段时长 × 3 |

以上另加通信、参数读回和 AI 分析耗时。多目标与前馈按目标列表并回到首点计算；跟随模式每次收集一个窗口的新波形。

这些是便于上手的工程预设，按实际设备响应选择。Lizarraga 等 2026 年的[直流电机位置控制实验](https://doi.org/10.3390/eng7080397)第 5.1 节采用每个候选总长 3 秒、0.5 秒时施加阶跃；借鉴其中保持候选试验时序一致的做法，结合当前设备响应选择窗口。软件内“方法与论文依据”写明采用部分。

## V1.6.8 更新

- 修复无设备查看历史波形时，保存前馈参数定义与权限仍提示“请先断开设备”的问题。
- 历史回放只表示当前查看的是历史数据；没有串口、写入对象、采集任务或正在关闭的连接句柄时，可以保存本地前馈配置。
- 串口打开失败时清理残留的参数写入对象；正在关闭串口或尚在采集时，分别显示对应原因。
- 软件与 MCP 同步为 **V1.6.8**，保持 **56 个工具 / 5 个资源**；**547 项完整离线测试通过**，包括真实历史回放保存、历史数据保持和模拟连接失败的回归。未连接硬件或调用真实 AI。

## V1.6.7 更新

- 主界面的“诊断选定波形”和 MCP `diagnose_selected_waveform` 只提交 PID / 目标值及 SP/PV/OUT 响应，不再把 Kff/Bff 或线性前馈上下文带入 PID 诊断。
- 前馈菜单中的“AI 联合诊断 PID 与前馈”保留 Kff/Bff 和前馈上下文；两类诊断的候选参数按各自请求范围独立校验。
- 软件与 MCP 同步为 **V1.6.7**，保持 **56 个工具 / 5 个资源**；538 项离线测试通过，另有 1 项 Windows 单实例启动测试因环境时序超时，单独重跑仍为环境超时，未涉及本次诊断分流代码。

## V1.6.6 更新

- 修复点击“诊断选定波形”或“AI 联合诊断 PID 与前馈”后，AI 多返回说明字段导致整份诊断被拒收的问题。兼容 `additional_data_needed_note`、`candidate_parameters_notes` 及其单复数文本形式，统一保留在报告的“补充说明”中。
- AI 返回格式统一提供 `notes` 文本列表，说明只用于展示及记录。参数数值、权限、范围、步长和人工确认继续独立校验；其他不合法回复显示中文格式提示，不会自动重发 API 请求。
- 软件与 MCP 同步为 **V1.6.6**，保持 **56 个工具 / 5 个资源**，`get_ai_diagnosis` 同步返回 `diagnosis.notes`。**537 项完整离线测试通过**，含截图字段及两个诊断入口的 16 个新增回归用例；冻结程序启动及实际 MCP 连接核对通过，未进行实机调参验收。

## V1.6.5 更新

- 保留紧凑、加粗、可复制的 P/I/D 卡片，增加每项最近成功读回时间和本次连接标记。重连前记录与旧配置的确认状态不能当成本次已同步；待同步、保留记录和未启用项分别显示。
- AI 诊断固定请求时的参数、模型、来源实验与选区；报告优先显示结论，可点击“查看原波形”核对只读预览。历史波形诊断归入来源实验，参数快照不代表波形采集时的实际生效值；应用建议仍按当前权限与范围检查。
- 顶部集中显示通信状态，区分串口未连接、已打开但无有效波形、有效接收与数据延迟。常用 1080×720 窗口、约 348 px 右侧面板的普通 PID 值可同排显示，无整页滚动；极长值或更小窗口保留必要滚动。
- 包含 V1.6.3 的前馈优化：按每个目标的跟踪容差筛选连续稳定平台，检查漂移和窗口一致性，目标等权拟合并逐目标留出验证；提供拟合图、剔除原因及经人工核对采集条件的选区效果对比。
- 软件与 MCP 均为 **V1.6.5**，保持 **56 个工具 / 5 个资源**，同步提供读回证据、通信摘要和诊断来源。**521 项完整离线测试通过**，最终报告调整后相关 46 项再次通过；冻结程序启动及实际 MCP 连接核对通过，未进行实机调参验收。

## V1.6.2 更新

- 新增 PID 线性前馈 `u_ff = Kff × SP + Bff`，支持离线多工况设计、软件内 API 联合诊断及 MCP 有界连续调参。前馈由设备固件在控制周期内计算，软件读取和修改其系数。
- 前馈窗口提供可编辑的新手默认参数及“补齐默认参数”：Kff/Bff 参考值从 0 开始，建议地址优先为 20/21，避开本地已用地址；单位、范围、刻度与单次上限按配置估算。已有设置保留，建议地址必须与固件实际定义一致。
- 原“控制与安全”改为“控制与分析”。实际值绝对上限、输出绝对上限和遥测超时统一放在“AI 权限设置 → 连续调参权限 → 连续调参保护”，旧数值保持。只读波形诊断不发送这些运行阈值；连续下发的超限、超时和读回检查保留。
- 软件与 MCP 均为 **V1.6.2**，共 **56 个工具 / 5 个资源**。新增 `get_linear_feedforward_state`、`design_linear_feedforward`；完整回归 **483 项通过**，冻结程序与实际 stdio MCP 连接验证通过，未进行实机调参验收。

## V1.5.14 更新

- AI 诊断页依次显示“选择分析区间 / 诊断选定波形 / 诊断目标”，目标可下拉选择或直接输入；“启动 MCP 连续 AI 调参”独占下一行。诊断目标由 GUI 与 MCP 共用，修改目标只保存该项，不启用权限、不调用 AI 或下发参数。
- 下方“同步最新 PID 值”显示最近设备读回的 P / I / D；AI、手动或 MCP 写入后经读回确认会自动同步。目标值不在此摘要显示，原有读取、调节与保护功能保留；待写入建议不当作已生效值。
- 参数调试按 P / PI / PID 模式默认勾选已配置可用的参数及目标值，模式停用或未配置项保持不可用；刷新保留人工取消的选择，不授予下发权限。
- 软件与配套 MCP 均为 **V1.5.14**，保持 **54 个工具 / 5 个资源**，能力信息及随包指南同步更新。完整离线回归 **465 项通过**；未进行实机调参验收。

## V1.5.12 更新

- 新增 21 个 MCP 工具，覆盖串口扫描与连接错误日志、当前页一键读取配置、波形显示控制、PID 备份管理、离线 CSV / 实验 / 设备管理，以及软件内 AI 连接设置、测试和诊断。现在共 54 个工具、5 个资源。
- API 密钥由用户在软件密码框输入，只存系统凭据库；不要把密钥交给聊天或工具参数。备份恢复只暂存候选；设备删除、一键配置和 AI 连接改动先审阅确认。

- 新增完整配置读取、配置校验、人工审阅保存配置与连接已保存设备 4 个 MCP 工具。Codex 可提交通道关联、设备能力、安全范围和两套 AI 权限，确认保存后继续连接、采集和读取参数；配置保存不授予硬件写入或连续会话。

- 设置窗口保留“参数设置 / 波形诊断权限 / 连续调参权限”三个页面，各页底部金色按钮**只读取并配置当前页**，保留另一用途设置和人工限制。
- 统一使用“当前参数值”，指设备此刻的 P、I、D（按控制类型读取）及目标值；未读到的项需自行填写。控制类型位于页签左侧，带下拉箭头，并与本页增益勾选双向同步；目标值可独立选择。
- 参数调试分为“滑动调参 / 输入调参”，共用读取与 PID 备份；“写入选中参数”仅在输入页显示，MCP 单次候选也在此审阅。
- “选择分析区间”移到诊断按钮左侧，未开启浅蓝、开启深蓝。拖图只停止自动跟随，数据继续刷新；“暂停视图”才冻结显示，“自动缩放”恢复跟随。
- 波形诊断可在选区内定位目标阶跃；明确通道关联保留，通道身份有歧义时需核对。历史回放可对当前连接设备审阅和确认参数，历史数据与实时采集分开。
- “控制与安全”位于 AI 诊断的“测试连接”右侧。右键删除已保存实验无需二次确认，正在采集的实验保留。
- 软件与配套 MCP 同步为 **V1.5.12**，提供 **54 个工具 / 5 个资源**；工具说明、能力信息和随包指南同步当前操作。

## 软件快速上手

1. 解压软件包，打开 `PIDStudioStandard/PIDStudioStandard.exe`。
2. 在“串口设置”“通道设置”和“通信协议”中配置端口、波特率、通道接收位置与设备协议。通道标识、换算系数和单位须与实际数据一致。
3. 在 AI 诊断页“测试连接”右侧的“控制与分析”里核对当前任务的目标值、实际值、控制输出通道对应关系。例如 SP / PV / OUT 只是常用标识，实际以自己的配置为准。连续调参的实际值上限、输出上限和遥测超时在“AI 权限设置 → 连续调参权限 → 连续调参保护”中设置；只做波形诊断无需据此填写量程。
4. 点击“打开串口”，连接成功后软件自动采集；关闭串口时停止采集并保存实验。
5. 打开“PID 参数设置”，核对板端地址、数值类型、设备范围与步长。默认地址 P=1、I=2、D=3、目标值=4 仅在设备按同一约定实现时适用，不能当成任意板子的真实地址。
6. 参数与 AI 权限位于同一个设置窗口。三个设置页底部都有“一键读取当前参数值并配置上方参数”：参数页点击后选择 P / PI / PID，再读取并配置本页；两个 AI 权限页按已保存模式读取，仅配置本用途。读取不到的项自行核对设置，已有人工限制保留；AI 权限勾选和目标值安全上下限仍需按对应用途明确填写、保存。
7. 在“滑动调参”里修改步长或用滑块、箭头调节；步长与参数设置同步，实时下发须先确认本次会话。在“输入调参”里编辑待写入值，点击“写入选中参数”并确认。两页共用“读取设备当前参数值”“备份PID参数”“恢复备份PID参数”“导出PID参数”；表格中的数字不代表已写入。
8. 使用软件内 AI 波形诊断时，先配置自己的 API，在“波形诊断权限”页保存建议权限和参数限制，在主 AI 页面“诊断选定波形”右侧选择或输入诊断目标，再点击诊断按钮左侧的“选择分析区间”，框选包含阶跃和响应的波形并点击“诊断选定波形”。API Key 存入 Windows 凭据存储。

当前参数值指**设备此刻的 P、I、D（按所选控制类型读取）及目标值**。参数页的一键读取使用本次成功读值计算默认步长：当前参数值绝对值的 1%，小数默认按 0.000001 或已有更细刻度取整，零值退回该步长；之后仍可手动修改。手动滑条的显示范围以本轮读取值为基准，按基准 ± 步长 × 100 计算，并与设备上下限取交集。诊断建议的单次上限默认取步长 × 10；连续 PID 在已保存设备可写边界内按实测自动管理局部搜索范围与参考幅度，不受手动滑条显示窗口或旧人工单次上限限制。固定模式仍使用保存的单次上限。设备边界在“参数设置 → 高级设置”中核对，两套 AI 用途分别保存。

读取失败时保留旧数值和旧步长，不能把表格中的旧数字当成本次已读成功；**读回的 0 也可能是有效数值**。是否成功要看读取状态。读取数值不按写入范围或步长裁剪，实际写入仍检查范围、刻度、权限和本项实际启用的单次变化上限。

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

保存后重新加载 MCP 或重启 AI 客户端。V1.8.15 正常应显示 **57 个工具、5 个资源**。仅修改配置文件不会替换当前会话里已经启动的旧 MCP 进程，须以实际返回的版本为准。

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

AI 页“同步最新 PID 值”调用同一读取流程，下方仅展示最近设备读回的 P / I / D；AI 下发并读回确认后自动更新。卡片显示各项最近读回时间；悬停可查看完整时间及来源。状态与设备定义的 `parameter_readbacks` 提供 `current_connection`、时间和来源，只有本次连接成功读回才标记为当前确认，不能仅凭旧参数的 `status` 判断。它不会额外后台轮询，也不把候选值视为当前值。“诊断目标”位于诊断按钮右侧，GUI 与 MCP 使用同一已保存目标。

`get_connection_diagnostics` 的 `summary` 汇总通信状态；`get_ai_diagnosis` 的 `request_evidence` 保留请求时参数、模型、来源实验及选区。软件诊断显示“查看原波形”只读预览；历史来源或请求时参数不能当作采集时的实际生效值，也不能代替当前下发校验。

V1.6.6 的 `get_ai_diagnosis` 返回 `diagnosis.notes`，保留 AI 的附加说明；只作为报告文字，不能转换为参数或据此扩大权限。截图中已知的 note/notes 文本别名会兼容合并，其他未知控制字段仍拒绝。

`get_studio_capabilities` 还会返回 `parameter_setup_scope="current_page"`、`manual_parameter_modes=["slider","input"]` 和当前读取按钮名称，便于 AI 对照界面指导操作。

`task.roles` 中的目标值、实际值、控制输出若未关联，或协议缩放 / 单位明显不符，请先让用户核对配置。设备上下限、AI 允许范围、步长、单次变化限制及目标安全范围以当前返回结果为准，不照搬示例。

### 通过 MCP 配置和连接设备

1. 调用 `get_device_configuration`，取 `result.configuration` 的完整配置副本及 `result.configuration_revision`。它包含串口、全部通道与参数、控制关联、设备能力、安全限制，以及诊断和连续调参两套 AI 权限，不返回密钥。
2. 依据实际固件和用户请求修改副本，保留其余字段。将完整副本和读取时的版本传给 `validate_device_configuration(configuration, expected_revision)`，先检查全部变更。通道位置、参数地址、量程和运行中改参能力须有真实依据。
3. 断开设备、结束采集 / 回放 / 后台任务并关闭其他设置窗口后，调用 `configure_device(configuration, expected_revision, reason)`。软件弹出原值与拟保存值对照，返回 `needs_confirmation` 和操作 `id`。**请用户在软件点击“确认保存配置”，AI 不替用户确认。**
4. 调用 `get_operation(request_id=id)` 查询原操作；只有 `state=completed` 且 `result.saved=true` 表示保存成功。取消或失败应报告实际原因。若无变更，返回 `unchanged=true, saved=false`；配置版本过期须重新读取后合并，不重复提交旧草稿。
5. 保存后重新调用 `get_device_configuration`，用新的版本与当前设备 ID 调用 `connect_device(device_id, expected_revision)`。此工具只打开已保存串口；`started=true` 表示连接开始，用 `get_studio_status` 确认连接与采集结果，再用 `read_parameters` 读实际参数。

配置保存不写板端参数、不执行设备启停、不授权拖动或连续调参。配置参考值不能当作读回，单次参数下发及连续会话仍按下文确认。`online_tuning.runtime_writes` 必须由实际固件能力支持，不能为了绕过报错而设为 true。新接收格式只允许 raw / lxy / yzx；原始命令模板、插件及凭据不经配置工具修改。审阅期间发生配置 / 设备切换或停止操作，旧提案失效。

### 工作台操作：新增 21 个工具

- **串口诊断**：先用 `list_serial_ports` 枚举端口，再用 `get_connection_diagnostics` 查看当前阶段、最近尝试 / 错误和统计。`get_communication_log(level="ERROR", limit=100)` 获取详细错误，最多 500 条。扫描不打开端口；设备没插入时端口缺失属于正常情况，连接回执 started=true 不表示已连上。
- **当前页一键配置**：`read_and_configure_parameters(scope, control_type)`，scope 为 parameters / diagnosis / continuous。参数页必须明确提供 P / PI / PID，两个 AI 页用已保存模式。已连接空闲时，在软件确认后读取并只同步此页；保留人工限制，未读到的项明确返回，不发 SET、不授予会话。回执需查询到 completed。
- **波形显示**：`get_waveform_view` 查询；`set_waveform_view` 可暂停 / 恢复、跟随、自动缩放、设置相对时间范围或返回实时。暂停显示不停止采集。`set_channel_display` 调整颜色（#RRGGBB）及显示隐藏，首批数据前也可设置，不改通道映射。
- **PID 备份**：`list_parameter_backups(session_id)` 列备份，`restore_parameter_backup(snapshot_id, parameter_ids)` 按当前 AI 权限和范围校验后只暂存到输入调参，真实下发仍需用户确认；`export_parameter_backup` 导出备份或当前显示值到 exports，不读取设备，也不覆盖原文件。
- **数据与设备**：`import_waveform_csv(path, expected_revision, replay)` 导入用户明确指定的本地绝对 CSV 路径（最多 32 MiB / 200 万通道样本）为离线实验，须断开空闲且配置版本匹配，失败撤销本次新实验；离线数据不用于连续实时调参。`delete_experiment` 按明确实验 ID 和完整名称删除，无二次弹窗，禁止删除当前采集或拿用户数据做测试。`create_device` / `copy_device` 建立定义但不切换和连接；`set_default_device` 只设下次默认；`export_device_configuration` 导出定义；`delete_device` 经用户确认删除定义并保留历史实验。
- **软件内 AI**：`configure_ai_connection` 提交服务 / 模型 / 地址等设置供用户审阅，request_api_key=true 显示软件密码框，密钥不经聊天、工具参数或回执。`test_ai_connection` 发送无设备数据的最小测试，`diagnose_selected_waveform` 调用软件内 AI 诊断当前选区，二者会使用 API 配额，仅在用户要求时调用。`get_ai_diagnosis` 仅读已有诊断，不发请求，不自动应用建议。

导出工具只接受文件名，使用返回 path；所有 started / running / needs_confirmation 回执都要按原操作 id 查询实际结果。新增工具没有开放任意串口命令、固件烧录、设备启停或代替用户授权。

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

软件内“波形诊断权限”控制这种 AI 建议 / 单次写入用途；它不等于“连续调参权限”。单次写入的完整数值示例应由 AI 根据当前设备定义生成，不能套用其他设备的 P / I / D 数值。

### 场景三：MCP 连续 AI 调参

1. 先确认用户希望进行连续调参，并确认设备实现方支持运行中改参。只有波形上传或停机改参能力时，不能据此声明支持连续调参，也不能由 AI 擅自修改能力声明来绕过限制。
2. 调用 `prepare_continuous_tuning({})` 显示状态与操作提示。可通过配置工具提交安全范围、允许项与观察时长，经用户审阅保存；也可在“AI 权限设置 → 连续调参权限”中填写。默认总时间和轮数不限，专家可选有限会话；目标只使用一套安全上下限。运行中写入开关、目标安全范围和保护位于该页，Kff/Bff 的“连续”勾选位于前馈窗口“参数与判据”。随后用户点击主页面金色“启动连续调参”或前馈窗口底部入口，选择 **MCP**、测试方式、性能目标并核对确认。**prepare 只提示，不启动或授权；MCP 不能代替用户选择并启动 API。**
3. 调用 `get_continuous_tuning_state({})`。仅在 `active=true`、`state="active"` 且 `observation` 非空时，基于该观察、`evaluation`、`decision_parameter_ids` 与 `constraints` 决定参数。自动模式由软件执行固定轨迹，完整批次和结束读回核对前 `observation=null`，按状态等待；跟随模式等待新鲜同步窗口。历史选区不能代替实时观察。
4. 调用 `apply_continuous_tuning_step`，传入连续会话 `session_id`、本轮唯一 `step_id`、最新 `observation.id` 作为 `observation_id`、数值候选 `values` 和具体依据 `reason`。`values` 只能包含 `decision_parameter_ids`，不能改目标轨迹。前馈多点模式仅包含 Kff/Bff，PID 固定不动。
5. 小数候选会在当前允许范围内自动对齐配置写入刻度；有数值单次上限的项还会检查该上限，原始越界建议仍拒绝。自动 PID 返回 `constraints.max_change=null`、`single_change_limited=false`，表示本项无单次变化上限；`adaptive_change_hint` 仅供参考，不应作为下发上限。固定 PID、目标值和前馈按各自实际返回的数值上限执行。回执中的 `requested_values` 是原建议，`submitted_values` 是实际提交，`alignment` 是对齐明细，`actual_parameters` 是设备读回；只有 `readback_verified=true` 才表示本轮写入经应答和独立 GET 核对。对齐后无有效变化时返回 `needs_observation`，不下发、不增加成功调参轮次，继续等新观察。自动模式必须完成同一轨迹的下一组复测，最后一轮候选也要复测。用户停止、超时或保护异常可提前中断，未完成的试验不能称为已验证效果。
6. 默认用户停止模式下，达到目标可调用 `observe_continuous_tuning(session_id, observation_id, reason)` 保持参数并重新采样；旧观察失效，等待新完整观察，性能变差时再调整。用户要求结束或无法继续取得可信证据时调用 `stop_continuous_tuning`；有限模式仍受本次预算约束。失败或部分写入先核对当前状态，不自动回滚或重复发送未知结果的请求。新会话须用户重新确认。

连续会话确认后，合法步骤不再逐轮弹确认；仍受设备边界、两套用途权限、本次目标安全范围、显式启用的时间 / 轮数上限、遥测质量、写入应答和读回检查。软件的“停止写入”只停止后续命令，**不能代替设备的物理急停**。

API 模式由软件调用已保存的模型与密钥，每个完整有效批次最多分析一次；默认持续运行到用户停止，保持观察也会消耗 API 用量，有限模式按原分析预算计数。采样等待和末轮复测不额外请求模型；过期建议丢弃并重新采样。说明性字段和 JSON 外层代码块自动兼容，无法解析的回复保持参数并按连续异常次数等待 5/10/20/40/60 秒，再用新波形正常继续；每次真实请求仍计入预算，不立即重复请求同一窗口。等待期间暂停目标轨迹，遥测保护持续有效。网络、设备及实际写入错误仍按原保护停止。目标轨迹切换另计目标写入，受安全范围和单次上限约束。相对评分、AI 的 complete 建议和实测验收是不同结论；默认模式中达到目标后保持参数继续观察。

### 场景四：设计和调整 PID 线性前馈

当前模型为 `最终输出 = PID(SP − PV) + Kff × SP + Bff`，适用于负载一致时的稳态线性关系。先让基础 PID 闭环稳定，再用不同目标的工况设计前馈，最后复测整体响应。Kff 单位为 OUT/SP，Bff 与 OUT 同单位；它不替代加速度前馈或正反转摩擦模型。

1. 固件必须已实现真实的 Kff/Bff 参数读写，并在控制周期中与 PID 求和、统一限幅和处理积分饱和。没有对应接口时可离线研究系数，不能把软件保存配置描述成前馈已生效。MCP 不逐点发送瞬时控制输出。
2. 打开 AI 页的金色“PID 线性前馈设计”。新手草稿自动填入名称、建议地址、关联单位及估算范围；“补齐默认参数”只填空白。新建参考值 0 不等于设备读回；20/21 只表示本地未冲突的建议，不能当成任意固件的实际地址。
3. 核对不同的 SP/PV/OUT 关联、SP/PV 相同单位，确认 OUT 为包含已启用前馈的最终控制输出。修改映射前断开设备，保存本地配置；总开关决定前馈是否加入联合范围，每行“诊断 / 连续”决定参数用途。新参数用途默认勾选为草稿，已有明确保存的选择保留；总开关、OUT 核对及本次会话须由用户确认。前馈用途不复用 P/I/D，另一用途和本次会话授权分别保留。
4. 采集或导入至少三个不同目标的完整稳定平台，保持负载一致，框选分析区间。点击“离线设计选定波形”，或先 `get_linear_feedforward_state` 再调用 `design_linear_feedforward`。本地程序筛除过渡、跟踪偏差、明显波动和接近已配置输出上限的平台，按整组目标做留出检验；返回候选、误差及适用范围，不调用 API 或写设备。
5. 用户可以编辑下方 Kff/Bff，并“审阅前馈待写入值”进入输入调参表，确认后才下发。前馈菜单的“AI 联合诊断 PID 与前馈”使用已保存 API 配置分析选区；主页面“诊断选定波形”只处理 PID / 目标值，不带前馈上下文。建议仍受本用途权限、范围、刻度和单次变化上限约束。
6. 连续调整请点击前馈窗口底部“AI 连续调参 PID 与前馈”，选择 API 或 MCP。“前馈多点整定”固定 PID，只调 Kff/Bff；其他测试方式按本次允许项执行。沿用场景三的完整新观察、约束和独立读回；单次 `write_parameters` 仍等待软件确认。

MCP 配置字段位于完整设备配置的 `linear_feedforward[实际 task_id]`，示例为：

```json
{
  "enabled": true,
  "gain_parameter": "Kff",
  "bias_parameter": "Bff",
  "output_kind": "total_control_effort",
  "min_plateau_seconds": 0.5,
  "tracking_tolerance": 0.03,
  "max_relative_error": 0.1
}
```

此片段不能单独代替完整设备配置。对应参数还须有真实地址、单位、上下限、刻度及 `ai_workflows[task_id]` 中本用途的允许项。按前述配置读取、校验和用户审阅流程保存，不因新增映射自动授予写入会话。前馈报告中的候选值不能替代设备独立读回。

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

## MCP 工具速查：57 个

`—` 表示无输入参数。下表是 V1.8.15 的工具索引；调用时以客户端提供的实际工具 schema 为准。

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
| `get_linear_feedforward_state` | — | 读取已保存前馈映射、参数和两套用途约束，不读取设备 |
| `design_linear_feedforward` | — | 对当前选区做本地多稳态平台前馈设计，不保存、调用 API 或写设备 |
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
| `observe_continuous_tuning` | `session_id`、`observation_id`、`reason` | 保持参数并丢弃当前观察，重新收集完整新窗口；不调用软件 API 或扩大授权 |
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
先核对设备是否支持运行中改参，再提示我设置连续权限和本次安全范围，启动来源选择 MCP。
等我在软件里选择测试方式并确认会话后，按新鲜完整观察和本地评价逐轮分析，
只调整 decision_parameter_ids，保留确认轨迹，执行受限步骤并核对读回；
达到目标后保持参数继续观察；证据不足时重新观察，收到我的停止要求或保护异常时结束，并报告实际验证的结果。
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
| 连续候选小数多于配置刻度 | 软件自动对齐合法刻度；原始越界或超出本项实际启用的单次上限仍拒绝。自动 PID 的参考幅度不作上限；对齐后无有效变化会保持观察 |
| 连续 API 回复格式暂不可用 | 当前参数保持，延长等待并用新波形自动继续；状态会说明等待时间，实际请求仍计入预算 |
| 请求超时、返回 running | 用原回执 id 查询 get_operation，不重复发送参数修改 |

更详细说明随压缩包提供：`docs/CODEX_CONTROL.md`、`docs/MCP_DEVICE_CONFIGURATION.md`、`docs/MCP_WORKBENCH_TOOLS.md`、`docs/AI_PARAMETER_LIMITS.md`、`docs/MCP_CONTINUOUS_TUNING.md`、`docs/PID_LINEAR_FEEDFORWARD.md`。

## 使用许可

免费使用的授权范围见 [LICENSE.txt](LICENSE.txt)。未经作者书面许可，不得销售软件副本、换标收费分发或商业再分发本项目原创应用实现的衍生版本。第三方组件仍受各自许可证约束；协议接入示例可用于使用者自己的设备工程。
