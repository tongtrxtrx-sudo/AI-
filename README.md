# 自研EPM · v1.3 中文免Docker便携研发版（20261020-zh2）

[下载完整软件：自研EPM压缩文档.zip](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/%E8%87%AA%E7%A0%94EPM%E5%8E%8B%E7%BC%A9%E6%96%87%E6%A1%A3.zip) · [备用英文文件名](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-NoDocker-Windows-x64.zip)

Windows 10/11 64位。完整解压后双击 SelfDevelopedEPM 文件夹里的“自研EPM.exe”，浏览器显示中文启动进度。无需另装 Docker、Python 或 WSL；建议16GB及以上物理内存，数据所在盘至少留5GB，并为业务增长预留空间。

首次缺少基础环境时，程序自动从固定GitHub提交下载约1.26GB内置环境并逐块校验；首次建站可能数十分钟或更久。用户名 Administrator，随机密码在启动页“查看本机登录信息”中查看。结束时点“安全退出（保留数据）”；仅关闭浏览器会保留后台运行。

## 已经下载旧免Docker版

优先下载较小的[中文化＋内存修复更新包](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Chinese-Memory-Update.zip)。先在旧启动页安全退出，完整解压更新ZIP，将 app、vm、docs 等按原有层级复制到原 SelfDevelopedEPM 文件夹并替换同名文件，然后双击原“自研EPM.exe”。不要多套一层目录，不要删除账套或密码。

基础盘版本未变；已校验完成的约1.26GB环境可复用。默认数据位置是 %LOCALAPPDATA%\SelfDevelopedEPM\data。中文更新在内置系统业务服务启动前检查准确的旧SHA、备份并写入；未知用户改动会拒绝覆盖。

针对 cannot set up guest memory 'pc.ram'，启动器按Windows可用提交额度选择4GB或6GB，并将TCG缓存固定128MiB；额度不足先显示中文说明。16GB物理内存不等于当前有足够可提交额度。请保存工作、关闭闲置程序，必要时重启；虚拟内存可检查是否由Windows管理。程序不会自动修改系统分页或安全设置。

## 中文显示与工作台修复

本版缺少标准工作区时由原项目工作区工厂补建；技术名及路由仍为Konsolidat和/app/konsolidat，界面显示“自研EPM”。工作区存在且必要导航完整、中文版本与服务健康均通过后才宣告就绪。已有账套、业务数据和用户密码保留。

业务菜单、字段说明、状态、关账工作台、明确业务校验、报表系统表头和自有Excel加载项显示层使用简体中文。Administrator、K.EPM公式、API/数据库字段、技术错误码、币种和科目代码、企业名称与用户自由文本保留原值；第三方数据库/SQL等技术诊断可能保留原文。

完整平台以固定Konsolidat/Konsol源码为基础；新增产品目标与已有实现的区别见研发文档。实际Excel仍需支持的Office、Office.js访问及与部署网址匹配的加载项清单和侧载配置，原演示清单网址不是本机连接地址；Excel不是运行EXE网页平台的前提。

## 两套离线中文交互原型

- [完整产品原型：集团合并＋全面预算＋经营分析](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Full-Prototype-CN.zip)
- [合并与经营分析原型：预算仅保留静态界面](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Consolidation-Analysis-Prototype-CN.zip)

每个ZIP解压后双击其中对应的HTML。全部样式、脚本及图形内嵌，不需要Python、Docker或CDN。永久显示“交互原型·演示数据”，使用合成数据，不连接运行平台账套，刷新重置。完整版含预算编制、分摊、送审、审批、锁定及版本操作；精简版预算四页仅静态预留，操作禁用。

两版共40导航视图、钻取、三表勾稽、期间/范围/币种联动和主要状态流程已经浏览器复验，发现的7类问题均已修复。云端浏览器禁止file协议，因此实际测试读取同一HTML原文后加载DOM，没有声称Windows真机或file协议已经实测。各ZIP含中文说明、源码、重建方法与测试报告；[独立验收报告](prototypes/独立交互验收报告.md)。

## 四份v1.3可编辑中文Word

[一次下载四份Word](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/EPM-Chinese-Word-Documents.zip)。完整软件及更新包的 docs 也已包含这四份原生 .docx，可在Word/WPS编辑：

- [PRD/RRD研发需求规格v1.3：含字段、流程、页面、接口、数据表、权限与验收](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_PRD_CN.docx)
- [系统说明书](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_System_Manual_CN.docx)
- [用户手册](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_User_Manual_CN.docx)
- [安装与使用手册](https://raw.githubusercontent.com/tongtrxtrx-sudo/AI-/epm-download-20261019/docs/EPM_Install_Manual_CN.docx)

按用户所给SCM PRD的结构与表格风格整理，区分原实现、二开目标、原型演示和当前验证范围。原上传附件未公开。文档SHA见 docs/SHA256SUMS.txt。

## 实际验证与适用范围

同一固定基础盘、新随机种子和空差异盘，4GB客户机内存、128MiB模拟缓存、2个模拟CPU、客体禁止出网：首次24项严格就绪25分48.31秒；dbt330节点全部成功（78模型、249测试、3 hooks）。17项实际HTTP/工作台检查、7个分析模型与代表性查询、后台队列任务及清理均通过；正常ACPI关机4.919秒、QEMU退出0、冷态磁盘检查0。161个更新目标均与冻结清单SHA逐件匹配。

Windows原生EXE→包内Windows解释器→Windows QEMU在Wine中，对已有隔离站点完成中文升级及幂等重启，两轮严格24项就绪分别541.888秒和537.629秒；工作台及中文设置正常，抽查四张财务表记录数、初始化状态、随机凭据和基础镜像保持。两次安全关机、控制器退出0和磁盘检查通过。

Linux Chromium实际登录、中文工作台、四类未保存业务表单和11个关账页面/API全部通过，并逐屏人工复核可见文字；所服务资源摘要与冻结构建一致。1472项模型/API回归、1646项前端测试、30项启动器检查、23项更新安全测试、33项中文原值保护及52项报表导出测试通过。测试范围及原始英文基线明确分开。

这是本地研发试用交付。Wine兼容层不是Windows真机；空账套数仓通过不是集团真实财务数字验收。实际Windows硬件、真实Excel侧载、ERP/ODBC、完整越权矩阵、财务金样、生产容量与合规仍需专项验收。详细证据和边界见 CHINESE-UPDATE-VERIFICATION.json、release-verification.json 与小更新包的 build/localization/evidence。

## 源码、版本与校验

原始项目：[Konsolidat](https://github.com/grynn-in/konsolidat)、[Konsol](https://github.com/grynn-in/konsol)。完整软件保留固定两项目原始源码1515个文件及原生C入口源码，归属与组件条款保留。完整中文前端、服务端、词典、Excel显示层、安全升级器、准确载荷和构建说明在小更新ZIP的 build/localization 中；为满足GitHub单文件100MiB限制，这些开发材料与完整运行ZIP分开发布。SOURCE-VERSIONS.json及源码获取说明记录准确SHA与来源。

整包SHA见根 SHA256SUMS.txt。完整软件使用 PACKAGE-SHA256.json，小更新包使用 PATCH-SHA256.json，两种清单对应各自的ZIP。内置基础环境14分块仍固定在提交 0e3cb26ef322a196213acf2180b270765490976a，基础盘SHA未改。

旧 EPM.zip 是依赖Docker的历史包。此前英文免Docker原版保留在固定提交 b6ea7297fbd713835b7b5caba53539239379e4c2，需要复现原始initrd时从该版本取文件。当前交付都在独立 epm-download-20261019 分支，master 保持原样。
