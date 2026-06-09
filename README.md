# FPV Immersive Video Prompting Skill

> A route-first prompting skill for cinematic first-person AI video. It turns static scenes, character references, aerial maps, and drawn paths into physically coherent FPV video prompts for Seedance, Kling, Runway, Veo, and similar image-to-video models.

[中文说明](#中文) · [English](#english) · [Install](#install) · [Examples](#examples)

---

## 中文

### 这是什么

`fpv-immersive-video-prompting` 是一个面向 AI 视频生成的 Hermes / Claude Skill。

它解决的不是“画面怎么更漂亮”这个单点问题，而是 FPV 视频里更容易翻车的部分：行动轨迹。

在很多 image-to-video 场景里，提示词只写了风格、镜头感、人物和环境，但没有交代摄像机到底是谁、从哪里出发、先靠近谁、怎么穿过空间、每一段是否物理可达、最后停在哪里。结果视频模型会自己补运动，常见问题就是瞬移、跳切、穿墙、人物数量变化、编号残留、红线出现在成片里，或者 POV 身份前后漂移。

这个 Skill 把 FPV 提示词当成一个小型可游玩的场景来设计。它会先确定空间路线，再安排人物和互动，最后输出可直接复制到视频模型里的中文提示词。

### 适合什么场景

- 室内、庭院、展厅、派对、宫殿等一镜到底 FPV 穿行
- 多人物顺序互动，比如镜头依次经过 3 到 8 个角色
- GPT Image / GPT-Image-2 首帧资产包设计
- Seedance 2.0、Kling、Runway、Veo 这类 image-to-video 工作流
- 航拍地图、红线路径控制、世界地图飞行、游戏世界穿越
- 非人类 POV，比如猫、无人机、机器人吸尘器、鸟、幽灵、车辆、物体视角

### 它的核心判断

这个 Skill 有两个主要路线模式。

**编号停靠点模式**

适合近距离人物互动、室内空间、复杂家具、多人顺序出场。首帧里只放小编号 1、2、3，不画连续红线。最终视频提示词会要求模型按编号顺序移动，并明确数字只是路线参考，成片里不能出现。

**红线路径控制模式**

适合大场景和路线形态很强的画面，比如世界地图飞行、峡谷穿越、城市到地标、赛车线路、Seedance 2.0 路径控制演示。红线只作为摄像机路径控制，最终视频要完全移除红线、箭头、标注和地图 UI。

### 为什么它有用

一个好的 FPV 视频提示词不能只描述“看到什么”，还要描述“怎么经过”。

这个 Skill 会把下面这些信息写清楚：

- 摄像机是谁，是人、猫、无人机、机器人还是其他 POV
- 从哪里开始，按什么顺序移动
- exactly 有多少个主要人物或目标
- 每个停靠点发生什么动作或反应
- 路线是否能真实穿过空间
- 哪些东西必须保持一致
- 哪些东西绝对不能出现在最终画面里

它的价值不在于保存一个万能提示词，而在于保存一套判断流程。面对不同场景，它会自动判断该用编号停靠点，还是红线路径控制；该写人类步行，还是无人机飞行；该让每个人都有动作，还是把多人压缩成区域和群组节奏。

---

## English

### What this is

`fpv-immersive-video-prompting` is a Hermes / Claude Skill for route-first AI video prompting.

It is designed for first-person, one-shot, image-to-video workflows where motion coherence matters as much as visual quality. Instead of writing only about style, lighting, characters, and atmosphere, the skill treats an FPV video as a small playable scene with a start point, ordered stops, physically reachable movement, identity-aware POV constraints, timed interactions, and explicit negative constraints.

The result is a copy-ready prompt that helps video models understand how the camera should move through the scene.

### What it is good for

- Cinematic one-shot FPV walkthroughs
- Indoor scenes with multiple characters and ordered interactions
- GPT Image / GPT-Image-2 first-frame asset packs
- Seedance, Kling, Runway, Veo, and similar image-to-video models
- Red-line route-control images for world-scale flythroughs
- Fantasy maps, city routes, racing lines, canyon flights, and game-world traversal
- Non-human POVs such as drones, cats, robot vacuums, birds, spirits, vehicles, and object cameras

### Core idea

Most failed FPV generations do not fail because the prompt lacks adjectives. They fail because the camera path is under-specified.

This skill asks route-level questions before writing the final prompt:

- Who or what is the camera?
- Where does it start?
- How many main targets exist, exactly?
- What order should the camera visit them in?
- Can each movement segment physically happen?
- What should happen at each stop?
- Which visual identities must remain stable?
- What should never appear in the final video?

### Two route-control modes

**Numbered stop markers**

Best for close character interactions, interiors, social scenes, and exact target-count workflows. The first frame uses small numbered markers near each target instead of a continuous path line. The final video prompt makes it clear that the numbers are only planning references and must not appear in the generated video.

**Red-line path control**

Best for large-scale routes such as aerial maps, fantasy continents, city-to-landmark flythroughs, racing lines, and Seedance-style path-control demos. The red line is treated as camera-path geometry, not final visual content. The final prompt explicitly removes all red lines, arrows, annotations, labels, and map-view artifacts.

---

## Install

### Option 1. Install as a Hermes skill

Clone the repository into your Hermes skills directory:

```bash
git clone https://github.com/zhouluobo/fpv-immersive-video-prompting.git \
  ~/.hermes/skills/creative/fpv-immersive-video-prompting
```

Then restart Hermes or reload your agent session. The skill will be available as:

```text
fpv-immersive-video-prompting
```

### Option 2. Use the skill content manually

If you are not using Hermes, open `SKILL.md` and use it as a structured prompt guide inside Claude, ChatGPT, or your preferred agent environment.

For best results, also read the reference files under `skill/references/` when working with red-line routes, GPT Image asset packs, or public tutorial writing.

---

## Repository structure

```text
.
├── README.md
├── SKILL.md
├── skill/
│   ├── SKILL.md
│   └── references/
│       ├── gpt-image-asset-packs.md
│       ├── liyue-ai-redline-fpv-case.md
│       ├── mayz-seedance-world-route-case.md
│       ├── public-article-angle.md
│       └── session-patterns.md
├── examples/
│   ├── numbered-stop-example.md
│   └── redline-route-example.md
└── LICENSE
```

`SKILL.md` is duplicated at the repository root for quick reading. The installable skill package lives under `skill/`.

---

## Examples

### Example 1. Numbered stop markers for an indoor FPV scene

User request:

```text
帮我做一个现代客厅里 5 个人依次互动的 FPV 视频提示词，15 秒，像客人走进房间一样。
```

The skill will route this to numbered stop markers rather than a red line, because the scene depends on close character interaction and furniture-aware movement.

Output direction:

```text
使用上传图片作为首帧、编号路线参考和 5 个角色外观参考，生成一段 16:9、15 秒、一镜到底的现代客厅客人视角 FPV 视频。首帧中的编号 1 到 5 只作为镜头停靠顺序参考，不要出现在最终画面里。

观众是一位刚进入房间的客人，从客厅入口出发，按编号顺序移动到沙发、落地窗、茶几、开放式吧台和阳台门口。全片包含 exactly 5 个主要人物，不要增加或减少主目标。镜头保持人类眼平高度，有轻微步行摆动、自然减速和短暂停留，不能穿过沙发、茶几、墙面或人物身体。
```

### Example 2. Red-line path control for a world-scale flythrough

User request:

```text
我想做一个 Seedance 2.0 的世界地图飞行，从雪原穿过峡谷、王城，最后到火山。
```

The skill will use red-line path control, because this is a large-scale route-shaped scene.

Output direction:

```text
使用上传图片作为路线规划图。图中的红色路线只作为摄像机路径控制，不是最终画面内容。生成一段 16:9、12 秒、一镜到底的无人机 FPV 飞行视频，镜头必须严格沿红线几何从雪原出发，穿过峡谷和王城上空，最后抵达火山口。

最终视频不要出现红线、箭头、地图标注、文字标签、UI 或俯视地图感。镜头要有连续前进、自然倾斜、贴近地形掠过、逐步加速、穿越地标时的前景视差和稳定地平线。
```

---

## Reference files

- `skill/references/session-patterns.md` records the original workflow patterns and edge cases.
- `skill/references/gpt-image-asset-packs.md` explains how to request first-frame and character-reference image packs.
- `skill/references/mayz-seedance-world-route-case.md` documents Seedance-style world-route control.
- `skill/references/liyue-ai-redline-fpv-case.md` documents a red-line FPV route case.
- `skill/references/public-article-angle.md` captures the public-facing explanation of action trajectory prompting.

---

## Design philosophy

A strong FPV prompt is closer to level design than image description.

It should make the model understand where the camera can go, what it is allowed to be, what it should notice, and what continuity must survive from frame to frame. This skill makes that design process explicit, repeatable, and easier to reuse.

---

## License

MIT License.
