---
title: 重新思考玻尔氢原子模型
tags:
    - Hydrogen Atom Model
    - Bohr
    - Correspondence Principle
    - Rydberg Constant
    - Hydrogen Spectra
categories: 
    - AtomicPhysics
date: 2019-3-26 23:31:40
---

# 经典理论的困境

1. 卢瑟福通过 $\alpha$ [粒子散射实验](https://en.wikipedia.org/wiki/Rutherford_scattering)确定了原子的结构。人们知道了原子的正电荷全部集中在很小的一部分上；$\newcommand{\dif}{\mathop{}\mathrm{d}}$ 

2. 按照经典电磁学理论，当电子的运动有加速度时，[会辐射出电磁波](https://physics.stackexchange.com/questions/65339/how-and-why-do-accelerating-charges-radiate-electromagnetic-radiation)，电子的动能会以电磁波的形式释放。如果电子是绕原子核做圆周运动，那么由于向心加速度的存在，电子会损失动能，并最终落在原子核上，原子的寿命将很短，但事实上原子并不都在辐射电磁波，并且相当它们都很稳定；

3. 按照经典理论，电子跃迁产生的波长应电子当前的轨道半径有关，那么在电子逐渐失去动能时，轨道半径是连续缩小的，此时产生的辐射应该是连续光谱，但氢原子的光谱谱线不是连续，而是分立的，并且在可见光范围内谱线的波长满足 [Balmer 公式](https://en.wikipedia.org/wiki/Balmer_series)
   $$
   \frac{1}{\lambda} = R (\frac{1}{4} - \frac{1}{n^2})
   $$
   式中 $R$ 是 <em>Rydberg</em> 常数，$n$ 是大于等于 3 的整数。

   P.S. 当时对于氢原子光谱谱线的实验已经比较成熟，获得的数据也很相当精确，虽然在可见光范围内氢原子的谱线只有 4 条，但就是凭借这四个数值， Balmer 在不断努力下终于推出了 Balmer 公式。 Balmer 是一名数学教师，个人觉得他的工作比 Kepler 从 Tycho 的数据中得到行星运动三定律还要难好多。Kepler 在总结规律时所拥有的数据量有很多，但 Balmer 手中有的数据仅有 4 个谱线的波长。要从这 4 个数中总结出一个正确的公式，难度可想而之，用在「黑夜之中摸索前进」形容再贴切不过。

# 玻尔的假设

玻尔认为，氢原子的结构像太阳系一样，核处在中间，电子在核的周围做匀速圆周运动。

为了「掩盖」掉经典理论在这个模型中的失效之处，玻尔假设：

1. 电子在核周围的轨道上运动；
2. 电子在轨道上做匀速圆周运动时不释放能量，并且轨道的半径只能取某些特定的值；
3. 电子在不同轨道间跃迁会释放/吸收能量，产生电磁波。

这些假设中的「轨道」仍然是经典意义上的轨道——半径固定的圆（或者行星三定律中的椭圆， whatever）。

其中第三条是具有突破性的假设，那时人们的思维禁锢在经典理论中：氢原子发出的光是由于电子在核周围做匀速圆周运动产生的，这样的想法无论如何也无法解释为什么原子处于基态时没有释放出电磁波。但根据跃迁假设，电子在不同轨道间跃迁才会释放、吸收能量，从而产生电磁波，人们才能观察到氢原子光谱。而在这之后人们用量子力学解释电子跃迁时仍然在使用这一条"假设"，可见这条假设的合理性是毋庸置疑的。

# 玻尔模型的数学表达

不妨取电子的质量为 $m_e$ ，核固定不动，且核和电子所带电量都为 $e$ ，电性相反。

那么电子在绕核运动时
$$
\frac{m_e v^2}{r} = \frac{e^2}{4\pi\epsilon_0 r^2}
$$
经过变形，得到
$$
\omega^2 = \frac{e^2/4\pi\epsilon_0}{m_e r^3} \label{eq3}
$$
而电子的势能与动能之和为
$$
\begin{aligned}
    E &={} \frac{1}{2} m_e v^2 - \frac{e^2/4\pi\epsilon_0}{r} \\
    &={} - \frac{e^2/4\pi\epsilon_0}{2r}
\end{aligned}
$$
那么两个能级的能量差就为
$$
\Delta E = -\frac{e^2/4\pi\epsilon_0}{2} (\frac{1}{r'} - \frac{1}{r})
$$

## Correspondence Principle

玻尔认为，他的模型在大量子数极限下，电子的行为与对应的经典情形中的电子行为相一致，这也被称为[对应原理](https://en.wikipedia.org/wiki/Correspondence_principle) 。

于是在 $r$ 很大时， $\Delta r = r' - r \ll r$ 。并且有
$$
\begin{aligned}
    \Delta E &={} -\frac{e^2/4\pi\epsilon_0}{2} (\frac{1}{r'} - \frac{1}{r}) \\
    &={} - \frac{e^2/4\pi\epsilon_0}{2} (\frac{r - r'}{r\cdot r'}) \\
    &={} - \frac{e^2/4\pi\epsilon_0}{2} (\frac{\Delta r}{r\cdot r'}) \\
    &\approx{} - \frac{e^2/4\pi\epsilon_0}{2} \frac{\Delta r}{r^2}
\end{aligned}
$$
上面的式子可以推出
$$
\omega = \frac{\Delta E}{\hbar} = \frac{e^2/4\pi\epsilon_0}{2\hbar} \cdot \frac{\Delta r}{r^2}
$$
在大量子数极限下，$\omega$ 应当满足经典理论下电匀速圆周运动的角频率，公式 $\eqref{eq3}$ 可以推出
$$
\omega = \sqrt{\frac{e^2/4\pi\epsilon_0}{m_e}} \cdot \frac{1}{r^{\frac{3}{2}}}
$$
以上两个 $\omega$ 的表达式相等，可以得到
$$
\frac{e^2/4\pi\epsilon_0}{2\hbar} \cdot \frac{\Delta r}{r^2} = \sqrt{\frac{e^2/4\pi\epsilon_0}{m_e}} \cdot \frac{1}{r^{\frac{3}{2}}} \\
\begin{aligned}
    \Delta r &={} \frac{r^2}{r^{\frac{3}{2}}} \cdot \frac{\sqrt{\frac{e^2/4\pi\epsilon_0}{m_e}}}{\frac{e^2/4\pi\epsilon_0}{2\hbar}} \\
    &={} r^{\frac{1}{2}} \cdot \textrm{Constant}
\end{aligned}
$$
即 $\Delta r \propto \sqrt{r}$ ， $\textrm{Constant} = \frac{2\hbar}{\sqrt{m_ee^2/4\pi\epsilon_0}{}} = 2\sqrt{a_0}$ ，整理得到
$$
\Delta r = 2\sqrt{a_0 r}
$$
我们把上面的式子看成是差分求导的结果，即
$$
\begin{aligned}
    \frac{\Delta r}{\Delta n} &={} 2\sqrt{a_0 r} \\
    \frac{\dif r}{\dif n} &={} 2\sqrt{a_0 r}
\end{aligned}
$$
其中 $\Delta n = 1$ 

应用玻尔假设第二条，电子的轨道半径是分立的，结合上面 $r$ 导数的表达式，我们「猜」
$$
r = an^x,\qquad n\in N_+
$$
于是

$$
\frac{\dif r}{\dif n} = 2 \sqrt{a_0 r} = axn^{x-1}\\
2\sqrt{a_0 an^x} = axn^{x-1}\\
axn^{x-1} \propto n^{x/2}
$$

很容易得到 $x = 2$ ， $a_0 = a$ 。于是乎我们得到了电子轨道半径的表达式
$$
r = a_0 n^2
$$
那么电子轨道角动量的表达式为
$$
\begin{aligned}
    L &={} m_e vr \\
    &={} m_e \sqrt{\frac{e^2/4\pi\epsilon_0}{m_e r}} r \\
    &={} \sqrt{m_e\frac{e^2}{4\pi\epsilon_0} r} \\
    &={} \sqrt{m_e \frac{e^2}{4\pi\epsilon_0} a_0n^2} \\
    &={} \sqrt{m_e \frac{e^2}{4\pi\epsilon_0} \frac{\hbar^2}{(e^2/4\pi\epsilon_0)m_e} n^2}\\
    &={} n\hbar
\end{aligned}
$$

角动量的量纲是 <em>L = mvr = pr</em> ，这恰好和不确定性原理公式体现出的量纲一致
$$
\Delta p \times \Delta x \ge \frac{\hbar}{2}
$$

## 里德堡常数的推导

我们可以求出不同量子数 $n$ 对应能级的能量为
$$
\begin{aligned}
E(n) &={} - \frac{e^2/4\pi\epsilon_0}{2r} \\
    &={} - \frac{e^2/4\pi\epsilon_0}{2a_0 n^2}
\end{aligned}
$$
不同能级 $n = i$、, $n = j$ 之间的的能量差为
$$
\begin{aligned}
    \Delta E &={} E(j) - E(i) \\
    &={} \frac{e^2/4\pi\epsilon_0}{2a_0}\cdot \left( \frac{1}{i^2} - \frac{1}{j^2} \right) \\
    &={} \frac{e^2/4\pi\epsilon_0}{2} \frac{m_e e^2}{4\pi\epsilon_0 \hbar^2} \left( \frac{1}{i^2} - \frac{1}{j^2} \right) \\
    &={} \frac{m_e e^4}{8 \epsilon_0^2 h^2} \left( \frac{1}{i^2} - \frac{1}{j^2} \right)
\end{aligned}
$$
令 $i = 2$ ， $j = n$ ，并且等式两边同除以 $c$ （以便于写成 $\frac{1}{\lambda}$ 的表达式）
$$
\frac{1}{\lambda} = \frac{m_e e^4}{8\epsilon_0^2 h^2c} \left( \frac{1}{4} - \frac{1}{n^2} \right)
$$
上式恰好是巴尔末给出的经验公式，其中原式中的常数是里德堡常数 $R = \frac{m_e e^4}{8\epsilon_0^2 h^2 c}$ ，约为 $1.0974 \times 10^7$ m<sup>-1</sup>，这与实验值非常接近（实验值的前几位也是 10974 ）。[相传](https://en.wikipedia.org/wiki/Bohr–Einstein_debates)爱因斯坦刚刚听说玻尔的模型时并未相信这个模型的正确性，但当他听说玻尔把里德堡常数也算出来的时候，爱因斯坦立即表示认可这个模型。

当然如果把 $i$ 换成其它数，得到的一系列 $\lambda$ 就是氢原子光谱的其他系。

# 总结

1. 巴尔末总结的公式在玻尔构建他的模型时起到了重要的作用，没有这个公式，就相当于写程序没有参考输出、写出的程序不知道怎么测试；
2. 玻尔提出的三个假设在一定程度上是合理的，虽然仍有很多经典力学的残留，但其第三条假设在之后仍被沿用（或许把它应该改称为一个正确的命题）；
3. 玻尔提出的对应性原理在其构建模型时也起到了举足轻重的作用。很多教科书上讲玻尔氢原子模型时直接给出「电子的角动量是量子化的 $L =  n\hbar$ 」，然后继续推出电子的轨道半径是量子化的，这样写不免有把结论当条件用的嫌疑；而事实上，玻尔是通过对应性原理推出电子轨道半径的表达式，进而推出电子的角动量表达式；
4. 玻尔的氢原子模型在**当时**相当圆满地解释了氢原子光谱；
5. 当然，从现在的眼光看，这个模型有很多缺陷，如忽略了电子的自旋、核的自旋、相对论效应等，是经典力学语言到量子语言的过渡产物。

# 参考&致谢

<em>Atomic Physics</em> , C.Foot, Exercise 1.12

Wikipedia
