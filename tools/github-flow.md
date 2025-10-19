# github flow


## 核心思想

main 分支是唯一的、永远可部署（稳定）的主干。

## 关键分支

* main：主分支，代表生产环境的代码。

* feature/ (或 fix/ 等)：功能分支，从 main 创建。

## 流程

1. 从 main 创建功能分支 (e.g., feature/user-login)。

2. 在功能分支上开发、提交。

3. 定期将 main 的最新代码同步（rebase 或 merge）到功能分支，尽早解决冲突。

4. 开发完成后，发起一个 Pull Request (PR) 请求合并到 main。

5. 代码审查 (Code Review) 通过。

6. 合并 (Merge) PR 到 main。

7. （可选）合并后立即自动部署 main 分支。

8. 删除功能分支。