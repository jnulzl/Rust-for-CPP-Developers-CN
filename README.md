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

或手工执行（**必须加 `-shell-escape`**；目录与书签需连跑三遍才完全稳定）。
**注意 `TEXMF_OUTPUT_DIRECTORY` 必须设成本工程的绝对路径**，否则代码块会全部高亮失败：

```bat
set TEXBIN=D:\ProgramData\texlive\2026\bin\windows
set PYTHON=D:\anaconda3
set PATH=%PYTHON%;%TEXBIN%;%PATH%
set PYTHONPATH=
set SELFAUTOLOC=%TEXBIN%

REM 【关键】必须设成【本工程根的绝对路径】（这里按你的实际路径改）
set TEXMF_OUTPUT_DIRECTORY=$ABS_ROOT_PATH\latex_source
set TEXMFOUTPUT=%TEXMF_OUTPUT_DIRECTORY%

xelatex -shell-escape -interaction=nonstopmode -synctex=1 main.tex
xelatex -shell-escape -interaction=nonstopmode -synctex=1 main.tex
xelatex -shell-escape -interaction=nonstopmode -synctex=1 main.tex
```

> 用 `build.bat` 时这些都不用手动设——脚本里用 `%~dp0` 自动填好了，并会在编译成功后
> 顺手清掉根目录残留的 `*.minted` / `*.listing`。

> ⚠ **`build.bat` 是纯 ASCII 文件，请勿在其中加入中文注释。**
> 批处理由 `cmd.exe` 逐行解析，而 cmd 按「当前代码页」（简体中文 Windows 默认
> GBK/936）读取文件。若文件是 UTF-8，中文注释会变成乱码，且乱码行会被当作
> 命令执行，报出一堆
> `'锛歍eX' 不是内部或外部命令，也不是可运行的程序或批处理文件`。
> 所以该脚本的注释一律用英文，中文说明放在本 README 里。

## 书末索引

`book/content/front/index.tex` 是**双栏排版**（`multicols`），共 391 个词条、
332 条带页码、482 处页码引用。

页码不是写死的数字，而是 `\pageref{idx:<hash>}` 内部跳转——正文段落的
`\label{idx:…}` 由回填脚本随文写入，因此**索引页码会随排版自动更新**，
改版式后无需重编索引译文，只需连跑三遍 `xelatex` 让 `\pageref` 收敛。

## 内容生成管线

`book/index.tex`、`book/content/**` 与 `book/content/front/index.tex` **全部由脚本生成**，
请勿手改——下次重跑会被覆盖。真正的源头是 `../work/out/*.json`（译文）与
`../work/units/*.json`（单元表）。

```
../work/
├── TRANSLATION_GUIDE.md     翻译规范与术语表（执行前必读）
├── tools/                   全流程脚本
│   ├── extract.py           PDF → 结构化翻译单元（含代码块缩进、提示框、索引锚点）
│   ├── make_batches.py      切分翻译批次（每批 ~22k 字符）＋ 索引批次 t900
│   ├── check_batch.py       译文校验（id 完整性、行内标签配对、实体、漏译）
│   ├── fix_out.py           定点修正表（补漏译、统一术语）
│   ├── build_latex.py       译文回填 → book/content/**/*.tex 与 book/index.tex
│   └── gen_index.py         索引页解析 → book/content/front/index.tex（双栏）
├── extract/                 PDF 抽取产物（units / anchors / index_entries …）
├── units/                   单元表、章标题别名表、索引补译表
├── batch/                   25 个待译批次（t001–t023 正文 ＋ t900 索引）
├── out/                     25 个译文文件
└── tmpl/                    LaTeX 骨架（含 @@id@@ 占位符）
```

重跑顺序：`extract.py` → `make_batches.py` → 翻译 `batch/*` → `check_batch.py`
→ `fix_out.py` → `build_latex.py` → `gen_index.py` → `build.bat`。

## 官方代码

[Rust-for-CPP-Developers](https://github.com/PacktPublishing/Rust-for-CPP-Developers)
