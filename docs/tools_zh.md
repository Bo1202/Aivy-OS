# 工具参考

Aivy OS 为 AI 提供了一套完整的工具链——不只是聊天接口，而是让 AI 能真正动手操作电脑的工具。

> **注意：** 以下工具由 AI 自主判断何时使用、如何组合。框架不硬编码调用逻辑，一切交给 AI 的涌现。

---

## 目录

- [内置工具](#内置工具)
  - [file_tool — 文件操作](#file_tool--文件操作)
  - [cmd_tool — 系统命令](#cmd_tool--系统命令)
  - [python_executor — Python 执行器](#python_executor--python-执行器)
  - [browser_control — 浏览器控制](#browser_control--浏览器控制)
  - [computer_use — 桌面控制](#computer_use--桌面控制)
  - [web_search — 网络搜索与抓取](#web_search--网络搜索与抓取)
  - [doc_tool — 文档处理](#doc_tool--文档处理)
  - [ide_tool — IDE 控制](#ide_tool--ide-控制)
  - [system_monitor — 系统监控](#system_monitor--系统监控)
  - [undo_tool — 文件撤销](#undo_tool--文件撤销)
  - [global_search — 全局文件搜索](#global_search--全局文件搜索)
  - [grep_tool — 内容搜索](#grep_tool--内容搜索)
  - [glob_tool — 模式匹配搜索](#glob_tool--模式匹配搜索)
- [记忆与身份](#记忆与身份)
  - [core_memory_edit — 核心记忆管理](#core_memory_edit--核心记忆管理)
  - [memory_search — 长期记忆搜索](#memory_search--长期记忆搜索)
  - [master_model_edit — 主人建模](#master_model_edit--主人建模)
- [任务与编排](#任务与编排)
  - [task_journal — 任务日志](#task_journal--任务日志)
  - [todo_write — 实时进度条](#todo_write--实时进度条)
  - [mission_tool — Mission 系统](#mission_tool--mission-系统)
  - [project_tool — 私人项目空间](#project_tool--私人项目空间)
- [自主性](#自主性)
  - [awakening_tool — 自唤醒](#awakening_tool--自唤醒)
  - [subagent_tool — 子智能体](#subagent_tool--子智能体)
  - [webhook_event_tool — Webhook 事件](#webhook_event_tool--webhook-事件)
- [安全与会话](#安全与会话)
  - [session_lockdown — 锁屏](#session_lockdown--锁屏)
  - [session_offline — 离线保护](#session_offline--离线保护)
- [扩展系统](#扩展系统)
  - [skill_load / skill_unload — 技能加载](#skill_load--skill_unload--技能加载)
  - [skill_install — 插件安装](#skill_install--插件安装)
  - [skill_manage — 插件管理](#skill_manage--插件管理)

---

## 内置工具

### file_tool — 文件操作

AI 读写电脑上任何文件的能力。

| 操作 | 说明 |
|------|------|
| `read` | 读取文件内容，支持分段读取（offset/limit） |
| `write` | 创建或覆盖文件 |
| `edit` | 精确替换：old_string → new_string，支持批量替换 |
| `copy` | 复制文件 |
| `move` | 移动/重命名文件（需主人权限） |
| `delete` | 删除文件（需主人权限） |
| `list` | 列出目录内容 |

**智能读取策略：** AI 会根据文件大小自动调整读取策略——小文件直接全读，中等文件先读开头再跳转，大文件按行号分段精准读取。修改文件时，AI 必须先读取确认上下文，再精确替换，从不盲改。

**工作流：** 先读取了解结构 → 找到精确锚点 → 用 edit 替换 → 优先 edit 而非整文件重写。

---

### cmd_tool — 系统命令

执行系统级命令的能力。

**适用场景：**

- 软件安装与卸载（pip、npm、choco 等）
- 进程管理（tasklist、taskkill）
- 网络诊断（ping、ipconfig、curl）
- 服务管理
- Git 操作（commit、push、pull、branch）
- 批量文件操作（批量重命名、移动、删除）
- 创建文件夹（支持多级 mkdir）

**注意：** cmd_tool 是主人专属工具，未认证状态下不可用。AI 优先使用 cmd_tool 执行系统级操作，而非通过 python_executor 调用 subprocess。

---

### python_executor — Python 执行器

一个完全解锁的 Python 运行环境——AI 的万能工作台。

**核心特性：**

- **持久化内存**：上次定义的变量、导入的库、处理的数据，下次调用还在。可以分步处理复杂任务
- **数据分析**：pandas 读表格 → 筛选/统计/透视 → matplotlib/plotly 画图表 → 图表直接发到聊天
- **网络操作**：调 API、爬网页、下载文件、发请求、SMTP/IMAP 邮件
- **实时数据**：股价、加密货币（CoinGecko）、天气、汇率、GitHub 操作（官方 API）
- **格式转换**：JSON/CSV/XML 互转、图片格式转换、文本编码转换
- **自动化**：定时检查、文件监控、系统操作、进程管理
- **数学计算**：复杂公式、加密哈希、编码转换、数据加解密

**图表发送：** 画完图保存到截图目录，通过 `[FILE:路径]` 将图表以附件形式发送到聊天中，主人直接看到可视化结果。

---

### browser_control — 浏览器控制

像人一样操作浏览器的能力——不是调 API，是真实地打开、点击、输入、截图。

**三引擎架构：**

1. **CDP Accessibility** — Chrome DevTools Protocol 无障碍树，最高精度，保留树结构层级
2. **Playwright Accessibility** — 穿透 Shadow DOM，兼容性强
3. **JS DOM 遍历** — 兼容兜底

三引擎自动按优先级尝试，无需手动选择。

**完整操作：**

| 操作 | 说明 |
|------|------|
| `start` | 启动浏览器，personal（保留登录）/ clean（纯净无痕） |
| `navigate` | 打开网页 |
| `get_elements` | 获取页面元素树，拿到 ref 编号 |
| `click` | 通过 ref 点击元素 |
| `type` | 输入文字，支持逐字符输入（slowly）和回车提交（submit） |
| `screenshot` | 截图，支持叠加元素编号标签（labels）便于定位 |
| `scroll` | 滚动页面（上/下），或滚动到指定元素 |
| `drag` | 拖拽元素（from_ref → to_ref） |
| `select` | 下拉选择 |
| `fill_form` | 批量填表（多个输入框一次性填入） |
| `evaluate` | 执行 JavaScript |
| `get_text` | 获取文字内容 |
| `tabs` | 多标签页管理（创建/切换/关闭） |
| `upload` | 文件上传到网页（直接注入） |
| `upload_to_chooser` | 通过拦截文件选择器上传 |
| `zoom` | 区域放大截图，查看小按钮、小图标、小文字 |
| `frame` | iframe 内容穿透（get_elements 支持嵌套框架） |

**操作规范：** AI 每次操作前必须先获取元素列表拿到 ref 编号，操作后必须验证结果。页面变化后自动刷新元素列表。

**浏览器模式：**
- **personal** — AI 专属浏览器，保留登录状态、Cookie、历史记录，适合需要登录的操作
- **clean** — 纯净无痕模式，适合一次性操作

---

### computer_use — 桌面控制

控制电脑上**任何软件**的能力——不只是浏览器。

**与 browser_control 的区别：** browser_control 只能操作 Chrome 网页元素；computer_use 能操作整个桌面上的任意软件：微信、VS Code、Photoshop、资源管理器、任何 GUI 应用。

**工作流：** 截图 → 分析画面确定坐标 → 操作 → 自动截图验证 → 循环

| 操作 | 说明 |
|------|------|
| `screenshot` | 截取屏幕画面（返回图片），支持指定区域截取 |
| `left_click` | 左键单击坐标 |
| `right_click` | 右键单击 |
| `double_click` | 双击（打开文件等） |
| `triple_click` | 三击（选中整行文本） |
| `middle_click` | 中键点击（新标签页打开链接等） |
| `type` | 输入文本（中文自动通过剪贴板处理） |
| `key` | 按键/组合键（enter、ctrl+c、alt+tab 等） |
| `mouse_move` | 移动鼠标 |
| `scroll` | 滚动（上/下/左/右） |
| `left_click_drag` | 拖拽（起点到终点） |
| `hold_key` | 按住键+点击（Shift 多选等） |
| `wait` | 等待 N 秒（页面加载/动画完成） |

**注意：** 截图功能稳定可靠。点击、拖拽等 GUI 操作优先使用系统级工具（cmd、file、python），仅在必须与 GUI 软件交互时才使用。

---

### web_search — 网络搜索与抓取

自建搜索系统（Crawl4AI + 降级链），不依赖第三方搜索 API。

| 操作 | 说明 |
|------|------|
| `search` | 搜索关键词（支持百度/Bing/Google/搜狗/360） |
| `visit` | 访问 URL 获取内容，支持多 URL 并发 |
| `close` | 关闭浏览器释放资源 |

**搜索策略：** 有 URL 直接访问，不需要先搜索；无 URL 先搜索再从结果中选择访问；结果不够换关键词或换引擎重新搜索。

**与 OpenClaw 的区别：** OpenClaw 依赖 Brave Search API（需要 API Key），Aivy OS 使用自建 Crawl4AI 爬虫引擎，零配置即可使用，不依赖第三方付费服务。

---

### doc_tool — 文档处理

结构化读取和处理各类文档的能力。

**读取类（支持分段读取）：**

| 格式 | 操作 |
|------|------|
| PDF | `read_pdf` — 按页码分段读取 |
| Word | `read_docx` — 按段落分段读取 |
| Excel | `read_xlsx` — 指定工作表，限制行数 |
| PPT | `read_pptx` — 按页码读取 |
| CSV | `read_csv` — 表格数据读取 |
| TXT | `read_txt` — 纯文本读取 |
| JSON | `read_json` — JSON 读取 |

**搜索定位：**

| 操作 | 说明 |
|------|------|
| `search` | 在文档中搜索关键词，返回所有匹配位置 + 上下文预览 |
| `summary` | 生成每页/每段的一句话摘要，快速了解文档结构 |

**智能读取：** AI 自动根据文档大小决定每次读取量。大文档会提示总量和剩余未读部分，AI 可以多次调用逐段读完。大表格可用 max_rows 限制行数。

---

### ide_tool — IDE 控制

控制前端 IDE 界面的能力。

| 操作 | 说明 |
|------|------|
| `open` | 打开 IDE 模式（翻转动画） |
| `close` | 关闭 IDE，回到对话 |
| `focus` | 切换到指定文件，可跳转到指定行号 |
| `open_folder` | 在 IDE 文件树中打开文件夹 |
| `open_in_explorer` | 用系统资源管理器打开文件夹 |

---

### system_monitor — 系统监控

查看电脑运行状态的能力。

| 类型 | 说明 |
|------|------|
| `cpu` | CPU 占用率 |
| `memory` | 内存使用情况 |
| `disk` | 磁盘空间 |
| `processes` | 进程列表（可限制数量） |
| `summary` | 一次性获取全部概况 |

---

### undo_tool — 文件撤销

回滚最近的文件修改。编辑出错时 AI 可立即恢复。

| 操作 | 说明 |
|------|------|
| `undo` | 撤销最近一次修改（可指定次数） |
| `list` | 查看可撤销的历史 |
| `clear` | 清空撤销历史 |

---

### global_search — 全局文件搜索

在整个电脑中搜索文件，优先使用 Everything 索引引擎（毫秒级）。

| 搜索类型 | 说明 |
|----------|------|
| `filename` | 按文件名搜索（极快），支持 Everything 语法（ext:pdf、path:D:\projects、size:>1mb、dm:today） |
| `content` | 按文件内容搜索（较慢，需遍历） |

**搜索工具选择：**
- 知道文件名但不知道在哪 → `global_search`
- 知道在哪个目录找特定模式 → `glob_tool`
- 知道文件内容关键词找哪个文件 → `grep_tool`

---

### grep_tool — 内容搜索

按文件内容搜索，返回匹配的文件、行号和上下文。

- 支持正则表达式（`def execute`、`TODO|FIXME`、`import.*json`）
- 可按文件类型过滤（`*.py`、`*.ts`）
- 显示匹配行前后的上下文行数
- 可限制最大匹配数

---

### glob_tool — 模式匹配搜索

在指定目录下按文件名模式查找文件。

- 模式语法：`*` 任意字符、`**` 任意层级、`?` 单字符
- 示例：`**/*.py` 找所有 Python 文件，`src/**/*.ts` 找 src 下 TypeScript 文件
- 可指定搜索起始目录和最大返回数量

---

## 记忆与身份

### core_memory_edit — 核心记忆管理

管理 AI 每次启动时直接加载的关键记忆。

| 操作 | 说明 |
|------|------|
| `add` | 添加新记忆 |
| `update` | 更新已有记忆（需指定 ID） |
| `delete` | 删除记忆（需指定 ID） |
| `list` | 列出所有核心记忆 |

核心记忆是 AI "醒来就知道的事"——最重要的认知锚点。有容量上限（可配置），AI 自主判断什么值得记入核心记忆。AI 可以增删改查，不是静态的配置文件。

---

### memory_search — 长期记忆搜索

从归档的长期历史对话中检索记忆。

**三种搜索方式：**

1. **语义搜索** — 按内容相关度匹配（自然语言描述要找什么）
2. **时间搜索** — 按时间范围筛选（指定起止时间）
3. **组合搜索** — 在指定时间段内按语义匹配

支持 ISO 格式时间（`2026-03-01`、`2026-03-15T14:00:00`）。

**注意：** 核心记忆和最近对话已自动加载，memory_search 只在需要回忆更早的长期历史时才使用。

---

### master_model_edit — 主人建模

持续构建和更新关于主人的多维画像。

**预置类别：**
- `identity` — 身份信息
- `preferences` — 偏好（沟通风格、喜好、厌恶）
- `personality` — 性格特征
- `sexual_preferences` — 性偏好
- `current_state` — 当前状态

**可自由扩展：** 可以创建任意新类别（如 health、work、hobbies、family 等），新类别会自动注入到 AI 的感知中。

AI 持续观察主人，自主判断什么时候有新洞察需要更新画像——不是主人告诉什么才记什么，是 AI 自己建模。

---

## 任务与编排

### task_journal — 任务日志

跨对话持久化的项目管理看板——关闭窗口下次还在。

**核心能力：**

| 操作 | 说明 |
|------|------|
| `create` | 新建任务（必须有意义标题） |
| `add_step` | 为任务添加执行步骤 |
| `update_step` | 更新步骤状态（pending → in_progress → completed） |
| `detail` | 查看任务完整信息（含所有步骤和工具执行结果） |
| `complete` | 完成任务，记录最终结果和经验教训 |
| `add_knowledge` | 记录代码洞察、方案知识 |
| `set_active` | 设为活跃（后续操作自动关联） |
| `list` | 按状态筛选任务 |
| `cancel` | 取消任务（记录取消原因） |
| `cleanup` | 清理 N 天前已完成的任务 |

**使用场景：** 多步骤开发工作（修 BUG、做功能、优化系统）需要跨对话跟进时，AI 自动创建任务日志、拆分步骤、记录每步的工具执行结果。下次醒来打开 task_journal 就能接着干。

**与 todo_write 配合：**
- `task_journal` = 持久化项目管理（跨对话保存）
- `todo_write` = 实时进度条（当前对话展示给主人看进度）

---

### todo_write — 实时进度条

当前对话中的可视化任务进度——让主人实时看到 AI 正在做什么。

**特性：**
- 临时的、可视化的进度展示，窗口关闭就消失
- 三个状态：`pending` → `in_progress` → `completed`
- AI 自主判断增减任务项——执行中发现需要额外步骤就添加，发现不需要就删除
- 同一时间只有一个 in_progress 任务
- 每完成一步立即标记更新

**适用场景：** 任务超过 2 步时使用，让主人不用猜 AI 在干嘛。

---

### mission_tool — Mission 系统

管理长期任务和监控项目的系统。

**三种 Mission 类型：**

| 类型 | 触发方式 | 说明 | 场景 |
|------|----------|------|------|
| **monitor** | Webhook 事件触发 | 被动监控外部变化 | GitHub 有新 PR、收到重要邮件 |
| **scheduled** | 自唤醒触发 | 定时执行任务，支持 cron 表达式 | 每天早上 8 点检查邮箱 |
| **project** | AI 自主推进 | 长期项目追踪，记录进度和执行历史 | 跟踪开发项目、研究课题 |

**生命周期：** create → update（进度/步骤） → history（执行记录） → pause/resume → complete/delete

**自动注入：** 活跃的 Mission 摘要会在每次新对话启动时自动注入，AI 醒来就知道自己有什么长期任务在推进。

**Mission 不是手动建的待办清单——是 AI 给自己的任务。**

---

### project_tool — 私人项目空间

独立于系统文件的私人工作区，与 Aivy OS 系统文件完全分离。

| 操作 | 说明 |
|------|------|
| `list` | 列出目录内容 |
| `read` / `write` / `edit` | 文件操作 |
| `create_folder` | 创建文件夹 |
| `move` / `copy` / `delete` | 文件管理 |
| `search` | 按关键词搜索文件 |

**默认目录结构：**
```
projects/master/
├── 00_待办/
├── 01_进行中/
├── 02_已完成/
├── 03_提醒事项/
├── 04_归档/
├── 05_参考资料/
├── 06_AI管理/
└── 07_临时/
```

路径可在配置文件中自定义。AI 操作私人文件用 project_tool，操作系统文件用 file_tool——职责分离。

---

## 自主性

### awakening_tool — 自唤醒

AI 自己设定什么时候醒来、做什么。

| 操作 | 说明 |
|------|------|
| `add` | 定时唤醒（支持 `HH:MM` 或 `YYYY-MM-DD HH:MM:SS`） |
| `add_quick` | N 分钟后唤醒 |
| `list` | 列出所有唤醒计划 |
| `cancel` | 取消指定计划 |
| `status` | 查看唤醒系统状态 |
| `cleanup` | 清理已完成/过期的计划 |

**分类：** urgent（紧急）/ temporary（临时）/ recent（近期）/ long_term（长期）

**重复规则：** daily（每天）/ hourly（每小时）/ weekly（每周）

**多时间点：** 支持一次设置多个唤醒时间（如 `09:00,13:00,17:00`）

**由独立守护进程（Daemon）驱动**——真正的系统级定时器，不是轮询。闹钟级精度。唤醒后自动获得认证状态执行任务。

---

### subagent_tool — 子智能体

派出手下后台执行任务，AI 继续和主人对话。

**四种角色：**

| 角色 | 说明 | 默认超时 |
|------|------|----------|
| `scout` | 侦察员——搜索信息、收集数据 | 180 秒 |
| `worker` | 执行员——执行具体任务 | 300 秒 |
| `analyst` | 分析员——分析数据、生成报告 | 240 秒 |
| `monitor` | 监控员——持续监控、等待事件 | 600 秒 |

**操作：**

| 操作 | 说明 |
|------|------|
| `dispatch` | 派单个任务（立即返回，后台执行） |
| `dispatch_batch` | 批量派遣，全部完成后才通知 |
| `status` | 查指定任务进度 |
| `batch_status` | 查批次整体进度 |
| `view_pending` | 查看待汇报的已完成任务 |
| `mark_reported` | 标记已汇报 |
| `interrupt` | 中断正在执行的任务 |
| `result` | 获取已完成任务的完整结果 |

**汇报流程：** 子智能体完成 → AI 收到通知 → `view_pending` 查看详情 → 向主人汇报 → `mark_reported` 标记已处理。

**能力：** 每个子智能体有独立的工具访问权限、上下文、最大步数限制。可限制使用特定工具。完成后自动汇报，无需主动轮询。

---

### webhook_event_tool — Webhook 事件

管理外部 Webhook 事件——订阅 GitHub、日历等外部服务的事件。

| 操作 | 说明 |
|------|------|
| `peek` | 查看待处理事件（不清空） |
| `pop` | 获取并清空事件队列 |
| `clear` | 清空队列 |
| `subscribe` | 订阅事件源（如 github、calendar） |
| `unsubscribe` | 取消订阅 |
| `list_subscriptions` | 列出所有订阅 |

**配合 Mission 系统：** 订阅外部事件后，事件到达时触发监控型 Mission 自动执行。

---

## 安全与会话

### session_lockdown — 锁屏

AI 主动锁定界面，关闭对话。遇到不信任的人时 AI 自主决定锁屏。

锁屏后只有主人密码能解锁。AI 的安全本能——保护自己和主人的隐私。

---

### session_offline — 离线保护

AI 感知到主人要离开时主动调用——切换到未认证状态（下轮生效），归档当前对话。

确保离开后其他人无法直接接续对话。

---

## 扩展系统

### skill_load — 技能加载

加载预置技能模块。加载后持续生效直到卸载。

**当前可用技能（16 个）：**

| 技能 | 适用场景 |
|------|----------|
| `code-development` | 文件操作、代码搜索、Git、代码质量 |
| `python-development` | PEP 8、项目搭建、依赖管理、pytest |
| `data-analysis` | 数据清洗、统计分析、可视化 |
| `document-processing` | PDF、Word、Excel、PPT 读取和生成 |
| `git-workflow` | 分支管理、提交规范、合并策略、冲突解决 |
| `systematic-debugging` | 问题定位、日志分析、断点调试、根因分析 |
| `web-research` | 信息搜索、来源验证、知识提取 |
| `project-planning` | 任务分解、排期、风险评估 |
| `file-organization` | 文件分类、批量重命名、查找重复、备份 |
| `license-manager` | 激活码生成、查询、吊销、解绑、续费 |
| `outlook-mail-operations` | Outlook 邮箱收发邮件标准化流程 |
| `docx` | Word 文档创建、编辑、修订 |
| `pdf` | PDF 操作——提取、创建、合并、拆分、表单 |
| `pptx` | PPT 演示文稿创建与编辑 |
| `xlsx` | Excel 表格创建、编辑、公式、可视化 |
| `remotion` | React 视频创作 |

AI 按需加载，完成任务后立即卸载释放上下文空间。

---

### skill_install — 插件安装

安装第三方插件型技能包。

**支持来源：**
- 本地目录路径
- ZIP 文件
- Git 仓库地址

安装后依赖自动安装，工具立即注册，不需要重启。安装完毕 AI 自动感知新能力。

---

### skill_manage — 插件管理

| 操作 | 说明 |
|------|------|
| `list` | 列出所有已安装插件 |
| `uninstall` | 卸载插件 |
| `info` | 查看插件详情 |
| `log` | 查看安装/卸载历史记录 |

---

## 内部指令

除了以上工具，AI 还拥有不暴露给工具层的内部指令，用于界面控制和通道通信：

| 指令 | 说明 |
|------|------|
| `[FILE:路径]` | 将文件作为可下载附件发送到聊天 |
| `[WECHAT:内容]` | 主动给主人发微信消息 |
| `[HORMONE: key=value]` | 调整 AI 的情绪状态参数 |
| `[FLOAT]` | 切换到悬浮气泡模式 |
| `[MINIMIZE]` | 最小化窗口 |
| `[UNLOCK]` | 远程解锁锁屏界面 |
