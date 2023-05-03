[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![license: CC BY-NC-SA 4.0](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-lightgrey.svg)][license-url]

<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/wx-chevalier/AlgoDS-Notes">
    <img src="https://s2.ax1x.com/2020/01/06/lr21MT.png" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">AlgoDS-Notes</h3>

  <p align="center">
    数据结构与算法系列笔记
    <br />
    <a href="https://github.com/wx-chevalier/AlgoDS-Notes"><strong>在线阅读 >> </strong></a>
    <br />
    <br />
    <a href="https://github.com/wx-chevalier/AlgoDS-Notes">速览手册</a>
    ·
    <a href="https://github.com/wx-chevalier/AlgoDS-Notes/issues">Report Bug</a>
    ·
    <a href="https://github.com/wx-chevalier/AlgoDS-Notes/issues">参考资料</a>
  </p>
</p>

<!-- ABOUT THE PROJECT -->

# Introduction | 前言

> 本书的精排目录导航版请参考 [https://ng-tech.icu/books/Algorithm-Notes](https://ng-tech.icu/books/Algorithm-Notes)。

> “Bad programmers worry about the code. Good programmers worry about data structures and their relationships.”
> — Linus Torvalds, creator of Linux

数据结构与算法的思想适用于任何语言，开发者也应当能够使用他们日常工作的语言来快速实现常见的数据结构或者算法。本系列文章包含了 Java, JavaScript, Go, Rust, Python 等不同的语言实现，请参考关联仓库 [algorithm-snippets](https://github.com/wx-chevalier/algorithm-snippets) 浏览。

首先，我们在数据结构与算法知识脑图中了解本系列包含的关键知识脉络：

![数据结构与算法](https://assets.ng-tech.icu/item/20230418155844.png)

## 为什么需要算法？

玩算法的码农，打拼靠蓝条，像是法师。数学就等于蓝条最大值，数学差，魔法值不高，很快就到瓶颈了。外语影响回蓝速度。经验和智力加急速和穿透的。不玩算法的码农，像是战士，打拼靠血条，体质加生命，精神加生命回复，经验和敏捷加急速和破甲。

的确很少有写业务代码的时候会直接用上二叉树。但是真的没有吗？XML/DOM 是什么？是不是一棵树？为什么 DOM 可以和 XML 一一对应？因为 XML 序列化就是树的遍历的结果。能和 XML 对应，也就能跟 JSON 对应，因为两者都可以对应到树（只是表示逻辑上有些区别）。

一个业务系统里有任务（Task），任务有相应的执行计划（Plan），计划可以用子任务组成，子任务可以是基础任务，也可以通过 Plan 拆分成更多的子任务。这是什么？这不就是树吗？那么怎么存储？JSON 不就很好吗。怎么从 JSON 加载、再保存会 JSON？树的遍历。怎么计算任务总共需要多少个基础任务？树的遍历。怎么计算计划总共需要多少时间？树的遍历。

一个社交系统里，用户可以加好友，好友还有别的好友，这是什么？无向图。如果是知乎这样的关注系统呢？有向图。一个用户点了个赞，扩散到另一个用户至少要经过几次转发？最短路径。我要画一个小圈子里的人之间的关系图，怎么做？最小生成树。我要整理信息路径，看这批用户里哪些生产内容，哪些阅读内容，按什么次序传播，怎么做？拓扑排序。

## 经典算法与机器学习

当我们谈起算法这个词时，必然会想到数据结构中相关的经典算法与数据挖掘、机器学习、深度学习等[数据科学与人工智能](https://github.com/wx-chevalier/AIDL-Notes)中讨论的算法。算法（Algorithm），在数学（算学）和计算机科学之中，为任何良定义的具体计算步骤的一个序列，常用于计算、数据处理和自动推理。精确而言，算法是一个表示为有限长列表的有效方法，也即解决问题的一系列步骤。数据结构中的经典算法是对确定的数据 使用如数组，链表，队列，图等一系列存储结构进行存储，通过优化时间复杂度以及空间复杂度提高效率来对数据进行处理，得出问题答案的过程。而机器学习中的算法是一类从数据中运用数学工具自动分析获得规律，并利用规律对未知数据进行预测的算法，注重数据来源以及数据规律。

- 从目的做比较：经典算法是对确定的数据进行显而易见的操作，并注重效率(时间复杂度和空间复杂度)。例如排序。数据挖掘算法则是建立一个模式，学会对未知的数据进行预测或者分类。

- 从应用数学的深度以及广度做比较：经典算法主要涉及初等数学、简单概率论、简单离散数学等。数据挖掘主要涉及高等数学、概率论、线性代数、数理统计、微积分、运筹学、信息论、最优化方法等。

- 从评价标准做比较：经典算法主要考虑执行效率、时间复杂度、空间复杂度等维度。数据挖掘则主要考虑准确率、泛化能力、经验风险、结构风险等方面，例如正确率，召回率等。

- 从解决问题的种类做比较：经典算法主要是为了解决传统 CS 领域问题，譬如对数据进行组织并进行 CURD 操作。数据挖掘则面向预测，分类等未知问题，研究数据内在的规律。

# Nav | 关联导航

# About | 关于

<!-- CONTRIBUTING -->

## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<!-- ACKNOWLEDGEMENTS -->

## Acknowledgements

- [Awesome-Lists](https://github.com/wx-chevalier/Awesome-Lists): 📚 Guide to Galaxy, curated, worthy and up-to-date links/reading list for ITCS-Coding/Algorithm/SoftwareArchitecture/AI. 💫 ITCS-编程/算法/软件架构/人工智能等领域的文章/书籍/资料/项目链接精选。

- [Awesome-CS-Books](https://github.com/wx-chevalier/Awesome-CS-Books): :books: Awesome CS Books/Series(.pdf by git lfs) Warehouse for Geeks, ProgrammingLanguage, SoftwareEngineering, Web, AI, ServerSideApplication, Infrastructure, FE etc. :dizzy: 优秀计算机科学与技术领域相关的书籍归档。

- https://github.com/trekhleb/javascript-algorithms

- https://zq99299.github.io/dsalg-tutorial/

## Copyright & More | 延伸阅读

笔者所有文章遵循[知识共享 署名 - 非商业性使用 - 禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)，欢迎转载，尊重版权。您还可以前往 [NGTE Books](https://ng-tech.icu/books-gallery/) 主页浏览包含知识体系、编程语言、软件工程、模式与架构、Web 与大前端、服务端开发实践与工程架构、分布式基础架构、人工智能与深度学习、产品运营与创业等多类目的书籍列表：

[![NGTE Books](https://s2.ax1x.com/2020/01/18/19uXtI.png)](https://ng-tech.icu/books-gallery/)

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[contributors-shield]: https://img.shields.io/github/contributors/wx-chevalier/AlgoDS-Notes.svg?style=flat-square
[contributors-url]: https://github.com/wx-chevalier/AlgoDS-Notes/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/wx-chevalier/AlgoDS-Notes.svg?style=flat-square
[forks-url]: https://github.com/wx-chevalier/AlgoDS-Notes/network/members
[stars-shield]: https://img.shields.io/github/stars/wx-chevalier/AlgoDS-Notes.svg?style=flat-square
[stars-url]: https://github.com/wx-chevalier/AlgoDS-Notes/stargazers
[issues-shield]: https://img.shields.io/github/issues/wx-chevalier/AlgoDS-Notes.svg?style=flat-square
[issues-url]: https://github.com/wx-chevalier/AlgoDS-Notes/issues
[license-shield]: https://img.shields.io/github/license/wx-chevalier/AlgoDS-Notes.svg?style=flat-square
[license-url]: https://github.com/wx-chevalier/AlgoDS-Notes/blob/master/LICENSE.txt

# Links
