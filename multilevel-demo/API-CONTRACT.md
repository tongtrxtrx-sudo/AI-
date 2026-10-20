# 多层合并中文 Demo 接口契约（2026-10-20）

本版本与旧 Demo 数据目录独立。schemaVersion=2，product=`SelfDevelopedEPM-Multilevel-Demo`。仅监听127.0.0.1，Python标准库，支持`python -I backend/server.py --data-dir PATH --port 0 --no-browser --static-dir PATH`。健康/启动器控制、会话、CSRF、runtime/open-data-dir/shutdown沿用旧协议；会话cookie改`epm_group_session`避免旧软件冲突。启动凭据只在私有进程管道/实例文件中，不公开。

## 读取与提交

GET `/api/bootstrap?group=TY_GROUP&month=9&currency=CNY&entity=all`。

context.group为动态合并单元id（初始TY_GROUP/HW_GROUP/OS_GROUP），month为1..12，entity=all或当前单元中法人ID。报表币种由所选单元/法人本位币决定；context.currency只接受与其一致的值，不能用旧单一汇率把整张报表任意换币。全资示例年度固定2026。

POST `/api/action`：`{action,payload,revision,context}`，同源Origin、cookie与`X-CSRF-Token`必须有效。成功返回完整bootstrap；失败`{error:{code,message},message}`。只有`revision_conflict`409需刷新；其他409显示准确业务原因。

## bootstrap 顶层字段

- 保留revision/companyName/settings/sessionRole/capabilities/budget/activities/accounts/statement/report/checks/context。
- `units:[{id,name,currency,members:[{type:'entity'|'unit',id}],version}]`。
- `entities:[{id,code,name,short,owner,currency,parentId,percent,...}]`，初始 TY/HW/HW_CAT/HW_ELEC/OTHER/OS_A/OS_B。管理成员与真实持股关系分开。
- `sources:{entityId:source}` 当前月的法人报送。source含version/currency/opening/closing/profit/monthlyProfit/cashflow/monthlyCashflow/capitalMovement。金额单位为各法人本位币万元（USD即万美元）。source closing.retained 已包括当期损益；不能重复结转。
- `rateSets:[{id,month,version,sourceCurrency,targetCurrency,opening,closing,historicalCapital,averages,flowRates,active}]` 当前月汇率历史；各表报价均为1USD=多少CNY。averages/flowRates的key是月份字符串。
- `eliminations` 当前月本层及下层抵销配置，含id/sourceId/unitId/month/kind/sellerId/buyerId/currency/amount/status/version/effects。
- `results` 当前月各单元全部版本详情。每项含id/unitId/month/version/origin/createdAt/approvedAt/approvalStatus/valid/staleReason、statement/report/checks/passed/leafIds/eliminationIds/inputs/trace/dependencies。origin初始为系统初始化演示，其他为用户计算。历史结果快照保留，不覆盖。
- `currentResult` 当前单元active结果或null。`unitStatus:{unitId,locked,activeResultId,valid,status,staleReason}`，`unitStates:{unitId:unitStatus}`。
- `preconditions:[{code,label,passed,message}]` 当前单元计算前置检查。上级只允许有效已签核下级结果。
- `reportMode` 为已签核/已计算/历史失效结果/预览；主statement必须与此状态一致，不能把预览算成已签核。
- `ownership` 真实全资股权配置；`budgetComparable`仅集团人民币all法人视图为true。预算比较基准由budget.comparisonAnnual提供，不与USD实绩混加。

## 动作（本表为唯一正式名称）

| action | payload | 说明 |
| --- | --- | --- |
| role_switch | `{role}` | 会话演示角色，非生产身份；不改业务版本 |
| entity_save | `{id,code,name,short,owner,currency,parentId?,percent?}` | 负责人；币种CNY/USD，演示仅100%权益 |
| ownership_save | `{entityId,parentId,percent,method:'全额合并'}` | 真实股权；不随管理范围自动改 |
| unit_save | `{id,name,currency,members:[{type,id}]}` | 负责人；拒绝环、重复叶子/节点；有结果需先重新开启 |
| source_save | `{entityId,source:{opening,closing,profit,monthlyProfit,cashflow,monthlyCashflow,capitalMovement,equityHistory?}}` | 当前月；typed字段校验，币种由法人固定；服务端递增版本 |
| source_import | `{csv,sourceName}` | 完整财务源CSV，整批原子校验；格式见下 |
| tb_import | `{csv,sourceName}` | 兼容旧5列试算：须有本期源报送，保留明确现金流源；现金流不符拒绝，不能自动估算填平 |
| rate_set_save | `{id?,sourceCurrency:'USD',targetCurrency:'CNY',opening,closing,historicalCapital,averages:{'1':7,...},flowRates:{'1':7,...}}` | 当前月，正数最多6小数；新增不可覆盖的版本激活，关联结果stale；缺期计算拒绝 |
| elimination_save | `{id?,sourceId,unitId,sellerId,buyerId,kind:'内部销售'|'投资权益',amount,currency}` | 保存草稿；服务端生成effects；内部销售必须双方最低共同单元；sourceId不可跨层重复 |
| elimination_cancel | `{id,reason}` | 负责人；先重开关联已签核层，草稿或已批准事项停用，历史快照保留；草稿误建可取消恢复计算 |
| elimination_approve | `{id}` | 负责人审批，影响本层及依赖祖先 |
| consolidation_run | `{unitId?}` | 默认context.group；本月新结果版本，严格引用有效已签核子结果，不能直接递归预览充当审批结果 |
| result_approve | `{resultId}` | 负责人；只签核当前active未失效结果且所有check通过，锁当前单元 |
| unit_reopen | `{unitId?,reason,cascade:true}` | 负责人；明确将此单元及依赖祖先解锁并标旧结果失效，保留历史批准快照 |
| commentary_save | `{text}` | 保存当前单元经营说明 |
| settings_save | `{companyName?,fiscalYear:2026}` | 固定年度2026 |
| account_save | `{code,name,type,field,normalSide}` | 内置报表字段映射；有锁定结果先重开 |

预算动作`budget_layer_save/budget_spread/budget_driver/budget_plan_save/budget_submit/budget_approve/budget_lock/budget_copy/budget_switch/budget_adjust_create/budget_adjust_approve/budget_adjust_reject`沿用旧字段，预算只做集团CNY独立演示，不改变财务结果。

旧financial动作journal_create/journal_save/journal_approve/rate_save/checks_run/submission_confirm/ic_confirm/signoff/period_reopen不再作为新合并体系旁路。界面改用本表多层动作，旧动作调用须返回准确中文不支持消息，不得成功却只写旧state。

## 完整财务源CSV

UTF-8（可带BOM），表头：`法人编码,期间,数据区,项目编码,金额,月份`。

数据区为`opening/closing/profit/cashflow/monthlyProfit/monthlyCashflow`（模板同时给中文说明）。opening/closing使用内置BS key；profit为revenue/cost/expense/finance/tax；cashflow为operating/investing/financing/fx；monthlyProfit/monthlyCashflow必须填写1..当前月的月份，累计区月份留空。整批同context月，每个法人整套必填字段齐全，禁止重复区/项目/月。cashflow是明确报送输入，不能从期末现金差倒挤经营净额。source结构与字段键以CALCULATION-CONTRACT为准。

源数据、成员或抵销有已签核依赖时先unit_reopen(cascade:true)。汇率新增版本可显式使使用该币种转换的本期结果失效，并保留原汇率/报表历史。首启seed由同一计算/审批逻辑生成预置真实演示版本，不使用hardcode报表数字。

历史权益equityHistory可维护capitalByEntity（仅当前法人且等于期初资本）、openingRetainedCny（已确认期初人民币留存收益）、dividendsByMonth（本版必须为空）。有USD期初留存收益时必须提供确认人民币数。内部销售可传monthlyAmounts逐月金额分配；不传时明确认定本事项全部发生于当前月。

结果另含sourceInfo（产品/模型/源码SHA）、sourceSnapshots、rateSnapshots、unitSnapshots、eliminationSnapshots、ownershipSnapshot；均为计算时输入留档。entityActuals按所展示结果的源快照和报表币种分项折算，basis为“法人贡献（合并抵销前）”，不能将其直接相加当合并结果。
