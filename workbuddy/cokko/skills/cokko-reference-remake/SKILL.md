---
name: cokko-reference-remake
description: 用户给一条参考视频（广告、短片、MV 片段），想参考它的镜头、运镜和节奏，用自己的人物、产品或故事在可咔复刻一版时使用：逐帧拉片、按镜头生成、剪辑导出。
display_name: 可咔拉片复刻
display_name_en: Cokko Reference Remake
description_zh: 参考一条视频的镜头、运镜和节奏，用自己的人物或产品在可咔复刻一版。
description_en: "Remake a reference clip's shots, camera moves and pacing on Cokko with your own characters or product."
examples_zh:
- "参考这条视频的镜头和节奏，用我的人物复刻一版"
- "拆一下这条广告的分镜，换成我的产品重拍"
- "照这段 MV 的运镜，用我的角色做一版"
examples_en:
- "Remake this clip's shots and pacing with my own character"
- "Break down this ad's shots and reshoot it with my product"
- "Follow this MV's camera moves with my own character"
version: 1.0.0
author: "启辰视界（天津）科技有限公司"
---

# 可咔拉片复刻

参考一条视频的镜头结构和节奏，用用户自己的主体重新拍一版。复刻的是镜头语言和节奏，不照搬原片的人物、品牌和受版权保护的内容。

## 1. 准备画布和素材
1. 先 `doctor`，再 `target_project`（或 `create_project`），并把 canvasUrl 发给用户。
2. 上传参考视频和用户的素材（人物照、产品图等）：
   - 能用卡片的宿主：`show_upload`。
   - 其他宿主：`upload_ticket`，PUT 文件，`upload_complete`，再 `upload_media`。
   - 参考视频 3 分钟以内、50 MB 以内。
3. 同一个人物或产品要在多个镜头里保持一致时，新建一个 subject 节点（`create_node`，nodeType=subject），把这个人物或产品的图接进去，名称写人物名或产品名。

## 2. 拉片
4. 对参考视频调 `breakdown_video`。它会在右侧放出每个镜头的截图节点和一份拆解文本（景别、运镜、动作、音乐节奏、台词），也会扣少量积分，调用前告诉用户。
5. 用 `read_project` / `read_node` 读拆解结果。整理一张镜头表给用户确认：第几镜、几秒、拍什么、怎么动；把原片的人物和产品换成用户自己的。

## 3. 按镜头生成
6. 选模型：
   - `recommend_video_models` 按每镜要用的参考选。例如一张首帧用 images=1；同时用主体和参考视频，按实际数量填。
   - 再 `get_model` 读参数。
7. 每个镜头一个 video 节点：
   - 用 `create_node`：left 接主体节点和这一镜的截图（截图只用来参考构图）。
   - 提示词写画面里看得见的东西：谁、做什么、景别、机位、运镜、时长。不写剧情总结；引用接入的素材写 {{Node <ID>}}。
   - 时长用模型的 duration 参数，按原镜头时长选最接近的可用值。
8. 先把预计总积分报给用户（每个节点跑一次 `validate_generation_plan`），同意后逐个 `generation_submit`，再用 `task_status` 回读。不满意的镜头：
   - 局部重做：`edit_video_selection`。
   - 整条重做：改提示词后重新提交。每次都先问。

## 4. 剪辑成片
9. 新建 edit 节点：
   - left 按镜头顺序接入各个视频；有配乐就接入音频节点（需要的话先用 `generate_audio` 做配乐）。
   - prompt 写节奏要求（例如"按原片节奏，在鼓点处切"）。
10. `generation_submit` 出一版：
    - `read_timeline` 核对每段时长。
    - 用户要改就 `revise_timeline`。
    - 最后 `export_video` 导出，用 `export_status` 回读。
11. 用 `prepare_project_delivery` 交付；下载后核对时长和画面。
