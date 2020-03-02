---
title: 物理化学复习札记（一）：理想气体热力学量求解总结
date: 2017-06-26 17:07:41
tags:
    - 物理化学
    - 化学
categories: 
    - PhysicalChemistry
---


**这是这个系列的第一篇，也是第一次写这种类型的札记，如果有纰漏，还请在评论区指出，谢谢各位！**

## 热力学量说明
物理化学中，热力学主要研究7个热力学量，这7个量如下：

1. $U$，系统的热力学能，或称内能，属于状态函数，$U=f(T)$，无法求出$U$的值，但可以求出$\Delta U$的值，单位为J；
2. $Q$，系统与环境交换的热量，当系统放热时，$Q$为负值；系统吸热，则$Q$为正值，单位为J；
3. $W$，环境对系统所做的功，分为体积功和非体积功，单位为J；
4. $H$，焓，定义为 $H=U+pV$，无实际含义，仅为计算方便而产生，属于状态函数，$H=f(T)$，与$U$一样，无法求出$H$的值，但可以求出$\Delta H$的值，单位为J；
5. $S$，大名鼎鼎的熵，定义为$S=\int_A^B (\dfrac{\delta Q}{T})_R$，属于状态函数，处于绝对零度时完美晶体的熵为0，单位J$\cdot $K$^{-1}$
6. $A$，亥姆霍兹自由能，定义为$A=U-TS$，为状态函数，无法求出$A$的值，但可以求出$\Delta A$的值，单位为J；
7. $G$，吉布斯自由能，定义为$G=H-TS$，为状态函数，无法求出$G$的值，但可以求出$\Delta G$的值，单位为J；

以上说明中，状态函数是指当系统状态一定时，其值即确定，亦即在某个过程中其变化量$\Delta f$只与始末状态有关，与过程细节无关。这个性质**非常重要**，物理化学常见的设计过程求解热力学量的理论基础即在于此。

## 热力学四大定律

是的，热力学有四大定律，分别为热力学第零定律、热力学第一定律、热力学第二定律和热力学第三定律。

### 热力学第零定律
**如果两个系统分别与处于确定状态的第三个系统达到热平衡，则这两个系统彼此也将处于热平衡。**

这个定律定义了温度的概念，当两个系统相互接触处于热平衡后，它们的性质不再变化，我们称之具有相同的温度。而定义中提到的第三系统为温度计的产生奠定了理论基础。

### 热力学第一定律
**在变化过程中，系统的热力学能变化量表示为系统和环境的热交换量与外界对系统所做功的总和。**
写成公式即为 $ \Delta U = Q + W $，
或者写成微分形式$ \mathrm{d}U = \delta Q + \delta W $

### 热力学第二定律

文字可表述为：

> 克劳修斯表述：不可能把热从低温物体传到高温物体，而不引起其他变化；
> 开尔文表述：不可能从单一热源取出热使之完全变为功，而不发生其他变化。

数学形式：

- 克劳修斯不等式表述形式：$ \Delta S_{A\to B}-\sum_A^B \dfrac{\delta Q}{T} \geq 0 $
- 常用表述形式：$ \mathrm{d}S - \dfrac{\delta Q}{T} \geq 0 $


上式表明，在一个隔离系统中，熵永不减少。 隔离系统是绝热的，即$\delta Q=0$，因此$\mathrm{d}S_{\mathrm{iso}} \geq 0$。


### 热力学第三定律
可以表述为**在温度趋于热力学温度0K的等温过程中，系统的熵值不变 **，也可以表述为“**在0K时，任何完美晶体的熵等于零**”，还可以表述为”**绝对零度不可能达到**“。


## 特殊过程的热力学量求值

**本节所有过程均为$p$,$V$,$T$过程，不考虑非体积功！**

常见的变化过程（均为理想气体的变化过程）有：
1. 等温过程：系统由状态1变到状态2，变化过程中以及始态和终态的温度不变，**且等于环境温度**；
2. 等压过程：系统在变化过程中，始态和终态压力相等，且等于环境压力；
3. 等容过程：系统在变化过程中保持体积不变。在刚性容器中发生的变化一般是等容过程；
4. 绝热过程：系统在变化过程中与环境之间没有热的交换，或者是由于有绝热壁的存在，或者是因为变化太快而与环境来不及热交换，或者热交换量极少可近似看作是绝热过程；
5. 环状过程：系统从始态出发，经过一系列变化后又回到了原来状态。经此过程，所有状态函数的变化量都为零。

另外，自由膨胀过程可以看为外压恒为0的等压膨胀过程；节流膨胀过程属于实际气体的不可逆过程，不在本札记讨论范围内。

### 自由膨胀过程
此过程不可逆，可看作外压恒为零的等压过程。

- $\Delta U=0$，由`Gay-Lussac-Joule`实验得出，而$U=f(T)$，故此过程中气体$T$不变；
- $W=0$，显然，自由膨胀时外压恒为零，也无非体积功，故$W=0$；
- $Q=0$，由热力学第一定律，$\Delta U=Q+W$，式中$\Delta U=0$，$W=0$，因此$Q=0$；
- $\Delta H=0$，$H$是状态函数，由上文气体$T$不变可以推出$H$不变。 另外也可由公式$\Delta H=\Delta U + \Delta(pV)$，$pV=nRT$（n、T不变，所以$pV$不变）推出$\Delta H=0$；
- $\Delta S>0$，显然该过程不可逆，其对应的可逆过程为等温可逆膨胀，计算得$\Delta S=\int\dfrac{\delta Q}{T} = \dfrac{Q}{T} = nR\ln\dfrac{V_2}{V_1} = -nR\ln\dfrac{p_2}{p_1}$；
- $\Delta G<0​$，由公式$\Delta G=\Delta H-\Delta (TS)​$，并且$\Delta H=0​$、$T​$不变得出$\Delta G=-T\Delta S​$；
- $\Delta A<0$，与计算$\Delta G$的过程类似，$\Delta A=-T\Delta S$。

### 等温可逆过程
此过程满足 $pV=nRT=\mathrm{Constant}$。

- $\Delta U=0$，$U$为状态函数，等温过程系统温度不变，因此$U$不变、$\Delta U=0$；
- $W$，由公式$W=-\int_{V_1}^{V_2}p_{\mathrm{e}}\mathrm{d}V$，且$p_e=p=\dfrac{nRT}{V}$（可逆过程中环境压力始终近似于系统压力），所以$W=-nRT\int_{V_1}^{V^2} \dfrac{1}{V} \mathrm{d}V=-nRT\ln\dfrac{V_2}{V_1}$；此外，若已知系统的始末压力，也可以将前式变换为$W=nRT\dfrac{p_1}{p_2}$
- $Q$，由热力学第一定律，$\Delta U=W+Q$，得$Q=-W=nRT\ln\dfrac{V_2}{V_1}$；
- $\Delta H$，与$U$类似，为状态函数，$\Delta H=0$；
- $\Delta S$，此过程已经为可逆过程，不必再设计可逆过程。因此$\Delta S=\int\dfrac{\delta Q}{T} = \dfrac{Q}{T} = nR\ln\dfrac{V_2}{V_1} = -nR\ln\dfrac{p_2}{p_1}$；
- $\Delta G$，直接代入公式$\Delta G=\Delta H-\Delta(TS)$，得$\Delta G=-nRT\ln\dfrac{V_2}{V_1}=nRT\ln\dfrac{p_2}{p_1}=W $；
- $\Delta A$，同$\Delta G$，得到$\Delta A=\Delta G=W=-nRT\ln\dfrac{V_2}{V_1}=nRT\ln\dfrac{p_2}{p_1} $。

### 等压过程
此过程满足 $\dfrac{nRT}{V}=\text{Constant}$

- $\Delta U$，此时$\Delta U\neq 0$，应用公式$\Delta U=Q_{\mathrm{V}}=nC_{\mathrm{V,m}} (T_2 - T_1)=\dfrac{p}{R}C_{\mathrm{V,m}} (V_2-V_1)$计算。单原子分子理想气体的$C_\mathrm{V,m}=\dfrac{3}{2}R$。
- $W​$，显然 $W=-\int_{V_1}^{V_2}p_e\mathrm{d}V=p(V_2 - V_1)​$；
- $Q$，等压过程中的热交换量用$Q_\mathrm{p}$表示，并且$Q_\mathrm{p}=nC_\mathrm{p,m}\Delta T$或者使用$C_\mathrm{p,m}=C_\mathrm{V,m}+R$以及$Q=\Delta U-W$求解。
- $\Delta H$，直接使用公式$\Delta H = \Delta U + \Delta(pV) = nC_\mathrm{P,m}(T_2-T_1)$
- $\Delta S$，等压过程对应的可逆过程为物体的可逆加热或冷却，有$\delta Q_R=C\mathrm{d}T=nC_\mathrm{p,m}\mathrm{d}T$，因此，$\Delta S = \int_{T_A}^{T_B} \dfrac{nC_\mathrm{p,m}}{T}\mathrm{d}T=nC_\mathrm{p,m}\ln\dfrac{T_{B}}{T_{A}} $，根据公式$pV=nRT$可以求出$T$，这里不再对式子进行展开；
- $\Delta G$，直接代入式子$\Delta G=\Delta H-\Delta(TS)$即可（题目会给出气体的标准摩尔熵，结合上面的$\Delta S$可以使用$\Delta (TS)=T_2 n(S_m+\Delta S) - T_1 nS_m$求出$\Delta G$）；
- $\Delta A$，同$\Delta G$，直接代入$\Delta A=\Delta U-\Delta (TS)$求解。

### 等容过程
此过程满足$\dfrac{nRT}{p}=\mathrm{Constant}$。

- $\Delta U$，与等压过程一样，直接使用公式$\Delta U=Q_\mathrm{V}=nC_\mathrm{V,m}\Delta T$。单原子分子理想气体的$C_\mathrm{V,m}=\dfrac{3}{2}R $；
- $W$，因为体积无变化，$\mathrm{d}V=0$，故$W=\int_{V_1}^{V_2} p\mathrm{d}V=0$；
- $Q$，等容过程的热交换量也成为恒容热$Q_\mathrm{V}$，$Q_\mathrm{V}=nC_\mathrm{V,m}\Delta T$；
- $\Delta H$，可以结合式子$pV=nRT$求出$\Delta (pV)=nR\Delta T$，最后代入$\Delta H=\Delta U-\Delta (PV)$求出$\Delta H$；
- $\Delta S$，等容过程对应的可逆过程为物体的可逆加热或冷却，有$\delta Q_R=C\mathrm{d}T=nC_\mathrm{V,m}\mathrm{d}T$，因此，$\Delta S = \int_{T_A}^{T_B} \dfrac{nC_\mathrm{V,m}}{T}\mathrm{d}T=\Delta S=nC_\mathrm{V,m}\ln\dfrac{T_{B}}{T_{A}} $，根据公式$pV=nRT$可以求出$T$，这里不再对式子进行展开；
- $\Delta G$，同等压过程，因为温度发生改变，必须知道$S$的初始值才能计算出$\Delta (TS)$，所以题目会给出或间接给出$S$，此时直接代入公式$\Delta  G=\Delta H-\Delta (TS)$即可求出吉布斯自由能的变化值；
- $\Delta A$，与$\Delta G$类似，仅将$\Delta H$替换为$\Delta U$其余不变，即可计算出亥姆霍兹自由能的变化值。

### 绝热可逆过程
这是一个非常重要的过程，可逆，满足方程$pV^\gamma=K=\mathrm{Constant}$，式中$\gamma=\dfrac{C_\mathrm{p,m}}{C_\mathrm{V,m}}$，对于理想气体$\gamma=\dfrac{5}{3}$。

- $\Delta U$，由热力学第一定律，$\Delta U=Q+W$，$Q=0$，故$\Delta U=W=nC_\mathrm{V,m}\Delta T$；
- $Q$，绝热过程定义直接表明$Q=0$；
- $W​$，$W=-\int_{V_1}^{V_2}p\mathrm{d}V = -\int_{V_1}^{V_2}\dfrac{K}{V^\gamma}\mathrm{d}V = -\big[ \dfrac{K}{(1-\gamma)V^{\gamma-1}} \big]_{V_1}^{V_2} = -\dfrac{K}{1-\gamma} \big[ \dfrac{1}{V_2^{\gamma-1}} - \dfrac{1}{V_1^{\gamma-1}} \big] $
   因为$p_1 V_1^\gamma = p_2 V_2^\gamma = K$，上式写为$ W=\dfrac{p_2 V_2- p_1 V_1}{\gamma -1}=\dfrac{nR(T_2 - T_1)}{\gamma -1}  $，又因为$\dfrac{nR}{C_V}=\gamma-1$，所以$W=C_V(T_2 -T_1)$
- $\Delta H$，计算$\Delta H$必须知道$\Delta (pV)$，此时没有更加简单的办法，只能分别求出$p_1$，$V_1$，$p_2$，$V_2$然后计算出$p_2 V_2 - p_1 V_1$最后代入$\Delta H=\Delta U +p_2 V_2 - p_1 V_1$即可求出$\Delta H$；
- $\Delta S$，该过程为绝热可逆过程，$\delta Q_R=0$，故熵变为０，即$\Delta S=0$；
- $\Delta G$，该过程为变温过程，故题目会直接或间接给出$S$的初始值，然后利用$\Delta G=\Delta H-\Delta (TS)=\Delta H-S\Delta T$计算出$\Delta G$即可；
- $\Delta A$，与$\Delta G$类似，将$\Delta G$中的$\Delta H$换为$\Delta U$即可。

### 环状过程
此过程没有具体的方程，但可以多个过程组成，系统最终状态与初始状态相同。

- 由于系统初始状态与终末状态相同，故**系统**中所有状态函数的变化量为零，即$\Delta U=\Delta H=\Delta S=\Delta G=\Delta A=0$，**注意，此式子描述的是系统的热力学量，而此系统与外界有能量交换，故为非孤立体系，熵变可以为０**。
- $W$与$Q$，计算这两个热力学量时**不能**设计过程来求，只能按照过程的具体细节，分解成容易求出$W$和$Q$的过程分别计算出两者，然后求和。

以上为本渣的总结，如有纰漏，还请大神指正。
