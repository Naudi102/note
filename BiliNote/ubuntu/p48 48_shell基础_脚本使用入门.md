# Shell 脚本入门：Ubuntu 系统脚本编写与执行

## 目录
- [Shell 与 Shell 脚本简介](#shell-与-shell-脚本简介)
- [Linux 中的 Shell 解析器](#linux-中的-shell-解析器)
- [编写第一个 Shell 脚本](#编写第一个-shell-脚本)
- [Shell 脚本的两种执行方式](#shell-脚本的两种执行方式)
- [AI 总结](#ai-总结)

## Shell 与 Shell 脚本简介

- **Shell** 是 Linux 系统中用于解析用户命令的**命令行解释器（Command-line Interpreter）**，它作为用户与操作系统内核之间的桥梁。
- 用户可通过图形界面或命令行与操作系统交互，但底层最终都是通过 Shell 执行命令。
- **Shell 脚本（Shell Script）** 是将多个命令按逻辑顺序组织成一个可重复执行的程序文件，具有结构化、可复用、可自动化的特性。
- 编写 Shell 脚本意味着从“单条命令操作”进阶到“程序化控制”，是系统管理与自动化任务的核心技能。

## Linux 中的 Shell 解析器

- Linux 系统支持多种 Shell 解析器，常见的包括：
  - `bash`（Bourne Again Shell）
  - `sh`（Bourne Shell）
  - `zsh`、`csh`、`ksh` 等
- 查看系统中可用的 Shell 列表：
  ```bash
  cat /etc/shells
  ```
- 查看当前默认使用的 Shell：
  ```bash
  echo $SHELL
  # 输出示例：/bin/bash
  ```
- 在脚本开头指定解析器是**约定俗成的最佳实践**，确保脚本在不同环境中行为一致。

*![](http://localhost:8000/static/screenshots/screenshot_0706912b6-5293-42e2-ae8f-766d06ba2d4a.jpg)*

## 编写第一个 Shell 脚本

### 脚本存放位置建议
- 推荐将个人脚本统一存放在用户主目录下的专用目录，例如：
  ```bash
  ~/bin/
  ```
- 若 `~/bin` 不存在，需手动创建：
  ```bash
  mkdir ~/bin
  ```

### 脚本编写步骤（以输出 "Hello World" 为例）

1. 使用 `vim` 创建脚本文件：
   ```bash
   vim ~/bin/hello.sh
   ```

2. 在脚本第一行添加 **Shebang（#!）**，指定解析器：
   ```bash
   #!/bin/bash
   ```
   - `#!` 是必须的，否则系统会将该行视为注释，无法正确识别解析器。
   - `/bin/bash` 表示使用 Bash 解析器执行此脚本。

3. 第二行编写实际命令，例如使用 `echo` 输出字符串：
   ```bash
   echo "Hello World"
   ```

4. 保存并退出 `vim`：
   - 按 `Esc` 进入命令模式
   - 输入 `:wq` 保存退出

完整脚本内容：
```bash
#!/bin/bash
echo "Hello World"
```

*![](http://localhost:8000/static/screenshots/screenshot_1ef604a70-c1df-4692-a638-747fb6bbb599.jpg)*

## Shell 脚本的两种执行方式

### 方式一：通过显式调用 Shell 解析器执行（无需执行权限）

- 语法：
  ```bash
  bash 脚本路径
  # 或
  sh 脚本路径
  ```
- 示例（相对路径）：
  ```bash
  bash hello.sh
  sh hello.sh
  ```
- 示例（绝对路径）：
  ```bash
  bash /home/atguigu/bin/hello.sh
  ```
- **优点**：不要求脚本文件具有执行权限（`x` 权限），因为实际执行的是 `bash` 或 `sh` 命令。
- **注意**：`bash` 和 `sh` 在 Ubuntu 中通常指向同一解析器（`/bin/bash`），效果相同。

### 方式二：直接运行脚本（需赋予执行权限）

1. 为脚本添加执行权限：
   ```bash
   chmod +x hello.sh
   ```
   - 执行后，文件在终端中会显示为绿色（表示可执行）。

2. 执行脚本：
   - **必须使用路径前缀**（不能直接写 `hello.sh`，除非当前目录在 `PATH` 中）：
     ```bash
     ./hello.sh          # 相对路径（推荐）
     /home/atguigu/bin/hello.sh  # 绝对路径
     ```
   - ❌ 错误方式：`hello.sh`（会报“command not found”）

> **原因**：当前目录（`.`）通常不在系统的 `PATH` 环境变量中，因此 shell 无法在标准路径中找到该命令。使用 `./` 显式指明“当前目录下的文件”。

*![](http://localhost:8000/static/screenshots/screenshot_267a95209-066a-4363-995d-0d77414fea66.jpg)*

### 两种方式的本质
- 无论哪种方式，底层最终都是由 `/bin/bash`（或其他指定的 Shell）解析并执行脚本内容。
- 方式一更灵活（适合临时测试），方式二更符合“程序运行”的习惯（适合部署）。

## AI 总结

本节介绍了 Shell 脚本的基础概念与实战入门。Shell 作为命令解释器，是用户与 Linux 内核交互的关键组件。通过编写以 `#!/bin/bash` 开头的脚本文件，可将多条命令封装为可复用的程序。脚本可通过 `bash script.sh`（无需执行权限）或 `./script.sh`（需 `chmod +x` 赋权）两种方式运行。关键要点包括：正确使用 Shebang、理解执行权限的作用、以及避免因 PATH 问题导致的执行失败。掌握这些基础，是迈向自动化运维和嵌入式开发的重要一步。