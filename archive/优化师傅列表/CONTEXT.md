# 需求上下文

## 需求概述

- 需求名称：优化师傅列表
- 需求英文名：`optimize-worker-list`
- 当前阶段：需求澄清
- 需求分级：暂定 L2 标准需求，待确认是否涉及接口、权限、SQL、配置或多服务后调整。
- 涉及应用：
  - 待确认：师傅列表所在前端应用。
  - 候选后端：`worker-center`，GitNexus 已索引路径 `/Users/pis0sion/Pis0sion/PhpCode/WorkerApp/20260224`。
- 建议功能分支：`feature/pis0sion/optimize-worker-list`

## 范围边界

### 本期实现

- 梳理现有师傅列表入口、查询条件、列表字段、分页、排序、权限与数据来源。
- 根据确认后的产品目标优化师傅列表展示或查询体验。
- 如涉及接口字段或筛选条件调整，同步更新接口设计、影响面和测试用例。

### 本期不实现

- 不默认新增师傅主数据模型、订单状态模型或新的补偿链路。
- 不默认调整与师傅列表无关的师傅详情、订单分配、结算、消息通知等业务。
- 不在未完成既有业务复核前新增表、重写状态流或引入新事件。

## 当前进展

| 日期 | 阶段 | 进展 | 证据 |
| --- | --- | --- | --- |
| 2026-06-24 | 需求启动 | 初始化需求资料仓并创建“优化师傅列表”五件套骨架。 | `task/demands/active/优化师傅列表/` |
| 2026-06-24 | 现状复核 | 通过 GitNexus 初步定位候选后端 `worker-center`，疑似通用师傅列表链路为 `Workers/MemberController::list -> MemberLogic::getList -> MemberLogic::getListByWhereCondition`。 | GitNexus repo `worker-center` 查询结果 |

## 关键结论

- 需求资料仓已挂载在主仓 `task/demands/`，具体需求资料只在该独立仓追踪。
- 需求实例中的 `plans/` 默认不进入 Git；模板计划文件仍可追踪。
- 当前只有需求标题，具体优化范围、涉及端、接口和验收标准待确认。
- GitNexus 辅助结论：`MemberLogic::getListByWhereCondition` 会组装师傅列表字段，并批量补充保证金、停接标记、预警规则、匠铭管控、推荐人和推荐人数等信息。
- GitNexus 影响面提示：直接改 `MemberLogic::getListByWhereCondition` 会影响 `Workers/MemberController::list`、`Providers/TransferOrderController::getWorkerList` 和 `MemberLogic::assertPaginatorAbilityList` 等调用，不能在目标不清时直接改共享方法。

## 待办与阻塞

| 优先级 | 事项 | 负责人 | 状态 | 说明 |
| --- | --- | --- | --- | --- |
| P0 | 确认师傅列表所在产品入口和前端应用 | 产品/开发 | 待处理 | 需要明确是管理端、客户端还是其他后台页面。 |
| P0 | 确认本次“优化”的具体目标 | 产品/开发 | 待处理 | 例如字段展示、筛选项、排序、性能、权限、分页、导出或交互。 |
| P0 | 确认是否使用 `worker-center` 作为目标后端仓 | 开发 | 待处理 | 当前仅有 GitNexus 索引辅助结论，还需确认实际代码工作区和分支。 |
| P1 | 补齐测试数据和验收标准 | 测试/开发 | 待处理 | 需要可执行的 P0/P1 和回归场景。 |

## 续期恢复提示

继续本需求时，Codex 必须先读取：

1. 本文件。
2. `impact-map.md`。
3. 当前 `plans/*.md`，若本地存在。
4. `system-design.md`。
5. `测试用例.md`。
6. `发布执行手册.md`。

然后输出“续期恢复结果”，不得凭对话记忆继续。
