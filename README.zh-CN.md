# GPTWOL 懒猫应用

本仓库将 [GPTWOL](https://github.com/Misterbabou/gptwol) 打包为懒猫应用。GPTWOL 可以唤醒或关闭局域网计算机、检测在线状态，并设置 Cron 定时任务。

## 登录方式

默认同时启用两种登录方式：

- 懒猫 OIDC：在登录页点击 `Login with SSO`，使用当前懒猫账号登录。
- GPTWOL 本地账号：使用安装向导中设置的用户名和随机密码。登录页会自动填充本地凭据，但不会自动提交。

可以在安装向导中独立关闭任意一种登录方式。至少保留一种登录方式可以避免局域网电源控制界面被未授权访问。

## 网络唤醒

GPTWOL 使用主机网络，以便向局域网发送 Wake-on-LAN 数据包。目标计算机需要在 BIOS、网卡和操作系统中启用网络唤醒，并与懒猫设备处于可达的局域网。

默认使用 UDP 广播发送 WOL。启用二层 WOL 或手动指定 ARP 扫描网卡时，需要填写懒猫主机真实的局域网接口名称；容器中的默认 `eth0` 不一定是实际的局域网接口。

Sleep-on-LAN 会向目标设备 UDP 9 端口发送反向 MAC 数据包，目标设备需要按上游文档安装并配置 [sleep-on-lan](https://github.com/SR-G/sleep-on-lan)。

## 数据持久化

计算机数据库保存在 `/app/db`，Cron 配置保存在 `/etc/cron.d`，两者都映射到懒猫应用数据目录，应用升级和重启不会丢失。

## 自动发布

GitHub Action 跟踪 `misterbabou/gptwol` 的稳定语义化版本标签，复制 `linux/amd64` 镜像到懒猫镜像仓库，构建版本化 LPK，并向官方商店和私有商店发布同一份已校验构建产物。
