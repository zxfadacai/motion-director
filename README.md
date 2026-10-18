# Motion Director

一个用于制作**代码驱动动效片**的 Codex Skill。它把创意 brief、分镜、风格帧、声音与时间轴、逐帧渲染、独立评审和最终交付组织成一套完整工作流。

适用类型：

- `showreel`：作品集 / 动效功力展示
- `product`：产品片
- `launch-story`：发布故事
- `explainer`：讲解片

适用场景：

- 用 HTML、Remotion 或 GSAP 制作可逐帧复现的动效视频
- 需要分镜、风格手册、`timing.json`、`facts.json` 和质检记录
- 需要真实产品 UI、品牌素材、声音设计和多轮评审

不适用：

- 只写视频脚本、文案或分镜文字
- 只做静态海报、封面或单张图片
- 直接用外部生图或生视频工具拼接成片

## 核心约束

- 成片画面必须由代码生成。
- 结果必须能逐帧复现。
- 工具链为 HTML 或留空时，必须交付可独立运行的单文件 HTML。
- 生成图片只能作为参考，不能直接成为成片画面，除非 brief 明确允许。
- 画面和配音中的事实必须能追溯到 `facts.json`。
- 评审应由未参与制作的独立评审完成；环境不支持时必须明确标记为降级评审。

## 目录结构

```text
motion-director/
├── SKILL.md
├── README.md
├── .gitignore
├── agents/
│   └── openai.yaml
└── references/
    └── motion_prompt_zh_v2.md
```

- `SKILL.md`：Skill 入口，定义适用范围和基线执行规则。
- `references/motion_prompt_zh_v2.md`：原始完整制作提示词，是实际工作流的权威来源。
- `agents/openai.yaml`：Codex 界面元数据和默认调用提示。
- `README.md`：项目说明、安装和调用方式。

## 安装

将整个 `motion-director` 文件夹放入 Codex skills 目录：

```text
~/.codex/skills/motion-director/
```

Windows 默认位置：

```text
C:\Users\<用户名>\.codex\skills\motion-director\
```

目录名必须与 `SKILL.md` 中的 `name` 一致，即 `motion-director`。

如果设置了 `CODEX_HOME`，则放入：

```text
$CODEX_HOME/skills/motion-director/
```

安装后重启或重新加载 Codex，使 Skill 被重新发现。

## 调用

显式调用：

```text
$motion-director
```

最小 brief 示例：

```text
$motion-director

mode: product
subject: [产品链接或主题]
viewer: [观众是谁，他们在乎什么]
end state: [最后一秒的感受、认知或行动]
length: 15 秒 16:9
frame rate: 60
brand: [logo、颜色、字体，或从产品中提取]
facts: [事实来源]
assets: [素材路径，或从线上产品采集]
style: [视觉方向或参考]
sound: 音乐加音效
autonomy: gated
toolchain: HTML
sign-off: [结尾标识和行动号召]
```

字段说明：

| 字段 | 作用 |
| --- | --- |
| `mode` | 选择作品集、产品片、发布故事或讲解片 |
| `subject` | 产品链接、主题，或“手艺本身” |
| `viewer` | 目标观众及其关注点 |
| `end state` | 结尾要让观众感受到、知道或做什么 |
| `length` | 时长和画幅，可同时要求 16:9 与 9:16 |
| `frame rate` | 帧率；留空时按原提示词规则选择 |
| `brand` | 品牌资产和视觉要求 |
| `facts` | 所有事实性说法的唯一来源 |
| `assets` | 真实素材或采集范围 |
| `style` | 视觉方向或参考作品 |
| `sound` | 音乐、人声、音效或无声 |
| `autonomy` | `gated` 按检查点确认；`unattended` 全程继续 |
| `toolchain` | Remotion、HTML + GSAP、指定套件仓库，或留空 |
| `sign-off` | 结尾标识和行动号召 |

## 工作流

Skill 会按照原始提示词执行：

1. 锁定事实并建立 `facts.json`
2. 研究参考作品和观众语境
3. 提出 2–3 个创意方向并选择
4. 制作分镜表
5. 制作风格手册和风格帧
6. 制作声音与时间轴
7. 构建、渲染和评审
8. 交付成片与制作资料

当 `autonomy` 为 `gated` 时，需要在 `◆` 检查点等待确认；为 `unattended` 时由 Skill 自行决策并继续。

## 预期交付物

- 每种画幅的最终 MP4
- 纯音乐版音频
- 10–20 帧缩略图拼版
- `brief.md`
- `facts.json`
- 风格手册
- `timing.json`
- `ledger.md`
- 质量标准检查结果
- 需要人工复核的说明
- 可复现的源工程或单文件 HTML

具体交付内容以原始提示词和当次 brief 为准。

## 基线说明

当前版本是对 `motion_prompt_zh_v2.md` 的**原样封装**，用于先跑案例、建立效果基线。

`SKILL.md` 只负责调用和边界说明，不修改原提示词中的创意、工艺、评审和交付规则。后续优化应作为独立版本进行，并通过实际案例对比验证，避免在基线阶段混入未经验证的改动。

## 验证

结构校验命令（需要 Python 和 `skill-creator`）：

```powershell
python "$env:CODEX_HOME\skills\.system\skill-creator\scripts\quick_validate.py" .
```

通过时应输出：

```text
Skill is valid!
```

该检查只验证 Skill 结构、frontmatter 和占位符，不验证成片质量。

## 注意事项

- 真实产品采集可能包含个人信息、客户数据、内部地址或密钥，发布前必须脱敏并人工复核。
- 音乐、字体、logo、肖像、配音和参考素材的授权需单独确认。
- 独立评审依赖运行环境是否支持子代理；不支持时不能声称已完成独立评审。
- 音视频渲染需要浏览器、`ffmpeg` 等本地能力，具体可用性取决于运行环境。
- 当前版本未包含自动安装脚本和第三方依赖；请先确认运行环境具备相应工具链。

## License

本仓库尚未声明开源许可证。公开发布前，请确认 `references/motion_prompt_zh_v2.md` 的版权和再分发权利，并根据需要补充 `LICENSE`。
