# Git 使用教程

## 1. 初始化 Git 仓库

在项目目录中，使用以下命令初始化一个新的 Git 仓库：

```bash
git init
```

这将在当前目录中创建一个新的 `.git` 文件夹，表示该目录现在是一个 Git 仓库。

## 2. 添加文件到暂存区

在您尝试提交之前，您需要将文件添加到暂存区。使用以下命令将当前目录下的所有文件添加到暂存区：

```bash
git add .
```

您也可以添加特定文件，例如：

```bash
git add filename.txt
```

## 3. 提交更改

添加文件后，您可以提交更改并添加提交信息：

```bash
git commit -m "初始提交"
```

确保提交信息简洁明了，描述您所做的更改。

## 4. 连接到远程仓库

如果您有一个远程 Git 仓库（例如 GitHub），可以使用以下命令连接：

```bash
git remote add origin <远程仓库URL>
```

例如：

```bash
git remote add origin https://github.com/YourUsername/YourRepo.git
```

## 5. 推送更改到远程仓库

在成功提交更改后，您可以将更改推送到远程仓库：

```bash
git push -u origin master
```

如果您使用的是 Git 的较新版本，默认分支可能是 `main` 而不是 `master`，请根据实际情况调整命令：

```bash
git push -u origin main
```

## 6. 查看状态

使用以下命令查看当前仓库的状态，包括已修改的文件和未跟踪的文件：

```bash
git status
```

## 7. 查看提交历史

使用以下命令查看提交历史：

```bash
git log
```

## 8. 创建分支

如果您想在新功能上工作，可以创建一个新分支：

```bash
git checkout -b new-feature
```

## 9. 合并分支

完成新功能后，可以将其合并回主分支：

```bash
git checkout master
git merge new-feature
```

## 10. 删除分支

如果您不再需要某个分支，可以使用以下命令删除它：

```bash
git branch -d new-feature
```

## 11. 处理错误

如果在推送时遇到错误，例如 `src refspec master does not match any`，这通常意味着您没有任何提交在 `master` 分支上。确保您已经成功提交更改。

如果您看到多个 `origin`，可以使用以下命令删除错误的远程仓库：

```bash
git remote remove origin
```

然后，重新添加正确的远程仓库。

## 其他建议

- **查看远程仓库**：
  使用以下命令查看当前的远程仓库：

  ```bash
  git remote -v
  ```

- **拉取远程更改**：
  如果您需要从远程仓库拉取更改，可以使用：

  ```bash
  git pull origin master
  ```

- **解决冲突**：
  如果在合并时遇到冲突，Git 会提示您解决冲突。您需要手动编辑冲突的文件，然后再次提交。

---
