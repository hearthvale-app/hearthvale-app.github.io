---
layout: page
title: MCP 配置
include_in_header: true
permalink: /mcp-config/
---

# HearthVale MCP Server 配置指南

HearthVale 官方提供 MCP (Model Context Protocol) Server，允许您的 AI Agent（如 **Cursor**、**Windsurf**、**Claude Desktop** 等）无缝连接 HearthVale 数据库。您可以让 AI 直接发布约拍需求、寻找同城摄影搭子，或查询杭州最新拍照打卡点，实现真正的"无感约拍"。

<div align="center">
  <img src="https://img.shields.io/badge/MCP-Enabled-blue?style=for-the-badge&logo=python" alt="MCP Enabled">
  <br>
</div>
---

## 1. 获取 API Token

在使用 MCP Server 之前，您需要获取您的专属 API Token。

1. 打开 **HearthVale (墟里) App**。
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

请将 `YOUR_TOKEN_HERE` 替换为您在 App 中获取的真实 Token。

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

配置成功后，您的 AI Agent 将自动获得以下 **8 个工具**，覆盖发布需求、个人数据查询和同城发现三大场景：

### ✍️ 约拍与创作

| 工具名称 | 说明 | 参数 |
|---------|------|------|
| `submit_shoot_request` | **发布约拍需求** — 想要周末去西湖拍汉服？直接告诉 AI，它会帮你生成并发布约拍邀请！ | `title` (标题), `location` (地点，如"太子湾"), `style` (风格: 日系/汉服/胶片等), `role` (你的身份: 摄影师/模特) |
| `submit_portfolio` | **上传代表作** — 通过 AI 快速整理并发布你的摄影作品或模特样片，丰富你的作品墙。 | `title` (标题), `description` (描述), `style_tags` (风格标签数组) |

### 📊 个人档案与信用

| 工具名称 | 说明 | 参数 |
|---------|------|------|
| `get_my_profile` | **查看档案与信用** — 查看你的芝麻信用徽章状态、近期收到的双向互评和违约记录。 | 无 |
| `get_my_requests` | **查看我的约拍** — 回顾你发布的约拍请求，以及当前的订单搓合状态。 | `status` (状态: 招募中/进行中/已完成，可选) |

### 🌍 发现与匹配

| 工具名称 | 说明 | 参数 |
|---------|------|------|
| `get_local_recommendations` | **同城推荐** — 寻找杭州同城风格匹配、距离近且信用分高的摄影师或模特搭子。 | `role_needed` (需要: 摄影师/模特), `style` (期望风格) |
| `get_hangzhou_locations` | **杭州拍照地图** — 查询杭州当季最热门的网红打卡点及周边动态（如樱花季、桂花季专属推荐）。 | `season` (季节，如"春季/樱花"), `region` (区域，如"西湖区") |
| `get_hot_portfolios` | **热门作品墙** — 浏览最近在杭州本地大受欢迎的代表作摄影图集，寻找拍摄灵感！ | `page_size` (返回条数，1-10，默认5) |

---

## 5. 功能演示

配置成功后，您可以直接对 AI 说：

- **发布约拍**：
  > "我是一个新人摄影师，想这个周末去太子湾拍一套春日胶片写真，帮我发个模特互勉招募。"

- **寻找搭子**：
  > "帮我找找杭州同城擅长拍汉服且信用分高于 650 分的优质摄影师。"

- **查询地点**：
  > "现在杭州哪里最适合拍情绪大片？帮我看下最新的杭州拍照地图。"

- **个人档案**：
  > "帮我查下我现在的履约信用评估是多少，还有几个待完成的约拍订单。"

- **浏览作品**：
  > "帮我看看最近比较热门的日记风格的作品墙，我想找点构图灵感。"


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

---

<br>
*如有任何约拍纠纷或系统问题，欢迎在 App 内反馈或联系客服。*

