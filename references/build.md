# 工程与构建：从已有环境到可追溯 HAP

## 1. 定位，不先安装

- 在用户指定目录工作；没有指定且需要新建时，选可用工作目录，先确认不覆盖现有工程。查看项目约定、`AppScope/app.json5`、根 `build-profile.json5`、模块配置、项目脚本与构建日志。包名和版本以实际配置为准，模块不一定叫 `entry`，产品与 target 不一定叫 `default`。
- 复用已安装的 DevEco Studio、同套官方 SDK 与命令行工具。确认 `hvigorw`、SDK、JDK、Node 与工具要求匹配，涉及原生库时确认 CMake/NDK 与 ABI。别套用别人的固定路径或安装全局包解决“命令不在 PATH”。用 IDE SDK 设置、项目 wrapper、已安装工具自带版本/帮助识别来源。
- 没有工具时指向[华为官方 DevEco Studio 下载](https://developer.huawei.com/consumer/cn/deveco-studio/)，按主机操作系统和架构选官方版本。新手可在 IDE 用原生应用模板建空工程，Agent 接手代码与可自动化验证；不要手工拼凑整套 SDK 配置。缺工具不妨碍做能独立运行的逻辑测试。
- 用目标手机实测系统/API 和所用原生接口的最低 API 决定兼容版本。编译 SDK、目标 SDK、最低兼容版本不是同一个值。不要仅为安装成功而把最低 API 调低；原生方法（例如关闭服务器）也要核对，不只看能否启动。

## 2. 代码与验证

先做一条最短可用流程：进页面 → 执行核心操作 → 看见结果。测试规则/状态、错误输入、重复操作和权限等实际风险。鸿蒙专有部分使用真实 SDK 编译与真机验收；在 Node 里剥离类型执行纯逻辑，不能替代 ArkTS 编译、权限和设备测试。改既有应用时跑原功能回归；不要为了教程统一目录或引入新依赖。

## 3. 构建

优先使用项目已有的构建入口。下面是**产品名和 target 都是 default、模块是 entry 时**的官方 CLI 示例；从工程配置替换真实参数：

```sh
hvigorw assembleHap --mode module -p product=default -p module=entry@default -p buildMode=debug --no-daemon
```

- 从工程根目录运行。用真实的 wrapper 或官方命令行路径；需要的依赖安装、代理、SDK 配置遵守项目和用户约定。
- 查看退出状态和构建日志，定位实际输出。单模块常见位置是 `entry/build/default/outputs/default/`；不要只按固定文件名选包。核验产物确实对应本次构建，并记录包名、版本、签名状态、文件大小与 SHA-256（macOS 可用 `shasum -a 256`）。
- 新打包前判断生成目录是否会刷新已有签名 HAP；不要默认执行 clean、删除缓存、覆盖保留的发布物。必要时先向用户说明影响并按其规则保留最小回退产物。构建失败先处理第一条根因；不要自动升级整套工具链。
- “成功构建但没签名”是可交付的本地阶段，**未签名包不能作为可直接安装的真机包**。要签名与安装走 [签名与装机](device.md)。

官方依据：[命令行编译构建](https://developer.huawei.com/consumer/cn/doc/HarmonyOS-Guides/ide-command-line-building-app/)；命令、SDK/JDK 兼容范围随版本变化，按实际工具与对应文档核对。
