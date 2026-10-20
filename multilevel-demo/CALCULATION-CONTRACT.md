# 分级合并纯计算契约

本文件由计算模块维护。函数只处理传入的内存数据，不写库，不管理会话。

## 顶层数据

- `entities`: `[{id,code,name,short,owner,currency,parentId,ownershipPercent,version}]`；法人本位币CNY/USD。
- `units`: `[{id,name,currency,members:[{type:"entity"|"unit",id}],version}]`。初始 `TY_GROUP`、`HW_GROUP`、`OS_GROUP`。
- `sources`: `{月份字符串:{法人id:source}}`，每月独立累计报送。`source` 含 `version,currency,opening,closing,profit,monthlyProfit,cashflow,monthlyCashflow,capitalMovement,equityHistory`。`closing.retained` 已含当期损益，计算不得重复结转。
- `fxRates`: `[{id,month,version,active,sourceCurrency:"USD",targetCurrency:"CNY",opening,closing,historicalCapital,averages:{月份:汇率},flowRates:{月份:汇率}}]`。报价1USD=若干CNY，美元乘汇率。旧版本保留且active=false，每期间只允许一个有效版本。允许可选 `capitalRates:{法人id:历史资本汇率}`。
- `eliminations`: `[{id,sourceId,unitId,month,kind,status,currency,amount,sellerId?,buyerId?,effects,openingEffects,monthlyEffects?,monthlyAmounts?,reason}]`。kind为sales/investment/manual；基准示例已审批，新增草稿不影响报表。
- `ownership`: 实际全资持股，管理合并单元不作为虚构持股母公司。
- `results`: 后端保存，不由本计算模块维护。

## 标准报告

`report={opening,closing,profit,cashflow,monthlyProfit,monthlyCashflow,capitalMovement,equityHistory}`。

资产字段 `cash,receivable,inventory,prepaid,investment,fixed,intangible,deferred_asset`；负债字段 `payable,shortloan,taxpayable,otherliability,longloan,deferred_liability`；权益 `capital,retained,nci,cta`。缺省金额0。

损益 `revenue,cost,expense,finance,tax`，净利派生。现金流 `operating,investing,financing,fx`；fx只表示现金汇率影响。源本位币报告fx为0，翻译时独立计算。

月度损益/现金流映射保存各月发生额，合计必须与当前累计数一致。9期基准，1至9期直线样例；10至12期无新发生额，累计沿用9期，明确标示演示政策。

`equityHistory={capitalByEntity:{id:本位币资本},openingRetainedCny:期初已确认人民币留存收益,dividendsByMonth:{}}`。法人报送的资本明细必须且只能是该法人且等于期初/期末资本。美元期初留存非零必须显式提供已确认人民币余额；集团留存按期初历史余额加逐月折算利润滚动。本轮无增减资本、分红、少数股东；这些变化输入时明确拒绝，不用补差伪装实现。

美元层损益/现金流抵销必须提供完整`monthlyEffects={"1":{revenue:-4,cost:-4},...,"9":{...}}`；逐项目合计须等于`effects`的累计效果。动作层可接`monthlyAmounts`生成等额收入/成本抵销；未传则明确表示本事项全部发生于当前月并生成相应发生额，禁止把历史累计抵销悄悄用期末月率折算。人民币层可默认当期，且不支持人民币上卷美元。

投资抵销以`investmentPairs`列明真实母子法人、子公司原币资本、母公司本层报表币种投资。逐项与实际报送及持股关系核对；历史资本折算与对应投资必须相等，母公司明细总额须等于实报期初/期末全部投资，期初/期末等额消除。支持本轮完整期初全资投资演示，不隐含商誉/部分投资账面分摊或差额补平。

本轮公开汇率接口仅统一历史资本率。`investmentPairs`是抵销明细，尚不是母公司独立报送的按被投资法人历史投资台账；目前来源控制为子公司实际资本逐项与母公司投资总额核对。正式系统必须补原始投资台账、收购历史及差额处理，不能把本演示声称为已具备该完整账簿。

## 函数

- `seed_multilevel()->dict`
- `validate_tree(data)->{leafIdsByUnit,parentByUnit}`：拒绝环、未知引用、重复成员及叶子重叠；输入币种只允许同币种或USD→CNY。
- `collect_leaves(data,unitId)->list[str]`
- `lowest_common_unit(data,sellerId,buyerId)->str`
- `validate_source(data,entityId,month,source)->None`：字段、金额、三表及月度发生额验证。
- `calculate_unit(data,unitId,month,child_results=None,preview=False)->result`
  - 严格模式只用 `child_results={unitId:已批准结果快照}`，快照含完整纯计算输出以及id/resultVersion/status/approvalStatus/valid:true。校验单元/币种/期间/叶子/当前依赖版本、财务平衡与内容fingerprint。缺下级、非批准、失效或预览禁止上卷；不得绕过到单体重算。
  - `preview=True` 可递归现算，仅用于界面预览，结果显式 `preview:true`，不能保存/审批为正式结果。
  - 返回 `unitId,unitName,month,currency,preview,report,statement,checks,passed,leafIds,eliminationIds,inputs,dependencies,trace`。
- `make_statement(result)->dict`，旧 `actual/profit/balance/cash/financial_checks` shape。新增actual.cta/cashFx；资产负债表折算差额；现金流单列汇率变动影响。
- `translate_result(data,result,targetCurrency,month)->result`：本轮只支持USD→CNY，其他对不冒充通用多币种。

`inputs`为本层输入，含来源类型/id/name/currency/sourceVersion/resultId/resultVersion。`trace`含 entities叶子源版本、eliminations本层/继承出处和月度效果。每条`trace.fx`含rateId/rateVersion、sourceReport/translatedReport完整三表、openingRate/closingRate/historicalCapitalRate、monthlyAverageRates/monthlyFlowRates以及rows逐项目原币/人民币值。`dependencies`含源版本、下级结果版本、树配置版本、实际使用汇率版本。新增/撤销本子树已审批抵销也使旧下级快照过期。

纯seed不保存结果；应用初始化由动作层使用相同run/approve逻辑按HW→OS→TY生成真实系统演示签核快照，不能把预览结果标成正式结果。

## 固定虚构验收数据

- 鸿旺管理范围三独立法人：`HW/HW_CAT/HW_ELEC`，毛收入1500，抵销100，结果1400；费用成本示例使净利450，资产/现金650。
- 其他境内法人 `OTHER` 收入600；集团跨范围抵销50，因此境内收入1950。
- `OS_A/OS_B` 各占海外组件一半：合计USD收入100/费用80/净利20，期初现金资本100，期末现金200/借款80/资本100/留存20。子集团 `OS_GROUP` 为USD。
- 汇率 opening/historical6.5、monthly average/flow7、closing7.2。海外CNY资产1440/负债576/资本650/留存140/CTA74，现金流650+140+560+90=1440。
- 本部 `TY` 期初及期末对子公司投资950、资本950；根级显式投资资产/子资本抵销950（300境内+650海外历史资本）。最终集团收入2650、净利770、资产/现金2370、负债576、资本950/留存770/CTA74；期初现金950，CFO770/CFF560/现金FX90。

以上为本轮全资、无税、无分红、无复杂股权变更、普通非高通胀假设，不代表所有合并会计准则已实现。
