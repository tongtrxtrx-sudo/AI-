> 此页记录2026年10月20日前交付的V1中文Demo与文档。多层合并及正式美元报表折算请使用[新版Demo](README.md)。以下内容按当时版本保留。

# 杭州天元宠物用品有限公司 · 全中文 EPM Demo

本轮交付依据完整产品原型重新实现中文工作台，提供本机数据库和可保存的业务操作。默认公司名称为 **杭州天元宠物用品有限公司**，演示法人、人员、交易和金额均为虚构样例。

**[下载 Windows EXE 运行包（约16.5 MB）](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/a68390de1e21e857f9db12a903f2718d37d79edc/TianyuanEPMDemo-Windows-x64.zip)**

Windows 10/11 64 位。完整解压 ZIP，双击 **天元EPM Demo.exe**，自动打开本机中文工作台。无需自行安装 Python、Docker、WSL 或虚拟机；首次启动无需下载其他运行环境，也不需要邮箱登录或英语初始化。

![实际中文工作台](demo/overview.png)

## 已实现的演示操作

- 完整原型的 20 项中文导航，覆盖集团财务合并、全面预算、经营分析、基础档案和审计。
- 试算余额 CSV 整批校验与导入、期间独立数据、平衡凭证草稿及审批、三表重算与勾稽、报送确认、内部交易说明、经营说明及关账签核。
- 四层销售目标、量价与全年分摊、业务预算明细、提交审批锁定、完整版本复制、独立预算调整版及经营差异分析。
- 业务操作在 SQLite 事务中保存；审计只读和审批职责由服务端检查。浏览器刷新与整个服务重启后保留已保存数据。

## 运行、退出和备份

1. 解压后保留程序同目录的全部文件夹，双击新包中的 **天元EPM Demo.exe**。
2. 点击左侧菜单体验业务。右上角“本机环境”可查看实际数据路径、打开文件夹和安全退出。
3. 完成后选择“安全退出”。再次双击同一个 EXE，会读取先前已保存的数据。

默认数据库保存在 `%LOCALAPPDATA%\TianyuanEPMDemo`，与程序解压位置分开。备份时先安全退出，再复制整个数据目录。关闭浏览器标签页时服务会继续运行。

本轮是按完整原型实现的独立演示应用，旧 Konsolidat 账套的数据迁入需另行适配。请从新 EXE 打开工作台，使用它自动选择的本机端口。

## 详细中文文档三件套（V1.0）

**[一次下载三份Word＋PDF＋案例CSV（约7.6 MB）](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/7191c5dac2c38396b4886b5ead9589bd8cd6c94d/EPM-Documentation-CN.zip)**。三份PDF共65页，Word可在Word/WPS编辑；均依据当前独立中文Demo及实际源码编写。

| 文档 | Word下载 | 内容 |
| --- | --- | --- |
| 系统研发与交互说明书（PDF 21页） | [Word](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/7191c5dac2c38396b4886b5ead9589bd8cd6c94d/demo-docs/EPM-System-Development-Guide-CN.docx) | 研发架构、源码、前后端交互、接口字段、SQLite、计算与构建交付。 |
| 产品说明书·含演示案例（PDF 20页） | [Word](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/7191c5dac2c38396b4886b5ead9589bd8cd6c94d/demo-docs/EPM-Product-Guide-With-Demos-CN.docx) | 20项菜单、产品联动、8个可照做的案例、输入与预期金额。 |
| 用户操作手册（PDF 24页） | [Word](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/7191c5dac2c38396b4886b5ead9589bd8cd6c94d/demo-docs/EPM-User-Manual-CN.docx) | 安装、字段、报送、凭证、预算审批、关账、备份和排错。 |

随包提供架构图、实际页面截图和三份已用HTTP验证的案例CSV。案例区分全新环境与承接关系，明确演示计算口径和后续研发能力；不会把设计目标当作已有功能。[阅读文档说明](demo-docs/README-文档使用说明.md)。 [文档包及三份Word实际下载核验](demo-docs/public-download-verification.json)。

## 中文 Word 手册和源码

- [下载可编辑中文 Word 使用手册](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/a68390de1e21e857f9db12a903f2718d37d79edc/demo/天元EPM_Demo中文版使用手册.docx)
- [在线阅读中文版手册](demo/中文版Demo使用手册.md)
- [查看独立验收报告](demo/独立验收报告.md)
- 运行 ZIP 内包含完整前后端、原生 C 入口、可替换构建工具参数、接口字段契约、CSV 模板和 Python 原始许可。

## 验证和边界

实际 HTTP 和 Chromium 已验证中文导航、预算审批与独立版本、真实文件导入、凭证审批影响报表、签核与重新开启、本机路径、安全退出及重启保留。Windows 原生 EXE → 包内 Windows Python → Windows HTTP/SQLite 已在 Wine 中实际运行，含中文空格路径、重复启动复用与重启数据保留。Wine 验证不能替代 Windows 真机验收；EXE 当前未签名。

初始演示账套以 2026 年 9 月为基准：收入 13,000 万元、净利润 1,720 万元、资产 9,580 万元、负债 4,730 万元、权益 4,850 万元，三表勾稽通过。会计年度固定 2026；未独立导入后续期间时，10—12 月沿用 9 月累计数。预算按全年 12 期编制。

美元为展示币换算；CSV 导入后的经营现金流含演示估算。业务预算明细独立保存，成本费用预算比较使用演示基准。真实持股链、合并准则、完整三表预算模型、ERP 对接及生产多用户身份与组织隔离，仍属于正式项目研发及财务验收范围。角色切换用于单机演示。

## 其他已有材料

- [完整产品交互原型](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/a68390de1e21e857f9db12a903f2718d37d79edc/EPM-Full-Prototype-CN.zip)
- [合并与分析交互原型（预算静态预留）](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/a68390de1e21e857f9db12a903f2718d37d79edc/EPM-Consolidation-Analysis-Prototype-CN.zip)
- [已有四份中文 Word / PRD 研发需求材料](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/a4b2b46120a20d0102abfba5f84e86f8aee9ecf9/EPM-Chinese-Word-Documents.zip)
- [此前 Konsolidat 运行平台交付记录](HISTORY-Konsolidat-Delivery.md)，含上游来源与旧包说明。

本交付仅更新 `epm-download-20261019` 分支；原仓库 `master` 保持原样。整包 SHA256 见根 `SHA256SUMS.txt`，包内另有逐文件校验清单。当前实际验证结果见 [demo/release-verification.json](demo/release-verification.json)。

公开 ZIP、Word 手册及实际工作台截图已通过 HTTPS 实际下载，全部与本地交付字节一致：[下载验证记录](demo/public-download-verification.json)。
