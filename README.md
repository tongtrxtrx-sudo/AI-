# 自研EPM下载包

## 中文研发需求文档（Word 完整版）

[直接下载可编辑 Word：EPM_PRD_CN.docx](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_PRD_CN.docx)

本版参照用户提供的PRD样式整理，包含财务合并、全面预算、经营分析和平台规格，以及全部源码字段、接口、权限和静态仓库字段。所有正文及表格均可在Word/WPS直接编辑，不要求打开Excel。源功能、二开目标、待确认政策与运行验证分别标识。文件校验见docs/SHA256SUMS.txt。

免Docker、双击EXE为新增目标，当前尚未交付经过验证的新EXE。保留原平台的方案需要随包Linux环境；当前官方组件下载403阻断，完整便携包与Windows实机验证仍待完成。下方EPM.zip仍为此前依赖Docker的研发包。

[下载 EPM.zip](https://github.com/tongtrxtrx-sudo/AI-/raw/refs/heads/epm-download-20261019/EPM.zip)，可在下载后改名为“自研EPM压缩文档.zip”。大小56,932,414字节，SHA-256见SHA256SUMS.txt。

内容为固定版本Konsolidat/Konsol源码、Windows x64 EXE启动器、Docker部署材料、中文系统说明书/用户手册/安装手册、105页反向研发规格及字段字典。无需本机另装Python，需Docker Desktop和首次构建网络。

当前验证边界：214项既有源码测试通过；基础四服务、停止重启、基础导出通过；兼容修正版完整构建被云HTTPS代理503阻断。完整登录、dbt、Cube、应用级备份恢复和Windows实机未通过，不代表即用成品。二开需求与源功能已分别标识。

没有包含云机器的实际密码、运行数据、数据库备份或私密日志。原始source保留上游归属；Konsolidat含MIT，Konsol固定版本未附仓库级完整LICENSE，商业再分发需核实权利来源。

该下载交付使用独立epm-download-20261019分支，不替换现有master分支。
