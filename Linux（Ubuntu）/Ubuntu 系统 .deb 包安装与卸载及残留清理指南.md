# Ubuntu 系统 .deb 包安装与卸载及残留清理指南
本文整理了 Ubuntu 系统中 **.deb 包的安装、软件卸载、依赖处理及残留数据清理** 的常用方法，按场景分类，步骤清晰且兼顾新手与进阶用户。

## 一、.deb 包安装方法
### 1. 基础方法：使用 dpkg 命令
`dpkg` 是 Debian/Ubuntu 系统的底层包管理工具，可直接安装 .deb 文件，但**不会自动处理依赖**。
```bash
# 替换为 .deb 文件的实际路径（绝对路径/相对路径均可）
sudo dpkg -i /path/to/your/package.deb
# 示例：安装下载目录中的 xxx.deb
sudo dpkg -i ~/Downloads/xxx.deb
```
- 参数说明：`-i` 表示「安装（install）」。

### 2. 处理 dpkg 安装后的依赖问题
若使用 `dpkg` 安装时提示「缺少依赖」，可通过 `apt` 自动修复依赖关系：
```bash
sudo apt install -f
```
- 参数说明：`-f` 表示「修复（fix-broken）」，会自动下载并安装缺失的依赖包。

### 3. 推荐方法：使用 apt 直接安装（自动处理依赖）
Ubuntu 16.04 及以上版本支持用 `apt` 直接安装 .deb 文件，**会自动检测并解决依赖问题**，是最推荐的方式。
```bash
# 替换为 .deb 文件的实际路径
sudo apt install /path/to/your/package.deb
# 示例：安装当前目录中的 yyy.deb
sudo apt install ./yyy.deb
```

### 4. 图形化安装（适合新手）
直接双击 .deb 文件，系统会调用自带的「软件安装工具」打开，点击「安装」即可。
- 注意：部分 Ubuntu 版本（18.04/20.04/22.04）可能出现双击闪退的情况，此时建议改用命令行安装。

## 二、已安装软件的卸载方法
卸载分为「知道软件名称」和「不知道软件名称」两种场景，以下方法适用于通过 .deb 包或 apt 安装的软件。

### 场景1：知道软件的具体名称（推荐）
#### 方法1：彻底卸载（含配置文件）+ 清理无用依赖
```bash
# 第一步：卸载软件并清除配置文件（--purge 可省略，效果相同）
sudo apt remove --purge 软件名称
# 第二步：自动清理因该软件安装的依赖包（可选，推荐执行）
sudo apt autoremove --purge
```

#### 方法2：简化版（与上述效果一致）
```bash
sudo apt purge 软件名称
sudo apt autoremove
```

### 场景2：不知道软件的具体名称
先通过关键词查找已安装的软件包，再卸载：
```bash
# 替换「软件相关名称」为关键词（如 chrome、docker），查找匹配的软件包
dpkg --get-selections | grep 软件相关名称
# 示例：查找所有包含 chrome 的软件包
dpkg --get-selections | grep chrome
```
- 查找结果中，第二列显示 `install` 的即为已安装的软件包。
- 执行卸载：根据查找结果，使用上述 `apt purge 软件名称` 命令卸载（若结果中有带 `core` 的包，优先卸载该包，其余按实际情况处理）。

## 三、清理系统残留的软件数据
部分软件卸载后，会残留一些标记为「rc」状态的配置文件，可通过以下命令一键清理：
```bash
# 列出所有 rc 状态的包，并强制删除（dpkg -P 等同于 --purge）
dpkg -l | grep ^rc | awk '{print $2}' | sudo xargs dpkg -P
```
- 命令说明：
  1. `dpkg -l`：列出所有已安装/残留的包；
  2. `grep ^rc`：筛选出状态为 `rc`（已卸载但残留配置文件）的包；
  3. `awk '{print $2}'`：提取包名称；
  4. `xargs dpkg -P`：强制删除这些包的残留配置文件。

## 补充说明
1. 路径建议：使用 .deb 包安装时，若文件在当前目录，可使用 `./package.deb` 代替绝对路径，更便捷。
2. 权限说明：所有安装/卸载/清理命令均需 `sudo` 提升权限（管理员权限）。
3. 区别：`apt` 是 `apt-get` 的升级版，命令更简洁，推荐优先使用 `apt` 而非 `apt-get`。