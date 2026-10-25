---
name: cokko-product-ad
description: 用户给出产品图和卖点，想用可咔做一条 10–30 秒的产品宣传短视频（电商主图视频、社媒广告）时使用：锁定产品外观，规划镜头，生成画面、配乐并剪成片。
---

# 可咔产品短片

用用户自己的产品图做一条短视频。产品外观必须和图一致；卖点只用用户给的，不编造功效、数据或认证。

## 1. 锁定产品
1. 依次调 `doctor`、`target_project`（或 `create_project`），并把 canvasUrl 发给用户。
2. 上传产品图：
   - 能用卡片的宿主：`show_upload`。
   - 其他宿主：`upload_ticket`，`upload_complete`，再 `upload_media`。
   - 最好是干净背景、正面清晰的图。只有一张图时，可以先用图片模型生成另外一两个角度（`generate_image`，left 接产品图，提示词写"同一产品，侧面 45 度，白底"），用户确认后再用。
3. 新建 subject 节点：名称写产品名，把确认过的产品图接进去。后面每个镜头都接这个主体，产品外观才能前后一致。

## 2. 规划
4. 跟用户确认三件事：时长（默认 15 秒）、画面比例（电商常用 9:16 或 1:1）、要突出的卖点（最多三个）。
5. 写镜头表，一般 3–5 镜：开场吸引、卖点特写、使用场景、收尾露出产品。每镜写清楚：几秒、画面里看得见什么、景别和运镜。
6. 可以建一个 text 节点放镜头表，方便用户在画布上改。

## 3. 生成
7. 选模型：
   - 视频：`recommend_video_models`，images 填接入的参考数。
   - 配乐：`search_models`（nodeType=audio）。
8. 每镜一个 video 节点，left 接产品主体，提示词里用 {{Node <主体ID>}} 指代产品。
9. 配乐：新建 audio 节点，选配乐模型，提示词写风格和情绪，时长和成片一致。
10. 先报总价（`validate_generation_plan`），用户同意后再提交（`generation_submit`），用 `task_status` 回读。产品变形、文字错乱的镜头，先给用户看，再决定是否重拍。

## 4. 成片
11. 新建 edit 节点：
    - left 依次接入各镜视频和配乐。
    - 要显示大字卖点，就把文字写在对应视频节点的 overlay 里，最多 20 字。
12. 依次 `generation_submit`、`read_timeline`、`revise_timeline`（按需）、`export_video`、`export_status`。
13. 用 `prepare_project_delivery` 交付，并提醒用户：投放前核对产品外观和文案。
