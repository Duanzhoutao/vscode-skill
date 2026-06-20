# VS Code 配置文件说明

本文记录 VS Code 系 IDE 的通用配置文件。适用于 VS Code、VS Code Insiders、VSCodium、Cursor、Trae CN 等沿用 VS Code 配置模型的产品；具体路径和支持项以实际产品为准。

官方参考：

- Settings: https://code.visualstudio.com/docs/configure/settings
- Command line: https://code.visualstudio.com/docs/configure/command-line
- Keybindings: https://code.visualstudio.com/docs/configure/keybindings
- Tasks: https://code.visualstudio.com/docs/debugtest/tasks
- Launch config: https://code.visualstudio.com/docs/debugtest/debugging-configuration
- Snippets: https://code.visualstudio.com/docs/editing/userdefinedsnippets
- Variables: https://code.visualstudio.com/docs/reference/variables-reference
- Profiles: https://code.visualstudio.com/docs/configure/profiles

## 配置层级

常见优先级从低到高：

- 默认设置：产品内置，不直接修改。
- 用户设置：影响当前用户所有窗口。
- Profile 设置：影响当前 profile。
- 工作区或项目设置：通常位于项目 `.vscode\settings.json`，只影响该项目。
- 语言专属设置：例如 `[python]`、`[markdown]`，只影响某类文件。

处理原则：

- 用户偏好放用户设置，例如字体、主题、窗口行为。
- 团队共享规则放项目设置，例如格式化、lint、测试、语言工具。
- 本机绝对路径、账号、token、私有 server、临时目录默认不要提交。
- 写入前优先保留现有 key，只改目标 key。

## 用户配置路径

Windows 常见路径：

- VS Code 用户设置：`%APPDATA%\Code\User\settings.json`
- VS Code Insiders 用户设置：`%APPDATA%\Code - Insiders\User\settings.json`
- Trae CN 用户设置：`%APPDATA%\Trae CN\User\settings.json`
- 用户快捷键：同级 `keybindings.json`
- 用户 snippets：同级 `snippets\`
- VS Code 扩展：`%USERPROFILE%\.vscode\extensions`
- VS Code Insiders 扩展：`%USERPROFILE%\.vscode-insiders\extensions`
- Trae CN 扩展：`%USERPROFILE%\.trae-cn\extensions`

Profile 相关设置通常在用户目录下的 `profiles\` 子目录中。不要盲改未知 profile；先确认用户当前使用哪个 profile。

## 项目 `.vscode` 文件

`.vscode\settings.json`

- 保存项目级设置。
- 适合提交：格式化、检查、语言服务器、测试发现、文件排除等协作规则。
- 不适合提交：个人字体主题、机器绝对路径、账号和密钥。

`.vscode\launch.json`

- 保存调试配置。
- 常见字段：`name`、`type`、`request`、`program`、`args`、`cwd`、`env`、`preLaunchTask`。
- `env` 不应直接放真实密钥；优先引用环境变量或本地未提交文件。

`.vscode\tasks.json`

- 保存构建、测试、脚本、外部命令任务。
- 常见字段：`label`、`type`、`command`、`args`、`group`、`problemMatcher`、`dependsOn`。
- `launch.json` 可通过 `preLaunchTask` 调用 task。

`.vscode\extensions.json`

- 保存推荐扩展。
- 常见字段：`recommendations`、`unwantedRecommendations`。
- 这只是推荐，不代表自动安装。

## 常用 Settings Key

- `editor.fontFamily`：编辑器字体。
- `editor.fontSize`：字号。
- `editor.lineHeight`：行高。
- `editor.wordWrap`：自动换行，常见值 `on`、`off`、`wordWrapColumn`、`bounded`。
- `editor.tabSize`：Tab 宽度。
- `editor.formatOnSave`：保存时格式化。
- `editor.defaultFormatter`：默认格式化扩展。
- `files.autoSave`：自动保存，常见值 `off`、`afterDelay`、`onFocusChange`、`onWindowChange`。
- `files.exclude`：资源管理器隐藏规则。
- `search.exclude`：搜索排除规则。
- `workbench.colorTheme`：颜色主题。
- `workbench.iconTheme`：文件图标主题。
- `workbench.productIconTheme`：产品图标主题。
- `terminal.integrated.defaultProfile.windows`：Windows 默认终端 profile。
- `python.defaultInterpreterPath`：Python 扩展默认解释器。

## 变量

`tasks.json` 和 `launch.json` 常用变量：

- `${workspaceFolder}`：当前 workspace 根目录。
- `${file}`：当前文件完整路径。
- `${fileBasename}`：当前文件名。
- `${relativeFile}`：当前文件相对 workspace 路径。
- `${env:NAME}`：环境变量。
- `${config:KEY}`：读取 VS Code 设置。
- `${command:COMMAND_ID}`：调用命令返回值。

变量能减少绝对路径，但不能代替安全边界。涉及密钥时仍应使用系统环境变量或本地私有文件。

## JSON 注意事项

VS Code 设置文件常用 JSON with Comments，可以有注释和尾随逗号。PowerShell `ConvertFrom-Json` 和 Python 标准库只能处理标准 JSON；脚本自动读写前要确认文件不含注释或需要先用支持 JSONC 的解析器。

自动化修改时：

- 保留原有 key 顺序不是硬性要求，但要避免删除无关 key。
- 不要把未知对象整体重写成空对象。
- 对用户设置先读、改、写；对项目设置只写明确要求的项。
- 写入前后验证目标 key 是否存在且值正确。

## 可提交与不可提交

通常可以提交：

- `.vscode\settings.json` 中的项目协作配置。
- `.vscode\tasks.json`
- `.vscode\launch.json` 中不含密钥和私人路径的通用调试配置。
- `.vscode\extensions.json`

通常不提交：

- 用户目录下的 `settings.json`、`keybindings.json`、`snippets\`。
- Profile 目录。
- 含 token、cookie、API key、私钥、真实账号、内网地址的配置。
- 只对单台机器有效的绝对路径，除非仓库明确就是本机工作区。

## 产品差异

VS Code-like 产品可能共享 `.vscode` 项目配置，但用户目录、扩展目录、命令名和远程 server 目录不同。

- VS Code CLI 常用 `code`。
- VS Code Insiders CLI 常用 `code-insiders`。
- Trae CN CLI 常用 `trae-cn.cmd`，其聊天、模型、Agent、MCP 和 SQLite 状态是 Trae adapter 专属能力。
- Cursor、VSCodium 等产品应先确认 CLI 名称和用户目录，再复用本说明的配置模型。
