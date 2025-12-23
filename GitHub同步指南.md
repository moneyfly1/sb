# GitHub 同步指南

## 方法一：使用 Personal Access Token（推荐）

### 步骤1：创建 GitHub Personal Access Token

1. 登录 GitHub：https://github.com
2. 点击右上角头像 → **Settings（设置）**
3. 左侧菜单选择 **Developer settings（开发者设置）**
4. 选择 **Personal access tokens** → **Tokens (classic)**
5. 点击 **Generate new token** → **Generate new token (classic)**
6. 填写信息：
   - **Note（备注）**：`sb项目推送`
   - **Expiration（过期时间）**：选择 `90 days` 或 `No expiration`
   - **Select scopes（权限）**：勾选 `repo`（完整仓库权限）
7. 点击 **Generate token（生成令牌）**
8. **重要**：复制生成的token（只显示一次，格式类似：`ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`）

### 步骤2：使用Token推送代码

在命令行中执行：

```bash
cd C:/Users/Administrator/Desktop/singbox

# 推送时使用token作为密码
git push -u origin main
```

当提示输入用户名时：
- **Username（用户名）**：输入 `moneyfly1`
- **Password（密码）**：输入刚才复制的token（不是GitHub密码！）

### 步骤3：配置Git凭据（可选，避免每次输入）

**Windows系统：**

```bash
# 配置Git凭据管理器
git config --global credential.helper manager-core

# 或者使用store（不推荐，安全性较低）
git config --global credential.helper store
```

## 方法二：使用SSH密钥（更安全）

### 步骤1：生成SSH密钥

```bash
# 生成SSH密钥对
ssh-keygen -t ed25519 -C "your_email@example.com"

# 按提示操作（可以直接回车使用默认路径）
# 如果需要设置密码，可以设置一个密码短语
```

### 步骤2：添加SSH密钥到GitHub

1. 复制公钥内容：
   ```bash
   # Windows PowerShell
   cat ~/.ssh/id_ed25519.pub
   
   # 或使用记事本打开
   notepad ~/.ssh/id_ed25519.pub
   ```

2. 登录GitHub → Settings → **SSH and GPG keys**
3. 点击 **New SSH key**
4. **Title**：填写 `我的VPS` 或任意名称
5. **Key**：粘贴刚才复制的公钥内容
6. 点击 **Add SSH key**

### 步骤3：修改远程仓库地址为SSH

```bash
cd C:/Users/Administrator/Desktop/singbox

# 删除HTTPS远程地址
git remote remove origin

# 添加SSH远程地址
git remote add origin git@github.com:moneyfly1/sb.git

# 推送代码
git push -u origin main
```

## 方法三：手动上传（最简单）

如果命令行推送遇到问题，可以手动上传：

### 步骤1：在GitHub创建仓库

1. 访问：https://github.com/new
2. **Repository name**：`sb`
3. **Description**：`Sing-box多IP支持增强版`
4. 选择 **Public** 或 **Private**
5. **不要**勾选 "Initialize this repository with a README"
6. 点击 **Create repository**

### 步骤2：上传文件

1. 在仓库页面点击 **uploading an existing file**
2. 将以下文件拖拽上传：
   - `sb.sh`（主脚本）
   - `README.md`（说明文档）
   - `VPS多IP配置说明.md`
   - `快速使用指南.md`
   - 其他需要的文件
3. 点击 **Commit changes**

## 方法四：使用GitHub Desktop（图形界面）

1. 下载安装：https://desktop.github.com/
2. 登录GitHub账户
3. File → Clone repository → 选择 `moneyfly1/sb`
4. 将文件复制到克隆的文件夹
5. 在GitHub Desktop中提交并推送

## 验证推送是否成功

推送成功后，访问以下地址查看：

```
https://github.com/moneyfly1/sb
```

应该能看到所有文件，包括：
- ✅ `sb.sh` - 主脚本
- ✅ `README.md` - 使用说明
- ✅ `VPS多IP配置说明.md` - 配置文档

## 常见问题

### Q1: 提示 "Authentication failed"

**解决方案**：
- 确认使用的是Personal Access Token，不是GitHub密码
- Token需要有 `repo` 权限
- Token可能已过期，重新生成一个

### Q2: 提示 "Connection reset" 或网络错误

**解决方案**：
- 检查网络连接
- 尝试使用SSH方式（方法二）
- 使用VPN或代理
- 使用手动上传方式（方法三）

### Q3: 提示 "remote origin already exists"

**解决方案**：
```bash
# 删除现有远程仓库
git remote remove origin

# 重新添加
git remote add origin https://github.com/moneyfly1/sb.git
```

### Q4: 如何更新代码

推送成功后，后续更新代码：

```bash
cd C:/Users/Administrator/Desktop/singbox

# 修改文件后
git add .
git commit -m "更新说明"
git push origin main
```

## 推荐方案

**对于Windows用户，推荐使用方法一（Personal Access Token）**，最简单直接。

如果遇到任何问题，可以使用**方法三（手动上传）**，虽然不够优雅，但最可靠。

---

**需要帮助？** 如果以上方法都无法解决，请提供具体的错误信息。

