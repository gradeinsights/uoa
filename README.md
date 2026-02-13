# UoA Grade Insights

![status](https://img.shields.io/badge/status-live-brightgreen)
![data](https://img.shields.io/badge/data-UoA%20Official-blue)
![frontend](https://img.shields.io/badge/frontend-Vue.js-black)
![charts](https://img.shields.io/badge/charts-Chart.js-orange)
![style](https://img.shields.io/badge/style-TailwindCSS-38bdf8)
![ai](https://img.shields.io/badge/dev-chatbot%20driven-purple)

---

## UoA Grade Insights

UoA Grade Insights 是一个分析奥克兰大学全部课程通过率以及分数分布的网页。

---

## 起因

在使用coursespy.com查看大学课程评价的时候，我发现了其位于右上角的Grade Distribution功能，但该功能在coursespy.com上已经名存实亡，许多课程代码的信息全部为11%能看出明显的错误。通过该功能的介绍我搜索到了奥克兰大学的Student Grade Distributions Dashboard介绍，并通过[数据下载链接](https://microstrategy.auckland.ac.nz/MicroStrategyLibraryExternal/app/config/CFDF93E548564A87A831B59B5E0A5373/D75D071F6C4A184F1ED60DA65891FD4C/1FEE2DC6BB40D667F7AAABBA55D6E482/K53--K46)下载到我想要的数据拖入tableau进行一些基础的分析。但每次下载数据并进行相同操作的繁琐（为了减少在BI软件内的filter筛选，我一般只下载当时需要的数据）以及Mac端tableau糟糕的体验让我萌生了只做制作这个网站的想法。

---

## 过程

本网站使用Gemini+Claude进行开发，全程采用面向chatbot编程甚至没有vibe coding（下了codex之后我才知道我之前收的什么罪……）。使用ChatGPT生成网站PRD，由Gemini生成所有功能界面后使用Claude debug（虽然说最后也没有修改成功，最后人工干预强制sleep0.5秒……）

---

> ！！！以下内容由Gemini生成：

---

### 1. 数据解析与聚合 (Data Engineering)

这是项目的底层基石。

多维数据清洗：针对奥大官方导出的原始 CSV，利用 Python (Pandas) 进行自动化处理。我们不仅解决了 UTF-16 编码带来的读取难题，更关键的是对零散的成绩列（A+ 到 DNC）进行了语义化重新建模。

指标定义：将原始 headcount 转化为三个直观的 KPI 指标：Pass Rate（生存率）、A Range（优异率）及 Risk Rate（挂科风险），使数据从“原始记录”变成了“决策依据”。

静态分发策略：为了实现无后端部署，脚本将 1.5MB 的原始文件拆解为轻量级的 search-index.json（全局搜索索引）和按课划分的 course-data.json。这种“空间换时间”的策略保证了极高的前端加载速度。

---

### 2. 交互式视觉建模 (Visual Experience)

我们将 Apple 的视觉规范引入到了教务数据的展示中。

Bento Box 布局：采用便当盒式布局，将核心 KPI 放在视觉中心，让用户在进入页面的第一秒就能获取最关键的信息。

多维对比图表：使用 Chart.js 构建了双图表联动系统。

时间线趋势：通过纵向条形图展示该课在过去数年间的表现起伏。

等级分布：利用横向条形图展示某一特定学期的详细成绩梯度。

Glassmorphism 视觉：全站应用深色模式与毛玻璃特效，提升了工具的现代感和极客气息。

---

### 3. 学期优先级算法 (Logic Orchestration)

这是本项目最核心的逻辑创新，确保了数据的直观性。

时间序逻辑重组：官方数据中，年份与学期往往是零散的字符串。我们设计了一套权重算法，在前端实时对课程记录进行二次排序。

SS/S1/S2 排序引擎：通过自定义映射表（Summer=0, S1=1, S2=2），强制让 Summer School 永远排在同一年份的最前面。

语义化配色系统：为不同学期分配了专属色彩（橙、蓝、紫），并将这些颜色贯穿于条形图和 KPI 卡片中，用户无需阅读文字，仅凭颜色直觉就能分辨出是在查看哪一个学期的数据。

---

### 4. 技术栈 (Tech Stack)

数据引擎：Python (Pandas) —— 负责将复杂的 UTF-16 原始数据清洗、聚合并分块。

前端逻辑：Vue.js 3 —— 处理响应式状态与复杂的学期排序逻辑。

图形渲染：Chart.js —— 打造丝滑的条形图与联动动画。

样式框架：Tailwind CSS —— 快速构建精致的毛玻璃 UI。

---

## 技术

---

## 功能

网站对于成绩数据的分析基本上涵盖了我所有在tableau中对于成绩数据所在乎的点：

查看课程的通过率

查看课程的成绩分布

查看课程历年通过率的变化

查看历年单一学期查看课程的通过率变化

对比不同学期课程通过率变化

---

## 数据范围说明

！！！！本网站只包含semester制下的课程，并不包含任何quater制课程信息。
