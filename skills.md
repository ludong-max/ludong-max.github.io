# Paper Bank Ego Skills

这份文件定义本论文库的工作方式：当用户给出一篇论文链接、项目页、PDF 或 arXiv 编号时，目标不是只写普通摘要，而是生成能直接加入本库的研究 digest。

## 数据库定位

本仓库是一个 egocentric computer vision paper bank，当前以静态网页组织：

- `index.html`: 论文库总入口，左侧列表 + 右侧 iframe digest。
- `subpages/`: 每篇论文、数据集、趋势或研究方向各一个 HTML digest。
- `pdfs/`: 可选的本地 PDF 副本。

当前主题集中在第一人称视觉、AR/VR、人体/手部姿态估计、多模态数据集和未来研究方向。

## 收到论文链接后的工作流

1. 读取论文来源：优先使用 arXiv/PDF/项目页/GitHub/官方数据集页；不要只依赖标题或摘要。
2. 提取元信息：标题、作者、机构、年份、venue、arXiv/PDF/project/code 链接。
3. 判断本库分类：按 `index.html` 里的 topic/subtopic 放入最接近的研究槽位。
4. 按下面的分析维度读论文，并区分论文明确声明、实验结果、合理推断和不确定信息。
5. 生成 `subpages/<slug>.html`，保持现有深色 card 风格、英文/中文 toggle、表格和 TL;DR 结构。
6. 更新 `index.html` 的 `PAPERS` 数组，补充 `id / abbr / year / note / path / arxiv / pdf / topic / subtopic`。
7. 最后检查页面能从 `index.html` 打开，链接、标题、分类和搜索关键词可用。

## 必须分析的重点方面

### 1. 论文定位

- 这篇文章解决什么问题，一句话问题是什么。
- 它属于本库的哪一类：数据集、SMPL/SMPL-X body regression、MANO hand regression、ego-exo benchmark、趋势综述、研究 proposal 等。
- 和库中已有工作的关系：前序、后续、竞争方法、补充数据集，或可作为未来工作的模块。

### 2. 输入、输出与坐标系

- 输入信号：ego RGB、multi-view、SLAM/head trajectory、IMU、audio、eye gaze、controllers、exo cameras 等。
- 输出表示：2D/3D joints、SMPL、SMPL-X、MANO、FLAME、mesh、trajectory、future pose、language labels 等。
- 坐标系：camera-space、headset-space、world-space、allocentric、ego/exo aligned。
- 是否在线/因果推理，是否需要固定窗口，是否支持实时。

### 3. 核心方法

- 主体架构：Transformer、diffusion、ViT、MAE、teacher-student、IK/FK、optimization、tracking pipeline 等。
- 关键设计：conditioning 方式、temporal modeling、kinematic prior、data scaling、visibility/occlusion handling、multi-modal fusion。
- 方法流程要能写成一条清晰 pipeline：Input -> Encoder/Model -> Output -> Refinement。
- 对 egocentric 场景最关键的技术点要单独解释，例如头部轨迹条件化、手腕运动学约束、遮挡处理、ego-exo 对齐。

### 4. 数据与监督

- 训练数据来源、规模、标注类型和数据混合策略。
- 是否使用真实数据、合成数据、AMASS/Project Aria/Ego4D/Ego-Exo4D/Nymeria/HOT3D 等。
- 监督类型：3D GT、2D keypoints、pseudo labels、manual verified subset、weak supervision、self-supervision。
- 对数据集论文，要列出统计表：小时数、参与者、设备、活动类别、标注数量、benchmark tasks。

### 5. 实验与结果

- 使用的 benchmark、metrics 和 baselines。
- 关键数值必须进表格：MPJPE、PA-MPJPE、MPVPE、PCK、F-score、FID、latency、jitter、RTE 等。
- 说明数字单位和方向：lower/higher is better。
- 如果只能近似摘录，明确标注 approximate，并指向原论文表格。

### 6. 消融与设计结论

- 哪个模块真正带来提升。
- 数据规模、模型容量、时序建模、坐标系选择、IK/FK、kinematic constraints 的贡献。
- 写出可迁移的 insight：对我们后续做 EgoSMPL-X、body+hand+face、world-space avatar 有什么启发。

### 7. 局限与失败模式

- 输入限制：是否依赖多相机、SLAM、controller、外部 tracker、特定硬件。
- 输出限制：是否只有 joints、没有 mesh、没有 hand/face、不是 SMPL-X、只在 camera-space。
- 实验限制：是否缺少 in-the-wild 3D GT、跨数据集泛化、实时性、遮挡、接触、长序列稳定性。
- 工程限制：代码是否公开、是否可复现、速度/显存/训练数据是否现实。

### 8. 对本库和研究方向的价值

- 能否作为未来项目的 body branch、hand branch、face prior、dataset source、evaluation benchmark 或 baseline。
- 和 EgoAllo、EgoPoseFormer v2、HaMeR、HaWoR、Nymeria、Ego-Exo4D 等已有页的差异。
- 是否暴露新的 research gap。
- 对 "single egocentric camera -> world-space SMPL-X body + hands + face" 这条方向的帮助。

## 标准 digest 页面模板

每篇普通论文优先使用以下结构：

1. Title card
   - `Paper Digest · arXiv xxxx.xxxxx · Venue Year`
   - 标题、作者、机构、tags、arXiv/PDF/project/code 链接
   - One-sentence problem / 一句话问题
2. Key Contributions
   - 3 到 5 条，强调真正的新东西，不写泛泛优点。
3. Method / Architecture
   - 用一条 pipeline 展示输入到输出。
   - 解释关键模块和为什么适合 ego 场景。
4. Data / Training
   - 数据来源、规模、监督方式、训练策略。
5. Quantitative Results
   - 表格列出主要 benchmark 和关键指标。
6. Ablation / Design Insight
   - 如果论文有消融，提炼模块贡献。
   - 如果没有正式消融，写出从实验中能支撑的设计结论。
7. Limitations
   - 至少 3 条，聚焦使用条件、泛化、输出表示和工程可用性。
8. Relation to This Bank
   - 放入本库中的定位，和已有页面的对比或互补关系。
9. TL;DR
   - 1 段，包含任务、方法、最重要结果和本库价值。

数据集论文可额外加入：

- Dataset Statistics
- Benchmark Tasks
- Annotation Pipeline
- Access / License / Data Format
- Which downstream tasks it enables

趋势或综述页可改用：

- Anchor paper
- Recent works
- Comparison table
- Design space
- Open gaps
- Recommended research direction

## 分类规则

优先复用 `index.html` 现有 topic：

- `Public Datasets`: Nymeria、Ego-Exo4D、EPIC-KITCHENS、Project Aria 类数据集。
- `Single-View SMPLX regression`: single-view RGB/image-based SMPL-X、SMPL-X fitting、optimization baseline、VPoser / human body prior、pose latent space、IK prior、pseudo-label generation。
- `Ego-to-Exo SMPLX Regression`: full-body、sparse HMD、egocentric body、SMPL/SMPL-X pose。
- `Hand Regression`: MANO、hand mesh、hand-object、world-space hand motion。
- `CVPR Trend`: 多篇近期论文的趋势整理、benchmark 走势、post-paper follow-up。
- `Major Group`: 研究组、数据生态、机构路线图。
- `Future Work`: 自己的研究 proposal 或 gap-driven 方案。

如果新论文横跨多类，按本库使用价值放主类。例如 single-view RGB 到 SMPL-X、pose prior / IK latent prior 优先放 `Single-View SMPLX regression`，ego/headset/world-space body+hand 方法优先放 `Ego-to-Exo SMPLX Regression`，手部为主则放 `Hand Regression`。

## 写作规范

- 页面内容以英文为主，关键解释提供中文 toggle。
- 不确定信息不要写成事实；用 "paper reports", "approximately", "not directly comparable"。
- 不要只翻译摘要；必须写出方法、实验、局限和本库价值。
- 数字、指标、数据规模要尽量来自论文表格或官方页面。
- 对比时避免硬比不同 benchmark；必要时注明 not directly comparable。
- 保持现有视觉风格：深色背景、`.card`、小字号表格、tags、TL;DR 高亮。

## 完成检查清单

- 新 `subpages/<slug>.html` 能单独打开。
- `index.html` 中新增条目路径正确，搜索能搜到标题/年份/关键词。
- arXiv/PDF/project/code 链接可用。
- 至少包含：问题、贡献、方法、数据、结果、局限、TL;DR。
- 对本库研究方向的价值写清楚。
- 没有把猜测写成论文事实。
