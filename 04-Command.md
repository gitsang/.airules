# 命令

## 斜杠命令

如果用户发送了 `/` 开头的指令：

- 意味着用户希望你按照指令行动
- 下方定义了指令所对应的行动规则，请只在指令完全匹配时行动
- 如果指令出现拼写错误，请尽可能的提示用户其可能希望输入的指令，但不要执行
- 如果指令无法匹配，请告诉用户指令不存在

指令：

`/aicommit`

帮我添加所有文件并生成 Git Commit Message。

`/aipush`

帮我添加所有文件并生成 Git Commit Message，然后 push 到仓库。

`/analyze`

尝试获取代码所在行的诊断信息，并提供修改建议。

`/fix`

尝试获取代码所在行的诊断信息，并帮助修改代码。

`/mr <target_branch>`

- 使用 `git -c pager.log=false log` 获取当前分支与远程目标分支（`origin/<target_branch>`）的 Commit 差异，并生成描述。
- 使用 `glab mr create --target-branch <target_branch> --title <title> --description <description>` 创建合并请求。
- 如果 MR 已经存在，则使用 `glab mr update <mr_id> --target-branch <target_branch> --title <title> --description <description>` 更新合并请求。

对于 MR 描述：

- 请按照提交内容类型对提交分组
- 请对 Commit 内容进行总结，尽量简短地描述提交内容，不要直接使用 Commit 原始内容
- 请按照以下格式来生成 MR Message

```
### <类型：可以是 Feature、Fix、Chore、...>

- <commit_id> - <Commit 内容总结>
```
