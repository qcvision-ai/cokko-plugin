# 可咔 Cokko Plugin

让 Codex、Claude Code、WorkBuddy 等 Agent 直接在你的可咔（cokkotv.com）无限画布上创作：
- 生成图片、视频、配音和配乐；
- 用参考图保持人物和产品一致；
- 逐帧拉片、片段重拍；
- 剪辑并导出成片；
- 把完整故事交给导演台 Agent，从剧本拍到成片。

所有结果都留在画布上，你可以随时回到网页接着改。

插件本身只包含连接地址和几个工作流技能，能力都在可咔的远程 MCP 服务上：`https://cokkotv.com/mcp`，支持 Streamable HTTP 和 OAuth 2.1。

## 安装

**Codex**

```bash
codex plugin marketplace add https://github.com/qcvision-ai/cokko-plugin.git --ref main
codex plugin add cokko@cokko
```

**Claude Code**

```bash
claude plugin marketplace add qcvision-ai/cokko-plugin
claude plugin install cokko@cokko
```

也可以只添加 MCP 服务器，不装技能：

```bash
claude mcp add --transport http cokko https://cokkotv.com/mcp
```

**WorkBuddy**

在「连接器 → 自定义连接器」点「配置 MCP」，打开 `~/.workbuddy/mcp.json`（文件名前面没有点），把下面的 `cokko` 合并进 `mcpServers` 后保存：

```json
{
  "mcpServers": {
    "cokko": {
      "type": "streamable-http",
      "url": "https://cokkotv.com/mcp",
      "timeout": 1200000
    }
  }
}
```

不用填 `headers`：在「专家·技能·连接器」里信任并打开 `cokko`，新建任务后 WorkBuddy 会打开浏览器，请你登录可咔并授权。`workbuddy/cokko` 是提交到 WorkBuddy 连接器市场的连接器包（元信息、MCP 配置、图标和随包技能；技能正文与 `plugins/cokko/skills` 相同，只多了 WorkBuddy 技能市场要求的展示名、中英文介绍、示例、版本和作者，改动时两处一起改）。打包上传时压缩整个 `workbuddy/cokko` 目录。

**其他支持 MCP 的 Agent**：把 `https://cokkotv.com/mcp` 添加为远程 MCP 服务器。

装好后新建一个任务，Agent 会请你登录可咔，并列出它要获得的权限。确认授权后就可以开始。你可以随时在可咔的「账户设置 → 已授权的应用」里断开。

## 权限与积分

授权分六项：查看账户、查看项目、修改画布、上传素材、提交生成、导出。生成扣的积分和在网页上操作一样。

## 随包技能

| 技能 | 用途 |
| --- | --- |
| `cokko-director-film` | 把完整故事交给导演台 Agent，从剧本、形象、分段、3D 导演台一路拍到成片 |
| `cokko-reference-remake` | 参考一条视频的镜头和节奏，用自己的人物或产品复刻一版 |
| `cokko-product-ad` | 用产品图做一条 10–30 秒的产品短视频 |

## 联系

使用中的问题请通过可咔网页右下角的反馈入口告诉我们。
