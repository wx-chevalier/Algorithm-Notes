## 搜索关键词智能提示 suggestion

### 题目详情

百度搜索框中，输入“北京”，搜索框下面会以北京为前缀，展示“北京爱情故事”、“北京公交”、“北京医院”等等搜索词，输入“[结构之](http://www.baidu.com/s?wd=结构之&rsv_bp=0&ch=&tn=baidu&bar=&rsv_spt=3&ie=utf-8&rsv_sug3=8&rsv_sug=0&rsv_sug4=1075&rsv_sug1=3&inputT=2559)”，会提示“结构之法”，“结构之法 算法之道”等搜索词。
请问，如何设计此系统，使得空间和时间复杂度尽量低。

![](https://ngte-superbed.oss-cn-beijing.aliyuncs.com/book/The-Art-Of-Programming/images/36~37/36.1.jpg)

### 分析与解法

本题来源于去年 2012 年百度的一套实习生笔试题中的系统设计题（_为尊重原题，本章主要使用百度搜索引擎展开论述，而不是 google 等其它搜索引擎，但原理不会差太多。然脱离本题，平时搜的时候，鼓励用..._），题目比较开放，考察的目的在于看应聘者解决问题的思路是否清晰明确，其次便是看能考虑到多少细节。

我去年整理此题的时候，曾简单解析过，提出的方法是：

- 直接上**Trie 树**「Trie 树的介绍见：从 Trie 树（字典树）谈到后缀树」 + **TOP K**「hashmap+堆，hashmap+堆 统计出如 10 个近似的热词，也就是说，只存与关键词近似的比如 10 个热词」

方法就是这样子的：Trie 树+TOP K 算法，但在实际中，真的只要 Trie 树 + TOP K 算法就够了么，有什么需要考虑的细节？OK，请看下文娓娓道来。

#### 解法一、Trie 树 + TOP K

##### 步骤一、trie 树存储前缀后缀

若看过博客内这篇[介绍 Trie 树和后缀树的文章](http://blog.csdn.net/v_july_v/article/details/6897097)的话，应该就能对 trie 树有个大致的了解，为示本文完整性，引用下原文内容，如下：

**1.1、什么是 Trie 树**

Trie 树，即字典树，又称单词查找树或键树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：最大限度地减少无谓的字符串比较，查询效率往往比哈希表高。

Trie 的核心思想是空间换时间。利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

它有 3 个基本性质：

1. 根节点不包含字符，除根节点外每一个节点都只包含一个字符。
2. 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。
3. 每个节点的所有子节点包含的字符都不相同。

**1.2、树的构建**

举个在网上流传颇广的例子，如下：

题目：给你 100000 个长度不超过 10 的单词。对于每一个单词，我们要判断他出没出现过，如果出现了，求第一次出现在第几个位置。

分析：这题当然可以用 hash 来解决，但是本文重点介绍的是 trie 树，因为在某些方面它的用途更大。比如说对于某一个单词，我们要询问它的前缀是否出现过。这样 hash 就不好搞了，而用 trie 还是很简单。

现在回到例子中，如果我们用最傻的方法，对于每一个单词，我们都要去查找它前面的单词中是否有它。那么这个算法的复杂度就是 O(n^2)。（字符串比较的复杂度是否以 strcmp 次数计算更妥当，当此处为了讨论简化了细节）。显然对于 100000 的范围难以接受。现在我们换个思路想。假设我要查询的单词是 abcd，那么在他前面的单词中，以 b，c，d，f 之类开头的我显然不必考虑。而只要找以 a 开头的中是否存在 abcd 就可以了。同样的，在以 a 开头中的单词中，我们只要考虑以 b 作为第二个字母的，一次次缩小范围和提高针对性，这样一个树的模型就渐渐清晰了。

好比假设有 b，abc，abd，bcd，abcd，efg，hii 这 6 个单词，我们构建的树就是如下图这样的：

![](https://ngte-superbed.oss-cn-beijing.aliyuncs.com/book/The-Art-Of-Programming/images/36~37/36.2.jpg)

当时第一次看到这幅图的时候，便立马感到此树之不凡构造了。单单从上幅图便可窥知一二，好比大海搜人，立马就能确定东南西北中的到底哪个方位，如此迅速缩小查找的范围和提高查找的针对性，不失为一创举。

ok，如上图所示，对于每一个节点，从根遍历到他的过程就是一个单词，如果这个节点被标记为红色，就表示这个单词存在，否则不存在。

那么，对于一个单词，我只要顺着他从根走到对应的节点，再看这个节点是否被标记为红色就可以知道它是否出现过了。把这个节点标记为红色，就相当于插入了这个单词。

借用上面的图，当用户输入前缀 a 的时候，搜索框可能会展示以 a 为前缀的“abcd”，“abd”等关键词，再当用户输入前缀 b 的时候，搜索框下面可能会提示以 b 为前缀的“bcd”等关键词，如此，实现搜索引擎智能提示 suggestion 的第一个步骤便清晰了，即用 trie 树存储大量字符串，当前缀固定时，存储相对来说比较热的后缀。那又如何统计热词呢？请看下文步骤二、TOP K 算法统计热词。

##### 步骤二、TOP K 算法统计热词

当每个搜索引擎输入一个前缀时，下面它只会展示 0~10 个候选词，但若是碰到那种候选词很多的时候，如何取舍，哪些展示在前面，哪些展示在后面？这就是一个搜索热度的问题。

如本题描述所说，在去年的这个时候，当我在搜索框内搜索“北京”时，它下面会提示以“北京”为前缀的诸如“北京爱情故事”，“北京公交”，“北京医院”，且“ 北京爱情故事”展示在第一个：

![](https://ngte-superbed.oss-cn-beijing.aliyuncs.com/book/The-Art-Of-Programming/images/36~37/36.3.jpg)

为何输入“北京”，会首先提示“北京爱情故事”呢？因为去年的这个时候，正是《北京爱情故事》这部电视剧上映正火的时候（其上映日期为 2012 年 1 月 8 日，火了至少一年），那个时候大家都一个劲的搜索这部电视剧的相关信息，当 10 个人中输入“北京”后，其中有 8 个人会继续敲入“爱情故事”（连起来就是“北京爱情故事”）的时候，搜索引擎对此当然不会无动于衷。

也就是说，搜索引擎知道了这个时间段，大家都在疯狂查找北京爱情故事，故当用户输入以“北京”为前缀的时候，搜索引擎猜测用户有 80%的机率是要查找“北京爱情故事”，故把“北京爱情故事”在下面提示出来，并放在第一个位置上。

但为何今年这个时候再次搜索“北京”的时候，它展示出来的词不同了呢？

原因在于随着时间变化，人们对《北京爱情故事》这部电视剧的关注度逐渐下降，与此同时，又出现了新的热词，或新的电影，故现在虽然同样是输入“北京”，后面提示的词也相应跟着起了变化。那解决这个问题的办法是什么呢？如开头所说：定期分析某段时间内的人们搜索的关键词，统计出搜索次数比较多的热词，继而当用户输入某个前缀时，优先展示热词。

故说白了，这个问题的第二个步骤便是统计热词，我们把统计热词的方法称为 TOP K 算法，此算法的应用场景便是[此文](http://blog.csdn.net/v_july_v/article/details/7382693)中的第 2 个问题，再次原文引用：

**寻找热门查询，300 万个查询字符串中统计最热门的 10 个查询**

原题：搜索引擎会通过日志文件把用户每次检索使用的所有检索串都记录下来，每个查询串的长度为 1-255 字节。假设目前有一千万个记录（这些查询串的重复度比较高，虽然总数是 1 千万，但如果除去重复后，不超过 3 百万个。一个查询串的重复度越高，说明查询它的用户越多，也就是越热门），请你统计最热门的 10 个查询串，要求使用的内存不能超过 1G。

解答：由上面第 1 题，我们知道，数据大则划为小的，如一亿个 Ip 求 Top 10，可先将 Ip 地址使用 Ip2long 函数（各种语言或库皆提供本函数）转为长整型 Ipld 后，再按 Ipld%1000 将 ip 分到 1000 个小文件中去，并保证一种 ip 只出现在一个文件中，再对每个小文件中的 ip 进行 hashmap 计数统计并按数量排序，最后归并或者最小堆依次处理每个小文件的 top10 以得到最后的结果。（小提示：常用的 Ip hash 还有一种基于 bit 的方法，聪明的你一定想到了吧^\_^）

但如果数据规模本身就比较小，能一次性装入内存呢？比如这第 2 题，虽然有一千万个 Query，但是由于重复度比较高，因此事实上只有 300 万的 Query，每个 Query255Byte，因此我们可以考虑把他们都放进内存中去（300 万个字符串假设没有重复，都是最大长度，那么最多占用内存 3M\*1K/4=0.75G（实际占用会略大一点，因具体实现而异）。所以可以将所有字符串都存放在内存中进行处理），而现在只是需要一个合适的数据结构，在这里，HashTable 绝对是我们优先的选择。

所以对数据量较少（可以全部放入内存的时候）我们放弃分而治之+hash 映射的步骤，直接上 hash 统计，然后排序。针对此类典型的 TOP K 问题，采取的对策往往是：hashmap + 堆。如下所示：

1. **hashmap 统计：**先对这批海量数据预处理。具体方法是：维护一个 Key 为 Query 字串，Value 为该 Query 出现次数的 HashTable，即 hash_map(Query，Value)，每次读取一个 Query，如果该字串不在 Table 中，那么加入该字串，并且将 Value 值设为 1；如果该字串在 Table 中，那么将该字串的计数加一即可。最终我们在 O(N)的时间复杂度内用 Hash 表完成了统计；
2. **堆排序：**第二步、借助堆这个数据结构，找出 Top K，时间复杂度为 N‘logK。即借助堆结构，我们可以在对数量级的时间内查找和调整。因此，维护一个 K(该题目中是 10)大小的小根堆，然后遍历 300 万的 Query 对应的计数，分别和根元素进行对比。所以，我们最终的时间复杂度是：O（N） + N' \* O（logK），（N 为 1000 万，N’为 300 万）。

别忘了这篇文章中所述的堆排序思路：‘维护 k 个元素的最小堆，即用容量为 k 的最小堆存储最先遍历到的 k 个数，并假设它们即是最大的 k 个数，建堆费时 O（k），并调整堆(费时 O（logk）)后，有 k1>k2>...kmin（kmin 设为小顶堆中最小元素）。继续遍历数列，每次遍历一个元素 x，与堆顶元素比较，若 x>kmin，则更新堆（x 入堆，用时 logk），否则不更新堆。这样下来，总费时 O（k\*logk+（n-k）\*logk）=O（n\*logk）。此方法得益于在堆中，查找等各项操作时间复杂度均为 logk。’--第三章续、Top K 算法问题的实现。

当然，你也可以采用 trie 树，关键字域存该查询串出现的次数，没有出现为 0。最后用 10 个元素的最小推来对出现频率进行排序。

相信，如此，也就不难理解开头所提出的方法了：Trie 树+ TOP K「hashmap+堆，hashmap+堆 统计出如 10 个近似的热词，也就是说，只存与关键词近似的比如 10 个热词」。

而且你以后就可以告诉你身边的伙伴们，为何输入“结构之”，会提示出来一堆以“结构之”为前缀的词了：

![](https://ngte-superbed.oss-cn-beijing.aliyuncs.com/book/The-Art-Of-Programming/images/36~37/36.4.jpg)

方法貌似成型了，但有哪些需要注意的细节呢？如@江申\_Johnson 所说：“实际工作里，比如当前缀很短的时候，候选词很多的时候，查询和排序性能可能有问题，也许可以加一层索引 trie（这层索引可以只索引频率高于某一个阈值的词，很短的时候查这个就可以了。数量不够的话再去查索引了全部词的 trie 树）；而且有时候不能根据 query 频率来排，而要引导用户输入信息量更全面的 query，或者或不仅仅是前缀匹配这么简单。”

当然，在实际的工程中，排序的依据还有很多，例如对于突发的热点问题，虽然之前用户没有输入过，但是却能排在最前面，是因为在系统内部有一些相应的模块进行了所谓的“调权”。此外，上面我们谈到 trie 的时候，都采取了简化的模型，即 trie 通常只是针对英语单词而言的，如果是中文，会有一个严重的问题：），你想到了吗？（小提示：英语和中文的基本字符，差别是不是很大？）当然，如果考虑到我们喜欢用拼音来表述中文，这个问题不是没有解决方法的^\_^。

此外，在工程实现的时候，前端往往采用了 ajax 技术来保障速度上的优势，不然等用户输入完毕了咱的推荐词还在慢悠悠的传输中，那也就没有多少意义了。由于前端讨论和咱们这里的讨论关系不大，感兴趣的朋友可以参考相关资料。

###扩展阅读

除了上文提到的 trie 树，三叉树或许也是一个不错的解决方案：[点击此处](http://igoro.com/archive/efficient-auto-complete-with-a-ternary-search-tree/)。此外，StackOverflow 上也有两个讨论帖子，大家可以看看：[帖子 1](http://stackoverflow.com/questions/2901831/algorithm-for-autocomplete)，[帖子 2](http://stackoverflow.com/questions/1783652/what-is-the-best-autocomplete-suggest-algorithm-datastructure-c-c)。
