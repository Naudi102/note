# Ubuntu 用户组与 sudo 权限管理笔记

## 目录
- [sudo 命令的作用与原理](#sudo-命令的作用与原理)
- [默认 sudo 权限限制](#默认-sudo-权限限制)
- [配置 sudo 权限：/etc/sudoers 文件](#配置-sudo-权限-etc-sudoers-文件)
- [用户组管理：将用户加入 sudo 组](#用户组管理将用户加入-sudo-组)
- [免密码执行 sudo（NOPASSWD）](#免密码执行-sudo-nopasswd)
- [AI 总结](#ai-总结)

---

## sudo 命令的作用与原理

- `sudo`（Superuser Do）是一个在 Linux 系统中用于**临时提升权限**的命令。
- 它允许普通用户以 **root（超级用户）身份**执行特定命令。
- 工作机制：
  - 用户在 `sudo` 后输入命令；
  - 系统将该命令提交给 root 用户执行；
  - 本质上是“借用” root 的权限完成操作。

> 类比：像向“许愿池”说出愿望，由 root 用户帮你实现。

## 默认 sudo 权限限制

- 在 Ubuntu 系统安装过程中创建的**第一个用户**（如 `aitaigu`）**默认被加入 `sudo` 组**，因此拥有使用 `sudo` 的权限。
- 其他用户（如手动创建的 `wukong`）**默认无法使用 `sudo`**。
  - 执行 `sudo` 时会提示：
    ```
    user is not in the sudoers file. This incident will be reported.
    ```
  - 表示该用户未被授权使用 `sudo`，且此行为会被记录。

*![](http://localhost:8000/static/screenshots/screenshot_0c2cc243d-f39b-4df0-b497-1fde7e7ebe57.jpg)*

## 配置 sudo 权限：/etc/sudoers 文件

- `sudo` 的权限控制由 `/etc/sudoers` 文件定义。
- **必须使用 `sudo visudo` 命令编辑该文件**（而非直接用 `vim /etc/sudoers`），因为：
  - `visudo` 会在保存前检查语法，防止配置错误导致系统无法使用 `sudo`。
- 关键配置行示例：
  ```bash
  %sudo   ALL=(ALL:ALL) ALL
  ```
  - 含义：`sudo` 组中的所有用户可在任何主机上以任何用户身份执行任何命令。
- 编辑步骤：
  1. 切换回有 `sudo` 权限的用户（如 `aitaigu`）；
  2. 执行 `sudo visudo`；
  3. 找到 `%sudo` 行进行修改或确认。

*![](http://localhost:8000/static/screenshots/screenshot_19cdccb93-4c2a-4d90-9b74-30af0b45fb1b.jpg)*

## 用户组管理：将用户加入 sudo 组

- 要让其他用户（如 `wukong`）获得 `sudo` 权限，需将其加入 `sudo` 用户组。
- 查看用户组信息：
  ```bash
  cat /etc/group | grep sudo
  ```
  - 输出示例：`sudo:x:27:aitaigu,wukong`
- 添加用户到 `sudo` 组的命令：
  ```bash
  sudo usermod -aG sudo wukong
  ```
  - `-aG`：`-a` 表示追加（append），`-G` 指定附加组（supplementary group）。
- **注意**：用户需**重新登录**后，新组权限才会生效。

验证：
- 切换到 `wukong` 用户；
- 执行 `sudo ls`，若能成功执行（或提示输入密码），说明权限已生效。

*![](http://localhost:8000/static/screenshots/screenshot_245e60d30-22d5-4f35-9b40-221b3d4b554d.jpg)*

## 免密码执行 sudo（NOPASSWD）

- 默认情况下，每次执行 `sudo` 都需输入当前用户密码。
- 若希望**跳过密码验证**（适用于可信环境或自动化脚本），可修改 `/etc/sudoers`：
  ```bash
  %sudo   ALL=(ALL:ALL) NOPASSWD: ALL
  ```
  - 在原配置行末尾添加 `NOPASSWD:` 即可。
- 修改后保存退出（在 `visudo` 中使用 `:wq!` 强制保存）。
- 效果：属于 `sudo` 组的用户执行 `sudo` 命令时**不再提示输入密码**。

> ⚠️ 安全提示：`NOPASSWD` 会降低系统安全性，仅建议在受控环境中使用。

## AI 总结

本节详细讲解了 Ubuntu 中 `sudo` 权限的核心机制：只有属于 `sudo` 用户组的用户才能使用 `sudo` 命令。通过编辑 `/etc/sudoers` 文件（使用 `visudo`），可配置组权限及是否需要密码。为新用户授权的关键步骤是使用 `usermod -aG sudo <username>` 将其加入 `sudo` 组，并确保用户重新登录以应用变更。合理管理 `sudo` 权限对系统安全和团队协作至关重要。