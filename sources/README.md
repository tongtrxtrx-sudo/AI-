# 固定版本源码材料

原应用 Konsolidat 与 Konsol 源码保留在旧 EPM.zip 的 source 目录，也将包含在免 Docker 新启动包。原作者归属及各自许可保留。

此处另提供构建使用的 Frappe、Gunicorn、PyPika 精确修订源码，以及 Windows QEMU 本体对应的固定修订源码。QEMU 本体 archive 经 Git 对象与 PAX commit 核对；其 15 个 gitlink 子模块的完整固定版本另列 qemu-submodule-pins.json，archive 未包含子模块内容。它也不包含所有第三方 DLL、固件的对应源，不能把本体源码包等同于完整第三方源码集合。

软件许可与组件来源以新版运行包 licenses/、SOURCE-VERSIONS.json 和开源与源码说明.md 为准。校验见 SHA256SUMS.txt。
