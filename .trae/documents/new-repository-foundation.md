# 新仓库基础配置完善计划

## Context

`trae_data` 是公开的新建仓库，目前没有业务代码、明确技术栈或测试，仅有初始化 README 和空的 GitHub Actions 工作流。目标是在不虚构项目用途、不引入技术栈依赖的前提下，补齐通用且可立即使用的基础配置。

已确认：本次不添加许可证；远端采用个人精简配置，不启用 `main` 分支保护。

## 实施步骤

1. **补充忽略规则**：新增 `/workspace/.gitignore`，仅加入常见系统文件、日志及本地环境变量文件（保留 `.env.example`），不预先加入特定语言或框架规则。
2. **统一基础文本格式**：
   - 新增 `/workspace/.gitattributes`，启用文本自动识别并统一 LF 换行。
   - 新增 `/workspace/.editorconfig`，统一 UTF-8、LF、文件末尾换行和尾随空格处理；暂不规定缩进宽度，避免与未来的技术栈冲突。
3. **让空工作流变为有效 CI**：更新 `/workspace/.github/workflows/main.yml`：
   - 在 push、Pull Request 和手动触发时运行。
   - 使用最小 `contents: read` 权限和 `actions/checkout@v4`。
   - 检查 README 非空，并将 HEAD 与空树执行 `git diff --check`，发现空白错误及冲突标记。
   - 不添加虚假的依赖安装、构建或测试步骤。
4. **同步项目说明**：更新 `/workspace/README.md` 的当前状态和目录树，说明基础 CI 与文本配置；保留“项目初始化、尚未添加许可证”的真实描述。
5. **调整 GitHub 个人精简设置**：通过已认证的 `gh api` 将远端设置为仅允许 Squash 合并，并开启合并后自动删除分支。保持默认分支 `main`、现有仓库描述和未保护状态不变。

## 验证

- 本地执行 CI 中的 README 非空检查和全树 `git diff --check`，确保命令有效且当前文件通过。
- 检查 GitHub Actions YAML 的触发条件、权限和步骤，并使用可用的 YAML 解析工具验证语法。
- 通过 `gh api` 回读远端设置，确认仅 Squash 合并和自动删除分支已生效。
- 用 `git status` 和 `git diff` 审核最终改动；不提交、不推送，等待用户后续确认。

## 暂不处理

项目描述、技术栈专属忽略规则、依赖更新、测试/构建、分支保护及许可证，均在项目用途或许可证选择明确后再配置。
