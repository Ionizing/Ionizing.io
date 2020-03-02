---
title: 简谐振动系统浅析（一）
tags:
  - 物理
  - 振动
categories: 
  - Physics
date: 2017-09-30 22:21:37
---




# 何为简谐振动

引用 Wikipedia 上的[定义](https://zh.wikipedia.org/zh-cn/%E7%B0%A1%E8%AB%A7%E9%81%8B%E5%8B%95)，**简谐运动**（或**简谐振动**、**谐振**、**SHM**（Simple Harmonic Motion））既是最基本也是最简单的一种[机械振动](https://zh.wikipedia.org/wiki/%E6%8C%AF%E5%8A%A8)。当某物体进行简谐运动时，物体所受的[力](https://zh.wikipedia.org/wiki/%E5%8A%9B)跟[位移](https://zh.wikipedia.org/wiki/%E4%BD%8D%E7%A7%BB)成正比，并且力总是指向平衡位置。

典型的例子即为水平面内的弹簧振子，如下图所示（忽略一切摩擦）

![](http://owucpthrj.bkt.clouddn.com/Fkz3IAKLr9KLnGExvHe6RlsOB_Pr)

如果用 $F$ 表示物体受到的回复力，用 $x$ 表示小球相对于平衡位置的位移，由胡克定律，小球受力总有
$$
F = -kx
$$
简谐振动系统的机械能守恒（即动能和弹簧的势能之间来回转换，动能和势能之和不变）。

# 动力学方程

## 普通表述

由牛顿第二定律
$$
F = ma
$$
因此
$$
ma + kx = 0
$$
即
$$
m\ddot{x} + kx = 0
$$
解上面的方程（这是一个很简单的二阶线性齐次常系数微分方程，$mr^2 + k = 0$，推出 $r = \pm i\sqrt{\frac{k}{m}}$，直接代入解的表达式并用欧拉公式展开即可），可以得到
$$
\begin{aligned}
x &= c_1 \cos(\omega t) + c_2 \sin(\omega t)\\
 &= A\cos(\omega t + \varphi_0)
\end{aligned}
$$

式中
$$
\begin{aligned}
\omega &= \sqrt{\frac km} && \omega 即为角频率\\
	A &= \sqrt{c_1^2 + c_2^2} && A即为振幅\\
	\varphi_0 &= -\tan\frac{c_2}{c_1}& & \varphi_0 即为初相位
\end{aligned}
$$

显然，从 $x(t)$ 的表达式可以看出，小球的运动呈现一个周期性，周期为
$$
T = \frac{2\pi}{\omega} = 2\pi \sqrt{\frac mk}
$$

## 复数表述

这时 $x = A\cos(\omega t + \varphi_0)$ 可以用一个复数表示（欧拉公式：$e^{\mathrm i \theta} = \cos \theta + \mathrm i \sin \theta$，欧拉公式的推导借助了麦克劳林公式展开，感兴趣可以看[这里](https://zh.wikipedia.org/wiki/%E6%AC%A7%E6%8B%89%E5%85%AC%E5%BC%8F)）
$$
\begin{aligned}
x = \tilde{s}(t) &= Ae^{\mathrm i(\omega t + \varphi_0)} \\
  & = Ae^{\mathrm i\omega t}\cdot e^{\mathrm i\varphi_0} \\
  & = (Ae^{\mathrm i\varphi_0})\cdot e^{\mathrm i\omega t} \\
  & = \tilde{A} e^{\mathrm i\omega t}
\end{aligned}
$$
（为什么要用复数呢？~~因为可以装X，~~因为后面涉及求简正模问题时使用复数会变得非常简单）

此时小球的速度和加速度可以表示为
$$
\begin{aligned}
	\tilde{v} &= \frac{\mathrm d \tilde{s}}{\mathrm d t} = \frac{\mathrm d}{\mathrm dt} \tilde{A} e^{\mathrm i\omega t}  = \mathrm i \omega \tilde{A} e^{\mathrm i\omega t} = \mathrm i\omega \tilde{s}\\
	\tilde{a} &= \frac{\mathrm d^2 \tilde{s}}{\mathrm d t^2} = \frac{\mathrm d^2}{\mathrm d t^2} \tilde{A} e^{\mathrm i\omega t} =  -{\omega}^2 \tilde{A} e^{\mathrm i\omega t}  = -{\omega}^2 \tilde{s}
\end{aligned}
$$
显然，这种方式求导起来比普通表示法要方便很多。

# 能量转化分析

这个系统中值得我们关注的能量有两种：弹簧的弹性势能 $T(x)$ 和小球的动能 $V(x)$。

## 对动能分析

显然有
$$
V(s) = \frac 12 m\dot{s}^2 = -\frac 12 \omega^2 \tilde{s}^2
$$
~~没什么好说的~~

## 对势能分析

**注意，这一部分所分析的势能都是以平衡位置为零势能点所算起的势能。**

弹簧的势能有
$$
T(s) = \frac 12 k s^2
$$
对势能求二阶导
$$
\frac{\mathrm d^2}{\mathrm ds^2} T(s) =  \frac{\mathrm d^2}{\mathrm ds^2} \bigg(\frac12 ks^2 \bigg)= k
$$
我们发现势能对位移求二阶导后恰好等于劲度系数，因此有
$$
\begin{aligned}
	T &= \frac 12 T'' s^2 \\
	f_{\rm 回} &= -T''s
\end{aligned}
$$
此时，我们上一小节所讲的所有公式中的振幅 $A$ 和 角频率 $\omega$ 都可以用势能的二阶导数表示
$$
\begin{aligned}
	A = s_{\max} &= \sqrt{ \frac{2E}{k} } = \sqrt{ \frac{2E}{T''} } \\
		\omega	&= \sqrt{ \frac{k}{m} } = \sqrt{ \frac{T''}{m} }
\end{aligned}
$$

根据这个条件，当我们遇到某些特殊情况而无法很轻松求出 $k$ ，但 $T$ 又显而易见时，我们可以使用上面的式子来计算振动的相关参数（比如摆）。

# 简正模

当我们研究的系统不再由单一振子，而是多个振子耦合在一起时，各个振子可能会有各自不同的固有频率，此时系统内的振子如何运动？如果这个系统是孤立的，那么系统的动量守恒，所有的振子一定会按照统一的频率振动，那么这个频率是由谁来决定？我们用一个例子来说明这个问题：

下图表示一个线形三原子分子 $\rm A_2B$ ，相邻原子之间的化学键看成弹性力，它的大小正比于原子离开平衡位置的距离，假设原子只做纵向振动（即振动方向始终与原子核之间连线共线），试求分子可能的纵向运动形式和相应的振动角频率。

![](http://owucpthrj.bkt.clouddn.com/Fk4cNP6wcbGvH68621OHVFdoQIVz)

我们假设从左到右三个原子相对平衡位置的位移分别为 $x_1,\, x_2,\, x_3$ ，列出它们的运动方程为
$$
\begin{aligned}
	m_A \frac{\mathrm d^2}{\mathrm d t^2}x_1 &= -k(x_1 - x_2) \\
	m_B \frac{\mathrm d^2}{\mathrm d t^2}x_2 &= -k(x_2 - x_1) - k(x_2 - x_3) \\
	m_A \frac{\mathrm d^2}{\mathrm d t^2}x_3 &= -k(x_3 - x_2)
\end{aligned}
$$
因为这是个孤立系统，所有原子的振动角频率相同，不妨设所有原子的振动角频率都是 $\omega$ ，所以所有原子的位移可以表示为
$$
\tilde x_i = \tilde A_i e^{\mathrm i \omega t}
$$
上面的运动方程可以改写为
$$
\begin{aligned}
	-\omega^2m_A A_1 e^{\mathrm i\omega t} &= -k(A_1 e^{\mathrm i \omega t} - A_2 e^{\mathrm i\omega t} ) \\
	-\omega^2m_B A_2 e^{\mathrm i\omega t} &= -k(2A_2 e^{\mathrm i \omega t} - (A_1+A_3) e^{\mathrm i\omega t} ) \\
	-\omega^2m_A A_3 e^{\mathrm i\omega t} &= -k(A_3 e^{\mathrm i \omega t} - A_2 e^{\mathrm i\omega t} ) \\
\end{aligned}
$$
经过化简可以得到
$$
\begin{cases}
	(\omega^2-\frac{k}{m_A})A_1 + \dfrac{k}{m_A}A_2 = 0 \\
	\dfrac{k}{m_B}A_1 + (\omega^2-\dfrac{2k}{m_B})A_2 + \dfrac{k}{m_B}A_3 = 0 \\
	\dfrac{k}{m_A}A_2 + (\omega^2-\dfrac{k}{m_A})A_3 = 0 
\end{cases}
$$

上述方程组有解得条件是
$$
\begin{vmatrix}
	(\omega^2-\frac{k}{m_A})A_1 & \frac{k}{m_A}A_2  & 0\\
	\frac{k}{m_B}A_1 & (\omega^2-\frac{2k}{m_B})A_2 & \frac{k}{m_B}A_3 \\
	0 & \frac{k}{m_A}A_2 & (\omega^2-\frac{k}{m_A})A_3 
\end{vmatrix}
= 0
$$
即
$$
(\omega^2 - \frac{k}{m_A})^2 (\omega^2 - \frac{2k}{m_B}) - 2(\omega^2 - \frac{k}{m_A})\frac{k^2}{m_A m_B} = 0
$$
进一步因式分解可以得到
$$
(\omega^2 - \frac{k}{m_A}) (\omega^2 - \frac{k(2m_A+m_B)}{m_A m_B})\omega^2= 0
$$
此时 $\omega ^2$ 有三个根，分别为 $\frac{k}{m_A} $ ， $\frac{k(2m_A+m_B)}{m_A m_B}$ ， $0$ 。下面对这三个根进行讨论：

1. 当 $\omega^2 = \frac{k}{m_A}$ 时，$A_1 + A_3 = 0$ 且 $A_2 = 0$ ，即中间原子不动，两边原子做做振动方向相反、振幅和频率相同的运动；
2. 当 $\omega^2 = \frac{k(2m_A + m_B)}{m_A m_B}$ 时，$A_1 = A_3 = -\frac{m_B}{2m_A} A_2$ ，即两边原子运动状态相同，但与中间原子的运动方向相反，振幅之比为 $A_1 : A_2 = m_B: 2m_A$ ；
3. 当 $\omega^2 = 0$ 时，$A_1 = A_2 = A_3$ ，系统做刚性平动。

上面 $\omega^2$ 取三个根时，系统分别对应了三个不同的状态，其中除了 $\omega^2 = 0$ 外，其他两个状态都各自表明了一种特殊的振动模式，这些振动模式都对应了各自唯一的一个频率，称为**简正频率**。

一般来说，简正模时系统中各自由度运动的某种特殊组合，是整个系统的集体运动方式，不是其中个别振子的行为所决定的。

# 来两道题小试牛刀

## 求下面系统的简正模

三个质量为 $m$ 的质点和三个劲度系数为 $k$ 的弹簧串连在一起，紧套在光滑的水平圆周上（如下图）。求此系统的简正模（即简正频率和运动方式）。

<img src="http://owucpthrj.bkt.clouddn.com/FvHP-LeH9BzMuBco5XNJP2darkrV" width="400"/>



列出三个质点的动力学方程：
$$
\begin{aligned}
	m\frac{\mathrm d^2}{\mathrm dt^2} x_1&= -k(2x_1 - x_2 - x_3) = k(x_2 + x_3 - 2x_1) \\
	m\frac{\mathrm d^2}{\mathrm dt^2} x_2&= -k(2x_2 - x_1 - x_3) = k(x_1 + x_3 - 2x_2) \\
	m\frac{\mathrm d^2}{\mathrm dt^2} x_3&= -k(2x_3 - x_1 - x_2) = k(x_1 + x_2 - 2x_3) 
\end{aligned}
$$
不妨设 $x_i = \tilde{A_i}e^{\mathrm i \omega t}$ ，上面式子改写为
$$
\begin{aligned}
	-\omega^2 m A_1 e^{\mathrm i\omega t} &= k( A_2 e^{\mathrm i\omega t} + A_3 e^{\mathrm i\omega t} - 2A_1 e^{\mathrm i\omega t} ) \\
	-\omega^2 m A_2 e^{\mathrm i\omega t} &= k( A_1 e^{\mathrm i\omega t} + A_3 e^{\mathrm i\omega t} - 2A_2 e^{\mathrm i\omega t} ) \\
	-\omega^2 m A_3 e^{\mathrm i\omega t} &= k( A_1 e^{\mathrm i\omega t} + A_2 e^{\mathrm i\omega t} - 2A_3 e^{\mathrm i\omega t} ) 
\end{aligned}
$$
经过化简，上面式子变为
$$
\begin{aligned}
	( \omega^2-\frac{2k}{m} )A_1 + \frac{k}{m}A_2 + \frac{k}{m}A_3 &= 0\\
	\frac{k}{m}A_1 + ( \omega^2-\frac{2k}{m} )A_2 + \frac{k}{m}A_3 &= 0\\
	\frac{k}{m}A_1 + \frac{k}{m}A_2 + ( \omega^2-\frac{2k}{m} )A_2 &= 0
\end{aligned}
$$

上述式子成立的条件是矩阵对应的行列式值为零
$$
\begin{vmatrix}
	( \omega^2-\frac{2k}{m} )A_1 & \frac{k}{m}A_2 & \frac{k}{m}A_3 \\
	\frac{k}{m}A_1 & ( \omega^2-\frac{2k}{m} )A_2 & \frac{k}{m}A_3 \\
	\frac{k}{m}A_1 & \frac{k}{m}A_2 & ( \omega^2-\frac{2k}{m} )A_3 
\end{vmatrix}
= 0
$$

注意到这是一个特殊的行列式，使用结论可以~~口算~~很快算出
$$
(\omega^2 - \frac{2k}{m} + 2\frac{k}{m}) (\omega^2 -\frac{2k}{m} - \frac{k}{m}) ^2 = 0
$$
得到三个根 $\omega_1 = 0$ ，$\omega_2 = \omega_3 = \sqrt{\frac{3k}{m}}$ 。

这三个根代入方程组都得出
$$
A_1 = A_2 = A_3
$$
即当这个系统趋于稳定时，三个质点的运动是一致的，整个系统做刚体的平动（绕圆盘圆心转动）。



## 求下面振子的周期公式

竖直悬挂的弹簧振子，若弹簧本身质量不可忽略，试推导其周期公式。假设弹簧的质量不可忽略，k为其劲度系数，M为系于其上物体的质量（假设弹簧的伸长量由上到下与长度成正比地增加）。

<img src="http://owucpthrj.bkt.clouddn.com/FvTsySEGcax0xAS0Xa7MC3bQuS3q" width="150"/>

假设弹簧长度为 $L$ ，任意时刻 $M$ 的运动速度为 $v = v(t)$ ，则弹簧单位长度的质量为 $\mu = \frac{m}{L}$ ，又假设 $M$ 运动时弹簧上质点的速度是线性增加的，此时计算弹簧运动时的动能
$$
\begin{aligned}
V_{\rm spring} &= \int\mathrm d \bigg(\frac{1}{2} m v^2 \bigg) \\
		&= \frac{1}{2} \int_{0}^{L} \bigg(\mu \mathrm dl \cdot (v\frac{l}{L})^2\bigg) \\
		&= \frac{mv^2}{2L^3} \int_{0}^{L} l^2 \mathrm d l \\
		&= \frac{1}{2}\cdot \frac{m}{3} v^2
\end{aligned}
$$
 即弹簧的运动动能相当于在其末端悬挂了一个质量为 $\frac{m}{3}$ 的质点运动时的动能，因此这个模型可以等效为一个质量为 0 的弹簧在其末端悬挂了质量 $M + \frac{m}{3}$ 的质点做简谐运动，套用简谐运动的角频率公式即可解出 $M$ 运动时的角频率为
$$
\begin{aligned}
	\omega &= \sqrt{ \frac{k}{M + m/3} }\\
	T &= \frac{2\pi}{\omega} = 2\pi \sqrt{ \frac{M + m/3}{k} }
\end{aligned}
$$

# 总结

简正模在刚开始看时还是挺费劲的，但是读了5遍左右就开始慢慢明白（大概是明白了吧，雾）了其物理含义，而在求解简正模时用到了线性代数的一点知识，算是复习了一下下线代，心里还是蛮开心的。最后那道题在看钱伯初先生的论文时对平均速度的求解目的不是很理解，后来经物吧腿娘大佬的正确指导下弄明白了如何应对这个问题。

关于振动，不出意外，本渣应该会写两篇浅析，这是第一篇，第二篇已经在计划中，希望有生之年可以见到。

以上
