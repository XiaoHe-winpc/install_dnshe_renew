# DNSHE 自动续期部署脚本 🚀

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Raspbian-blue)](https://www.raspberrypi.org/)

一键部署脚本，让你的 [DNSHE](https://my.dnshe.com) 免费域名自动续期，永不过期。  
基于 Shell + curl，树莓派开箱即用。

---

## ✨ 功能特点

- ✅ **全自动安装**：一条命令完成依赖安装、脚本部署、定时配置。
- ✅ **智能续期**：全自动续期，自动检测是否可续期。
- ✅ **安全存储**：API 密钥写入系统环境变量 `/etc/environment`，不暴露在脚本中。
- ✅ **日志记录**：所有操作记录到 `/var/log/dnshe_renew.log`，方便排查问题。
- ✅ **兼容性强**：只需 `bash`、`curl` 和 `jq`（脚本会自动安装 `jq`）。
- ✅ **轻松卸载**：提供完整卸载命令，不留残留。

---

## 📋 前提条件

- 一台运行 **Debian/Ubuntu/Raspbian** 的 Linux 设备（树莓派完美支持）。
- 已注册 [DNSHE](https://my.dnshe.com) 账号，并在后台获取 **API Key** 和 **API Secret**（路径：登录后 → API 管理）。
- 拥有 `sudo` 权限。

---

## 🚀 快速开始

### 一键安装（推荐）

只需复制以下命令到终端执行，根据提示输入 API 密钥即可：

```bash
sudo bash -c "$(curl -fsSL https://ghfast.top/?q=https://github.com/XiaoHe-winpc/install_dnshe_renew/releases/download/v1.0/install_dnshe_renew.sh)"
```

### 带参数安装（无需交互）

如果你已经准备好 API 密钥，可以直接在命令中提供：

```bash
sudo bash -c "$(curl -fsSL https://ghfast.top/?q=https://github.com/XiaoHe-winpc/install_dnshe_renew/releases/download/v1.0/install_dnshe_renew.sh)" _ "你的_API_Key" "你的_API_Secret"
```

> **注意**：`_` 是占位符，不要删除。后面的两个参数依次为 `API_Key` 和 `API_Secret`。

---

## 🧪 测试与手动运行

安装完成后，你可以立即运行一次续期脚本来验证配置：

```bash
sudo /usr/local/bin/dnshe_renew.sh
```

查看运行日志：

```bash
cat /var/log/dnshe_renew.log
```

查看定时器状态（确认下次执行时间）：

```bash
systemctl list-timers dnshe-renew.timer
```

---

## 📄 许可证

本项目采用 MIT 许可证，详情请查看 [LICENSE](LICENSE) 文件。

Copyright (c) 2026 XiaoHe-winpc

---

## 🤝 贡献

欢迎提交 Issue 或 Pull Request！如果你改进了脚本或适配了新 API，请分享给更多人。

---

## 💬 问题反馈

如果遇到任何问题，请 [提交 Issue](https://github.com/XiaoHe-winpc/install_dnshe_renew/issues) 并附上日志内容（`/var/log/dnshe_renew.log`）。