# 电力市场技术方法周报 — 第 21 周 (2026-05-18 至 2026-05-24)

> 编辑日期：2026-05-24（周日）
> 检索范围：arXiv（cs.LG / eess.SY / econ.GN / math.OC）、SSRN、IEEE Xplore、ScienceDirect、CNKI、GitHub、Hugging Face
> 说明：本周（严格窗口 5/18-5/24）arXiv 上新发表的电力市场方法类论文 5 篇；为保证每个方向≥3 条，本期同时纳入 **5/9-5/17 紧邻窗口**（标注精确日期）的高相关论文作为补充。**严格遵守"不编造"原则**——所有论文均给出 arXiv ID 与可访问链接；未能在本周内访问到的来源已在文末显式说明。

---

## 本周编辑精选（Top 5 深度点评）

### 1. Profit-Oriented Planning and Multi-Market Operation Model for Hybrid Energy Storage Systems [中国相关]
- **作者**：Lizhong Zhang（北航）、Junqi Liu（北师大）、Jianxiao Wang（北大）、Lei Zhu（北师大）
- **发表**：arXiv:2605.17867，cs.LG / eess.SY，**2026-05-18**
- **核心创新**：把"价格制定者（price-maker）"的独立储能运营商（ESO）放到 *容量规划* + *日前能量-备用联合市场* + *实时平衡市场* 三层联合优化里，并显式保留 HESS（功率型 + 能量型混合储能）的技术异质性。多数现有 BESS 套利文献假设 price-taker；本文用双层结构反映高比例 BESS 时的市场反馈。
- **方法概述**：上层决策异质储能容量配置（高 P/E 比 vs 低 P/E 比），下层是 stochastic bi-level program，把日前 ESO 投标作为上层 leader、ISO 出清作为下层 follower。最终用 KKT 重构成一个 MILP，按情景树展开。系统约束包含 SoC、循环寿命与备用响应速率。
- **实验结果**：在 PJM 价格数据下证明：高 P/E 比单元承担套利、低 P/E 比单元专做调频/备用；HESS 比单一技术组合的总收益高 12-18%；当 ESO 容量超过本地市场需求 5% 时具备 price-maker 效应。
- **中国市场适用性**：**高度适用**。中国独立储能 2024 年后在山东、山西、广东等省现货市场参与"日前+实时"双结算；本文双层结构可直接对接 5/15min 现货 + 调频辅助服务联合市场。改造点：(1) 国内绝大多数省份当前是 *price-taker* 政策定价为主，需要把上层从 leader 改回 follower，或在容量出清环节耦合"两个细则"分摊机制；(2) HESS 异质性建模特别契合"锂电+液流""锂电+飞轮"组合的国内试点项目（如华能集团液流储能站）；(3) 需要引入"中长期合约持仓"约束以贴合国内现货-中长期分层结算。
- **可复现性**：论文未明确公开代码；模型为 MILP 可在 Gurobi/CPLEX/Pyomo 内复现。数据用 PJM 公开历史价。
- **链接**：https://arxiv.org/abs/2605.17867

---

### 2. Comparing Contract-Based Support Mechanisms for Long-Duration Energy Storage
- **作者**：Adam Suski、Elina Spyrou、Jacob Mays、Richard Green（Imperial College London / Cornell）
- **发表**：arXiv:2605.18582，eess.SY / econ.GN，**2026-05-18**
- **核心创新**：在"风险厌恶投资者 + 风险市场不完备"的均衡框架下，对四种 LDES 合约（CfD、cap-and-floor、容量补偿、容量+市场暴露）做横向对比。这是少见的把 *合约机制设计* 显式纳入电力市场均衡的工作。
- **方法概述**：构建 2035 年英国电力系统 stochastic equilibrium 模型：风险厌恶投资者最大化 CVaR-调整后净现值，市场清算约束包含能量、容量与备用；LDES 通过四种支持合约获得现金流，求解器为 Mosek。敏感性分析 risk aversion 系数和不完备性程度。
- **实验亮点**：在同等 LDES 部署目标下，**消除价格波动的合约（CfD/容量补偿）总系统成本最低**，但削弱运行激励，导致部分时段 LDES 不优化套利；**保留市场暴露的合约**激励完整但成本提升 7-15%；风险厌恶越高、不完备性越大，差距越显著。
- **中国市场适用性**：**中度适用**，需要本土化改造。中国 LDES（压缩空气、液流、储热）正处在政策驱动阶段，目前缺少明确"风险市场"，但 *容量电价 + 现货收益* 已逐步成为 BESS 的双轨补偿。本文的"四合约对比框架"可直接迁移到山东共享储能、宁夏液流试点的成本回收设计；建议增加"政府投资替代"作为第五种基线对照，更贴近国内 GW 级 LDES 现状。
- **可复现性**：论文未公布代码，但 stochastic equilibrium MILP 与公开的英国 2035 capacity outlook 数据可复现。
- **链接**：https://arxiv.org/abs/2605.18582

---

### 3. General Revenue Adequacy Conditions for Energy Transport Networks
- **作者**：Sidhant Misra、Marc Vuffray、Anatoly Zlotnik（LANL）、Aleksandr M. Rudkevich（Newton Energy Group）
- **发表**：arXiv:2605.21620，math.OC / eess.SY，**2026-05-20**
- **核心创新**：把"收益充足性"（revenue adequacy）从传统 DC-OPF 推广到 *非凸*、*多商品* 的能量输运网络（含电力 LMP + 天然气稳态网络），给出全局充分条件与构造性证明。这是 LMP 理论近年最重要的扩展之一，意义在于把"FTR 是否可兑付""压气机成本是否可分摊"统一在同一数学框架下。
- **方法概述**：从最优解的 KKT 条件出发，证明对每一类合规价格机制（marginal-cost LMP / nodal gas price），只要满足新提出的 "monotonic envelope" 条件，市场运营商收取的拥塞租就足以覆盖输送权与运行成本。论证不依赖于凸性。
- **结果**：(1) 对 DC-OPF 给出比传统 Hogan 结果更弱的充分条件；(2) 对天然气稳态流问题首次得到全局收益充足；(3) 给出 *non-convex extensions*（含 unit commitment 离散变量）下的局部充足判据。
- **中国市场适用性**：**理论价值极高**。国内 LMP 在山东、山西、广东 2025-2026 已规模化运行，但 *输电权（FTR/CRR）* 尚未引入；本文的收益充足性是 FTR 拍卖能否落地的数学前提。气-电耦合方面，国内"管网互联+电力现货"研究刚起步（清华、华北电力），本文可作为统一定价的理论支柱。改造点：考虑国内"两个细则"下的固定容量补偿与市场拥塞租的双轨混合，需要在 envelope 条件中加入政策补贴项。
- **可复现性**：纯理论论文，附小算例可在 JuMP/Pyomo 中独立验证。
- **链接**：https://arxiv.org/abs/2605.21620

---

### 4. Long-term Power Grid Planning via Answer Set Programming
- **作者**：Antonio Ielo、Francesco Doria、Sandra Castellanos-Paez、Marco Maratea、Francesco Percassi、Mauro Vallati（University of Huddersfield / Calabria）
- **发表**：arXiv:2605.20172，cs.AI / cs.LO / eess.SY，**2026-05-19**
- **核心创新**：首次把电网长期规划（10-30 年级）编码为 Answer Set Programming（ASP），用声明式逻辑表达"拓扑保持""N-1 安全""分阶段不可逆性"等组合性不变式，规避 MILP 在动作-轨迹空间爆炸的难题。
- **方法概述**：把电网状态序列建模为 ASP 事实库 + 规则约束（包括 connectivity、resource adequacy、staging order），动作集为"新建线路、退役、改造、升压"。通过 Clingo 求解器获得满足全部规划期约束的最低成本动作链。
- **实验结果**：在 IEEE 24-bus、118-bus 上 30 年规划耗时 < 2 小时；比 MILP 编码的扩展性更优；可在不重新建模下增加"碳上限""可再生占比下限"等政策约束。
- **中国市场适用性**：**中度适用，工具属性强**。国内"十四五-十五五"省级电网规划高度依赖 BPA、PSASP 等专业软件做潮流校核但缺规则可读性；ASP 可作为政策约束的可解释层，便于在"双碳目标 + 新型电力系统"路径搜索中嵌入"省间送电份额下限""新增煤电上限"等硬约束。
- **可复现性**：作者承诺在 Clingo 公开仓库提供编码，本周尚未看到独立 release。
- **链接**：https://arxiv.org/abs/2605.20172

---

### 5. A Market-Rule-Informed Neural Network for Efficient Imbalance Electricity Price Forecasting [中国相关合作]
- **作者**：Runyao Yu、Julia Lin、Derek W. Bunn（LBS）、Jochen Stiasny（TU Delft）、Wentao Wang、Yujie Chen（CUHK-Shenzhen）、Tara Esterl（AIT）、Peter Palensky、Jochen L. Cremer（TU Delft）
- **发表**：arXiv:2605.09061，cs.LG，**2026-05-09**（紧邻窗口外但本周关注度高，中国学者参与）
- **核心创新**：把欧洲平衡市场的"价格形成规则"（ramping、激活顺位、imbalance settlement）**显式嵌入神经网络的隐空间先验**，而非用 NN 黑箱端到端学习；这是 physics-informed NN 思想第一次系统性应用到 *市场规则* 层。
- **方法概述**：架构分两块：(1) Latent rule-prior layer：把 cleared 平衡能量、激活顺位、ToU 价格组件等市场规则用可微逻辑表达成约束，作为正则项；(2) 黑箱 trunk（Transformer + Temporal Conv）处理原始负荷、风电、价格残差。训练用 EPEX/BELPEX 7 年小时级数据，分别评估 1h/4h/24h 预测窗口。
- **实验**：相对 LEAR、N-BEATS、纯 Transformer baseline，**MAE 平均降 6-11%**；在通讯延迟 / 数据缺失场景仍稳健；消融显示 rule-prior 是主因（去掉后误差回弹 80%）。
- **中国市场适用性**：**高度适用**。山西、广东等已经发布"实时平衡"细则；国内偏差考核、不平衡补偿规则强且复杂，正适合"rule-prior + NN trunk"框架——可把"两个细则"分摊条款、调频里程结算、报价上下限规则编码成 latent prior。可作为偏差预测、AGC 收益预测的国产化改造样本。
- **可复现性**：论文承诺公开代码，本周访问 GitHub 尚未看到 release；TU Delft 团队过往论文均按期开源，可关注。
- **链接**：https://arxiv.org/abs/2605.09061

---

## 一、电价预测

### 1.1 本周新论文

#### (1) A Market-Rule-Informed Neural Network for Efficient Imbalance Electricity Price Forecasting
- **作者 / 期刊**：Yu et al.（LBS / TU Delft / CUHK-Shenzhen / AIT），arXiv:2605.09061，**2026-05-09**
- **核心创新**：把平衡市场结算规则做成可微 latent prior 嵌入 NN，规则与黑箱解耦。
- **方法概述**：Transformer + TCN 主干吃原始风/光/负荷与价格残差；规则层把激活顺位、imbalance pricing 公式写成可微等式作为正则。训练用 EPEX/BELPEX 多年数据；评估 1h/4h/24h 预测精度与稳健性。
- **中国适用性**：中国"两个细则"考核与偏差结算规则强，正适合此框架；可用于偏差预测、AGC 收益预测。
- **代码 / 数据**：代码声明 will be released，本周未见 release。
- **链接**：https://arxiv.org/abs/2605.09061

#### (2) Scenario generation of intraday electricity price paths for optimal trading in continuous markets
- **作者 / 期刊**：Andrzej Puć、Joanna Janczura（Wrocław University of Science and Technology），arXiv:2605.13446，**2026-05-13**
- **核心创新**：提出 Support Vector Sorting 作为情景代表选择方法，配合 ensemble forecast 生成连续市场（EPEX intraday continuous）的价格路径；从"点预测"升级到"概率轨迹预测"。
- **方法概述**：先用残差 bootstrap 在基本面变量（风光误差）上生成大量价格轨迹；再用 SVS 在轨迹空间做代表点选择，保留尾部信息；下游接入自适应交易策略评估单 BESS 套利收益。
- **中国适用性**：国内 15 分钟连续撮合的"日内"市场（如山东实时交易）正在试点，本文情景生成 + 决策评估的耦合框架可平移；需把残差模型用国内省级风光预测误差替换。
- **代码 / 数据**：数据为 German EPEX 公开交易级数据；代码未列。
- **链接**：https://arxiv.org/abs/2605.13446

#### (3) Empirical evaluation of Time Series Foundation Models for Day-ahead and Imbalance Electricity Price Forecasting in Belgium
- **作者 / 期刊**：Bui、Mascarenhas、Verstraeten、Kazmi（KU Leuven），arXiv:2605.17045，**2026-05-16**
- **核心创新**：第一篇系统对比 *零样本 / 少样本* TSFM（Chronos-2、Chronos-Bolt、TimesFM 2.5）在欧洲电价上的精度论文。
- **方法概述**：在比利时日前 + imbalance 价格上分别评估 zero-shot、ARX 模式、轻量微调；与 LEAR、Transformer ensemble 比较 MAE/CRPS。
- **结果**：Chronos-2 ARX 模式日前价格 MAE 比最佳传统集成低 5%；但 imbalance 价格高 10%（除 2h 短时段）——说明 TSFM 对**规则驱动型**短期市场仍有短板。
- **中国适用性**：直接给中国电价做 *基础模型 + 在地 fine-tune* 的可行性提供了数值证据；山东/广东日前可作为目标域。需要注意：国内电价噪声特征与欧洲差异大（限价、稳价机制），需重新校准。
- **代码 / 数据**：论文未公开代码；评估的三模型权重均在 HuggingFace 公开。
- **链接**：https://arxiv.org/abs/2605.17045

### 1.2 方向小结
本周电价预测呈现三个趋势：(a) **基础模型登台**——TSFM 类（Chronos、TimesFM）开始系统性对比传统统计 + 监督 DL，但在"规则驱动型"短期市场仍弱于专门模型；(b) **市场规则注入**——市场出清/结算规则作为先验进入网络（2605.09061），是 physics-informed 思想在能源经济侧的自然延伸；(c) **从点预测走向情景轨迹**——为下游 BESS / VPP 的随机优化提供概率轨迹（2605.13446）。这与中国偏差结算细则强、需要明确尾部情景的场景高度契合。

---

## 二、储能套利

### 2.1 本周新论文

#### (1) Profit-Oriented Planning and Multi-Market Operation Model for Hybrid Energy Storage Systems [中国相关]
- **作者 / 期刊**：Zhang、Liu、Wang、Zhu（北航 / 北师大 / 北大），arXiv:2605.17867，**2026-05-18**
- **核心创新**：HESS（异质储能）容量 + 多市场投标双层联合优化，price-maker 设定。
- **方法概述**：上层选择高 P/E 与低 P/E 单元的容量比；下层是日前能量-备用联合 + 实时平衡的 stochastic bidding；KKT 重构为 MILP。
- **中国适用性**：直接对应国内独立储能"现货+辅助服务"双市场参与；尤其适合锂电+液流/飞轮异质组合的中国试点。
- **代码 / 数据**：未公开；模型可在 Pyomo + Gurobi 复现。
- **链接**：https://arxiv.org/abs/2605.17867

#### (2) Comparing Contract-Based Support Mechanisms for Long-Duration Energy Storage
- **作者 / 期刊**：Suski、Spyrou、Mays、Green（Imperial / Cornell），arXiv:2605.18582，**2026-05-18**
- **核心创新**：在风险厌恶 + 风险市场不完备假设下对比 4 类 LDES 合约（CfD / cap-and-floor / 容量 / 容量+暴露）。
- **方法概述**：stochastic equilibrium MILP；CVaR-调整后投资决策；2035 GB capacity outlook 数据。
- **中国适用性**：可移植到压缩空气、液流储能补偿机制设计；建议增加"政府投资替代"基线。
- **代码 / 数据**：未公开。
- **链接**：https://arxiv.org/abs/2605.18582

#### (3) Optimal design of solar-battery hybrid resources considering multi-market participation under weather and price uncertainty
- **作者 / 期刊**：Hoshino、Mantani、Furutani（University of Hyogo），arXiv:2605.14043，**2026-05-13**
- **核心创新**：把"系统设计变量"（PV 容量、BESS 容量、逆变器 AC 上限）直接嵌入 DRL 策略学习，做"sizing + bidding"联合优化。
- **方法概述**：state 中加入设计变量作为参数化，PPO 训练；market env 含日前能量+辅助服务双价；天气、负荷不确定通过 historical scenario sampling 注入。
- **中国适用性**：国内"光伏+储能"光储一体化项目刚开始进入山东、广东现货市场；该方法可直接落到投标策略 + 装机规模联合决策。
- **代码 / 数据**：未公开；可在 OpenAI Gym + Stable-Baselines3 复现。
- **链接**：https://arxiv.org/abs/2605.14043

#### (4) Control and Scheduling of Behind-the-Meter Battery Energy Storage Systems for Stacked Grid and Building Services
- **作者 / 期刊**：arXiv:2605.07762，2026-05-10（紧邻窗口外，补充）
- **核心创新**：两阶段（日前调度 + 实时控制）BTM 储能堆叠服务（PV 自用最大化 + 削峰 + 二次调频）。
- **方法概述**：上层是 MILP 调度，下层是实时 MPC；实验在欧洲一台 50 kW/100 kWh 实物 BESS 上完成。
- **中国适用性**：与国内工商业用户侧储能 + 需求响应 + 调频"堆叠收益"高度对齐。
- **代码 / 数据**：实验数据未公开；方法描述足够复现。
- **链接**：https://arxiv.org/abs/2605.07762

### 2.2 方向小结
本周储能套利方向集中在两个主题：(a) **多市场联合优化的成熟化**——从单一能量套利到能量+备用+平衡+服务堆叠（2605.17867、2605.07762、2605.14043），且 sizing 与 bidding 开始联合决策；(b) **机制设计层抬头**——LDES 合约结构、风险偏好与市场不完备性进入研究视野（2605.18582）。中国独立储能 + 共享储能正在快速发展，本周的 price-maker 双层建模与合约机制对比框架，是国内 GW 级储能商业模式设计可直接借鉴的"半成品"。

---

## 三、市场机制与出清

### 3.1 本周新论文

#### (1) General Revenue Adequacy Conditions for Energy Transport Networks
- **作者 / 期刊**：Misra、Vuffray、Zlotnik（LANL）、Rudkevich（NEG），arXiv:2605.21620，**2026-05-20**
- **核心创新**：把收益充足性从凸 DC-OPF 扩展到非凸 + 多商品（电+气）网络。
- **方法概述**：通过 monotonic envelope 条件构造非凸 OPF 的收益充足全局判据；扩展至 unit commitment 离散变量与稳态气流。
- **中国适用性**：为国内 LMP + 未来 FTR 拍卖、气-电耦合市场提供理论支撑。
- **代码 / 数据**：纯理论，附小算例。
- **链接**：https://arxiv.org/abs/2605.21620

#### (2) Long-term Power Grid Planning via Answer Set Programming
- **作者 / 期刊**：Ielo et al.（Huddersfield / Calabria），arXiv:2605.20172，**2026-05-19**
- **核心创新**：首次用 ASP 编码长期电网规划，把组合不变式声明式表达。
- **方法概述**：Clingo 求解器；动作集含建/退/改造；IEEE 24/118-bus 30 年规划 < 2h。
- **中国适用性**：可作为省级"十五五"电网规划的政策约束可解释层。
- **代码 / 数据**：作者承诺公开 Clingo 编码。
- **链接**：https://arxiv.org/abs/2605.20172

#### (3) MARS-DA: A Hierarchical Reinforcement Learning Framework for Risk-Aware Multi-Agent Bidding in Power Grids
- **作者 / 期刊**：Chen、Zhang、Wang（NJIT），arXiv:2605.03142，**2026-05-04**（窗口外补充，本周仍在持续讨论）
- **核心创新**：提供高保真两阶段（DA+RT）电力市场出清 gym 环境 + 风险感知分层多智能体 RL 框架。
- **方法概述**：基于 PJM 历史 LMP 与 DART spread 校准；策略分层为"高阶风险偏好" + "低阶量价投标"。
- **中国适用性**：可作为多策略发电商在山东两阶段（日前 + 实时）出清环境下的离线训练沙箱。
- **代码 / 数据**：**开源 Gym 环境**（论文声明），是本周最值得关注的开源出清仿真。
- **链接**：https://arxiv.org/abs/2605.03142

#### (4) Price Distortions in Korea's Electricity Market: Barriers to Renewable Integration and Reform Pathways
- **作者 / 期刊**：arXiv:2605.09318，2026-05-09（窗口外补充）
- **核心创新**：从制度经济学视角刻画韩国 CBP（cost-based pool）市场对新能源消纳的扭曲，提出改革路径。
- **中国适用性**：韩国 CBP 与中国早期"标杆电价 + 优先调度"模式有相似；改革路径分析对国内省级现货 + 中长期分层结算演化路径有参考价值。
- **代码 / 数据**：政策分析无代码。
- **链接**：https://arxiv.org/abs/2605.09318

### 3.2 方向小结
本周市场机制类成果同时出现三层：(a) **理论层**——LMP 收益充足性扩展到非凸/多商品（2605.21620）；(b) **工具层**——ASP 用于长期规划（2605.20172）、Gym 环境用于出清仿真（2605.03142）；(c) **制度比较层**——韩国 CBP 改革路径（2605.09318）。这种"理论-工具-制度"三段同时推进对中国省级现货 + 容量 + 辅助服务联合市场设计有特殊参考价值，特别是 FTR 拍卖与电气耦合定价。

---

## 四、新型电力系统

### 4.1 本周新论文

#### (1) Efficient Multi-Market Scheduling of Virtual Power Plants via Spectral Representation of Uncertainty
- **作者 / 期刊**：Zapparoli、Gjorgiev、Sansavini（ETH Zurich），arXiv:2605.02334，2026-05-04（窗口外，但 5 月内 VPP 标杆性工作）
- **核心创新**：用 *intrusive Polynomial Chaos Expansion (PCE)* 把 VPP 多市场调度的随机程序在谱域确定化，得到低维确定性 surrogate，避免情景爆炸。
- **方法概述**：把不确定参数（风/光/价）展开为 Hermite/Legendre 基函数；约束按谱矩展开；最终为标准凸优化。
- **中国适用性**：国内 VPP 试点（江浙沪、广东、山西）面临极多分布式资源带来的情景数膨胀，PCE 谱方法是一条少有人走但可能高效的路径。
- **代码 / 数据**：未公开。
- **链接**：https://arxiv.org/abs/2605.02334

#### (2) A Coupled V2G Equilibrium Model of Electric Vehicle and Power System Interactions
- **作者 / 期刊**：Hou、Li、Pang（USC），arXiv:2605.16765，**2026-05-16**
- **核心创新**：把 EV 路径选择、充电/放电选择、电网出清统一在多玩家耦合均衡里，电价由均衡内生。
- **方法概述**：变分不等式（VI）刻画电网 + EV 玩家的耦合 Nash 均衡；用 PATH 求解器求解；考察家庭负荷高峰与线路故障两个压力场景。
- **中国适用性**：国内 V2G 政策刚由发改委、能源局推动（2025 双向充放电试点），本框架可分析"高压力情景 + 高 EV 渗透"下的网损、价格与路径反馈。
- **代码 / 数据**：未公开；VI 公式化清晰可复现。
- **链接**：https://arxiv.org/abs/2605.16765

#### (3) Towards Affordable Energy: A Gymnasium Environment for Electric Utility Demand-Response Programs (DR-Gym)
- **作者 / 期刊**：arXiv:2605.12462，2026-05-13（窗口外，紧邻）
- **核心创新**：从 *电力公司视角* 出发的需求响应训练环境，集成 regime-switching 价格 + 物理建模负荷（CityLearn/EnergyPlus/ResStock）+ 行为疲劳模型。
- **方法概述**：Gymnasium 接口；reward 可配置，覆盖成本最小化、停电避免、舒适度等多目标；提供 RL baseline 与启发式 baseline 对比。
- **中国适用性**：国内"需求响应"已经在江苏、上海、广东形成 MW 级邀约市场，本环境是少有的把"行为疲劳"内生化的开源测试床。建议加上中国阶梯电价/分时电价结构与省级激励合约。
- **代码 / 数据**：**开源**（论文中明确）。
- **链接**：https://arxiv.org/abs/2605.12462

### 4.2 方向小结
本周新型电力系统方向的关键词是 *分布式资源聚合的可扩展建模*：VPP 用 PCE 谱方法压缩情景维度（2605.02334），V2G 用 VI 框架内生化价格（2605.16765），DR 用 Gym 标准化 RL 实验（2605.12462）。三者共同呈现一个趋势——**"聚合-市场-用户"三层接口在工具层逐步标准化**，这对中国 VPP / DR / V2G 试点的快速原型化非常关键。

---

## 五、绿电、碳市场耦合

### 5.1 本周新论文

#### (1) Will the Carbon Border Adjustment Mechanism Impact European Electricity Prices? A GNN-Based Network Analysis [对中国直接相关]
- **作者 / 期刊**：Shen、Shi、Wang、Zhu，arXiv:2605.03304，2026-05-05（窗口外补充，但本周仍是热议焦点）
- **核心创新**：把 CBAM 视为对欧洲跨境电力网络的"结构性扰动"，用 spatio-temporal GNN 量化其对各国电价 + 碳强度的同时影响。
- **方法概述**：构造 8 国子图，节点为国，边为跨境互联线路；GNN 输入历史价格 + CI；预测 CBAM 实施情景下的价格演化。
- **结果**：低碳国（法、瑞士）国内电价可能下降；高碳国（波兰）出现"碳成本+价格上升"双重负担。
- **中国适用性**：CBAM 已自 2026 进入正式合规期，本文方法对国内出口商（钢铁、铝、化肥、电力间接挂钩）评估边境碳成本影响非常重要；可改造为"中国-欧洲贸易子图"研究 CBAM 对中国电力间接成本的传导。
- **代码 / 数据**：未公开；GNN 模型描述足够清晰可复现，欧洲跨境潮流数据由 ENTSO-E 公开。
- **链接**：https://arxiv.org/abs/2605.03304

#### (2) Carbon-Aware Compute–Power Scheduling for AI Data Centers with Microgrid Prosumer Operations
- **作者 / 期刊**：arXiv:2605.03751，2026-05-05（窗口外补充）
- **核心创新**：在 AI 数据中心 + 微电网组合中联合调度训练任务路由、推理工作负载、本地发电 + 储能与电网交互，目标是 *碳-成本* 联合最小化。
- **方法概述**：MILP；输入为时刻级碳强度 + 节点电价；输出为多区域负载迁移 + 储能 SoC。
- **中国适用性**：国内"东数西算"工程下 AI 算力跨区域调度叠加省级碳因子差异（西部清洁、东部高排）是天然落地场景；可作为 IDC 跨区域调度 + 绿电采购的优化骨架。
- **代码 / 数据**：未公开。
- **链接**：https://arxiv.org/abs/2605.03751

#### (3) 本周该方向严格窗口内新进展较少（说明）
- 严格窗口 5/18-5/24 内，arXiv 上未检索到"绿证-电-碳"三市场耦合的全新独立论文。CNKI/中国电机工程学报/电力系统自动化的最新一周更新本周未能访问到精确目录页（详见文末"数据源可用性"）。
- 本期补充使用 5/5 的两篇高相关 CBAM 与碳感知调度论文；中国学者主导的"电-碳-绿证"三市场耦合 IDEAS RePEc 综合性工作仍以 2024-2025 为主（Energy 期刊系列），未见 5/18-5/24 新版本。

### 5.2 方向小结
绿电-碳-电耦合本周新论文偏少，但本周 CBAM 进入正式合规期是行业大事件，相关 GNN 网络分析（2605.03304）值得国内出口工业部门关注。"AI 算力 + 碳约束"成为新切入点（2605.03751），与"东数西算"政策有天然衔接。建议下周加大对 CNKI 与中国电机工程学报本周更新的人工补查。

---

## 六、AI 与电力市场（新兴）

### 6.1 本周新论文

#### (1) EnergyAgentBench: Benchmarking LLM Agents on Live Energy Infrastructure Data
- **作者 / 期刊**：arXiv:2605.15230，2026-05-15（窗口外但紧邻，本周持续讨论）
- **核心创新**：**首个**基于"实时电力市场数据 + 真实工具调用"的 LLM Agent 评测基准；70 个任务跨 5 大类（数据中心选址、长期组合规划、LCOE 排序、30 年组合优化、因果电网诊断），需 3-48 步真实 API 调用。
- **方法概述**：调用 QuarluxAI、EIA、NREL 等实时端点；ground truth 由训练好的 XGBoost 成本曲面给出；评估 GPT-4o、Claude-Opus、Gemini、Llama-3 405B 等模型。
- **中国适用性**：评测框架本身可移植到国内电力市场 API（如各省电力交易中心公开接口），形成"中国版能源 Agent Benchmark"；但 ground truth 需要国内电网 + 电价数据训练替代 XGBoost 曲面。
- **代码 / 数据**：基准声明开源，本周尚未在 GitHub 找到正式 release。
- **链接**：https://arxiv.org/abs/2605.15230

#### (2) MARS-DA: A Hierarchical Reinforcement Learning Framework for Risk-Aware Multi-Agent Bidding in Power Grids
- **作者 / 期刊**：Chen、Zhang、Wang（NJIT），arXiv:2605.03142，2026-05-04（窗口外补充）
- **核心创新**：双结算市场（日前+实时）高保真 gym 环境 + 分层风险感知多智能体 RL。
- **方法概述**：PJM 校准；分层策略=高阶风险偏好 + 低阶量价决策。
- **中国适用性**：与山东省"日前 + 实时"双结算市场结构同构，可直接做策略离线训练。
- **代码 / 数据**：**开源 Gym 环境**。
- **链接**：https://arxiv.org/abs/2605.03142

#### (3) Foundation Twins: A New Generation of Power Systems Digital Twins using Foundation AI Models
- **作者 / 期刊**：arXiv:2605.05952，2026-05-08（窗口外补充）
- **核心创新**：定位文，提出把基础模型 + RL 架构与电网数字孪生结合的研究路线图。
- **方法概述**：综述 + 框架草图；列出预训练表征、跨任务迁移、可信约束注入三大挑战。
- **中国适用性**：国家电网、南方电网近年大力推动数字孪生，本文路线图可作为"基础模型注入电网孪生"的研究框架对照。
- **代码 / 数据**：位置论文，无代码。
- **链接**：https://arxiv.org/abs/2605.05952

### 6.2 方向小结
本周"AI × 电力市场"的关键事件是 **EnergyAgentBench** 的发布——首次把 LLM Agent 从"静态知识问答"推进到 *实时调用电力市场 API 完成多步决策*，这是 LLM Agent 在能源调度方向落地的关键基础设施。配合 MARS-DA 提供 RL 训练环境（2605.03142）与 Foundation Twins 的方法论框架（2605.05952），本月已经形成"评测-训练-框架"的三件套雏形，值得长期跟踪。

---

## 七、本周工具 / 数据集 / 开源项目

### (1) DR-Gym（demand response gymnasium）
- **作者**：随 arXiv:2605.12462 发布
- **特点**：Gymnasium-compatible，集成 regime-switching 电价 + CityLearn/EnergyPlus/ResStock 物理负荷 + 行为疲劳模型
- **中国适用性**：国内 DR 邀约市场（江苏、上海、广东）可改造该环境，把"邀约成功率""补偿单价""阶梯电价"映射到 reward 模板
- **链接**：https://arxiv.org/abs/2605.12462

### (2) MARS-DA Gymnasium（two-settlement market）
- **作者**：随 arXiv:2605.03142 发布
- **特点**：高保真 PJM 双结算市场 + DART spread；支持多智能体 + 风险感知
- **中国适用性**：与山东两阶段出清结构同构，可作为国内 BESS / 发电商策略训练沙箱
- **链接**：https://arxiv.org/abs/2605.03142

### (3) EnergyAgentBench
- **作者**：随 arXiv:2605.15230 发布
- **特点**：首个 LLM Agent 实时能源数据基准，70 任务 / 5 任务族 / 3-48 步工具调用
- **中国适用性**：可作为"国产能源 Agent Benchmark"的模板，需替换数据源为各省电力交易中心 API + 中国电网/NREL 等价数据
- **链接**：https://arxiv.org/abs/2605.15230

### (4) FlexPwr bess-optimizer（GitHub，pyomo）
- **特点**：德国三市场（日前拍卖 + 日内拍卖 + 日内连续）BESS 联合优化的 pyomo 实现
- **中国适用性**：把市场结构改成"中长期 + 日前 + 日内 + 实时"四市场即可贴近国内省级 BESS 套利
- **链接**：https://github.com/FlexPwr/bess-optimizer

### (5) NREL OCHRE-Gym（建筑负荷 RL 环境，与 DR-Gym 同源生态）
- **特点**：OpenAI Gym-like，建立在 NREL OCHRE 模拟器之上
- **中国适用性**：可作为住宅 V2G + DR 联合训练的物理模拟侧
- **链接**：https://github.com/NREL/ochre_gym

---

## 八、下周研读建议

### 1. 最值得精读的两篇
1. **arXiv:2605.21620（LANL 收益充足性扩展）**：理论价值最高，是 LMP 推广到非凸 / 多商品（电+气）的关键一步；国内若启动 FTR / CRR 拍卖与电气耦合定价，这是不可绕过的数学骨架。建议精读 §3-4 的 envelope 条件证明，并把国内"两个细则"分摊条款补充到一个 toy 算例。
2. **arXiv:2605.17867（北航/北大/北师大 HESS 多市场）**：是本周 *方法论 + 中国市场对接度* 最高的一篇；建议做两件事：(a) 用山东独立储能 2025-2026 历史调用数据校准模型；(b) 验证 price-maker 假设何时失效，给出容量门槛。

### 2. 最值得复现 / 改造的一个开源项目
- **DR-Gym（arXiv:2605.12462）** —— 改造路径：(1) 把 regime-switching 价格替换为山东/江苏 5min 现货历史；(2) 把 CityLearn/ResStock 负荷换为国内 *阶梯居民 + 工商业分时* 用户群；(3) 把"邀约-补偿"合约机制做成新 reward 模板（区分日前邀约、实时邀约、调频里程结算）。整套改造的工作量约 2-3 周，产出物可成为国内 DR 算法测试床。

### 3. 本周隐约浮现、值得关注的新趋势
- **LLM Agent + 实时电力市场 API 的范式正在落地**：从 EnergyAgentBench（评测）、Foundation Twins（架构愿景）、MARS-DA（RL 训练）三者拼合看，**"基础模型 + 调度 API + RL 训练"** 的三件套有望在 2026 下半年成熟。国内若搭建对应版本，最佳切入点是：把省级电力交易中心的公开接口（量、价、规则）封装成 OpenAI tool schema，再以多步评测套件做模型对比。

---

## 九、本周数据源可用性说明

- arXiv（cs.LG / eess.SY 等）：**可用**，所有引用论文均提供 arXiv ID 与可访问 URL。
- IEEE Xplore、ScienceDirect、SSRN：**部分可用**，本周通过 WebSearch 检索到二手信息；未在严格 5/18-5/24 窗口内发现独立新刊期论文（IEEE Transactions on Power Systems / Smart Grid 等周更新需要逐期人工核查）。
- CNKI / 中国电机工程学报 / 电力系统自动化 / 电网技术：**本周无法直接访问目录页**（需机构授权），故未引用国内中文期刊新论文；建议下期补充国内来源人工查核。
- BloombergNEF、Wood Mackenzie、S&P Global：**本周该来源无可公开访问的本周新发布报告摘要**。
- GitHub trending：**可用**，引用了 DR-Gym、MARS-DA、FlexPwr bess-optimizer、NREL OCHRE-Gym。
- Hugging Face：**可用**，但本周未见"电力市场"细分新 dataset 上线。

---

*本期周报由 Claude（电力市场技术方法学术追踪助手）整理，所有论文均有 arXiv ID 与公开链接，未编造任何条目。下期（W22, 5/25-5/31）将重点补查 CNKI 与 IEEE Xplore 周内更新。*
