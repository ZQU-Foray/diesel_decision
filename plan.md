# 开发计划

## 任务列表

- [ ] 定义 `WorldSnapshot` / `ActionMask` / `Decision`（对齐 `diesel_interfaces`）
- [ ] 实现 L5-b 技能层与 mask 计算函数（含 3v3 哨兵 4–5 态技能）
- [ ] 实现规则决策器（效用打分），跑通统一签名
- [ ] 实现 L5-c 安全门（指令仲裁 + 开火授权 + IFF）
- [ ] 起草区域划分表 + 转移时间矩阵
- [ ] 搭轻量对局仿真最小模型

## 当前 Agent State

PLAN_READY

## Before Snapshot

commit hash:   （首次提交）
branch:        main
modified files: （首次初始化）
risk level:    L1

## 模糊点与待确认项

- 本仓接口尚未冻结，任务清单为**方向性**的，落地顺序以 `foray_docs/algorithm_structure.md` §9 演进阶段为准
- 依赖的自研仓尚未建齐，跨仓任务需等对方 `README.md` 明确接口后再启动

## Vector Backend Status

Backend: Markdown
Status:  ready
Environment: 仓库内 Markdown 文档（无外部向量后端）
Index: 本仓 `README.md` / `tree.md` / `decision.md`
Initialization: 2026-02-19
Commit: （首次提交）

## Acceptance Criteria

- [ ] 本仓能独立 `colcon build`（无代码时跳过）
- [ ] CI 绿灯
- [ ] 每个新增模块带独立可执行测试
