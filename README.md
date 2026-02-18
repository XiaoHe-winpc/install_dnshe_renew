# DNSHE 自动续期部署脚本 🚀

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Raspbian-blue)](https://www.raspberrypi.org/)

一键部署脚本，让你的 [DNSHE](https://my.dnshe.com) 免费域名每 160 天自动续期，永不过期。  
基于 Shell + curl，树莓派开箱即用。

---

## ✨ 功能特点

- ✅ **全自动安装**：一条命令完成依赖安装、脚本部署、定时配置。
- ✅ **智能续期**：每 160 天触发一次续期，即使设备关机也会在开机后补执行。
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

## ⚙️ 工作原理

- 脚本通过 DNSHE 官方 API（`api005.dnshe.com`）获取你的域名列表，并尝试续期。
- Systemd timer 每 160 天触发一次续期任务（`OnUnitActiveSec=160d`），若错过则开机补执行（`Persistent=true`）。
- 续期窗口为域名到期前 180 天内，160 天的周期确保你有足够缓冲。

---

## 🛠️ 自定义配置

如需修改 API 接口地址或续期参数，请编辑 `/usr/local/bin/dnshe_renew.sh` 中的以下部分：

```bash
API_BASE="https://api005.dnshe.com/index.php"   # API 入口
# 续期 URL 构造（第50行左右）
renew_url="${API_BASE}?m=renew&id=$id"
```

修改后保存，手动运行测试即可。

---

## 🧹 卸载方法

如需完全移除自动续期服务，执行以下命令：

```bash
sudo systemctl stop dnshe-renew.timer
sudo systemctl disable dnshe-renew.timer
sudo rm /etc/systemd/system/dnshe-renew.{service,timer}
sudo rm /usr/local/bin/dnshe_renew.sh
sudo sed -i '/^DNSHE_API_/d' /etc/environment
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

如果遇到任何问题，请 [提交 Issue](https://github.com/XiaoHe-winpc/dnshe-auto-renew/issues) 并附上日志内容（`/var/log/dnshe_renew.log`）。