# harmonyos-local-app

面向第一次开发原生鸿蒙手机 App 的 Agent Skill。覆盖工程检查、构建 HAP、调试签名、USB 安装和真机验收，另附手机热点离线方案。它不会代替用户登录、信任设备或自动提交上架。

## 使用

- **Codex**：把整个仓库克隆到尚不存在的技能目录：

  ```sh
  git clone https://github.com/hummingg/harmonyos-local-app.git "${CODEX_HOME:-$HOME/.codex}/skills/harmonyos-local-app"
  ```

  如果已经安装同名 Skill，请先检查现有版本，不要直接覆盖。新会话中可用 `$harmonyos-local-app`。
- **其他支持 Skills 的 Agent**：下载整个目录放入其技能目录，或让 Agent 读取 [SKILL.md](SKILL.md)。保留 `references/`，不要只复制入口文件。

示例提示：用这个 Skill 带我做第一个原生鸿蒙 App。先检查我的电脑并完成能代办的步骤；需要我操作时，一次告诉我该做什么。

## 许可

[MIT](LICENSE)。本仓库不包含应用工程、签名材料、设备数据或编译产物。
