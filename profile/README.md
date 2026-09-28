# BORUIX

BORUIX 是一个用 Rust 从零实现的 x86_64 操作系统。它不是一个教学内核，而是一个**完整的系统**：
内核、系统调用层、C 标准库、用户态驱动、命令解释器与一套可运行的验收程序。

系统的设计取向是**把危险的代码赶出内核**。文件系统、设备驱动、音频混音与终端都运行在用户态，
内核只保留必要的收口：地址空间、进程与调度、系统调用入口、以及「把访问硬件的权限授予某个进程」
这件事本身。驱动崩溃只影响它自己的进程，内核与其他程序不受影响。

## 从哪里开始

- **[wiki](https://github.com/BRX-Boruix/wiki)** —— 安装、使用教程与项目历史。**第一次了解本项目从这里开始。**
- **[kernel](https://github.com/BRX-Boruix/kernel)** —— 内核本体：内存管理、调度器、虚拟文件系统、系统调用。
- **[tools](https://github.com/BRX-Boruix/tools)** —— 构建内核、打包可引导镜像、在 QEMU 中启动、真机验收。

想动手写程序的话，[编写与安装驱动程序](https://github.com/BRX-Boruix/wiki/blob/main/tutorial/write-and-install-drivers.md)
是最好的入口：第一张只讲安装一个编译好的驱动，后面几章带你从零写一个自己的。

## 仓库导航

项目由多个独立仓库组成，各自负责一件事。主要的分组如下，完整索引见 [wiki](https://github.com/BRX-Boruix/wiki)。

**核心**

- [`kernel`](https://github.com/BRX-Boruix/kernel) —— 操作系统内核
- [`tools`](https://github.com/BRX-Boruix/tools) —— 系统工具链：构建、打包、启动、验收
- [`wiki`](https://github.com/BRX-Boruix/wiki) —— 面向使用者的文档
- [`brxLimine`](https://github.com/BRX-Boruix/brxLimine) —— 引导加载器（Limine 的分支，增加只读 EXT2 支持）

**系统程序与库**

- [`init`](https://github.com/BRX-Boruix/init) —— 第一个用户态进程，拉起并监督各项服务
- [`shell`](https://github.com/BRX-Boruix/shell) —— 交互式命令解释器
- [`login`](https://github.com/BRX-Boruix/login) —— 登录认证
- [`libsys`](https://github.com/BRX-Boruix/libsys) —— 用户态系统调用封装
- [`libc`](https://github.com/BRX-Boruix/libc) —— C 标准库
- [`libline`](https://github.com/BRX-Boruix/libline) —— 行编辑、历史与补全
- [`csrc`](https://github.com/BRX-Boruix/csrc) —— 独立式 C 运行环境

**守护进程**

- [`audiod`](https://github.com/BRX-Boruix/audiod) —— 音频混音
- [`consoled`](https://github.com/BRX-Boruix/consoled) —— 键盘事件转终端字节
- [`driverd`](https://github.com/BRX-Boruix/driverd) —— 驱动自动装载
- [`userd`](https://github.com/BRX-Boruix/userd) —— 账户与家目录
- [`volumed`](https://github.com/BRX-Boruix/volumed) —— 卷的挂载与拔除

**驱动**

- [`intel-hda`](https://github.com/BRX-Boruix/intel-hda) —— Intel HD Audio 声卡驱动
- [`userdrv`](https://github.com/BRX-Boruix/userdrv) —— 用户态驱动模板与调用约定

**验收与测试程序**

系统里的每个子系统都配有独立的验收程序，在真机上跑通才算完成：

- [`selftest`](https://github.com/BRX-Boruix/selftest) —— 按需自检宿主
- [`acee2e`](https://github.com/BRX-Boruix/acee2e) —— 访问控制
- [`audioe2e`](https://github.com/BRX-Boruix/audioe2e) —— 音频往返
- [`consoled-e2e`](https://github.com/BRX-Boruix/consoled-e2e) —— 控制台环阻塞唤醒
- [`synce2e`](https://github.com/BRX-Boruix/synce2e) —— 跨进程同步
- [`pwde2e`](https://github.com/BRX-Boruix/pwde2e) —— 账户查询
- [`trave2e`](https://github.com/BRX-Boruix/trave2e) —— 目录遍历权限
- [`fpcheck`](https://github.com/BRX-Boruix/fpcheck) —— 浮点状态跨核迁移

其余验收程序、示例程序与旧世代归档见 [组织仓库列表](https://github.com/orgs/BRX-Boruix/repositories)。

## 技术要点

- 从零实现，不基于任何既有内核
- 用户态驱动模型：内核只授予硬件访问权，驱动主体运行在用户态
- 每个子系统都有独立的端到端验收程序，验收通过才计入完成
- 目标平台 x86_64，目前在 QEMU 中验证

## 状态

项目处于活跃开发中，**尚未在真实硬件上完整验证**。各仓库 README 中的「已知限制」一节如实记录了
当前的能力边界。

## 许可

各仓库独立采用 MIT License，详见各仓库的 `LICENSE` 文件。包含第三方组件的仓库另有
`NOTICE.md` 说明。
