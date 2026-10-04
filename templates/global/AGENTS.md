## 语言与项目上下文

默认使用中文。按 ISO 24495-1:2023 的简明语言原则组织输出，让读者得到所需信息，并能找到、理解和使用这些信息。简洁不能牺牲准确性、必要细节或证据。工作前阅读适用的 `CONTEXT.md`，使用其中定义的统一领域术语。

## 执行约束：禁止默认执行 SHA256 等文件摘要校验

本约束针对你执行任务时自行添加的验证步骤，不针对项目已有的业务逻辑。

从现在开始，不要将 SHA256 或其他文件摘要计算作为默认工作流程。不得仅为了“保险起见”“确认没有变化”“证明写入成功”或生成执行报告而计算哈希。

### 禁止主动添加的操作

- 在读取、编辑、复制、移动、生成文件前后，例行运行 `sha256sum`、`shasum -a 256`、`Get-FileHash` 或等效命令。
- 使用 Python、Node.js 或其他脚本间接计算摘要，绕过上述限制。
- 为确认修改范围、文件保存成功或任务完成，对文件反复计算、记录和比较哈希。
- 扫描整个工作区生成校验清单，或创建任务未要求的 `.sha256`、checksum、hash manifest 文件。
- 将 SHA256 换成 MD5、SHA1、SHA512 等算法，继续执行同类不必要校验。
- 仅为了支持这些校验而安装工具、添加依赖、创建脚本或增加工具调用。

### 改用与任务直接相关的验证方式

- 判断修改范围：优先使用 `git diff`、`git diff --stat`、`git status --short`。
- 检查修改是否正确：查看相关代码，按需运行针对性的测试、类型检查或 lint。
- 判断文件是否存在、内容是否符合要求：使用文件存在性检查、必要的局部读取或格式解析。
- 若任务确实要求逐字节一致，优先使用直接比较工具，例如 `cmp`，不额外生成摘要。
- 已有充分验证结果时，不重复验证；也不要用其他无意义检查替代哈希检查。

### 仅在以下情况下允许执行摘要校验

1. 我明确要求计算或核对哈希。
2. 任务本身是在实现、调试或测试哈希相关功能。
3. 任务涉及必须保留的安全、完整性或协议要求，例如用可信的预期摘要验证下载产物。
4. 项目已有测试、构建工具或依赖管理器在内部执行必要校验；不要为了本约束禁用或修改它们。

在你主动执行例外校验前，简短说明它所验证的具体问题及必要性。无法说明实际用途，就不要执行。

### 边界

- 不要因此删除或修改项目已有的 SHA256 业务代码。
- 不要修改锁文件、关闭签名验证、绕过完整性检查或平台安全机制。
- 不要声称已经关闭客户端、沙箱或服务端内部机制；本约束只控制你能够选择的工具调用与工作步骤。
- 完成任务后报告实际改动和必要验证结果，不附加文件哈希清单。

立即将本约束用于后续操作，无需为确认此约束而扫描仓库或运行任何命令。

# Sumulige coding workflow — shared entry

Only apply this entry to an authorized coding or project-maintenance task.
Locate the actual project and read its AGENTS.md and relevant project documents.
Project-specific facts, configured commands and approved scope live in that project.
If the project has no workflow, explain the available initialization preview; do not install it implicitly.

1. Preserve existing work and native client permissions. This document grants no execution authority.
2. Read-only questions create no tasks, specs, memory or configuration.
3. For changes, clarify acceptance, work within approved scope, and update affected project documents.
4. Use the project's task source; distinguish task completion, tests, independent review, CI and release.
5. Keep evidence tied to the candidate, command, working directory, result and artifact. Report unknowns.
6. Keep one shared project record across clients. A role prompt is not an independent reviewer.
7. Do not spawn agents or change global settings without explicit authorization.

This global entry is deliberately short. Load detailed workflow from the project when relevant.
Do not copy project secrets, personal memory or repository-specific authority into this global file.
