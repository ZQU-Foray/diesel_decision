# diesel_decision

> **L5** · owner 角色：决策

L5 三层决策 + L2 横向协同端口 + X5 分布式协同

决定「做什么」，但只输出**意图**——安全包络由技能层与安全门强制。

---

## 快速开始

> 本仓处于**初始化状态**，尚无源码。以下流程随 P0/P1 落地逐步可用。

```bash
# 1. 拉取（仓齐备后改为从元仓 diesel_ws 一键拉取）
git clone git@github.com:ZQU-Foray/diesel_decision.git
cd diesel_decision

# 2. 依赖安装
rosdep install -y -r --from-paths . --ignore-src --rosdistro humble

# 3. 构建
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release

# 4. 运行
# （待装配根就绪后补充）
```

## 依赖

| 类型 | 依赖 |
|---|---|
| **自研仓** | X2、L1、L2、L3、L4、L5、X5 |
| **第三方** | BehaviorTree.CPP · （红队 RL，仿真侧）PyTorch / Gymnasium |

## 接口

**唯一接口**（`foray_docs/algorithm_structure.md` §4）：

```cpp
Decision decide(const WorldSnapshot & world, const ActionMask & mask);
```

| 子层 | 频率 | 职责 |
|---|---|---|
| L5-a 战术层 | 1–5 Hz | 选技能。**RL ∥ 规则，同一接口可互换** |
| L5-b 技能层 | 10–50 Hz | 展开 + 前置条件检查 + **产出 `ActionMask`** |
| L5-c 安全门 | 事件驱动 | 指令仲裁 · 开火授权 · IFF |

## `mask` 必须进接口（本仓最关键的签名决策）



因为 `(state, mask) → action` 正是 **masked policy** 的标准形式。

把 mask 放进接口，让 FSM / 行为树 / 效用打分 / RL **结构上完全同构**：



- 对 RL：mask 逻辑训练与部署**共用同一份代码** → 消除「训练-部署不一致」这个最大的 RL 工程坑

- 对规则：mask 即「前置条件检查」

- **两者共用同一个 mask 计算函数（由 L5-b 提供）**



## 安全纪律



- **RL 只能输出意图**，安全包络由 L5-b / L5-c 强制，**不可由 RL 自觉**

- 训练时也必须施加 mask 与观测噪声

- 步兵开火必须人工授权；**安全门不得由 RL 或视觉模块自行放宽**



## 机间协同（X5）



- 共享的是**态势语义**，不是原始点云

- 一致性模型：**最终一致** + 时间戳 + 置信度（非强一致）

- **绝不在实时路径上**；本地感知优先级永远高于共享信息

- 带宽分级：`敌方位置+置信度` ≪ `占据栅格增量` ≪ `降采样点云` ≪ `原始点云`



## 对局仿真（临时）



轻量对局仿真（**无物理引擎**的区域级随机博弈）起步期放本仓，成熟后拆出 `diesel_battle_sim`。

硬性要求：输出的 `WorldSnapshot` schema 与实车**完全一致**；单局耗时 << 1 s；同种子可复现。

## 上下游

| 方向 | 对象 |
|---|---|
| **上游**（本仓依赖谁） | `diesel_interfaces` · `diesel_platform` · `diesel_vision` |
| **下游**（谁依赖本仓） | `diesel_robots` · `diesel_ws` |

## 参考

- [算法结构](https://github.com/ZQU-Foray/foray_docs/blob/main/algorithm_structure.md)
- [仓库结构](https://github.com/ZQU-Foray/foray_docs/blob/main/repository_structure.md)
- [组织贡献指南](https://github.com/ZQU-Foray/.github/blob/main/CONTRIBUTING.md)
