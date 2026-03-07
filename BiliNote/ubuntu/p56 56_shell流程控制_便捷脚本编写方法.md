# Ubuntu 教程：Shell 流程控制与便捷脚本编写方法

## 目录
- [使用专业编辑器编写 Shell 脚本](#使用专业编辑器编写-shell-脚本)
- [Shell 脚本语法自动补全演示](#shell-脚本语法自动补全演示)
- [Windows 与 Linux 换行符差异问题](#windows-与-linux-换行符差异问题)
- [文件编码与 BOM 问题处理](#文件编码与-bom-问题处理)
- [推荐的脚本开发工作流](#推荐的脚本开发工作流)
- [AI 总结](#ai-总结)

## 使用专业编辑器编写 Shell 脚本

在实际开发中，Shell 脚本往往远超几十行，可能达到上百甚至几百行。手动在终端或简易编辑器中编写容易出错且效率低下。因此，**强烈建议使用支持语法高亮和自动补全的专业文本编辑器**。

- 推荐编辑器：
  - **Sublime Text**（视频中使用）
  - **Notepad++**
  - 其他支持 Bash 语法的 IDE 或编辑器

- 设置步骤：
  1. 新建文件后，将文件语法格式设置为 **Bash**。
  2. 编辑器会自动识别 `#!/bin/bash` 并进行语法高亮。
  3. 输入关键字（如 `if`）时，编辑器会提供结构模板，一键补全整个流程控制块。

*![](http://localhost:8000/static/screenshots/screenshot_0883f0606-b622-4d77-83a3-ed250a3fb279.jpg)*

## Shell 脚本语法自动补全演示

以 `if-elif-else-fi` 条件判断结构为例：

```bash
#!/bin/bash

if [ "$1" = "1" ]; then
    echo "班长真帅"
elif [ "$1" = "2" ]; then
    echo "老师实在是太美"
else
    echo "什么都没有"
fi
```

- 编辑器（如 Sublime）输入 `if` 后按回车，会自动生成完整结构。
- 注意：部分编辑器会在外层多加一对花括号 `{}`，这在 Shell 中是**无效且多余**的，应删除。
- 内部的 `[ ]` 是条件测试命令，必须保留。

*![](http://localhost:8000/static/screenshots/screenshot_196c1c1bb-4c50-4fda-8238-5cbe3236026c.jpg)*

## Windows 与 Linux 换行符差异问题

**核心问题**：  
Windows 使用 **CRLF**（`\r\n`）作为换行符，而 Linux/Unix 使用 **LF**（`\n`）。若在 Windows 编辑器中编写脚本并直接在 Ubuntu 中运行，会导致语法错误。

- 表现：执行脚本时报错，如 `command not found` 或 `syntax error`。
- 原因：Shell 解释器无法正确解析以 `\r` 结尾的行。

### 解决方案

**推荐方法：通过 Vim 粘贴自动转换换行符**

1. 在 Windows 编辑器中编写并复制脚本内容。
2. 在 Ubuntu 终端中用 `vim script.sh` 打开文件。
3. 进入插入模式（`i`），直接粘贴（`Ctrl+Shift+V` 或鼠标右键粘贴）。
4. Vim 会**自动将 CRLF 转换为 LF**。
5. 保存退出（`:wq`）。

> 此方法避免了手动转换换行符的麻烦，是最高效可靠的实践。

*![](http://localhost:8000/static/screenshots/screenshot_23d3d2237-7fa3-4e69-bbc6-0f0e81e6ac3b.jpg)*

## 文件编码与 BOM 问题处理

当脚本中包含中文字符时，需特别注意**文件编码格式**：

- **推荐编码**：`UTF-8 with BOM`
  - 某些编辑器（如 Notepad++）在保存 UTF-8 时默认不带 BOM。
  - 若使用带 BOM 的 UTF-8，可确保中文正常显示且脚本头 `#!/bin/bash` 被正确识别。
- 若编码设置不当，可能导致：
  - 第一行解释器路径识别失败（报错 `bad interpreter`）
  - 中文乱码

> **最佳实践**：尽量避免在脚本中直接写中文。如必须使用，确保编辑器保存为 **UTF-8-BOM** 格式。

## 推荐的脚本开发工作流

结合以上要点，推荐以下高效开发流程：

1. **在 Windows 下使用 Sublime/Notepad++ 编写脚本**  
   - 设置语法为 Bash
   - 利用自动补全快速构建结构
2. **复制完整脚本内容**
3. **在 Ubuntu 中用 `vim` 打开目标 `.sh` 文件**
   - 删除原有内容（如有）
   - 进入插入模式后直接粘贴
4. **保存并赋予执行权限**  
   ```bash
   chmod +x script.sh
   ./script.sh arg
   ```
5. **避免直接从 Windows 传输未处理的 `.sh` 文件到 Linux 执行**

该流程能有效规避换行符和编码问题，大幅提升开发效率与稳定性。

## AI 总结

本节强调了在 Shell 脚本开发中使用专业编辑器的重要性，尤其在涉及流程控制（如 `if`、`for`、`while`）时，自动补全可显著减少语法错误。同时，重点指出了跨平台开发中的两大陷阱：**Windows 与 Linux 的换行符差异（CRLF vs LF）** 和 **文件编码（特别是中文与 BOM 处理）**。通过“Windows 编写 → Vim 粘贴”的工作流，可自动解决换行符问题，是实际开发中最推荐的做法。最终目标是提升脚本编写效率与可靠性，避免低级错误浪费调试时间。