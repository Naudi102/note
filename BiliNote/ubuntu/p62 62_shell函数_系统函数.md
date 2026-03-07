# Ubuntu 教程：Shell 函数 — 系统函数

## 目录
- [系统函数概述](#系统函数概述)
- [basename 命令详解](#basename-命令详解)
- [dirname 命令详解](#dirname-命令详解)
- [实际应用场景](#实际应用场景)
- [AI 总结](#ai-总结)

## 系统函数概述
在 Shell 脚本中，系统函数（system functions）指的是一些内置或常用命令，用于处理文件路径、字符串等常见任务。本节重点介绍两个用于处理文件路径的系统命令：
- `basename`
- `dirname`

这两个命令常用于从完整路径中提取文件名或目录路径，在批量处理文件时非常实用。

## basename 命令详解
`basename` 用于从给定的完整路径中提取**文件名部分**。

### 基本用法
```bash
basename /完整/路径/到/文件名.扩展名
```
输出结果仅为 `文件名.扩展名`，即去除所有前面的路径部分。

*![](http://localhost:8000/static/screenshots/screenshot_01c1b49f1-adce-4058-8e43-3d79bddf8dd1.jpg)*

### 去除扩展名
若需同时去掉文件扩展名，可在命令后指定要移除的后缀：
```bash
basename /完整/路径/白龙马.txt .txt
```
输出结果为 `白龙马`。

> 注意：第二个参数必须与文件实际扩展名完全一致（包括点号），否则不会被移除。

*![](http://localhost:8000/static/screenshots/screenshot_1f943c444-2ba6-4fa5-a3b5-e35e3e8be55e.jpg)*

## dirname 命令详解
`dirname` 与 `basename` 功能互补，用于提取路径中的**目录部分**。

### 基本用法
```bash
dirname /完整/路径/到/文件名.扩展名
```
输出结果为 `/完整/路径/到`，即去除最后的文件名部分。

*![](http://localhost:8000/static/screenshots/screenshot_28f3083e7-440d-4b26-bc73-6625121426cc.jpg)*

### 与 basename 的关系
- `dirname` + `basename` = 完整路径
- 二者可将一个绝对路径拆分为“目录”和“文件名”两部分，便于脚本中分别处理。

## 实际应用场景
在 Shell 脚本中，尤其是需要**批量处理文件**时，这两个命令极为常用：

- 遍历多个带路径的文件，仅保留文件名进行日志记录或重命名。
- 提取文件所在目录，用于创建备份或输出到同级目录。
- 结合循环使用，自动化处理大量数据文件。

例如：
```bash
for file in /home/user/documents/*.txt; do
    name=$(basename "$file" .txt)
    dir=$(dirname "$file")
    echo "Processing $name in $dir"
done
```

## AI 总结
本节介绍了 Shell 中两个关键的系统命令：`basename` 和 `dirname`。前者用于从完整路径中提取文件名（可选去除扩展名），后者用于提取路径中的目录部分。二者在 Shell 脚本编写、尤其是批量文件处理场景中具有极高实用性，能有效简化路径解析逻辑，提升脚本的健壮性与可读性。掌握这两个命令是 Linux 自动化任务的基础技能之一。