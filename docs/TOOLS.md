# 工具职责与获取方向

[返回首页](../README.md)

本仓库记录 Codex 如何组织制作、编译与验证。工具由使用者从上游自行获取；这里不附软件、SDK、工具源码或游戏资源。能启动工具只是环境检查通过，不等于 MOD 已构建成功。

| 工具 | Codex 自动实现时的职责 | 上游入口 |
| --- | --- | --- |
| **Blender 后台 Python** | 读取工程，检查网格、UV、骨骼、权重、材质及图像；在独立副本中提取贴图或执行明确需要的模型导出。纯改色通常不必重新导出模型。 | [Blender 官方下载](https://www.blender.org/download/) · [官方版本说明](https://www.blender.org/releases/) |
| **Source 2 Viewer / ValveResourceFormat** | CLI 用于已支持的资源提取、反编译及批处理；GUI 用于必要的可视检查。用解析结果定位资源引用、检查 VPK 内容和回读编译结果。 | [项目主页](https://s2v.app/) · [GitHub 项目](https://github.com/ValveResourceFormat/ValveResourceFormat) · [Releases](https://github.com/ValveResourceFormat/ValveResourceFormat/releases) |
| **社区 SDK 与 Source 2 资源编译器** | 把编辑源编译为兼容的纹理、材质、粒子或模型资源。Codex 分别记录输入、编译器、参数、日志与输出，再检查文件内容。 | [CSDK12 社区介绍](https://deadlockmodding.pages.dev/modding-tools/csdk-12) · [UI 纹理编译方法](https://deadlockmodding.pages.dev/modding-guides/compiling-ui-vtex) |
| **Deadlock Mod Builder（可选）** | 使用项目支持的准备、转换和构建能力，或参考其公开工作流。是否需要 Builder 取决于当前路线；已有纯彩图补丁不应因此重新绑骨。 | [中文分支](https://github.com/gugongmen/deadlock-mod-builder-zh) · [中文分支发布页](https://github.com/gugongmen/deadlock-mod-builder-zh/releases) · [上游项目](https://github.com/OverseerAlbedo/deadlock-mod-builder) |

Source 2 Viewer 的图形程序是 `Source2Viewer.exe`。Windows x64 命令行包通常名为 `cli-windows-x64.zip`，其中使用 `Source2Viewer-CLI.exe`。工具配置要求 CLI 时，不能用图形程序替代。平台、架构及当前发布文件以 [上游 Releases](https://github.com/ValveResourceFormat/ValveResourceFormat/releases) 为准。

## 自动化入口怎样选择

- 先检查本机工具版本和可执行文件，再读取其实际帮助或对应版本文档；不要根据别的版本猜 CLI 参数。
- Blender 的后台脚本在工作副本中执行，保存关键数据对比与输出记录。只有明确需要的模型路线才改几何或绑定；改色路线重点检查真实图像连接和图像内容。
- CLI、程序接口、现有构建脚本能完成的操作优先用于批处理。不能把图形界面具备的功能直接推断成 CLI 也有同名参数。
- 编译输入目录和编译输出目录分开。CSDK 案例中 `content` 放编辑源，`game` 放编译结果；复制文件或改扩展名不等于编译。
- 既有 VPK 是编译资源容器，不能直接当作完整 Blender 或 Builder 源工程。维护现成补丁时，以已确认的资源基线和本次变化清单合并。
- 自动化要留下版本、输入指纹、允许变化的资源路径、编译日志和验证结果；静态回读通过之后，游戏内效果仍须单独验证。

## 案例版本与来源边界

方法记录中出现的 **CSDK12** 和 **Source2Viewer 20.1** 是当时使用过的工具链，不是永久的版本要求。2026-11-04 核验时，Source 2 Viewer 的项目发布页列有 20.1；CSDK12 介绍及 UI 编译教程可读取，但本次没有核实可替代的后继 SDK 版本。

社区页面、下载入口与游戏资源格式会变化。链接用来找到上游说明，不代表所有新版本都与旧脚本兼容，也不代表本仓库已经重新测试当前游戏。换版本后，先做一个小输入的编译与回读验证，再扩大处理范围。

工具识别、提取或重编译资源的能力，也不等于资源可以重新发布。此仓库仅保留方法记录；实际使用的模型、图片与成品由制作者自行管理。
