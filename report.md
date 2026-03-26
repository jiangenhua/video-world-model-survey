# 视频世界模型开源数据集调研分析报告
---

## 第一章：调研概览

### 1.1 调研范围与领域分布

本次调研系统梳理了当前视频世界模型（Video World Model）领域 **23 个**具有代表性的开源数据集，涵盖以下主要应用领域：

| 领域分类 | 数据集数量 | 占比 | 代表数据集 |
|---------|-----------|------|-----------|
| 游戏 | 6 | 26.1% | Sekai, GameGen-X, Open-P2P, GF-Minecraft, Nitrogen, WildWorld |
| 日常真实场景 | 4 | 17.4% | VIPE-dynpose-100k, SpatialVID, MiraData, DL3DV-10K |
| 自动驾驶 | 3 | 13.0% | OpenDV-YouTube, DrivingDojo, Waymo-Open |
| 具身智能 / 无人机 / SLAM | 3 | 13.0% | Mid-Air, ScanNet, TartanAir |
| 音视频理解 / 分割 | 2 | 8.7% | SAM2, VidChapters-7M |
| 渲染 / 合成场景 | 2 | 8.7% | Context-as-Memory, MatrixCity |
| 动作识别 | 1 | 4.3% | Action100M |
| 人体 / 影视 | 1 | 4.3% | OpenHumanVid |
| 日常 / 厨房 | 1 | 4.3% | EPIC-KITCHENS-100 |

从领域分布来看，**游戏类数据集占比最高（26.1%）**，其次是日常真实场景（17.4%）和自动驾驶（13.0%）。这一分布反映了当前世界模型研究的两大驱动力：游戏引擎提供了天然的交互式环境与 ground truth 标注条件，而自动驾驶领域有明确的产业需求和数据积累。

### 1.2 核心维度概览

| 数据集 | 机构 | Domain | 总数量 | 总时长 | 平均时长 | Caption | Camera Traj. | Action |
|-------|------|--------|--------|--------|---------|---------|-------------|--------|
| VIPE-dynpose-100k | 英伟达 | 日常场景 | 99.5K clips | 155.8h | 5.6s | 有(模型) | 有(模型) | 无 |
| Sekai | 上海AI Lab | 游戏 | 343.8K clips | 5,064h | 1min | 有(模型) | 有(18.3%,模型+GT) | 无 |
| GameGen-X | 港科技 | 游戏 | 860K clips | 2,950h | 12.4s | 有(模型) | 无 | 无 |
| OpenDV-YouTube | 上海AI Lab | 自动驾驶 | 2.1K clips | 2,059h | 57.7min | 有(模型) | 无 | 无 |
| SAM2 | Meta | 视频分割 | 50.9K clips | 196h | 13.7s | 无 | 无 | 无 |
| Action100M | Meta | 动作识别 | 147M clips | ~128Kh | 4-5s | 有(模型) | 无 | 有(模型) |
| EPIC-KITCHENS-100 | 布里斯托大学 | 日常/厨房 | 700 videos | 100h | 8.6min | 有(人工) | 无 | 有(人工GT) |
| SpatialVID | 南大 | 日常场景 | 2.71M clips | 7,089h | 9.42s | 有(模型) | 有(模型) | 无 |
| MiraData | 腾讯 PCG | 日常场景 | 475K clips | 14,969h | 113.3s | 有(模型) | 无 | 无 |
| DL3DV-10K | 普渡大学 | 日常场景 | 10.5K videos | ~200h | 69.5s | 无 | 有(GT/COLMAP) | 无 |
| Context-as-Memory | 快手 | 渲染/游戏 | 100 videos | 7h | 253.4s | 有(模型) | 有(GT/UE5) | 无 |
| Open-P2P | elefant-ai | 游戏 | 109K videos | 8,351h | 未说明 | 有(模型) | 无 | 有(GT/键鼠) |
| GF-Minecraft | 快手 | 游戏 | 18K clips | 70h | 125s | 有(模型) | 有(GT/API) | 有(GT/键鼠) |
| MatrixCity | 港中文 | 渲染/驾驶 | 519K frames | - | - | 无 | 有(GT/UE5) | 无 |
| DrivingDojo | 美团 | 自动驾驶 | 18.2K videos | 150h | 30s | 有(模型+人工) | 有(GT/车载) | 无 |
| Mid-Air | 列日大学 | 无人机 | 420K frames | 79min | 1.47min | 无 | 有(GT/AirSim) | 无 |
| ScanNet | 斯坦福 | 3D室内 | 2.49M frames | ~23h | - | 无 | 有(GT/BundleFusion) | 无 |
| TartanAir | CMU | 具身智能 | 1M+ frames | ~27.8h | 96.4s | 无 | 有(GT/AirSim) | 无 |
| VidChapters-7M | 牛津大学 | 视频理解 | 817K videos | ~307Kh | 22.6min | 有(ASR) | 无 | 无 |
| Waymo-Open | 谷歌 | 自动驾驶 | 4K videos | 12h | 20s | 无 | 有(GT/车载) | 无 |
| Nitrogen | 英伟达 | 游戏 | 38.7K videos | 40Kh | 21.6min | 无 | 无 | 无 |
| OpenHumanVid | 百度 | 人体/影视 | 13.2M clips | 16,700h | 5s | 有(模型) | 无 | 有(模型) |
| WildWorld | 盛大网络 | 游戏 | 108M frames | ~1,000h | ~9min | 有(模型) | 有(GT/UE5) | 有(GT/450+类) |

### 1.3 数据来源分布

| 数据来源类型 | 数据集数量 | 占比 | 典型数据集 |
|-------------|-----------|------|-----------|
| YouTube 抓取 | 10 | 43.5% | VIPE, Sekai, GameGen-X, OpenDV-YouTube, Action100M, SpatialVID, MiraData, VidChapters-7M, Nitrogen, OpenHumanVid(影视) |
| 引擎渲染 / 合成 | 6 | 26.1% | Context-as-Memory, MatrixCity, Mid-Air, TartanAir, GF-Minecraft, WildWorld |
| 专业设备采集 | 5 | 21.7% | EPIC-KITCHENS-100(GoPro), DL3DV-10K(手机+无人机), ScanNet(iPad), DrivingDojo(车载), Waymo-Open(车载) |
| 混合来源 | 2 | 8.7% | Open-P2P(Recap采集), Sekai(YouTube+游戏) |

**YouTube 抓取仍然是最主要的数据来源**，占比超过 43%。引擎渲染数据（26.1%）是第二大来源，其优势在于可以提供精确的 ground truth 标注。专业设备采集虽然数据质量较高，但受限于成本和规模。

---

## 第二章：开源数据集现状分析——普遍特征

### 2.1 数据规模与时长分布

当前开源数据集在规模上呈现**显著的两极分化**：

**超大规模数据集（>10万小时）**：VidChapters-7M（~307,000h）和 Action100M（~128,000h）在总时长上遥遥领先，但前者为通用视频理解数据集，后者以极短片段（4-5s）为主，并非为世界模型训练而设计。

**中等规模数据集（1,000-50,000h）**：MiraData（14,969h）、OpenHumanVid（16,700h）、Nitrogen（40,000h）、Open-P2P（8,351h）、SpatialVID（7,089h）、Sekai（5,064h）、GameGen-X（2,950h）、OpenDV-YouTube（2,059h）构成了主体阵营。

**小规模数据集（<1,000h）**：WildWorld（~1,000h）、SAM2（196h）、VIPE-dynpose-100k（155.8h）、DrivingDojo（150h）、DL3DV-10K（~200h）、EPIC-KITCHENS-100（100h）、GF-Minecraft（70h）、Mid-Air（79min）、TartanAir（~27.8h）、ScanNet（~23h）、Waymo-Open（12h）、Context-as-Memory（7h）。

在**平均时长**维度上：

| 时长区间 | 数据集 | 特征 |
|---------|--------|------|
| <10s | VIPE-dynpose-100k(5.6s), Action100M(4-5s), OpenHumanVid(5s), SpatialVID(9.42s) | 数据量大但单条极短 |
| 10s-60s | SAM2(13.7s), GameGen-X(12.4s), Waymo-Open(20s), DrivingDojo(30s), Sekai(1min) | 短至中等片段 |
| 1-10min | DL3DV-10K(69.5s), MiraData(113.3s), GF-Minecraft(125s), Mid-Air(1.47min), TartanAir(96.4s), Context-as-Memory(253.4s), EPIC-KITCHENS-100(8.6min), WildWorld(~9min) | 中等时长 |
| >10min | Nitrogen(21.6min), VidChapters-7M(22.6min), OpenDV-YouTube(57.7min) | 单条长视频 |

存在明显的**"数据量大但单条短"现象**：Action100M 拥有 1.47 亿条片段但平均仅 4-5 秒，OpenHumanVid 有 1,320 万条但平均仅 5 秒，SpatialVID 有 271 万条但平均仅 9.42 秒。相反，OpenDV-YouTube 仅有 2,139 条视频但平均长达 57.7 分钟。**适合世界模型训练的"中等时长（1-10分钟）、大规模"数据集仍然稀缺**。

### 2.2 领域与场景分布

当前数据集在 domain 分布上存在**明显偏向**：

- **游戏类占据主导**（6/23，26.1%）：Sekai、GameGen-X、Open-P2P、GF-Minecraft、Nitrogen、WildWorld 均来自游戏领域。游戏引擎天然提供可控环境、确定性交互和 ground truth 标注，使其成为世界模型研究的"天然实验场"。
- **自动驾驶类形成稳定集群**（3/23，13.0%）：OpenDV-YouTube、DrivingDojo、Waymo-Open 均专注于驾驶场景，受益于行业成熟的数据采集基础设施。
- **日常真实场景类偏少且碎片化**（4/23，17.4%）：VIPE-dynpose-100k、SpatialVID、MiraData、DL3DV-10K 虽然涵盖日常场景，但多数以 YouTube 抓取为主，场景覆盖的系统性不足。
- **具身智能类规模偏小**（3/23，13.0%）：Mid-Air、ScanNet、TartanAir 均为合成或室内扫描数据，总量有限（合计不足 130 小时），与真实世界具身场景的多样性需求存在较大差距。

### 2.3 数据来源与质量

**YouTube 抓取**（43.5%）是最普遍的数据来源策略，其优势在于成本低、规模大、场景多样，但存在以下固有局限：（1）视频内容不可控，需要大量后处理过滤；（2）无法获得 ground truth 的 camera trajectory 和 action 标注；（3）视频质量参差不齐。MiraData、SpatialVID、GameGen-X 等数据集均投入了大量精力在质量过滤环节（美学评分、运动过滤、OCR 文本过滤等）。

**引擎渲染 / 合成数据**（26.1%）能够提供精确的 ground truth 标注（camera pose、depth map 等），但存在与真实世界的 domain gap。Context-as-Memory 和 MatrixCity 使用 UE5 渲染，TartanAir 和 Mid-Air 使用 AirSim，GF-Minecraft 使用 Minecraft API，WildWorld 从《怪物猎人：荒野》3A 游戏引擎采集——后者的画面质量接近照片级写实，在一定程度上缓解了 domain gap 问题。

**专业设备采集**（21.7%）数据质量最高，但规模受限于成本。EPIC-KITCHENS-100（GoPro 头戴式拍摄）和 DL3DV-10K（手机+无人机）代表了高质量小规模的路线，Waymo-Open（仅 12 小时）则体现了工业级数据的"精而少"特征。

### 2.4 标注体系现状

三大核心 feature 的标注覆盖情况如下：

| 标注类型 | 有标注 | 无标注 | 覆盖率 | 人工/GT 标注 | 模型标注 |
|---------|--------|--------|--------|------------|---------|
| Caption | 15/23 | 8/23 | 65.2% | 1（EPIC-KITCHENS） | 14 |
| Camera Trajectory | 13/23 | 10/23 | 56.5% | 10（GT来源） | 3（模型估计） |
| Action | 6/23 | 17/23 | 26.1% | 3（GT来源） | 3（模型标注） |

**Caption** 是覆盖率最高的标注类型（65.2%），但**几乎全部依赖模型自动生成**（14/15）。标注模型涵盖 GPT-4o（GameGen-X）、GPT-4V（MiraData）、Qwen2.5-VL-72B（Sekai）、Gemini-2.0-Flash（SpatialVID）、BLIP-2（OpenDV-YouTube）、MiniCPM-V（Context-as-Memory, GF-Minecraft）等。仅 EPIC-KITCHENS-100 采用人工标注。标注粒度差异较大：GameGen-X 提供 5 维结构化描述（607 词/分钟），MiraData 区分 short/dense/main object/background/camera/style，而 VidChapters-7M 仅依赖标题和 ASR 转写。

**Camera Trajectory** 覆盖率为 56.5%，其中**GT 来源占绝大多数**（10/13）。GT 来源包括 UE5 引擎（Context-as-Memory, MatrixCity, WildWorld）、AirSim 模拟器（Mid-Air, TartanAir）、Minecraft API（GF-Minecraft）、车载定位系统（DrivingDojo, Waymo-Open）和 COLMAP SfM（DL3DV-10K）。模型估计方面，VIPE-dynpose-100k 使用 ViPE 模型（BA+光流+深度估计），SpatialVID 使用 MegaSaM+UniDepth v2+Depth Anything v2，Sekai 部分使用 MegaSAM 改进版。

**Action 标注严重缺失**（仅 26.1%），是当前最大的标注瓶颈。拥有 GT action 标注的仅有 3 个数据集：EPIC-KITCHENS-100（人工标注的动词+名词结构）、Open-P2P（键鼠控制信号）、GF-Minecraft（键鼠控制信号），以及 WildWorld（450+ action types，游戏引擎 GT）。模型标注的 action 仅有 Action100M（GPT-OSS-120B）和 OpenHumanVid（MiniCPM/CogVLM），粒度均为 global action caption，缺乏帧级精细标注。

### 2.5 镜头视角分布

| 视角类型 | 数据集 | 数量 |
|---------|--------|------|
| 第一人称为主 | EPIC-KITCHENS-100, Open-P2P, GF-Minecraft, Context-as-Memory, ScanNet, Mid-Air, TartanAir, OpenDV-YouTube | 8 |
| 第三人称为主 | WildWorld, SAM2 | 2 |
| 混合视角 | VIPE-dynpose-100k, Sekai, GameGen-X, Action100M, SpatialVID, MiraData, VidChapters-7M, OpenHumanVid, Nitrogen | 9 |
| 车载专用 | OpenDV-YouTube, DrivingDojo, Waymo-Open | 3 |
| 无人机 / 俯视 | Mid-Air, MatrixCity | 2 |

第一人称视角数据集较多（8 个），这与具身智能和游戏领域的天然需求有关。混合视角的数据集多来自 YouTube 抓取，视角分布难以控制。**缺乏对视角类型进行系统标注的数据集**——仅 GameGen-X 提供了第一/第三人称的精确比例统计（47.3% vs 52.7%）。

---

## 第三章：开源数据集痛点深度剖析

### 3.1 Action 标注严重缺失

**这是当前世界模型数据集最关键的痛点。** Action 标注对于 action-conditioned generation（动作条件视频生成）至关重要，但在 23 个数据集中：

- **完全无 action 标注：17 个（73.9%）**，包括 Sekai、GameGen-X、OpenDV-YouTube、SAM2、SpatialVID、MiraData、DL3DV-10K、Context-as-Memory、MatrixCity、DrivingDojo、Mid-Air、ScanNet、TartanAir、VidChapters-7M、Waymo-Open、Nitrogen、VIPE-dynpose-100k。
- **仅有模型粗粒度标注：3 个（13.0%）**。Action100M 使用 GPT-OSS-120B 生成 global action caption，OpenHumanVid 使用 MiniCPM/CogVLM 生成动作描述——这些均为全局文本描述，缺乏帧级时间对齐和结构化动作分类。
- **拥有 GT 标注：3+1 个（13.0%+4.3%）**。EPIC-KITCHENS-100 提供人工标注的 89,977 个 action segments（动词+名词结构，如 put, take, open 等），但仅限于厨房场景。Open-P2P 和 GF-Minecraft 提供键鼠控制信号作为 GT action，但前者分辨率仅 640x480，后者仅有 Minecraft 单一游戏。**WildWorld 是唯一同时具备大规模、高质量、丰富 action 类型体系（450+ action types）的数据集。**

**已有 action 标注方式的差异**值得关注：

| 标注方式 | 数据集 | 优势 | 局限 |
|---------|--------|------|------|
| 键鼠原始信号 | Open-P2P, GF-Minecraft | 精确的底层控制信号 | 语义抽象度低，跨游戏迁移困难 |
| 人工动词+名词 | EPIC-KITCHENS-100 | 语义清晰 | 规模受限（仅 700 视频），领域单一 |
| 模型 global caption | Action100M, OpenHumanVid | 规模大 | 粒度粗，无帧级对齐，缺乏动作分类体系 |
| 引擎 GT + 动作分类 | WildWorld | 精确、大规模、结构化 | 限于游戏引擎环境 |

Action 标注的缺失直接制约了世界模型的**交互性建模能力**。没有 action 条件，模型只能做"被动预测"而非"主动生成"，无法实现真正的 interactive world simulation。

### 3.2 Camera Trajectory 标注不统一

虽然 camera trajectory 的覆盖率（56.5%）相对较好，但存在严重的**标准不统一和精度差异**问题：

**GT vs 模型估计的精度鸿沟**：

| 来源 | 数据集 | 精度特征 |
|------|--------|---------|
| UE5 引擎 GT | Context-as-Memory, MatrixCity, WildWorld | 无误差，逐帧精确 |
| AirSim GT | Mid-Air, TartanAir | 无误差，模拟器精确 |
| Minecraft API GT | GF-Minecraft | 无误差，API 精确 |
| 车载定位 GT | DrivingDojo, Waymo-Open | 高精度（厘米级） |
| COLMAP SfM | DL3DV-10K | 高精度，但依赖特征匹配质量 |
| BundleFusion | ScanNet | 实时 SLAM，精度中等 |
| ViPE 模型 | VIPE-dynpose-100k | 基于光流+深度估计，存在累积漂移 |
| MegaSaM + 深度估计 | SpatialVID, Sekai(部分) | 改进版 SfM，精度依赖视频质量 |

不同来源的 camera trajectory 在**坐标系定义、尺度归一化、内参格式**上缺乏统一标准。例如，UE5 使用左手坐标系，COLMAP 和 AirSim 各有自己的约定，MegaSaM 估计的位姿需要额外的尺度对齐。这种异构性严重阻碍了**跨数据集联合训练**。

此外，Sekai 仅有 18.3% 的数据具有 camera trajectory 标注，说明即使在单个数据集内部，轨迹覆盖率也可能严重不足。

### 3.3 数据类型与场景覆盖不均衡

**游戏 / 自动驾驶类数据集占比过高，日常真实场景类数据集稀缺**。如果将渲染/合成类（Context-as-Memory, MatrixCity）也归入游戏类广义范畴，则游戏+自动驾驶合计占比超过 47%（11/23）。

**合成数据与真实数据之间的 domain gap** 是一个根本性挑战：
- MatrixCity（UE5 城市场景）、Mid-Air（AirSim 户外）、TartanAir（AirSim 多场景）虽然提供了精确的 GT 标注，但其渲染质量与真实世界存在可感知的差距。
- GF-Minecraft 的 Minecraft 风格与真实世界差距更大（640×360 分辨率、方块化世界）。
- Context-as-Memory 仅有 100 个视频，规模极小。
- WildWorld 来自《怪物猎人：荒野》，虽然画面质量达到 3A 游戏水准（HEVC 16-20Mbps），但场景仍局限于游戏世界（5 个关卡），与日常真实场景存在差异。

**日常真实场景数据集**的问题在于：VIPE-dynpose-100k 和 SpatialVID 虽然规模较大，但完全无 action 标注；MiraData 平均时长 113.3 秒但也无 action 和 camera trajectory；DL3DV-10K 有 COLMAP GT camera trajectory 但无 caption 和 action。**没有一个日常真实场景数据集同时具备完善的多维标注**。

### 3.4 数据时长两极分化

数据时长分布呈现明显的**两极化**格局：

**短片段主导**（平均时长 <10s）：
- Action100M：4-5s（64% 的片段 <3s）
- OpenHumanVid：5s（57% 的片段 2-4s）
- VIPE-dynpose-100k：5.6s（65.2% 的片段 4-6s）
- SpatialVID：9.42s

**长视频少但单条极长**：
- OpenDV-YouTube：57.7min（个别视频超过 4 小时）
- VidChapters-7M：22.6min（最长 23.96 小时）
- Nitrogen：21.6min

**"中等时长+多样化场景"的均衡数据集极度稀缺**。世界模型训练理想的数据时长在 1-10 分钟范围——既足够长以建模时序依赖和场景变化，又不至于因过长而增加训练难度。在这个区间内：
- MiraData（113.3s ≈ 1.9min）接近下界
- GF-Minecraft（125s ≈ 2.1min）固定时长
- Context-as-Memory（253.4s ≈ 4.2min）仅 100 个视频
- EPIC-KITCHENS-100（8.6min）仅 700 个视频
- WildWorld（~9min）拥有 108M 帧

**WildWorld 是极少数在"中等时长"区间内同时保有大规模数据量的数据集**（~1,000h，平均 ~9min/sample）。

### 3.5 Feature 完备性不足

对 23 个数据集的五大核心 feature（caption、camera trajectory、action、镜头视角标注、控制主体标注）覆盖情况进行矩阵分析：

| 数据集 | Caption | Camera Traj. | Action | 视角标注 | 控制主体 | Feature 数 |
|-------|---------|-------------|--------|---------|---------|-----------|
| WildWorld | ✓ | ✓(GT) | ✓(GT) | ✓ | ✓ | **5** |
| GF-Minecraft | ✓ | ✓(GT) | ✓(GT) | ✓ | - | 4 |
| EPIC-KITCHENS-100 | ✓ | - | ✓(GT) | ✓ | - | 3 |
| Sekai | ✓ | ✓(部分) | - | ✓ | - | 3 |
| DrivingDojo | ✓ | ✓(GT) | - | ✓ | - | 3 |
| SpatialVID | ✓ | ✓(模型) | - | ✓ | - | 3 |
| GameGen-X | ✓ | - | - | ✓ | - | 2 |
| Open-P2P | ✓ | - | ✓(GT) | ✓ | - | 3 |
| OpenHumanVid | ✓ | - | ✓(模型) | - | - | 2 |
| VIPE-dynpose-100k | ✓ | ✓(模型) | - | - | - | 2 |
| MiraData | ✓ | - | - | - | - | 1 |
| Action100M | ✓ | - | ✓(模型) | - | - | 2 |
| Context-as-Memory | ✓ | ✓(GT) | - | ✓ | - | 3 |
| DL3DV-10K | - | ✓(GT) | - | - | - | 1 |
| OpenDV-YouTube | ✓ | - | - | ✓ | - | 2 |
| SAM2 | - | - | - | - | - | 0 |
| MatrixCity | - | ✓(GT) | - | - | - | 1 |
| Mid-Air | - | ✓(GT) | - | - | - | 1 |
| ScanNet | - | ✓(GT) | - | - | - | 1 |
| TartanAir | - | ✓(GT) | - | - | - | 1 |
| VidChapters-7M | ✓ | - | - | - | - | 1 |
| Waymo-Open | - | ✓(GT) | - | ✓ | - | 2 |
| Nitrogen | - | - | - | - | - | 0 |

**核心发现**：

- **Feature 数为 0 的数据集有 2 个**（SAM2, Nitrogen），它们本质上并非为世界模型训练设计。
- **Feature 数为 1 的数据集有 6 个**（DL3DV-10K, MatrixCity, Mid-Air, ScanNet, TartanAir, VidChapters-7M, MiraData），仅提供单一维度的标注。
- **同时具备 caption + camera trajectory + action 三大核心标注的数据集仅有 3 个**：GF-Minecraft（Minecraft 游戏，规模仅 70h）、WildWorld（怪物猎人，~1,000h）和一定程度上的 Open-P2P（键鼠信号，质量低）。
- **WildWorld 是唯一同时覆盖 5 个核心 feature 的数据集**。

这种碎片化标注格局意味着，研究者如果想训练一个同时支持 caption-guided generation、camera-controllable generation 和 action-conditioned generation 的统一世界模型，**目前几乎找不到一个标注体系完备的大规模数据集**。

### 3.6 其他痛点

**分辨率不一致**：各数据集的分辨率从 640×352（Context-as-Memory）到 4K（DL3DV-10K）不等，帧率从 5FPS（DrivingDojo）到 30FPS（多数数据集）不等。这种异构性增加了跨数据集混合训练的预处理成本。

**分类体系缺失**：对于视频内容类型、场景类型、天气条件等维度，缺乏统一的分类标准。Sekai 提供了场景分布（outdoor-urban 88%、outdoor-natural 7.3%等），GameGen-X 提供了游戏类型分布（RPG 55.2%、ACT 33.1%等），WildWorld 区分了战斗/巡游（66%/34%），但多数数据集对内容类型仅给出定性描述。

**License 与商用限制**：部分数据集基于 YouTube 内容（存在版权灰色地带），部分基于商业游戏（如 WildWorld 基于《怪物猎人：荒野》，Open-P2P 基于多款游戏），具体的使用许可和商业化限制需要逐一确认。

---

## 第四章：社区数据集缺口分析

### 4.1 从 Domain 角度

| 缺口方向 | 说明 | 现状 |
|---------|------|------|
| **日常真实场景 + 完备标注** | 目前没有一个日常真实场景数据集同时具备 caption + camera trajectory + action 标注 | VIPE/SpatialVID 无 action，MiraData/DL3DV 标注不完整 |
| **室内交互场景** | 具身智能需要的室内操作/导航场景，标注维度丰富 | ScanNet 仅有 camera pose，EPIC-KITCHENS 无 camera trajectory |
| **多角色交互场景** | 涉及人-人、人-物、多智能体交互的复杂场景 | 几乎为空白 |
| **自然环境多样性** | 不同天气、光照、季节条件下的系统性采集 | 仅 Mid-Air 和 WildWorld 有一定天气/时间变化 |

### 4.2 从 Feature 角度

最迫切缺失的标注组合：

1. **Caption + Camera Trajectory + Action（三位一体）**：仅 GF-Minecraft（70h, Minecraft）和 WildWorld（1,000h, 游戏场景）具备。**真实世界场景中此组合完全空白**。
2. **帧级细粒度 Action 分类体系**：除 WildWorld（450+ action types，逐帧 triplet 标注）和 EPIC-KITCHENS-100（动词+名词）外，其他 action 标注均为粗粒度文本描述。
3. **控制主体标注**：标注"谁在执行动作"的数据集几乎不存在——WildWorld 区分角色/怪物动作并提供骨架序列，是罕见的例外。
4. **统一格式的 Camera Trajectory**：跨数据集的坐标系、尺度、内参格式亟需标准化。

### 4.3 从 Scale 角度

| 规模区间 | 现有数据集 | 缺口 |
|---------|-----------|------|
| <100h | Context-as-Memory, GF-Minecraft, Mid-Air, TartanAir, ScanNet, Waymo-Open | 数量多但标注不全 |
| 100-1,000h | VIPE, DL3DV, DrivingDojo, SAM2, EPIC-KITCHENS, WildWorld | **需要更多具备完备标注的中等规模数据集** |
| 1,000-10,000h | Sekai, GameGen-X, OpenDV-YouTube, SpatialVID, Open-P2P | 规模尚可但标注碎片化 |
| >10,000h | MiraData, OpenHumanVid, Nitrogen, VidChapters-7M, Action100M | 规模大但标注体系单一 |

**在 100-10,000 小时区间内，同时具备三大核心 feature 的"标注完备型"数据集几乎是空白。**

### 4.4 从应用角度

| 下游任务 | 数据支持现状 | 评估 |
|---------|------------|------|
| Action-conditioned generation | 仅 GF-Minecraft、Open-P2P、WildWorld 提供 GT action | **严重不足** |
| Camera-controllable generation | SpatialVID、Sekai、DL3DV-10K 等提供 camera trajectory | 中等（但精度参差不齐） |
| Interactive world simulation | 需要 action + camera + state 联合标注，仅 WildWorld 初步具备 | **严重不足** |
| Long-horizon video generation | MiraData（113.3s）、WildWorld（~9min）提供中长时长数据 | 不足 |
| Multi-modal conditioned generation | 需要多 feature 并行，见第三章 Feature 矩阵 | **严重不足** |

---

## 第五章：我们的数据集定位与价值分析

### 5.1 数据集基本定位

我们团队正在准备开源的世界模型数据集具备以下核心特征：

| 维度 | 我们的数据集 |
|------|-------------|
| Domain | 日常真实场景为主 |
| 规模 | ~200 万 samples |
| Action 标注 | **GT 标注**，包含显式 action types 分类 |
| Caption | 提供 |
| Camera Trajectory | 提供相机轨迹信息 |
| 控制主体 | 标注 |
| 镜头视角类型 | 标注 |

### 5.2 填补的空白

#### 空白一：日常真实场景 + Action GT 标注的首次结合

这是**本数据集最核心的差异化价值**。如第三章分析所示，当前 23 个数据集中：
- 日常真实场景类数据集（VIPE、SpatialVID、MiraData、DL3DV）**全部没有 action 标注**。
- 拥有 GT action 标注的数据集（EPIC-KITCHENS、Open-P2P、GF-Minecraft、WildWorld）**全部来自受控环境**（厨房、游戏）。

我们的数据集首次在**日常真实场景**中提供 GT action 标注，弥补了"真实世界 + action-conditioned generation"训练数据的根本性缺失。~200 万样本的规模远超 EPIC-KITCHENS-100 的 700 个视频和 GF-Minecraft 的 18K 个片段。

#### 空白二：Feature 完备性的全面提升

我们的数据集同时具备 **caption + camera trajectory + action + 控制主体 + 镜头视角类型** 五大 feature，Feature 完备度达到 5/5。对比现有数据集：
- 现有 Feature=5 的数据集仅有 WildWorld（游戏场景）
- 现有 Feature≥3 的日常场景数据集：SpatialVID（caption + camera traj. + 视角，无 action）、仅此一个

我们的数据集将成为**首个在日常真实场景中达到 5-feature 完备标注的大规模数据集**。

#### 空白三：规模化的"真实世界交互数据"

在 action-conditioned generation 的训练数据方面：
- EPIC-KITCHENS-100：100h / 700 videos / 厨房单一场景
- Open-P2P：8,351h 但分辨率仅 640×480 / 游戏场景
- GF-Minecraft：70h / 18K clips / Minecraft 单一游戏
- WildWorld：~1,000h / 游戏场景

我们的 ~200 万样本规模在**日常真实场景的 action 标注数据集中将是数量级的突破**。

#### 空白四：控制主体标注的独特价值

几乎没有现有数据集提供系统化的"控制主体"标注。WildWorld 区分了角色/怪物动作实体，但限于游戏场景。我们的控制主体标注可以支持世界模型理解"谁在执行动作"，这对于 multi-agent interaction 建模和 embodied AI 具有重要意义。

### 5.3 差异化优势：与最相近数据集的正面对比

#### 对比一：vs SpatialVID（最相近的日常场景数据集）

| 维度 | 我们的数据集 | SpatialVID |
|------|-------------|-----------|
| Domain | 日常真实场景 | 日常真实场景 |
| 规模 | ~200万 samples | 2.71M clips |
| Caption | 有 | 有（Gemini+Qwen3） |
| Camera Traj. | 有 | 有（MegaSaM 模型估计） |
| Action | **有（GT）** | **无** |
| 控制主体 | **有** | **无** |
| 视角标注 | **有** | 有（多视角混合） |
| Camera 精度 | 待确认 | 模型估计，存在漂移 |

**核心优势**：action GT 标注和控制主体标注是 SpatialVID 完全不具备的，这使我们的数据集能够支持 action-conditioned generation 等 SpatialVID 无法支持的下游任务。

#### 对比二：vs WildWorld（标注最完备的数据集）

| 维度 | 我们的数据集 | WildWorld |
|------|-------------|-----------|
| Domain | **日常真实场景** | 游戏（怪物猎人） |
| 规模 | ~200万 samples | 108M frames (~1,000h) |
| Action 标注 | GT + action types | GT + 450+ types |
| Camera Traj. | 有 | GT（UE5） |
| Caption | 有 | 有（Qwen3-VL+Gemini） |
| 真实/合成 | **真实世界** | 游戏合成 |
| 场景多样性 | **日常多场景** | 5个游戏关卡 |
| Domain gap | **无** | 存在（游戏→真实） |

**核心优势**：WildWorld 的标注体系虽然极为丰富，但其数据来源于游戏引擎，训练得到的世界模型在迁移到真实世界时会面临 domain gap。我们的数据集基于真实场景，可以**直接支撑真实世界 world model 的训练**，无需跨域适配。

#### 对比三：vs EPIC-KITCHENS-100（日常场景+GT action）

| 维度 | 我们的数据集 | EPIC-KITCHENS-100 |
|------|-------------|-------------------|
| 规模 | **~200万 samples** | 700 videos / 100h |
| Domain 多样性 | **日常多场景** | 仅厨房 |
| Action 标注 | GT + types | 人工 GT（动词+名词） |
| Camera Traj. | **有** | **无** |
| Caption | **有** | 有（人工） |
| 控制主体 | **有** | 无 |
| 视角 | **多视角** | 仅第一人称 |

**核心优势**：规模是 EPIC-KITCHENS-100 的数个数量级提升，场景从单一厨房扩展到多样化日常场景，且补充了 camera trajectory 标注。

### 5.4 仍需改进的方向

**客观分析**，我们的数据集在以下维度仍有提升空间：

1. **数据时长分布**：需要评估样本的平均时长分布。如果样本偏短（<10s），则在 long-horizon video generation 任务上的支撑力有限。理想情况下应包含一定比例的 1-10 分钟级别的长样本。

2. **Action 标注粒度与体系化**：GT action types 的分类体系是否足够精细和系统化？WildWorld 的 450+ action types 以 (weapon, bank, motion) triplet 形式组织，形成了清晰的层次结构。建议建立类似的层次化动作分类法（hierarchical action taxonomy），以支持不同粒度的条件生成。

3. **Camera Trajectory 的精度与来源**：如果 camera trajectory 来自模型估计而非 GT，需要明确估计方法和精度水平。在与 WildWorld、DL3DV-10K 等 GT 轨迹数据集对比时，模型估计的轨迹可能在精度上存在劣势。

4. **Domain 多样性的系统性**：虽然定位为"日常真实场景"，但需要评估场景分布的系统性——是否覆盖了室内/室外、城市/自然、不同天气/光照条件等多维度变化？建议参考 Sekai 和 WildWorld 的场景分布统计方式，提供量化的场景分布数据。

5. **合成数据互补**：纯真实世界数据虽然避免了 domain gap，但也无法获得像游戏引擎那样精确的逐帧 ground truth。可以考虑引入少量合成数据作为补充，或与 WildWorld 等数据集进行联合训练策略。

6. **评测基准配套**：WildWorld 配套了 WildBench 评测基准（200 样本，包含 Action Following、State Alignment、Video Quality、Camera Control 四个维度）。建议我们的数据集也构建类似的标准化评测基准，以促进社区的可复现对比研究。

---

## 第六章：总结与建议

### 6.1 核心发现

1. **当前视频世界模型开源数据集的最大瓶颈是 action 标注的严重缺失**（73.9% 的数据集完全无 action 标注），这直接制约了 action-conditioned generation 和 interactive world simulation 的研究进展。

2. **多维标注的碎片化是普遍问题**——绝大多数数据集仅覆盖 1-2 个核心 feature，同时具备 caption + camera trajectory + action 三大标注的数据集仅有 2-3 个，且均来自游戏/受控环境，**真实世界场景中的"三位一体"标注完全空白**。

3. **数据时长两极分化**严重，适合世界模型训练的"中等时长（1-10min）+ 大规模 + 多维标注"数据集极度稀缺。

4. **游戏类和自动驾驶类数据集在 domain 分布上占据主导**，日常真实场景的高质量标注数据集是社区最迫切的需求之一。

5. 我们的数据集凭借**"日常真实场景 + GT Action 标注 + 五维 Feature 完备 + 百万级规模"**的组合，将在现有开源数据集版图中占据独特且关键的位置。

### 6.2 发展趋势展望

视频世界模型数据集社区正在经历从"规模驱动"向"标注驱动"的范式转变。早期工作以海量 YouTube 视频的无标注或弱标注预训练为主（如 VidChapters-7M、Action100M），而近期工作越来越强调**结构化、多维度、精细粒度的标注体系**（如 WildWorld 的 119 列逐帧标注、GameGen-X 的 5 维结构化 caption）。未来的竞争焦点将从"谁的数据更多"转向"谁的标注更完备、更统一、更可扩展"。

### 6.3 推广策略建议

1. **构建标准化 Benchmark**：参考 WildWorld 的 WildBench 设计，为我们的数据集构建专门的评测基准，覆盖 action-conditioned generation、camera-controllable generation、long-horizon consistency 等维度，使其成为社区标准评测平台。

2. **提供标注格式标准化工具**：开发与其他主流数据集（SpatialVID、WildWorld、DL3DV-10K 等）的标注格式转换工具，降低社区使用门槛，促进跨数据集混合训练。

3. **建立社区合作生态**：
   - 与 SpatialVID（南大）、Sekai（上海 AI Lab）等团队合作，推动 camera trajectory 格式标准化
   - 与 Action100M（Meta）团队合作，探索 action 标注体系的互操作性
   - 与 WildWorld（盛大）团队合作，研究合成数据与真实数据的联合训练策略

4. **分阶段开源策略**：先开源数据集的核心子集和评测基准，吸引社区早期用户和反馈，再逐步开放完整数据集。配套发表技术报告 / 论文，详细阐述数据采集、标注流程和质量控制方法。

5. **面向下游任务的预训练模型**：在数据集基础上训练并开源基线模型（如 action-conditioned video generation 模型），降低社区使用门槛，加速数据集的学术影响力积累。

---

