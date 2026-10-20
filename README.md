# 自研EPM · 免Docker便携研发版

[下载：自研EPM压缩文档.zip](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/%E8%87%AA%E7%A0%94EPM%E5%8E%8B%E7%BC%A9%E6%96%87%E6%A1%A3.zip) · [备用英文文件名](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-NoDocker-Windows-x64.zip)

Windows 10/11 64位。将整个ZIP解压到独立文件夹，双击里面的`自研EPM.exe`，浏览器自动显示启动进度。电脑无需另装Docker、Python或WSL；建议16GB及以上内存，用户数据所在盘至少留5GB空闲并预留后续业务增长空间。

首次启动会自动从固定GitHub提交下载约1.26GB内置环境，逐块和总SHA256校验，支持保留部分下载供重试。首次建站可能数十分钟或更久。用户名`Administrator`，随机密码在启动页点击“查看本机登录信息”；每台电脑独立生成。

数据默认位于`%LOCALAPPDATA%\SelfDevelopedEPM\data`。结束时点启动页“安全退出（保留数据）”，等待Stopped；关闭浏览器会保留后台运行。旧Docker包应与新版分开放置。

## 两套离线中文交互原型

- [完整产品原型：合并＋预算＋经营分析](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Full-Prototype-CN.zip)
- [合并与经营分析原型：预算只保留静态界面](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Consolidation-Analysis-Prototype-CN.zip)

各ZIP解压后双击其中的HTML即可查看，不需要安装Python或Docker。永久显示“交互原型·演示数据”，使用合成数据，不连接运行软件账套；刷新重置。源文件、中文说明、重建方法及测试报告均随包。

两版共40个导航视图及预算/关账/钻取等已实际浏览器复验，7类发现问题已修复。云端浏览器禁止file协议，测试采用读取同一HTML原文后加载DOM；没有声称Windows真机或file协议已经实测。[独立验收报告](prototypes/独立交互验收报告.md)。

## 中文Word文档

[一次下载四份中文Word](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Chinese-Word-Documents.zip)。以下为v1.3可编辑`.docx`，字段表可在Word/WPS编辑；软件原始ZIP中的旧文档可用这组文件替换：

- [研发需求文档：247页（v1.3），含流程、页面、字段、接口、数据表、权限与验收](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_PRD_CN.docx)
- [系统说明书](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_System_Manual_CN.docx)
- [用户手册](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_User_Manual_CN.docx)
- [免Docker版安装与使用手册](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_Install_Manual_CN.docx)

字段级研发说明参照用户提供的PRD样式编排；源代码已有行为、二开目标、待确认政策和实测范围分别标识。原上传附件未公开。文档校验值见`docs/SHA256SUMS.txt`。

## 实测范围

最终相同基础盘、新种子、空站点、客体禁止出网：首次严格就绪30分50秒，23项探测通过；dbt330节点全部成功（78模型、249测试、3 hooks）。真实登录、字段元数据、私有测试File、Cube查询、队列和调度通过。

正常关机后复制业务盘与匹配种子，以同一不可变基础盘重开：4分29秒严格就绪，File记录及内容SHA一致，测试文件随后清理，最终正常ACPI退出0。原生备份导出通过，SQL/tar独立恢复尚未验证。

Windows原生EXE→包内Windows Python→Windows QEMU另在Wine中完成两轮真实运行、22项HTTP检查、重复启动复用和正常退出，使用旧初始化隔离快照。Wine兼容层及Linux Chromium测试均不等同Windows真机验收；最终公开基础盘未在Wine从零首装。空账套dbt通过不等同集团财务数字金样。

这是基于Konsolidat/Konsol的本地研发试用交付。Windows真机、真实ERP/Excel/ODBC、全权限越权矩阵、财务数字全场景、生产容量和合规仍需验收。完整边界见[运行验证说明](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/Runtime_Verification_CN.md)及`release-verification.json`。

## 源码和校验

原始项目：[Konsolidat](https://github.com/grynn-in/konsolidat)、[Konsol](https://github.com/grynn-in/konsol)。包内`source`保留两项目固定版本源码，`build`含便携部署和启动器源码；其他固定源归档入口和哈希在`SOURCE-VERSIONS.json`。上游归属和各组件条款继续适用，源码覆盖范围按该文件及开源说明如实记录。

下载ZIP的校验值见根目录`SHA256SUMS.txt`；包内`PACKAGE-SHA256.json`记录逐文件校验。内置环境的14个下载分块固定在提交`0e3cb26ef322a196213acf2180b270765490976a`，不依赖浮动分支内容。

旧根文件`EPM.zip`是早期Docker版留档，继续依赖Docker。当前免Docker版使用本页顶部两个等价下载链接。全部交付位于独立`epm-download-20261019`分支，`master`保持原样。
