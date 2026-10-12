# 《Rust for C++ Developers》中文版 —— LaTeX 工程（AI 翻译）

原书：*Rust for C++ Developers — Leverage your C++ expertise to write safer and faster
systems code in Rust*，Dan Olson 著，Packt Publishing，2026 年 3 月首版。

成品：**237 页**，三部分 13 章 + 附录 + 书末双栏索引。

## 目录结构

```
latex_source/
├── main.tex                 根文件
├── build.bat                一键编译脚本（纯 ASCII，勿加中文注释）
├── book/
│   ├── ccs.tex              导言区：字体、版式、全部自定义命令与环境  ← 改版式改这里
│   ├── index.tex            封面、目录、全书章节编排  ← 调章节顺序改这里
│   ├── packt-pygments-style.tex      代码高亮样式（浅色，Packt 配色）
│   ├── packtdark-pygments-style.tex  代码高亮样式（深色，终端 / 输出块）
│   ├── manning-pygments-style.tex    模板自带备用样式（当前未启用）
│   └── content/
│       ├── front/           版权页、贡献者、前言、书末索引（index.tex 自动生成）
│       ├── main/            part1–part3 ＋ 第 01–13、15 章
│       ├── appendix/        附录：搭建开发环境
│       └── back/            你可能喜欢的其他图书
├── fonts/                   JetBrainsMonoNL（模板自带备用代码字体，当前版式用 Consolas）
└── images/                  封面与宣传图（8 张）
```

正文共 23 个 `.tex`、约 33.5 万字符，含 **455 个代码块**（347 个浅色 `codebox` +
108 个深色 `codeboxdark`）、14 个「注意」框、1 个「提示」框。

版式取自 Packt 原书开本：版心 `531.36 × 628.83 bp`（≈ 7.38 × 8.73 in）。
中文正文 Source Han Serif SC，西文正文 Cambria，代码 Consolas（8bp / 10.11bp）。

## 编译

需要 **TeX Live 2026**（自带 `latexminted` 0.7+）与 **Python 3**（`latexminted` 是一个被
`xelatex` 调起的 Python 脚本，代码块用它与 Pygments 做语法高亮）：

```bat
build.bat                          REM Windows 双击即可
```

> 如果环境配置遇到问题或者编译失败可以直接到[releases](https://github.com/jnulzl/Rust-for-CPP-Developers-CN/releases)下载最新的pdf文件

## 官方代码

[Rust-for-CPP-Developers](https://github.com/PacktPublishing/Rust-for-CPP-Developers)
