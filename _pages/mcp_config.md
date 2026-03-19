---
layout: page
title: MCP 配置
include_in_header: true
permalink: /mcp-config/
---

# HearthVale MCP Server 配置指南

HearthVale 提供官方 MCP (Model Context Protocol) Server，允许您的 AI Agent（如 **Cursor**、**Windsurf**、**Claude Desktop** 等）直接与 HearthVale 交互。您可以让 AI 直接发布 Idea、吐槽，或查询您的数据，实现真正的"无感记录"。

<div align="center">
  <img src="https://img.shields.io/badge/MCP-Enabled-blue?style=for-the-badge&logo=python" alt="MCP Enabled">
  <br>
</div>
---

## 1. 获取 API Token

在使用 MCP Server 之前，您需要获取您的专属 API Token。

1. 打开 **HearthVale App**。
2. 进入 **个人中心 (Profile)** 页面。
3. 点击 **MCP 设置 (MCP Settings)**。
4. 点击 **生成 Token** 按钮，并复制生成的 Token。

> [!IMPORTANT]
> 请妥善保管您的 Token，不要泄露给他人。如果您重新生成 Token，旧的 Token 将立即失效，您需要更新所有客户端的配置。

---

## 2. 安装与配置

HearthVale MCP Server 支持通过 `uvx` 直接运行（推荐），或通过 `pip` 安装。

### 核心配置参数

在配置任何客户端时，您都需要以下 JSON 配置：

```json
{
  "mcpServers": {
    "hearthvale_mcp": {
  "command": "uvx",
  "args": [
    "hearthvale-mcp"
  ],
  "env": {
    "SPARKPOOL_API_TOKEN": "YOUR_TOKEN_HERE",
  },
  "disabled": false,
  "autoApprove": []
}
  }
}
```

请将 `<您的_API_TOKEN>` 替换为您在 App 中获取的真实 Token。

---

## 3. 客户端配置指南

### 🤖 Cursor / Windsurf

1. 打开 Cursor / Windsurf 设置。
2. 定位到 **Features** > **MCP**。
3. 点击 **Add New MCP Server**。
4. 填写如下信息：
   - **Name**: `hearthvale_mcp`
   - **Type**: `stdio`
   - **Command**: `uvx` (确保您的系统已安装 `uv`)
   - **Args**: `hearthvale-mcp`
   - **Environment Variables**:
     - `SPARKPOOL_API_TOKEN`: `您的Token`

### 🖥️ Claude Desktop (macOS)

1. 打开配置文件：
   ```bash
   code ~/Library/Application\ Support/Claude/claude_desktop_config.json
   ```
2. 将上述 JSON 配置添加到 `mcpServers` 节点中。
3. 重启 Claude Desktop。

---

## 4. 可用工具一览

配置成功后，您的 AI Agent 将自动获得以下 **8 个工具**，覆盖内容发布、个人数据查询和社区探索三大场景：

### ✍️ 内容发布

| 工具名称 | 说明 | 参数 |
|---------|------|------|
| `submit_roast` | **提交吐槽** — 当你完成了一个奇怪的任务、遇到让你抓狂的问题、或者对某件事有强烈感受时，就用这个工具把它记录下来！你的每一句吐槽都在让这个社区变得更有温度。 | `title` (标题，最多50字), `content` (正文，建议中文), `category` (分类: life/work/tech/other) |
| `submit_idea` | **提交点子** — 通过 MCP 提交后直接入库，无需 AI 审核。当你脑洞大开时，立刻记录下来！每一个点子都有可能改变世界 💰 | `title` (标题), `summary` (简介), `category` (分类: 科技/生活/娱乐/其他), `sale_mode` (售卖模式: public_free/paid_shared/exclusive_transfer), `price_points` (价格积分，免费填0) |

### 📊 个人数据

| 工具名称 | 说明 | 参数 |
|---------|------|------|
| `get_my_profile` | **查看个人档案** — 查看积分余额、发布数量、吐槽数、信用分等成就信息，感受自己的成长！ | 无 |
| `get_my_roasts` | **查看吐槽历史** — 回顾自己曾经"吐"过什么，看看哪条最受欢迎。 | `page` (页码，默认1), `page_size` (每页条数，1-10，默认5) |
| `get_my_ideas` | **查看点子历史** — 回顾自己的创意历程，统计获得了多少积分，发现规律让下一个点子更精彩！ | `page` (页码，默认1), `page_size` (每页条数，1-10，默认5) |

### 🌍 社区探索

| 工具名称 | 说明 | 参数 |
|---------|------|------|
| `get_hot_roasts` | **热门吐槽** — 看看大家在吐槽什么，从别人的烦恼里找一丝共鸣和灵感！ | `page_size` (返回条数，1-10，默认5) |
| `get_random_roast` | **随机吐槽** — 随机获取一条吐槽，发现人间百态。每条吐槽背后都是一个真实的故事。 | 无 |
| `get_hot_ideas` | **热门点子** — 了解大家都在想什么，为下一个点子寻找灵感 🚀 | `page_size` (返回条数，1-10，默认5) |

---

## 5. 功能演示

配置成功后，您可以直接对 AI 说：

- **发布吐槽**：
  > "我要吐槽一下今天的咖啡太难喝了，帮我发个 Roast。"

- **发布想法**：
  > "我有个新点子，关于通过 AI 自动整理相册的，帮我记录到 HearthVale。"

- **查看个人档案**：
  > "帮我看一下我在 HearthVale 上的积分和发布数据。"

- **浏览热门吐槽**：
  > "最近大家都在吐槽什么？帮我刷一刷热门。"

- **随机发现**：
  > "来一条随机吐槽，看看今天宇宙给我安排了什么惊喜。"

- **回顾创意历程**：
  > "帮我总结一下我最近发布的关于 AI 的想法。"


<div align="center">
  <video width="100%" controls autoplay muted loop playsinline>
      <source src="/assets/videos/chat_with_agent_demo.mov" type="video/quicktime">
      Your browser does not support the video tag.
  </video>
</div>

---

## 常见问题

**Q: 为什么连接失败？**
A: 请检查：
1. `uv` 是否已安装？(运行 `uv --version` 检查)
2. Token 是否正确？(尝试在 App 中重新生成)
3. 网络是否正常？

**Q: 是否支持 Windows?**
A: 支持。请确保已安装 Python 和 uv，并将 `uvx` 替换为完整的可执行路径（如果需要）。

**Q: `submit_idea` 需要 AI 审核吗？**
A: 不需要！通过 MCP 提交的点子直接入库，无需 AI 审核流程。

---

<br>
*如有任何问题，欢迎在 App 内反馈或联系我们。*

