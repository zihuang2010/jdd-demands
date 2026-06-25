# 影响面分析

> 写代码前先完成 P0 信息；实现过程中持续更新。

## 需求目标

- 背景：当前仅确认需求标题为“优化师傅列表”，具体痛点和业务入口待补充。
- 目标：在不破坏既有师傅列表业务语义的前提下，完成列表展示、查询或交互优化。
- 非目标：不默认改造师傅主数据模型、订单分配链路、结算链路、消息通知链路或其他非列表能力。
- 成功标准：用户确认的师傅列表优化项完成；核心查询、分页、权限、原有字段展示和回归链路通过验证。

## 受影响应用

| 应用 | 仓库路径 | 当前分支 | 期望功能分支 | 影响原因 |
| --- | --- | --- | --- | --- |
| 待确认前端应用 | 待确认 | 待门禁 | `feature/pis0sion/optimize-worker-list` | 师傅列表页面或组件可能调整。 |
| `worker-center`（候选） | `/Users/pis0sion/Pis0sion/PhpCode/WorkerApp/20260224` | 待门禁 | `feature/pis0sion/optimize-worker-list` | GitNexus 初步命中师傅列表查询和字段组装逻辑。 |

## 受影响接口

| 接口 | 调用方 | 提供方 | 变更类型 | 说明 |
| --- | --- | --- | --- | --- |
| 待复核 `Workers/MemberController::list` | 师傅列表页面 | `worker-center` | 待确认 | GitNexus 显示该入口调用 `MemberLogic::getList`，疑似通用师傅列表查询。 |
| 待复核 `Providers/TransferOrderController::getWorkerList` | 服务商转单链路 | `worker-center` | 受影响调用 | GitNexus impact 显示可能复用同一列表逻辑，若改共享方法需回归。 |

## 受影响代码符号

| 文件 | 类/函数/组件 | 变更类型 | 说明 |
| --- | --- | --- | --- |
| `app/Controller/Workers/MemberController.php` | `MemberController::list` | 复用/可能修改 | GitNexus 命中列表入口，需用实际代码确认路由、参数和权限。 |
| `app/Logic/MemberLogic.php` | `MemberLogic::getList` | 复用/可能修改 | 负责组装查询条件并调用通用列表方法。 |
| `app/Logic/MemberLogic.php` | `MemberLogic::getListByWhereCondition` | 高风险共享方法 | 负责字段选择和批量补充列表附加信息，直接改会影响多个调用。 |
| `app/Repository/MemberRepository.php` | `MemberRepository::getMemberList` | 复用/可能修改 | 底层列表查询，是否需要调整 SQL/索引待确认。 |

## 数据库影响

| 库表 | 变更类型 | SQL 路径 | 说明 |
| --- | --- | --- | --- |
| 待复核 | 暂定无 | 无 | 当前不默认新增或修改表；如查询性能优化需要索引，需升级风险等级并补 SQL。 |

## 配置影响

| 配置项 | 所属应用 | 环境 | 说明 |
| --- | --- | --- | --- |
| 暂无 | 暂无 | local/dev/test/pre/prod | 当前不默认涉及配置。 |

## MQ / MQTT / XXL-Job 影响

| 类型 | 名称 | 所属应用 | 说明 |
| --- | --- | --- | --- |
| 暂无 | 暂无 | 暂无 | 当前不默认涉及 MQ、MQTT 或 XXL-Job。 |

## 外部接口文档

| 文档 | 本地路径 | 是否已核对 | 说明 |
| --- | --- | --- | --- |
| Easy开放平台 | `docs/references/easy` | 否 | 当前未确认涉及 Easy 接口；若后续涉及，优先检索本地离线文档。 |

## 风险与边界

- 权限/角色边界：待确认师傅列表入口角色和数据权限，不默认放宽权限。
- 幂等与重试：列表查询通常无写入幂等；若新增批量操作或导出任务需重新评估。
- 状态流转：不默认调整师傅状态、审核状态或订单状态流。
- 兼容性：如调整响应字段，需保证现有前端字段使用不被破坏；共享方法还需保护转单取师傅列表等复用方。
- 回滚点：前端页面改动可回退到原组件；后端接口字段或 SQL 改动需单独登记回滚方式。

## 验收命令

```bash
# 待确认具体服务后替换
mvn -B -ntp -nsu test -DskipTests=false

# worker-center 若确认为目标仓，需改为该 PHP 仓实际测试命令。
```

## 更新记录

| 日期 | 更新人 | 变更 |
| --- | --- | --- |
| 2026-06-24 | Codex | 初始化需求影响面骨架。 |
| 2026-06-24 | Codex | 记录 GitNexus 初步定位的 `worker-center` 候选后端链路和共享方法风险。 |
