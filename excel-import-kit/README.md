# 用友NC6.5 Excel资料模板与截图导入说明

适用ERP-20261021。本次补充模板及操作指南，现有EXE无需升级；原四册全模块文档继续适用。

**[下载模板、已填示例和图文说明整包（约9.5MB）](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/TianyuanEPM-NC65-Excel-Import-Kit-D1.0.zip)** · [下载Word操作说明](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/EPM-NC65-Excel-Import-Guide-CN.docx) · [查看PDF](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/EPM-NC65-Excel-Import-Guide-CN.pdf)

说明书24页、16张真实实机截图，逐项解释：填什么、在哪个按钮导入、成功后保存什么、怎样生成单体/自动抵销/小合并及集团上卷、出错如何恢复。

|模板|下载Excel|系统位置与使用方式|
|---|---|---|
|01 科目映射|[下载](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/templates/01_%E7%A7%91%E7%9B%AE%E6%98%A0%E5%B0%84_%E7%A9%BA%E7%99%BD%E6%A8%A1%E6%9D%BF.xlsx)|ERP凭证与期初→①科目字段映射→选择文件并导入|
|02 年初余额|[下载](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/templates/02_%E5%B9%B4%E5%88%9D%E4%BD%99%E9%A2%9D_%E7%A9%BA%E7%99%BD%E6%A8%A1%E6%9D%BF.xlsx)|ERP凭证与期初→②年初科目余额→选择文件并导入|
|03 NC原48列凭证|[下载](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/templates/03_NC65%E5%8E%9F48%E5%88%97%E5%87%AD%E8%AF%81_%E7%A9%BA%E7%99%BD%E6%A8%A1%E6%9D%BF.xlsx)|下方NC6.5导出适配→导入NC6.5凭证|
|04 NC适配准备|[下载](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/templates/04_NC%E9%80%82%E9%85%8D_%E9%A1%B5%E9%9D%A2%E9%85%8D%E7%BD%AE%E5%87%86%E5%A4%87%E8%A1%A8.xlsx)|没有文件上传入口；对照维护NC6.5适配页面填写|
|05 现金流分类准备|[下载](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/templates/05_%E7%8E%B0%E9%87%91%E6%B5%81%E5%88%86%E7%B1%BB_%E5%87%86%E5%A4%87%E8%A1%A8.xlsx)|没有独立上传入口；对照现金原行补充核对信息|
|06 通用凭证替代|[下载](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/excel-import-kit/templates/06_%E9%80%9A%E7%94%A8%E5%87%AD%E8%AF%81_%E6%9B%BF%E4%BB%A3%E6%96%B9%E6%A1%88%E6%A8%A1%E6%9D%BF.xlsx)|上方③完整已记账凭证→选择文件并导入；与03二选一|

可导入模板保留第1行表头，从首sheet“导入数据”的A2开始粘贴值。说明、字典和参考示例在后sheet，默认不会导入；空白首表直接导入应报无数据。保留科目/账簿/凭证号文本前导0，不上传公式。

本包另有4份已填Excel，NC03与通用06为相同业务的两种格式，不能同时导入。01/02/03为84条映射、14行期初、48条凭证；假设1—8月无发生、9月集中发生。鸿旺3项准确匹配，收入1400/净利450/资产650；集团1项跨范围销售匹配，收入2650/净利770/资产2370/CTA74/现金汇率影响90，单位人民币万元。真实数据仍需年初至测试月完整已记账凭证，不拿演示值当企业实际。

本币借贷以法人本位币元入账；原币/集团币/全局币仅原值留存核对，不重复记账。单价、数量、汇率不统一截断精度。输入/单位/期初人民币留存特例详见字段字典和说明。

先备份；已签层级重开。七法人整套先分别重开鸿旺和海外及祖先，并回集团范围生成全部。普通文件模式明确停用本层预置销售抵销，保留投资和用户手工事项。导入成功后仍要生成单体→匹配→草稿→负责人审批→计算签核。金额差异不自动补平，企业NC在线接口尚未配置。

[6组真实XLSX验收](Excel模板真实导入验收.md)及[独立语义核对](导入模板说明独立核对.md)记录实际交付10个Excel的SHA，4可导入/2准备表的界限、元分/前导零/负例和NC/通用分别完整闭环。截图由最终已填Excel真实上传并逐层生成。公开下载校验在public-download-verification.json中记录。
