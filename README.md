# gitcmd

**交互式 Git 命令助手** —— 用菜单选择代替手敲命令的纯 Bash 脚本。

不用再记参数、拼命令、查分支名。gitcmd 会从当前仓库里读出分支、远程、标签，让你用数字选择，每一步确认后才真正执行。

## 为什么需要它

日常使用 Git 时,常见的麻烦是:

- 命令参数记不住,`rebase`、`stash`、`filter-repo` 的写法要反复查
- 分支名、远程名、标签名要手敲,容易敲错
- 删除分支要分本地和远程,合并后还要记得清理
- 重置、清理、重写历史这类危险操作,一行命令就可能无法挽回

gitcmd 的做法:

- **选择而不是输入**:分支、远程、标签、提交都从仓库里实时读取,用数字选
- **逐步确认**:执行前展示完整命令,确认后才运行
- **危险操作有护栏**:工作区不干净时提醒,历史重写前明确警告,不允许删除当前分支
- **可预览**:`DRY_RUN=1` 只显示命令不执行,适合学习和排查

## 功能一览

| 类别 | 操作 |
|------|------|
| 查看 | status、log、diff(目标分支..HEAD) |
| 同步 | fetch、pull、push(可选远程、分支和常用参数) |
| 分支 | checkout / switch(本地、远程、新建)、merge、rebase |
| 清理 | 合并或变基后询问是否删除被合并分支;删除分支可选本地 / 远程 |
| 暂存 | stash 的 push、pop、apply、list、drop |
| 标签 | 创建、删除、推送 |
| 文件 | 普通删除(`git rm`)、彻底清除历史(`git-filter-repo`) |
| 重置 | reset 的 soft / mixed / hard、clean(危险操作) |

## 安装

依赖:`bash`、`git`,支持 macOS 和 Linux。彻底清除文件历史功能额外需要 [`git-filter-repo`](https://github.com/newren/git-filter-repo),不用该功能可不装。

```bash
git clone git@github.com:sharesdoc/gitcmd.git
cd gitcmd
chmod +x gitcmd
sudo mv gitcmd /usr/local/bin/gitcmd
```

不想放进系统路径的话,也可以直接运行 `./gitcmd`。

## 使用

进入任意 Git 仓库,运行:

```bash
gitcmd
```

出现主菜单:

```
Main Menu  (current: main)
  1) status
  2) fetch
  3) log
  4) diff (target..HEAD)
  5) checkout/switch
  6) merge
  7) rebase
  8) pull
  9) push
 10) stash
 11) tag
 12) delete branch (local/remote)
 13) delete file (normal/purge history)
 14) reset
 15) clean (danger)
 h) help
 q) quit
```

输入编号进入对应流程,按提示选择参数,最后确认执行。多数菜单里输入 `q` 可退出或返回。

### 示例:合并分支

1. 主菜单输入 `6`(merge)
2. 从列表里选择要合并进来的分支
3. 选择合并参数(如 `--no-ff`)
4. 确认命令后执行
5. 合并完成后,脚本询问是否删除已合并的分支

### 预览模式

```bash
DRY_RUN=1 gitcmd
```

所有 Git 命令只打印、不执行。

## 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `DRY_RUN` | `0` | 设为 `1` 只预览命令,不实际执行 |
| `COLOR` | `1` | 彩色输出,仅在终端中生效;设为 `0` 关闭 |

## 安全说明

- 所有会修改仓库的操作都需要你确认
- 合并、变基、重置等操作在工作区有未提交改动时会提醒
- 彻底清除文件历史会重写所有提交,**之后需要强制推送,并且协作者要重新克隆**。执行前请先备份仓库
- `reset --hard` 和 `clean` 会丢弃未提交内容,无法恢复

## 文档

- [QUICK_REFERENCE.md](QUICK_REFERENCE.md):快速参考
- [GITCMD_ACTIONS.md](GITCMD_ACTIONS.md):各操作的详细说明
- [IMPROVEMENTS_SUMMARY.md](IMPROVEMENTS_SUMMARY.md):功能改进记录
- [BUGFIX_SUMMARY.md](BUGFIX_SUMMARY.md):缺陷修复记录

## 参与贡献

欢迎提交 Issue 和 Pull Request。新增操作时:

1. 新建 `wf_*()` 函数,遵循「读取数据 → 展示选项 → 确认 → 执行」的流程
2. 参数选择使用 `choose_one()` 或 `choose_multi_flags()`
3. 危险操作加上工作区检查和二次确认
4. 在 `main_menu()` 和 `print_help()` 中加入入口
5. 用 `DRY_RUN=1` 在干净、有改动、多分支等不同状态的仓库中验证,并确认 macOS 和 Linux 都能运行
