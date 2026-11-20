# Agentic Generation

收集 **Agentic Generation** 相关论文、项目与评测基准，关注智能体如何通过规划、编程、工具调用、执行反馈与迭代修正生成视觉内容。

覆盖方向包括 **agentic video generation、agentic 3D/world generation、code-to-video、video-to-code 和 visual coding agents**。

## 目录

| 方向 | 收录范围 |
| --- | --- |
| [Code to 3D (to Video)](#code-to-3d-to-video) | 以可执行代码构建 3D 场景或世界，可进一步渲染为视频，或用代理视频引导视频生成模型；兼收相关 visual coding harness。 |
| [Video to 3D](#video-to-3d) | 从参考视频重建可编辑的 3D 场景、动画或可执行程序，包括 video-to-code 评测。 |
| [Text to 3D](#text-to-3d) | 从文本出发，通过资产、布局、相机、动画等工具编排生成 3D 内容，以场景构建为主要接口。 |
| [Text to Video](#text-to-video) | 智能体围绕故事、镜头和视觉参考进行规划、检查与重试，调用 DiT 等视频生成模型完成生成。 |

### 分类与阅读说明

- **先按主要生成路径分类，再按 Training-free / Training 和年份组织。** 以自由编程为核心的工作优先归入 Code to 3D；以结构化场景工具编排为核心的工作归入 Text to 3D；参考视频重建优先归入 Video to 3D。交叉工作只保留一个主条目，并在简介中说明输入与输出。
- **Training-free**：所提出的生成方法不新增模型参数训练，可使用现成的 LLM/VLM、生成模型、提示词、工具与技能更新。
- **Training**：完整方法中任一核心组件引入新增训练或微调，即归入此类。表格注明训练对象，区分 agent、视频模型和控制模块；使用预训练模型本身不算新增训练，下游具身智能体的训练也不等于环境生成器训练。
- **Benchmark** 的训练分类不适用。为保持目录结构统一，暂附在对应方向的 Training-free 表中，并显式标注；这不表示其评测的模型均无需训练。
- 年份与月份按论文首次公开时间记录；无 arXiv 条目的项目按官方技术报告时间记录。同一年内按月份倒序排列。链接指向原论文或官方项目，表中的训练划分依据公开方法。

## Code to 3D (to Video)

### Training-free

#### 2026

| 月份 | 工作 | 类型 / 范围 | 核心思路 | 资源 |
| --- | --- | --- | --- | --- |
| 10 | **Code Video Model** | 方法 · Code → 3D → Video | Coding agent 编写 Three.js 场景并渲染代理视频；仅在视频模型的早期采样步骤使用代理条件，在保留结构的同时减少对白模外观的照搬，无需额外训练。 | [项目 / 技术报告](https://code-video-model.github.io/) · [代码](https://github.com/code-video-model/CodeVideoModel) |
| 10 | **PROWBench: Do Video Models Render What the Program Specifies?** | Benchmark · 训练分类不适用 | 用程序记录的实体状态、事件时间线及同步代理视频，评估视频模型对实体控制、交互结果、长时记忆和多视角一致性的遵循；分别考察状态符合程度与视觉质量。 | [论文](https://arxiv.org/abs/2610.02205) |
| 09 | **MaLiang-Harness: A Programmable Path to Image and Video Generation** | Harness · 兼含 2D visual coding | 让现成多模态模型编写 Canvas、SVG、Scene2d 或 Three.js 程序，通过渲染与视觉反馈迭代修改，并绑定程序版本、修改历史与验证证据；图像模型可提供素材，主要生成路径是代码渲染。 | [论文](https://arxiv.org/abs/2609.34309) · [代码](https://github.com/gulucaptain/MaLiang-Harness) |
| 09 | **Recursive Code World Models: Building Complex Worlds through Recursive Scene Programs** | 方法 · Image → Code → 3D | 冻结的视觉 coding agent 从单张参考图递归构建场景程序；在整体构建、局部递归和整体复查之间迭代，比较参考图与渲染结果，逐步修正复杂 3D 场景。 | [论文](https://arxiv.org/abs/2609.11499) · [项目](https://zhiqili-cg.github.io/RecursiveCWM) · [代码](https://github.com/ZhiqiLi-CG/RecursiveCWM_code) |
| 07 | **SimWorlds: A Multi-Agent System for Dynamic 3D Scene Creation** | 框架 · Text → Code → 3D | Planner 制定全局与分阶段计划，coding agent 编写 Blender 代码；通过确定性场景验证、运行时状态检查及多视角、多帧视觉反馈迭代修正，生成可编辑的动态场景。 | [论文](https://arxiv.org/abs/2607.01766) · [项目](https://dynsimworlds.github.io/) |
| 05 | **SimWorld Studio: Automatic Environment Generation with Evolving Coding Agent for Embodied Agent Learning** | 框架 · Text/Image → 3D 环境 | SimCoder 通过 Unreal Engine 代码与工具构建可验证的环境，利用检查反馈积累工具和技能，并导出 Gym 接口供具身智能体学习；自演化不更新底层 LLM 权重。 | [论文](https://arxiv.org/abs/2605.09423) |

### Training

#### 2026

| 月份 | 工作 | 训练对象 | 核心思路 | 资源 |
| --- | --- | --- | --- | --- |
| 10 | **AgentGarten: Code Worlds for Evolving Agents** | 神经视频渲染器：几何条件适配、因果适配与蒸馏 | Coding agent 构建并检查可执行世界，仿真器维护状态、碰撞与交互规则；共享神经渲染器将深度或法线条件转换为实时视觉观测，支持智能体持续交互。 | [论文](https://arxiv.org/abs/2610.12374) |
| 09 | **Programmable World Model** | 空间 ControlNet 与跨片段条件模块；视频主干冻结 | Coding agent 编写实体与交互规则，执行器根据用户输入维护持久世界状态；将 3D 包围盒状态编译为身份、语义和运动条件，再由视频模型实时渲染。 | [论文](https://arxiv.org/abs/2609.10540) · [项目](https://alaya-lab.github.io/pwm/) |
| 08 | **Code World Model: Coding Agent as World Brain** | 视频生成器：MiniMax-H3 的 LoRA 微调 | Coding agent 通过代码维护世界规则与状态，处理事件引起的语义变化和连续的空间变化；执行器生成代理视频，再由微调后的视频模型生成高保真观测。 | [论文](https://arxiv.org/abs/2608.25927) |
| 07 | **VideoCoCo: Code-as-CoT for Physically-Consistent Video Generation via an Agentic Dual-Engine System** | 视频编辑器：LoRA / 全量微调；另有 Tune-Free 变体 | Coding agent 编写 Blender 程序生成动态白模，再由视频编辑器生成真实感视频；使用 VideoCoCo-3K 适配编辑器，未报告专门训练 coding agent。 | [论文](https://arxiv.org/abs/2607.27380) · [代码](https://github.com/micky-li-hd/VideoCoCo) |

## Video to 3D

### Training-free

#### 2026

以下为使用现有智能体进行视频重建的评测基准，训练分类不适用。

| 月份 | 工作 | 类型 / 范围 | 核心思路 | 资源 |
| --- | --- | --- | --- | --- |
| 10 | **CineRecon: Benchmarking Frontier Models for Agentic Cinematic Reconstruction** | Benchmark · Video → 3D | 评测智能体将参考视频重建为可编辑动态 3D 场景的能力，设置 Proxy、Simplified、Detailed 三种表示要求，并以绑定实体与时间的检查清单评估相机、静态与动态一致性。 | [论文](https://arxiv.org/abs/2610.16577) |
| 09 | **BVB: Benchmarking Agentic Video Understanding via Programmatic Reconstruction in Blender** | Benchmark · Video → Blender code | 在统一 Mini-BVB harness 与成本预算下，让现有多模态智能体编程重建参考视频；以 Dual VQA 衡量时空事实保留，以 Latent Similarity 衡量感知相似度。 | [论文](https://arxiv.org/abs/2609.15478) |

### Training

暂无条目，欢迎补充。

## Text to 3D

### Training-free

#### 2026

| 月份 | 工作 | 类型 / 范围 | 核心思路 | 资源 |
| --- | --- | --- | --- | --- |
| 04 | **StoryBlender: Inter-Shot Consistent and Editable 3D Storyboard with Spatial-temporal Dynamics** | 框架 · Text → 3D storyboard / animation | Director 用连续记忆图组织分镜与上下文，统一资产库保持角色和道具身份；专门的 agent 编排布局、相机、灯光与动画，并结合 Blender 几何检查和 VLM 视觉反馈迭代生成可编辑的 3D 分镜。 | [论文](https://arxiv.org/abs/2604.03315) |

### Training

暂无条目，欢迎补充。

## Text to Video

智能体编排 DiT 等视频生成模型的调用；通过可执行 3D 代理控制视频生成的工作列在 Code to 3D (to Video)。

### Training-free

#### 2026

| 月份 | 工作 | 类型 / 范围 | 核心思路 | 资源 |
| --- | --- | --- | --- | --- |
| 09 | **StoryEngine: A State-Grounded Agentic Framework for Video Storytelling** | 框架 · 多镜头叙事 | 先建立跨镜头故事状态与标准视觉参考，再调用图像和视频模型生成镜头，通过连续性检查与有限重试修复局部错误，避免生成偏差改写既定故事状态；公开流程未引入参数训练。 | [论文](https://arxiv.org/abs/2609.33627) |
| 06 | **GroundShot: Visually Consistent Multi-Shot Long Video Generation via Entity-Grounded Shot Scheduling** | 框架 · 跨镜头实体一致性 | 按实体参考价值重排镜头生成顺序，从已生成镜头提取、验证并保存实体级视觉参考，再检索参考生成后续镜头，最后恢复叙事顺序，以减少外观漂移与错误累积。 | [论文](https://arxiv.org/abs/2606.20799) |

### Training

暂无条目，欢迎补充。

## 贡献

欢迎通过 Issue 或 Pull Request 补充相关工作、修正分类或更新链接。新增条目请提供：

- 工作名称、首次公开年月，以及论文或官方项目链接。
- 所属方向与 Training-free / Training 分类；涉及训练时注明具体组件。
- 一段简短说明，交代智能体的输入、输出、工具或反馈机制；评测基准请标注为 Benchmark。

优先收录明确包含智能体规划、编程、工具调用或反馈迭代机制的工作。普通视频 / 3D 生成模型仅在与上述机制直接相关时收录。
