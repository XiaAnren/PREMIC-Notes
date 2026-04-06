## 撤回

### 基本状态

```mermaid
graph LR
  origin[原始代码] -- 编辑 --> edition[工作区代码]
  edition -- git add --> add[暂存区代码]
  add -- git commit --> commit[本地仓库代码]
```

### 撤回命令

撤回`N`次：
```bash
git reset [mode] HEAD~N
```

其中的`mode`参数包括：
- `--soft`：撤回到暂存区代码状态，即撤销`git commit`。
- `--mixed`（默认）：撤回到工作区代码状态，即撤销`git commit`、`git add`。
- `--hard`：撤回到原始代码状态，即撤销`git commit`、`git add`、编辑操作。
