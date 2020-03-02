---
title: 统计力学笔记——系综的概念
tags:
  - Statistical Mechanics
  - Physical Chemistry
Categories: 
  - Physics
date: 2019-03-22 21:50:44
---


# 何为「系综」？

简单来说 [系综][ensemble-wikipedia] (Ensemble) 就是一个宏观态对应所有可能微观态的集合。 $\newcommand{\dif}{\mathop{}\mathrm{d}}$ 

对于宏观态，我们研究对象所含有的粒子的数量级在 10<sup>23</sup> 左右（即 1 mol 物质所含粒子的数量），而这个宏观态的宏观量（如压力 $p​$ ，温度 $T​$ ，体积 $V​$ ，粒子数目 $N​$ ，以及内能 $E​$ 等）可以保持不变，但是给定一个宏观态，固定其中某些宏观量后，却可能有极大量微观态与之对应。为了研究方便，我们给这些对应于同一个宏观态的微观态的集合起名字叫做「系综」。

为了简化模型，我们使用理想气体分子作为研究的粒子，通常研究的系综体系有：

- 微正则系综（[Microcanonical Ensemble](https://en.wikipedia.org/wiki/Microcanonical_ensemble)） $\Longrightarrow$ 等粒子数、等容、等能量的体系，即孤立体系（与外界完全隔离，与环境没有物质交换、没有做功、甚至没有能量交换的体系）所对应的系综，热力学参量为 $N$ 、 $V$ 、 $E$ ；
- 正则系综（[Canonical Ensemble](https://en.wikipedia.org/wiki/Canonical_ensemble)） $\Longrightarrow$  等粒子数、等容、等温的体系，即封闭体系（容器体积封闭固定无法对外界做功，与外界没有粒子交换，但是容器壁可以传热，体系与外界仅有能量交换，**且体系温度保持恒定**）所对应的系综，热力学参量为 $N$ ，$V$ ， $T$ ；
- 巨正则系综（[Grand Canonical Ensemble](https://en.wikipedia.org/wiki/Grand_canonical_ensemble)） $\Longrightarrow$ 等化学势、等容、等温的体系，即开放体系（容器体积固定，但并不封闭，体系内外有物质交换和能量交换，但体系的化学势与温度始终保持恒定）所对应的系综，热力学参量为 $\mu$ ， $V$ ， $T$ ；
- 等温等压系综（[Isothermal-Isobaric Ensemble](https://en.wikipedia.org/wiki/Isothermal–isobaric_ensemble)） $\Longrightarrow$ 等粒子数、等温、等压的体系（如一个自由的密闭活塞）所对应的系综，热力学参量为 $N$ ， $T$ ， $p$ 。

PS. 

1. 个人认为带「正则」的名字翻译得……很让人摸不着头脑，无法从名字知道系综的参量情况，可能是这个术语本身在命名时就比较随意吧。不过命名不是重点，其内在的逻辑才是应该关心的；
2. 其中的热力学「参量」表示我们给定体系的热力学量。类似于程序设计中「函数」的「输入参数」；
3. 给定上面的这些热力学「参量」并<span style="color:red">**不代表体系在任意时刻保持这些热力学量恒定**</span>。给定它们表示体系在宏观上保持这些热力学量不变，但在微观角度看，这些热力学量是在**时刻不停地涨落**，但是它们整体表现出的**平均值**是恒定不变的，这才是给定热力学参量的真正含义；
4. 为什么要定义系综？其实刚开始时人们研究宏观态时是只研究其对应的某一个微观态在一段时间内的演化过程，但是研究一个态需要知道这个态的初始参数（所有粒子的初速度、位置信息等），每次研究都要设置这个参数，很麻烦，并且很多时候人们并不知道这些参数；而且在很短时间内这个态经过演化已经经历过了所有可能的等价态，使得结果与初始参数的关系并不大。于是人们将<span style="color:red">**研究一个体系在一段时间的演化**</span>转化成<span style="color:red">**研究多个等价体系在同一时间内的统计分布**</span>，这样并不需要定义其中某个微观态的初始参数，而是给出这些微观态平均表现出的热力学性质，然后研究这些态的热力学量在统计上的分布就能达到想要的效果，系综的概念应运而生。

# [热力学基本关系式](https://en.wikipedia.org/wiki/Fundamental_thermodynamic_relation)

只有一个公式：
$$
\dif E = T\dif S - p\dif V + \mu\dif N
\label{eq:thermal_relation}
$$
式 $\eqref{eq:thermal_relation}​$ 中：

- $E$ ——内能，J；
- $T​$ ——温度，K；
- $S​$ ——熵，J/K；
- $p$ ——压力，Pa；
- $V$ ——体积，m<sup>3</sup>；
- $\mu$ ——化学势，J；
- $N$ ——粒子数目。

仔细观察这个公式，可以发现公式每一项的量纲都是能量： $E$ ， $TS$ ， $pV$ ， $\mu N$ 单位都是 J 。

并且，等式右边微分符号左边都是[强度量](https://en.wikipedia.org/wiki/Intensive_and_extensive_properties#Intensive_properties)—— $T$ ， $p$ ， $\mu$ ；被微分的量都是[广延量](https://en.wikipedia.org/wiki/Intensive_and_extensive_properties#Extensive_properties) ——$S$ ， $V$ ， $N$ 。

强度量不随物质的多少或系统的大小而改变，比如两份温度同样是 300 K 的气体混合在一块，并不意味着它们总体的温度是各自温度的加和，这样的相加是没有意义的，所以温度是强度量；但如果是两份体积为 1 L 的气体混合在一块，它们的总体积就可以是 2 L，那么体积就是广延量；

再次审视这个公式，我们发现等号右边的每一项的两个量都是一对共轭量，这一点在后面的公式推导中很重要。

## $\beta$ 的定义及对热力学基本关系式的变形

为了后面研究的方便，我们定义温度与玻尔兹曼常数乘积的倒数为 $\beta$ ：
$$
\beta = \frac{1}{kT}
$$
其中 $k$ 是[玻尔兹曼常数](https://en.wikipedia.org/wiki/Boltzmann_constant)， $k = \frac{R}{N_A} = \frac{8.314}{6.02\times 10^{23}} = 1.38064852\times 10^{−23}$ 单位是 J/K。

接着我们对式 $\eqref{eq:thermal_relation}$ 进行变形：
$$
\begin{align}\notag
	\dif E &={} T\dif S - p\dif V + \mu\dif N \\ \notag
    \frac{1}{T} \dif E &={} \dif S - \frac{p}{T} \dif V + \frac{\mu}{T}\dif N \\ \notag
    \frac{1}{kT} \dif E &={} \frac{1}{k}\dif S - \frac{p}{kT} \dif V + \frac{\mu}{kT}\dif N \\ \notag
    \beta \dif E &={} \frac{1}{k}\dif S -\beta p\dif V + \beta\mu \dif N\\ \notag
    \frac{1}{k}\dif S &={} \beta \dif E +\beta p\dif V - \beta\mu \dif N\\
   	\label{eq:thermal_relation_derivative}
\end{align}
$$

这时要请出一位热力学历史上的一位重要人物——玻尔兹曼，在他的墓碑上写着一个著名的公式

![玻尔兹曼的墓碑](boltzmann_tombstone.jpg)
$$
S = k\ln \Omega \label{eq:boltzmann}
$$
式 $\eqref{eq:boltzmann}$ 中，$k$ 为玻尔兹曼常数， $\Omega$ 是体系的状态数并且无量纲， $S$ 就是体系的熵，将式 $\eqref{eq:boltzmann}$ 代入 $\eqref{eq:thermal_relation_derivative}$ 中
$$
\begin{align} \notag
\frac{1}{k}\dif k\ln \Omega &={} \beta \dif E + \beta p\dif V - \beta \mu\dif N \\
\dif \ln \Omega &={} \beta \dif E + \beta p\dif V - \beta \mu\dif N \label{eq:eq5}
\end{align}
$$
式  $\eqref{eq:eq5}$ 在后面的推导中会**非常有用**。

# 两个假设

和量子力学存在五个假设类似，统计力学也是在几个假设的前提下延伸的，当然这里的假设数量比量子力学要少很多——只有两个，它们分别是：

1. [遍历性假设](https://en.wikipedia.org/wiki/Ergodic_hypothesis)
  当时间足够长，所有的粒子都能遍历整个体系，体系不存在对于所有状态 $\nu$ ，都使得 $P_{\nu} = 0$ 的区域。 

   $P_{\nu}$ 是体系某区域出现粒子的概率， $\nu$ 表示体系所处的状态。

2. [等几率假设](https://en.wikipedia.org/wiki/A_priori_probability)

   对于微正则系综（孤立体系，$N$ $V$ $E$ 为给定参数），每个状态出现的概率相等。这里我们使用 $\varOmega(N, V, E)$ 表示体系的总状态数，则每个状态出现的概率
   $$
   P_{\nu} = \frac{1}{\varOmega(N, V, E)}
   $$

# 态密度与简并度

1. 态密度 （Density of States, DOS）

   态密度被定义为体系能量在 $E \sim E + \delta E$ 之间的状态数：
   $$
   \bar{\varOmega}(N, V, E) := \frac{\Delta \varOmega(N, V, E)}{\delta E}
   $$

2. 简并度 （Degree of degeneracy）

   当 $\delta E \to 0$ 时，态密度即为简并度：
   $$
   g := \frac{\dif \varOmega(N, V, E)}{\dif E}
   $$

**个人认为**：简并度与态密度的区别是简并度是对于一个能级而言，态密度是对一段能级而言。

​	如果一个体系的能级是连续分布的，那么求其某一个点上的简并度，是不合适的，就像在一个二维图形上取一条线求这条线的面积一样，求出来的值是零；而如果一个体系的能级是量子化的，此时再求简并度就很合理，因为在两个分立能级之间没有态，态全部「确切」地分布在各个能级上，因此数/计算某个能级上有多少个态是可行的。

# 参考资料 & 致谢

[ensemble-wikipedia]: https://en.wikipedia.org/wiki/Statistical_ensemble_(mathematical_physics)

Wikipedia: 已在超链接中给出；

Roommate: Li Shihao；

《李政道讲义：统计力学》；

