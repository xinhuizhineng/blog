---
title: "Markdown 与 Pandoc 的完美结合"
author: "常香玉"              # 文章作者
description : "打造高效写作流：Markdown 与 Pandoc 的完美结合"    # 文章描述信息
lastmod: 2023-10-27T20:46:00+08:00     # 文章修改日期
date: 2023-10-27T20:46:00+08:00

tags : [                    # 文章所属标签
    "效率工具",
    "Markdown",
    "Pandoc"
]
categories : [              # 文章所属标签
    "Pandoc",
]

---

# 打造高效写作流：Markdown 与 Pandoc 的完美结合

在数字化写作的时代，我们经常面临一个痛点：**内容与格式的纠缠**。在 Word 中调整缩进和字体往往占据了我们一半的创作时间。今天，我想分享一套“一次编写，到处运行”的高效写作方案：**Markdown + Pandoc**。

## 为什么选择 Markdown？

Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。它的核心优势在于：

1.  **专注内容**：没有复杂的排版按钮，让你沉浸在写作逻辑中。
2.  **通用性强**：几乎所有的代码编辑器和笔记软件都支持。
3.  **版本控制**：纯文本文件非常适合用 Git 进行管理。

> “Markdown 的目标是实现‘易读易写’。” —— John Gruber

## 神器登场：Pandoc 是什么？

如果说 Markdown 是原材料，那么 [Pandoc](https://pandoc.org/) 就是那台万能加工机。它是一个开源的文档转换工具，被誉为文档转换界的“瑞士军刀”。

它支持在 Markdown、Word、PDF、LaTeX、HTML 等多种格式之间相互转换。这对于写简历、论文或技术文档来说，简直是救星。

### 核心功能

*   保留 Markdown 的结构和可读性
*   一键生成 PDF / Word / HTML
*   支持自定义 CSS 样式和 LaTeX 模板

## 实战演练：如何安装与使用

### 1. 安装 Pandoc

根据你的操作系统，选择对应的安装方式：

*   **Windows**: 访问官网下载 `.msi` 安装包，记得勾选 "Add to PATH"。
*   **MacOS**: 使用 Homebrew 安装。
    ```bash
    brew install pandoc
    ```
*   **Linux**:
    ```bash
    sudo apt install pandoc
    ```

安装完成后，在终端输入 `pandoc -v` 检查是否成功。

### 2. 转换命令速查

假设你写好了一份名为 `resume.md` 的简历，以下是几个常用的转换命令：

**生成 Word 文档**

`pandoc resume.md -o resume.docx`

生成 PDF (推荐使用 wkhtmltopdf 引擎) 如果你希望保留 Markdown 中的 CSS 样式（比如带颜色的标题），建议使用以下命令：

```bash
pandoc resume.md -o resume.pdf --pdf-engine=wkhtmltopdf
```
进阶技巧：套用模板
Pandoc 最强大的地方在于支持模板。你可以下载大神们做好的 LaTeX 模板，让你的文档瞬间拥有出版级的排版。

```bash
pandoc resume.md -o resume.pdf --template=mytemplate.tex
```
