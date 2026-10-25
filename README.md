# 可咔 Cokko Plugin

让 Codex、Claude Code 等 Agent 直接在你的可咔（cokkotv.com）无限画布上创作：
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
