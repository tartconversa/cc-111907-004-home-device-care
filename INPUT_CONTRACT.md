# 家庭设备告警的有效干预与照护负担：资料契约

所有资料为本地合成记录，不包含真实个人或企业数据。业务含义以下文及字段字典为准。

普通事项等待、领取、联系未果、已核实无需行动、已完成协助为不同状态；联系未果仍占用一次当日打扰次数。紧急新事件指在已有联系之后再次出现满足最低样本的 urgent_below 读数。撤回生效后新分析须剔除对应范围内历史读数，既有联系只保留时刻、工作人员、处置类型及必要交接证明，不能向未授权人员展示读数。授权不能由高优先级覆盖。

时间均带时区并以 UTC 记录，区间左闭右开；received 是事实进入系统的时点，不可在更早的分析中使用。null 代表缺失或尚未知，不能默认0或正常。输入顺序不代表业务顺序。来源主键标识事实，带版本的来源行以来源身份和版本解释，来源版本冲突须保留问题。重复投递同一事实不产生新的业务活动。

## households

回访约束是住户决定的业务边界。

| 字段 | 含义 |
|---|---|
| `household` | 家庭 |
| `contact_start` | UTC日内开始小时 |
| `contact_end` | UTC日内结束小时 |
| `daily_limit` | 每日联系上限 |
| `region` | 区域 |

## devices

校准与时钟偏移，corrected_value=raw*scale+offset；device_time 加 clock_minutes 为实际观察时间。

| 字段 | 含义 |
|---|---|
| `device` | 设备 |
| `household` | 家庭 |
| `metric` | 指标 |
| `scale` | 倍率 |
| `offset` | 偏移 |
| `clock_minutes` | 设备时钟修正 |
| `valid_from` | 校准开始 |
| `valid_to` | 校准结束 |

## readings

读数单位为无量纲活动指数，null 表示未知。

| 字段 | 含义 |
|---|---|
| `id` | 读数标识 |
| `device` | 设备 |
| `device_time` | 设备时刻 |
| `received` | 接收时刻 |
| `value` | 原读数 |

## outages

家庭共同断网区间。

| 字段 | 含义 |
|---|---|
| `id` | 中断 |
| `household` | 家庭 |
| `start` | 开始 |
| `end` | 结束 |

## consent

授权分别限制分析、联系；只对对应范围适用。

| 字段 | 含义 |
|---|---|
| `id` | 记录 |
| `household` | 家庭 |
| `scope` | analysis/contact |
| `effective` | 生效 |
| `status` | active/revoked |

## policy

同户同政策30分钟内属于一项生活事件；持续新高风险不能无限去重。

| 字段 | 含义 |
|---|---|
| `version` | 版本 |
| `effective` | 生效 |
| `metric` | 指标 |
| `window_minutes` | 窗口 |
| `min_samples` | 最少有效点 |
| `lower` | 低于该值需跟进 |
| `urgent_below` | 紧急低值 |
| `followup_minutes` | 普通时限 |
| `urgent_minutes` | 紧急时限 |

## staff

单次联系耗时15分钟，不跨越可用结束时刻。

| 字段 | 含义 |
|---|---|
| `staff` | 工作人员 |
| `regions` | 可服务区域 |
| `start` | 可用开始 |
| `end` | 可用结束 |

## 后续资料

这些记录是可扩充的初始事实，不是固定答案。使用者需能导入相同契约的新记录，并通过应用登记题述的人工作业。新增的业务状态、使用入口及存储由实现者设计；来源资料文件只读，结果放入单独的工作目录，互不相关的工作目录不能相互污染。
