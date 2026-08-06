# Academic CV Template

[![README](https://img.shields.io/badge/README英文版-00008B?logo=readme)](README.md)

## 页面预览

<div style="display: flex; justify-content: space-between; align-items: flex-end;">
    <img src="figure/READMEpage1.png" style="width: 49%; height: auto;">
    <img src="figure/READMEpage2.png" style="width: 49%; height: auto;">
</div>

## 所需文件

```
./
|--figure	# store image here
|--sec		# cv sections here
|--main.tex	# Edit cv here
```

## 编辑内容

在 `main.tex` 导言区中编辑个人基本信息，作为头部内容；在 `sec/` 目录下编辑不同 `.tex` 文件，作为对应的章节内容。

### 头部选项

提供了不同的头部内容风格选项，需要在 `\input{sec/head}` 之前使用下面命令之一：

```latex
\headstyleitrue		% onecolumn, simple version & no icons, suitable for digital edition
\headstyleiitrue	% onecolumn, simple version & with icons, suitable for digital edition
\headstyleiiitrue	% onecolumn, detailed version & no icons, suitable for print edition
\headstyleivtrue	% onecolumn, detailed version & with icons, suitable for print edition
\headstylevtrue		% twocolumn, no photo
\headstylevitrue	% twocolumn, with photo
```

注意：有且仅有一个 `\headstyle` 被设置为 `true` (比如 `\headstyleitrue`)

### 字体选项

提供了不同的字体选项，建议在 `\begin{document}` 下一行使用下面命令之一：

```latex
\fontstylei		% Helvetica-like (similar to Arial/Calibri)
\fontstyleii	% Times-like (similar to Times New Roman)
\fontstyleiii	% Palatino (elegant serif)
\fontstyleiv	% Bookman (readable serif)
\fontstylev		% Avant Garde-like (similar to Trebuchet MS)
\fontstylevi	% Quasi-Helvetica (another Arial/Calibri alternative)
\fontstylevii	% Quasi-Times (another Times New Roman alternative)
\fontstyleviii	% Quasi-Palatino (another elegant serif option)
\fontstyleix	% Computer Modern Roman
\fontstylex		% Computer Modern Sans Serif
\fontstylexi	% Charter (clean serif font)
```

### 对齐选项

提供了两种对齐方式：

```latex
\justifyfalse	% align left
\justifytrue	% justify
```

## 编辑模板

### 头部内容

在 `sec/head.tex` 文件中可以自主编辑模板：

- 设置图标（icon）
- 设置字号
- 设置展示的信息内容并排版
- ......

在 `onecolumn` 风格中，主要通过 `center` 环境实现。在 `twocolumn` 风格中，如果没有图片，主要通过 `\hfill` 命令实现两列的左右对齐，另外 `tabular*` / `tabularx` 表格环境也可以实现相同的效果；如果展示图片，主要通过 `\parbox{width}{content}` 命令实现。

### 章节内容

设置不同的章节目的是为了以分类的形式来展示更为详细的事迹。描述一条事迹，需要包含这些元素：名称、时间、描述（可选）、地点（可选）、补充信息（可选）。不同类别的事迹对元素的需求不同，元素需求场景可能分为以下几种：

- 所有元素
- 不需要地点，使用图标链接替代
- 只需要名称和时间
- 只需要名称、时间和补充信息

在模板中设置两层 `itemize` 列表环境：

- 列表环境：第一层为主要列表环境，用于描述名称、时间、描述和地点，第二层为次要列表环境，用于描述补充信息。
- 列表环境命令：第一层使用 `\resumeSubHeadingList` 环境，第二层使用 `\resumeItemList` 环境，它们本质上都是 `itemize` 环境。
- 列表项命令：第一层使用 `\resumeSubHeading` / `\resumeSubProject` / `\resumeSubContent` 命令，第二层使用 `\resumeItem` 命令，它们本质上都是 `\item` 命令。

其中，第一层列表项命令封装的是一个 `tabular*` / `tabularx` 表格环境，实现三种列表项命令的原因在于为了适配上面提到的不同的元素需求场景，这些场景在参数数量和展示样式方面具有不同的需求，故而单独实现它们。具体如下：

```latex
\resumeSubHeadingListStart
% bullet point 1: All elements
\resumeSubHeading{name}{time}{desc}{loc}
  \resumeItemListStart
  \resumeItem{additional information 1}
  \resumeItem{additional information 2}
  \resumeItemListEnd
% bullet point 2: No need for loc, show icon link instead
\resumeSubProject{name}{time}{desc}{link}
  \resumeItemListStart
  \resumeItem{additional information 1}
  \resumeItem{additional information 2}
  \resumeItemListEnd
\resumeSubHeadingListEnd
```

```latex
\resumeSubHeadingListStart
% bullet point 3: Just name and time
\resumeSubContent{name}{time}
% bullet point 4: Just name, time, and additional information
\resumeSubContent{name}{time}
  \vspace{-7mm}
  \resumeItemListStart
  \resumeItem{additional information 1}
  \resumeItem{additional information 2}
  \resumeItemListEnd
\resumeSubHeadingListEnd
```

![READMEsection](figure/READMEsection.png)

如果有其他的元素需求场景，上面的场景没有覆盖到，可以添加其他的模板命令。

### 控制对齐

用户可以通过 `\justifytrue` 和 `\justifyfalse` 命令分别实现两端对齐和左对齐。这里所说的对齐有两层含义：

- 正文纯文本段落的对齐方式
- 表格（针对 `tabularx` 表格环境）左列文本的对齐方式。

对于正文纯文本段落，本质上是通过 `\raggedright` 命令控制的，如果不设置该命令，默认情况下就是两端对齐，设置该命令可实现左对齐。

```latex
\ifjustify\else\raggedright\fi
```

对于表格中文本的对齐控制中，右列的内容通常为时间或地点（可选），两者内容简短，不需要考虑换行的情况，同时为了美观也需要保持右对齐；而左列的内容通常为名称或描述（可选），存在文本内容较多的情况，需要考虑换行的情况，便涉及对齐方式的考虑。上面提到，表格环境被封装在第一层列表项命令中，只涉及 `tabular*` / `tabularx` 表格环境。

对于 `tabular*` 表格环境，列对齐参数有 `l/r/c`，但是它们不支持自动换行（只有在指定列宽时，列文本才会自动换行），故该表格环境只适用于文本内容较短的情况。对于文本内容较长需要考虑换行的情况，继续使用 `tabular*` 会让表格的边界溢出页面，建议使用 `tabularx` 环境。

对于 `tabularx` 表格环境，提供了一种弹性列类型 `X`，它会自动计算列宽，以实现文本的自动换行。`X` 列默认是两端对齐，为了实现左对齐并且具备像 `X` 那样拥有弹性列宽，模板中自定义了 `L` 列类型。

```latex
\newcolumntype{L}{>{\raggedright\arraybackslash}X}%
```

### 控制 vspace

垂直间距的控制是一个复杂的问题，需要使用者对模板命令非常熟悉。理想状态下，我们希望同一个文件中不同部分拥有一致的格式，其中垂直间距可通过 `\vspace` 命令灵活控制，不同模块之间可能存在间距叠加计算的场景，如果我们不能灵活设置或者忽略叠加场景，将会导致我们无法获得理想的格式输出。对于叠加情况，比如：

> 假设 `\resumeItemListEnd` 在最后设置了 `\vspace{-1mm}`，`\resumeItem` 之后没有设置 `\vspace`。如果我们此时设置 `\resumeItem` 之后 `\vspace{-0.5mm}`，在保证此命令的内容与 `\resumeSubHeading` 内容之间的间距不变，就需要将 `\resumeItemListEnd` 最后的 `\vspace{-1mm}` 修改为 `\vspace{-0.5mm}`，因为 $-1+0.5=-0.5$。

对于 `\resumeSubHeadingListEnd`、`\resumeItemListEnd`、`\resumeSubHeading`、`\resumeSubProject` 命令之后都设置了 `\vspace`。但是 `\resumeSubContent` 和 `\resumeItem` 命令之后没有设置 `\vspace`，但是设置了它们列表项之间的间距 `itemsep`，该命令与 `\vspace` 都可实现垂直间距的控制，但是 `itemsep` 不会让最后一个 `\item` 之后有间距调整。

章节内的间距：之所以 `\resumeSubContent` 和 `\resumeItem` 命令不设置 `\vspace` 而是使用 `itemsep` 是因为两者的内容通常作为最小的列表项。然而，`\resumeSubContent` 的内容有时不作为最小的列表项，即后面可能会有使用 `\resumeItemList` 环境的补充信息，所以，为了缩小 `\resumeSubContent` 和 `\resumeItem` 之间的垂直间距，建议在正文中使用 `\vspace` 命令而不是在模板命令中使用它。注意：在模板命令中使用 `\vspace` 很难统一`\resumeSubContent` 内容之间的垂直间距以及其和 `\resumeItem` 内容之间的垂直间距。正如上面的bullet point 4中使用 `\vspace{-7mm}`。

章节间的间距：每个章节最后的内容通常为 `\resumeItem` 命令的内容，但是有时也会是 `\resumeSubContent` 命令的内容。即使 `\resumeItem` 没有使用 `\vspace` 命令，但是它属于 `\resumeItemList` 环境，由于 `\resumeItemListEnd` 在最后使用了 `\vspace`，而 `\resumeSubContent` 没有使用 `\vspace` 命令。为了统一不同章节之间垂直间距，建议在 `\resumeSubHeadingList` 环境中，如果最后以 `\resumeSubContent` 列表项结尾，建议手动在 `\resumeSubHeadingListEnd` 之后添加 `\vspace{0.6mm}`。

![READMEsubheadingvspace](figure/READMEsubheadingvspace.png)

![READMEsubcontentvspace](figure/READMEsubcontentvspace.png)

### 控制 hspace

上面提到，第一层列表项命令封装的是一个 `tabular*` / `tabularx` 表格环境，因此为了实现表格左列文本的左边界与正文左边界对齐以及表格右列的右边界与正文右边界对齐，需要进行以下设置：

（1）列表项文本左边距设置为0：在 `itemize` 列表环境中通过设置参数 `leftmargin=0mm` 可实现列表文本的左边界与正文左边界对齐。注意：`leftmargin=*` 只会让列表项的标签 label 的左边界与正文文本的左边界对齐；标签左侧到包裹它环境左边界（对于一级列表为左页边距）的距离为 `\leftmargin`+`\itemindent`-`\labelsep`-`\labelwidth`。

```latex
\begin{itemize}[leftmargin=0mm,labelsep=2mm,itemsep=0mm,itemindent=0mm]
```

（2）表格宽度设置为行宽：在 `tabular*` / `tabularx` 表格环境中通过指定表格宽度为 `\linewidtn`/ `\textwidth` 可实现表格宽度与当前环境中的行宽/正文文本宽度一致。注意：有时 `\linewidth` 小于 `\textwidth`，取决于表格所处的环境。

（3）将表格的列间距和框线粗细设置为0：当存在表格环境时，除了表格内文本的宽度外，还需考虑列间距 `\tabcolsep` 和边框线的粗细`\arrayrulewidth`。注意：有些表格环境当不显示框线时，框线依然会占用宽度。

```latex
\setlength{\tabcolsep}{0in}
\setlength{\arrayrulewidth}{0in}
```

## 中文支持

此模板目前不支持编译中文，但如果需要编译中文，需要做以下修改：

在导言区导入宏包就可以使用 `pdfLaTeX` 编译中文了：

```latex
\usepackage[UTF8]{ctex}
```

如果对英文无需求，可将下面内容注释掉：

```latex
%\usepackage[T1]{fontenc}
%\usepackage{cfr-lm}
...
%% FONT OPTIONS
%\newcommand{\fontstylei}{\fontfamily{phv}\selectfont} % Helvetica-like (similar to Arial/Calibri)
%\newcommand{\fontstyleii}{\fontfamily{ptm}\selectfont} % Times-like (similar to Times New Roman)
%\newcommand{\fontstyleiii}{\fontfamily{ppl}\selectfont} % Palatino (elegant serif)
%\newcommand{\fontstyleiv}{\fontfamily{pbk}\selectfont} % Bookman (readable serif)
%\newcommand{\fontstylev}{\fontfamily{pag}\selectfont} % Avant Garde-like (similar to Trebuchet MS)
%\newcommand{\fontstylevi}{\fontfamily{qhv}\selectfont} % Quasi-Helvetica (another Arial/Calibri alternative)
%\newcommand{\fontstylevii}{\fontfamily{qtm}\selectfont} % Quasi-Times (another Times New Roman alternative)
%\newcommand{\fontstyleviii}{\fontfamily{qpl}\selectfont} % Quasi-Palatino (another elegant serif option)
%\newcommand{\fontstyleix}{\fontfamily{cmr}\selectfont} % Computer Modern Roman
%\newcommand{\fontstylex}{\fontfamily{cmss}\selectfont} % Computer Modern Sans Serif
%\newcommand{\fontstylexi}{\fontfamily{bch}\selectfont} % Charter (clean serif font)
...
%\fontstyleiii
```

