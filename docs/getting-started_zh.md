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

### 🇨🇳 DeepSeek V4 Pro（推荐首选）

**优势**：
- **1M 长上下文** —— 整本书塞进去聊都行
- **v1.7.2 适配后 cache 99.47% 命中，首字延迟 <1 秒**
- 中文优秀，工具调用稳定
- **输入缓存价格 0.5 元/百万 token** —— 便宜到惊人

**配置步骤**：

1. 打开 [https://platform.deepseek.com](https://platform.deepseek.com) 注册登录
2. 左侧菜单 → "API keys" → 创建新 Key（复制保存，**只显示一次**）
3. 充值 5-10 元（够用很久）
4. Aivy OS 设置面板：
   - 厂商：选 `deepseek`
   - 模型名：填 `deepseek-v4-pro`
   - API Key：粘贴 `sk-xxxxxx`
   - base_url：**留空**（系统自动加 `/v1`）
5. 保存 → 主菜单切换到 `deepseek` 即可

---

### 🌙 Kimi K2.6（月之暗面 - 多模态强）

**优势**：**多模态视觉理解一流** —— 看图、PDF、截图、表格都吃。

1. 注册 [https://platform.moonshot.cn](https://platform.moonshot.cn)
2. 左下角 → "用户中心" → "API Key" → 创建
3. 充值（最低 50 元）
4. Aivy OS：
   - 厂商：`kimi`
   - 模型名：`kimi-k2.6`
   - API Key：粘贴
   - base_url：`https://api.moonshot.cn/v1` ⚠️ **必填，漏 /v1 报错**

---

### 🇨🇳 智谱 GLM-5.1

**优势**：中文优秀，thinking 模式强，国产推理首选。

1. 注册 [https://open.bigmodel.cn](https://open.bigmodel.cn)
2. 个人中心 → API Keys → 创建
3. 充值
4. Aivy OS：
   - 厂商：`glm`
   - 模型名：`glm-5.1`
   - API Key：粘贴
   - base_url：`https://open.bigmodel.cn/api/paas/v4` ⚠️ **必填，注意是 v4 不是 v1！**

---

### 🤖 Claude 系列

**优势**：推理质量高，工具调用稳定，复杂任务表现好。
**注意**：需要海外信用卡 + 网络。

最新模型：
- `claude-opus-4-7` — 旗舰款
- `claude-sonnet-4-6` — 平衡款（推荐）
- `claude-haiku-4-5` — 轻快款

1. 注册 [https://console.anthropic.com](https://console.anthropic.com)
2. Settings → API Keys → Create Key
3. 充值（最低 5 美元）
4. Aivy OS：
   - 厂商：`claude`
   - 模型名：选上面任一（推荐 `claude-sonnet-4-6`）
   - API Key：粘贴 `sk-ant-xxxxxx`
   - base_url：留空（默认正确）

---

### 🌐 OpenAI GPT-5

**优势**：通用能力强。

最新模型：`gpt-5` / `gpt-5-mini` / `gpt-5-nano`（按价格降序）

1. [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys) 创建 Key
2. 充值（最低 5 美元）
3. Aivy OS：
   - 厂商：`openai`
   - 模型名：`gpt-5`（或 `gpt-5-mini` 省钱）
   - API Key：粘贴
   - base_url：留空

---

### 🅖 Google Gemini 2.5

**优势**：多模态视觉 + 长上下文 + **有免费试用额度**。

1. [https://aistudio.google.com/apikey](https://aistudio.google.com/apikey) 创建 Key
2. Aivy OS：
   - 厂商：`gemini`
   - 模型名：`gemini-2.5-pro` 或 `gemini-2.5-flash`
   - API Key：粘贴
   - base_url：留空

---

### 🇨🇳 阿里通义千问

**优势**：中文好，国内访问稳定。

1. [https://bailian.console.aliyun.com](https://bailian.console.aliyun.com) 开通灵积服务 → 创建 API-Key
2. Aivy OS：
   - 厂商：`custom`
   - 模型名：`qwen3.6-plus`
   - API Key：粘贴
   - base_url：`https://dashscope.aliyuncs.com/compatible-mode/v1` ⚠️ **必填**

---

### 💻 Ollama 本地模型（零费用 + 完全隐私）

**优势**：完全离线，零费用，数据不出本机。
**劣势**：速度取决于你的硬件，工具调用稳定性看模型。

1. 安装 [Ollama](https://ollama.com)（一键安装，自动后台运行）
2. 命令行 `ollama pull qwen3.6:14b`（或 `deepseek-coder-v2`、`llama3.3` 等）
3. Aivy OS：
   - 厂商：`ollama`
   - 模型名：`qwen3.6:14b`（你 pull 的模型名）
   - API Key：留空
   - base_url：`http://localhost:11434`（默认）

**硬件建议**：
- 7B 模型：8 GB 内存够用
- 14B 模型：16 GB 内存
- 32B 模型：32 GB 内存（需要 GPU 加速更快）

---

### 🔧 其他 OpenAI 兼容服务

任何兼容 OpenAI 协议的服务都能用——Together AI、OpenRouter、One-API、本地代理等。

配置方式同阿里通义：选 `custom` 厂商 + 服务支持的模型名 + 服务的兼容端点 base_url（**注意带版本号结尾**）+ API Key。

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
