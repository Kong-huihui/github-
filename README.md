# GitHub 项目上传与维护完全指南

这份指南基于你完成本次 `swift_qwen3_vl` 项目备份的真实操作过程整理而成。你可以将此文档作为日后任何项目上传 GitHub 的标准操作手册。

## 一、 首次上传项目（从零开始）

当你有一个本地项目文件夹，想要第一次上传到 GitHub 时，请按此步骤操作。

### 1. 初始化本地仓库
打开终端，进入你的项目根目录：
```bash
cd /你的/项目/路径
# 初始化 git 仓库
git init
```

### 2. 配置忽略文件（至关重要）
在项目根目录下创建一个名为 `.gitignore` 的文件。这一步是为了防止将几 GB 的模型权重、虚拟环境或临时文件传到 GitHub，导致上传失败。

**推荐的 `.gitignore` 模板（适用于 Python/大模型项目）：**
```gitignore
# Python 编译文件
__pycache__/
*.py[cod]
*$py.class

# 核心权重文件 (大文件务必忽略)
*.pth
*.pt
*.bin
*.safetensors
*.ckpt
*.h5
*.onnx

# 训练输出与日志目录
output/
checkpoints/
runs/
logs/
cache/

# IDE 配置
.vscode/
.idea/

# 虚拟环境
.env
venv/
env/
```

### 3. 提交文件到本地暂存区
```bash
# 添加当前目录下除了被忽略文件以外的所有文件
git add .

# 提交并附带说明信息
git commit -m "Initial commit: 项目初始化备份"
```

### 4. 连接 GitHub 并推送到远程
1.  在 GitHub 网页上创建一个**新的空白仓库**（不要勾选 Initialize with README）。
2.  复制仓库地址，在终端运行：

```bash
# 1. 关联远程仓库 (将 URL 替换为你自己的仓库地址)
git remote add origin https://github.com/你的用户名/仓库名.git

# 2. 将主分支重命名为 main (现在的标准做法)
git branch -M main

# 3. 首次推送 (并建立关联)
git push -u origin main
```

---

## 二、 日常修改与更新（增量推送）

当你修改了代码、更新了文档（如 `README.md`）或者添加了新脚本后，只需执行以下“三部曲”即可同步到 GitHub。

### 1. 查看修改状态 (可选，但推荐)
```bash
git status
```
*这会告诉你哪些文件被修改了（红色显示）。*

### 2. 添加修改 (Add)
```bash
# 方式一：只添加指定文件 (例如只提交了 readme)
git add README.md

# 方式二：添加所有修改过的文件 (最常用)
git add .
```

### 3. 提交保存 (Commit)
```bash
git commit -m "Update README format: add training logs"
```
*引号内的内容是本次修改的说明，建议写清楚改了什么。*

### 4. 推送到远程 (Push)
```bash
git push -u origin main
```
*注意：第一次用了 `-u origin main` 后，以后只需要输入 `git push` 即可。*

---

## 三、 常见问题与技巧

### Q1: 我的 `README.md` 被覆盖了，怎么查看旧版本？
Git 的核心功能就是版本控制，你的每一次 `commit` 都是一个快照，旧版本永远存在。

**在 GitHub 网页上查看：**
1.  进入项目主页。
2.  点击文件列表右上方的 **"Commits"** 图标（通常是一个时钟图标，旁边写着提交次数，如 `3 commits`）。
3.  或者点击具体的 `README.md` 文件，然后点击右上角的 **"History"** 按钮。
4.  你可以看到每一次修改的记录，点击任意一次记录，即可查看当时的文件内容，甚至可以对比查看删改了哪些行。

**在本地终端查看：**
```bash
# 查看提交日志
git log

# 查看某个文件的修改历史
git log -p README.md
```

### Q2: 提示 "File too large" 无法推送怎么办？
通常是因为在这里之前没有配置好 `.gitignore`，导致 Git 试图追踪大文件。
**解决方法：**
```bash
# 1. 确保 .gitignore 里已经写了 *.safetensors 等规则

# 2. 从 Git 缓存中移除所有文件（不会删除物理文件，只是取消追踪）
git rm -r --cached .

# 3. 重新添加（这时 Git 会重新读取 .gitignore 规则）
git add .

# 4. 再次提交
git commit -m "Fix: remove large files from tracking"

# 5. 推送
git push
```
