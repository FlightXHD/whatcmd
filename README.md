# whatcmd — 用大白话查 Linux 命令的命令行工具

> 把口语/英文关键词变成该敲的 Linux 命令。基于本地命令库做模糊匹配，**零第三方依赖**，仅用 Python 标准库。

---

## 1. 这是什么

`whatcmd` 让你用中文口语或英文关键词提问，返回最相关的 Linux 命令、说明与示例。
例如敲 `whatcmd "怎么杀进程"`，它会列出 `kill -9 <PID>`、`ps aux` 等命令，并按相关度排序。

- **命令库**：内置 `commands.json`，收录 **102 条命令 / 10 个分类**
- **匹配方式**：中文走字符级模糊匹配，英文走词边界包含度 + 序列匹配
- **运行要求**：Python 3.8+，无任何 pip 依赖

---

## 2. 安装

```bash
# 方式一：dpkg 直接安装
sudo dpkg -i whatcmd_1.0.0_all.deb

# 若提示缺少 python3 依赖，自动补装：
sudo apt-get install -f

#方式二：单击/双击.deb安装包使用应用商店安装
```

> 注：本包为 `Architecture: all` 的纯 Python 包，可在任意 amd64 / arm64 等架构的
> Debian / Ubuntu / Linux Mint 及其衍生发行版上安装。

---

## 3. 快速上手

```bash
whatcmd "怎么杀进程"     # 中文口语查询
whatcmd process          # 英文关键词查询
whatcmd --random         # 随机学一条命令
whatcmd --list           # 按分类列出全部命令
whatcmd --category file  # 只看某个分类
whatcmd --version        # 查看版本
whatcmd --help           # 完整帮助
```

### 输出示例

```text
💡 为你找到 6 条相关命令（按相关度排序）

├─ kill -9 <PID>  [process]
│  ├─ 说明：用信号 9 强制结束指定进程
│  └─ 示例：kill -9 1234
├─ ps aux  [process]
│  ├─ 说明：列出当前运行的所有进程
│  └─ 示例：ps aux | grep nginx
...
```

---

## 4. 包内文件布局

| 安装路径 | 用途 |
| --- | --- |
| `/usr/bin/whatcmd` | 启动器（`sh` 脚本，调用主程序） |
| `/usr/share/whatcmd/whatcmd.py` | 主程序 |
| `/usr/share/whatcmd/commands.json` | 命令库（102 条命令） |
| `/usr/share/doc/whatcmd/install.sh` | 原跨平台安装脚本（参考存档） |

> 设计说明：主程序通过 `dirname(abspath(__file__))` 自动定位同目录的
> `commands.json`。本包将二者放在同一目录 `/usr/share/whatcmd/`，再由
> `/usr/bin/whatcmd` 启动器直接调用，**无需修改源码、无需硬编码路径**。

---

## 5. 卸载

```bash
sudo dpkg -r whatcmd        # 保留配置
# 或
sudo dpkg -P whatcmd        # 彻底清除
```

---

## 6. 常见问题

- **`whatcmd: command not found`**
  确认安装成功：`dpkg -l whatcmd`；并确认 `/usr/bin` 在 `$PATH` 中。
- **找不到命令库**
  正常情况下 `commands.json` 与 `whatcmd.py` 同目录，无需手动处理。
  报错时检查 `/usr/share/whatcmd/commands.json` 是否存在。
- **颜色/Emoji 不显示**
  非交互终端或设置 `NO_COLOR=1` 时会自动关闭彩色输出，属正常行为。

---

## 7. 上游与许可

- 上游项目：<https://github.com/FlightXHD/whatcmd>
- 本 `.deb` 由 WorkBuddy 依据上游三个源文件（`.py` / `.json` / `install.sh`）封装，未改动程序逻辑。
