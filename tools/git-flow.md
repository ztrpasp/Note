# git-flow

## 核心思想

通过设立不同职责的长期分支，严格分离“开发”、“发布”和“维护”。

## 关键分支 (长期存在)：

* main (或 master)：神圣的分支。 只存放正式发布的、带版本号 (tag) 的代码。绝对不允许直接提交。

* develop：开发主干。 这是所有功能开发的“集结点”。所有 feature 分支都从这里创建，并合并回这里。

## 关键分支 (临时存在)：

* feature/：功能分支。从 develop 创建，合并回 develop。

* release/：发布分支。当 develop 分支上的功能足够多，准备发布新版本时，从 develop 创建。

* hotfix/：紧急修复分支。当生产环境（main 分支）发现紧急 bug 时，从 main 创建。

## 流程（以开发一个功能为例）：

1. 从 develop 创建功能分支 (git switch develop -> git switch -c feature/user-login)。

2. 开发完成，发起 PR，请求合并到 develop。

3. Code Review 通过后，合并到 develop。

## 流程（以发布一个新版本为例）：

1. 当 develop 稳定后，从 develop 创建发布分支 (git switch -c release/v1.1.0)。

2. 代码冻结： 在 release 分支上，不再添加新功能，只做 bug 修复、文档完善等发布前准备。

3. 测试通过，准备正式发布。

4. 关键一步： 将 release/v1.1.0 分支同时合并到 main 和 develop。

5. 在 main 分支上打上版本标签 (git tag v1.1.0)。

## 流程（以修复生产环境 Bug 为例）：

1. 从 main 创建 hotfix/v1.0.1 分支。

2. 修复 bug 并提交。

3. 关键一步： 将 hotfix 分支同时合并到 main 和 develop。

4. 在 main 上打上新标签 v1.0.1。