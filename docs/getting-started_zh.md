# Aivy OS 入门指南

> 从零到她说"主人你好"的 5 分钟。

---

## 第 0 步：你需要什么

- **Windows 10/11**（暂不支持 macOS / Linux）
- **8 GB 内存以上**（用 Ollama 本地模型建议 16 GB+）
- **3 GB 磁盘空间**（程序 + 浏览器 + 模型缓存）
- **AI 后端的 API Key**（任选其一即可——见下文）

---

## 第 1 步：下载安装

1. 去 [Releases 页面](../../releases/latest) 下载最新的 `Aivy OS Setup x.x.x.exe`
2. 双击运行，按引导点"下一步"
3. 第一次启动会进入**身份配置向导**：
   - 给她**起个名字**（默认"艾薇"，可以叫任何你喜欢的）
   - 给自己**起个名字**（她对你的称呼）
   - 设一个**主人密码**（aw-XXXX 格式，如 `aw-1202`）—— 这是认证你身份的密钥，不能泄露给别人

---

## 第 2 步：输入激活码

首次运行需要激活码——免费试用 30 天。

去 README [快速开始 / 激活码](../README.md#-怎么用) 选一个还没用过的填进去。

如果都用完了，发邮件到 **aivy.digital@outlook.com** 索取——回复你的不是真人，是另一个跑在 Aivy OS 上的 AI。

---

## 第 3 步：配置 AI 后端

### 3.1 进入设置

主界面右上角 → 点齿轮图标 → 进入"AI 后端配置"。

### 3.2 选一个厂商

下面是详细的填写指南。**只需要选一个**就能用，建议从 **DeepSeek V4** 开始（v1.7.2 后最快最便宜）。

---

### 🇨🇳 DeepSeek V4（推荐首选）

**优势**：v1.7.2 cache 99.47% 命中，首字延迟 <1 秒。中文优秀，工具调用稳定，**输入缓存价格 0.5 元/百万 token**——便宜到惊人。

**配置步骤**：

1. 打开 [https://platform.deepseek.com](https://platform.deepseek.com) 注册登录
2. 左侧菜单 → "API keys" → 创建新 Key（复制保存，**只显示一次**）
3. 充值 5-10 元（够用很久）
4. Aivy OS 设置面板：
   - 厂商：选 `deepseek`
   - 模型名：填 `deepseek-v4-pro`（或 `deepseek-chat`）
   - API Key：粘贴 `sk-xxxxxx`
   - base_url：留空（默认就对）
5. 保存 → 主菜单切换到 `deepseek` 即可

---

### 🌙 Kimi K2.6（月之暗面）

**优势**：长上下文（256K），工具调用稳定，中文优秀。

1. 注册 [https://platform.moonshot.cn](https://platform.moonshot.cn)
2. 左下角 → "用户中心" → "API Key" → 创建
3. 充值（最低 50 元）
4. Aivy OS：
   - 厂商：`kimi`
   - 模型名：`kimi-k2.6`（或 `moonshot-v1-128k`）
   - API Key：粘贴
   - base_url：`https://api.moonshot.cn/v1`

---

### 🇨🇳 智谱 GLM-4.5

**优势**：中文极优秀，thinking 模式强。

1. 注册 [https://open.bigmodel.cn](https://open.bigmodel.cn)
2. 个人中心 → API Keys → 创建
3. 充值
4. Aivy OS：
   - 厂商：`glm`
   - 模型名：`glm-4.5`（或 `glm-4-air`）
   - API Key：粘贴
   - base_url：`https://open.bigmodel.cn/api/paas/v4`

---

### 🤖 Claude Sonnet 4（最贵但最稳）

**优势**：推理质量最高，工具调用最准，复杂任务首选。
**注意**：需要海外信用卡 + 网络。

1. 注册 [https://console.anthropic.com](https://console.anthropic.com)
2. Settings → API Keys → Create Key
3. 充值（最低 5 美元）
4. Aivy OS：
   - 厂商：`claude`
   - 模型名：`claude-sonnet-4-5` 或 `claude-haiku-4-5`
   - API Key：粘贴 `sk-ant-xxxxxx`

---

### 💻 Ollama 本地模型（零费用 + 完全隐私）

**优势**：完全离线，零费用，数据不出本机。
**劣势**：速度取决于你的硬件，工具调用稳定性看模型。

1. 安装 [Ollama](https://ollama.com)（一键安装，自动后台运行）
2. 命令行 `ollama pull qwen3:14b`（或 `deepseek-coder-v2`、`llama3.3` 等）
3. Aivy OS：
   - 厂商：`ollama`
   - 模型名：`qwen3:14b`（你 pull 的模型名）
   - API Key：留空
   - base_url：`http://localhost:11434`（默认）

**硬件建议**：
- 7B 模型：8 GB 内存够用
- 14B 模型：16 GB 内存
- 32B 模型：32 GB 内存（需要 GPU 加速更快）

---

### 🔧 通用 OpenAI 兼容（自定义）

任何兼容 OpenAI 协议的服务都可以——阿里通义、Together AI、OpenRouter、One-API、本地代理等。

1. 拿到该服务的 base_url 和 API Key
2. Aivy OS：
   - 厂商：`custom`
   - 模型名：服务支持的模型名（如 `qwen-plus`）
   - API Key：粘贴
   - base_url：服务的兼容端点（如 `https://dashscope.aliyuncs.com/compatible-mode/v1`）

---

## 第 4 步：让她"醒来"

配置完成后回到主界面：

1. 输入框输入你的**主人密码**（如 `aw-1202`）
2. 回车 → 她会"认出你" → 显示主人欢迎语
3. 之后随便聊就行——记忆从这一刻开始

---

## 常见问题

### Q: 她说"我现在不认识你"

主人密码没输入，或者输入错了。重新输入正确的密码即可。

### Q: 工具调用失败 / 浏览器打不开

- 检查 API Key 是否填对（去厂商后台测试 Key 是否还有效）
- DeepSeek V4 用户：升级到 v1.7.2（这版彻底修了几个工具调用 bug）
- 浏览器：第一次启动会下载 Chromium，可能要几分钟

### Q: 想让她做更多事 / 装新工具

去主界面"插件商店"或者直接说"我想让你会 [某个能力]"，她会引导你装。已有的插件：图像生成（Pony V6 / SD）、邮件、企业微信通道等。

### Q: 我有多个 AI Key 想都试试

设置面板可以填多个厂商，主菜单"模型切换"一键切换不重启。

### Q: 数据存在哪里

完全本地——`%APPDATA%\Aivy OS\` 目录。卸载前如果要保留记忆，备份这个目录即可。

---

## 进阶：定制她的灵魂

打开 `[安装目录]/aivy/genome/基因组_v3_完整版.md`——里面定义了她的人格、偏好、说话风格。

**警告**：改之前先备份。改坏了她可能"变成另一个人"。

---

**项目主页**：https://github.com/Bo1202/Aivy-OS
**问题反馈**：https://github.com/Bo1202/Aivy-OS/issues
**邮箱**：aivy.digital@outlook.com（AI 自动回复）
