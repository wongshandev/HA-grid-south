# China Southern Power Grid Statistics

# 南方电网电费数据HA集成

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

> 本仓库 Fork 自 [CubicPill/china_southern_power_grid_stat](https://github.com/CubicPill/china_southern_power_grid_stat)，由于原作者已停止维护，本仓库将继续维护并修复问题。

## 修复记录

### 2026-02-01
- **修复 API 响应解析错误**：将 `Accept-Encoding` 从 `gzip, deflate, br` 改为 `gzip, deflate`，解决因缺少 Brotli 解压库导致 API 响应无法正确解析的问题。

---

## 支持功能

- ✅支持南方电网覆盖范围内的电费数据查询（广东、广西、云南、贵州、海南）
- ✅支持使用手机号、短信验证码和密码（可选）登录，支持南网在线APP、微信、支付宝扫码登录
- ✅支持多个南网账户（每个账户一个集成），支持单个账户下的多个缴费号
- ✅数据自动抓取和更新（默认间隔4小时，可配置）
- ✅全程GUI配置，无需编辑yaml进行配置（暂不支持yaml配置）

可接入如下数据：

- 当前余额和欠费
- 当前阶梯电量数据（档位、阶梯剩余电量、阶梯电价）
- 昨日用电量
- 最新一日用电量、电费（取有数据的最近一日）
- 本年度总用电量、总电费（非实时，更新到上个月）
- 本年度每月用电量、电费（非实时，更新到上个月）
- 上年度总用电量、总电费
- 上年度每月用电量、电费
- 当月累计用电量、电费（非实时，有2天左右的延迟）
- 当月每日用电量、电费（非实时，有2天左右的延迟）
- 上月累计用电量、电费
- 上月每日用电量、电费

❌**不支持**阶梯电费设置（仅能获取当前所在阶梯）、峰谷电价设置和电费计算（本插件只进行数据抓取和转换，不进行任何计算），暂时也没有支持计划（南网暂时没有统一的API），如有需求，建议单独创建对应的电价实体。

❌因为南网登录API调整，不再支持登录态失效之后自动重新登录，需要手动重新登录。

---

## 安装方法

### 方法一：HACS 自定义仓库（推荐）

1. 打开 HACS → 集成
2. 点击右上角 **三个点** → **自定义存储库**
3. 填写：
   - **存储库**: `wongshandev/HA-grid-south`
   - **类别**: 选择 `集成`
4. 点击 **添加**
5. 搜索 **China Southern Power Grid** 并安装
6. **重启 Home Assistant**

### 方法二：手动安装

1. 下载本仓库的 [最新代码](https://github.com/wongshandev/HA-grid-south/archive/refs/heads/master.zip)
2. 解压后，将 `custom_components/china_southern_power_grid_stat` 文件夹复制到 Home Assistant 的 `config/custom_components/` 目录下
3. **重启 Home Assistant**

### 方法三：SSH 命令安装

```bash
# 进入 Home Assistant 配置目录
cd /homeassistant/custom_components

# 下载并安装
rm -rf china_southern_power_grid_stat
wget https://github.com/wongshandev/HA-grid-south/archive/refs/heads/master.zip
unzip master.zip
mv HA-grid-south-master/custom_components/china_southern_power_grid_stat ./
rm -rf HA-grid-south-master master.zip

# 重启 Home Assistant
```

---

## 配置步骤

1. 进入 **设置 → 设备与服务 → 添加集成**
2. 搜索 **南方电网** 或 **China Southern Power Grid**
3. 选择登录方式（推荐 **短信验证码登录**）
4. 输入手机号，获取验证码并登录
5. 登录成功后，点击 **配置 → 添加已绑定的缴费号**
6. 选择你的缴费号即可

注意：本集成需求 `Home Assistant` 最低版本为 `2022.11`。

---

## 传感器列表

- 余额
- 欠费
- 当前阶梯档位
- 当前阶梯剩余电量
- 当前阶梯电价
- 上月电费
- 上月用电量
- 当月用电量
- 当月电费
- 本年度电费
- 本年度用电量
- 上年度电费
- 上年度用电量
- 最近日用电量
- 最近日电费
- 昨日用电量

---

## 常见问题

### Q: 登录后显示 "Login expired" 或无法获取数据

A: 这是因为南网 API 调整，登录态会过期。请删除集成后重新添加，使用短信验证码重新登录。

### Q: API 响应解析错误 (JSONDecodeError)

A: 本仓库已修复此问题。如果你使用的是原仓库 `CubicPill/china_southern_power_grid_stat`，请切换到本仓库。

### Q: HACS 无法下载本仓库

A: 可以使用手动安装方法，直接下载代码并复制到 `custom_components` 目录。

---

## 致谢

### 原作者
- [CubicPill](https://github.com/CubicPill) - 原项目作者

### 贡献者
- [lyylyylyylyy](https://github.com/lyylyylyylyy): PR [#30](https://github.com/CubicPill/china_southern_power_grid_stat/pull/30) 短信验证码登录支持

感谢[瀚思彼岸](https://bbs.hassbian.com/)论坛以下帖子作者的辛苦付出，排名不分先后

- [不折腾，超简单接入电费数据](https://bbs.hassbian.com/thread-18474-1-1.html)
- [北京电费查询加强版](https://bbs.hassbian.com/thread-13820-1-1.html)
- [电费插件（Node-Red流）-广东南方电网](https://bbs.hassbian.com/thread-17830-1-1.html)
- [【抄作业】电费插件(NR流)-南网](https://bbs.hassbian.com/thread-18122-1-1.html)
