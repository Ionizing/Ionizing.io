---
title: 在 VASP(PBE) 计算中如何优雅地踩坑？
tags: 
  - VASP
categories:
  - FirstPrinciple
date: 2018-03-28 17:28:20
---

> **警告**：本渣刚学 `VASP` 不久，对 `VASP` 的算法、参数不甚了解，如果有大触偶遇此文，并发现有错误，请在评论区指出或邮箱联系本渣。

# 前言
本文是本渣从 3 月初以来刚接触 `VASP` 所学技能与遇到坑的总结，以纪念我为文献中一副图而逝去的最近一星期。何为 `VASP` 以及` VASP` 能做什么？本渣只是一个刚学的菜鸡，恕不能回答，请自行 Google 解决。

## VASP 的输入文件

想要执行一次最简单的计算，`VASP` 需要至少4个文件，它们分别是：

- `INCAR`：`VASP` 计算的**核心**文件，基本等同于本次计算的司令部。它包含了“**算什么**”、“**怎么算**”、和“**输入、输出那些**”这三个方面的内容，它的几项常用（PBE计算中）参数在后面我们会较为详细地了解到；
- `POSCAR`：决定元胞结构的文件。它包含了元胞的格子类型、格子大小、以和格子内所有原子的种类及坐标，在弛豫时，它还可以指定哪些原子可以在哪些方向上进行[弛豫](https://en.wikipedia.org/wiki/Relaxation_(physics))，在 `VASP` 中我们无需给出原子间的成键情况，仅给出原子的坐标即可；
- `KPOINTS`：描述 `VASP` 计算中对元胞格子的 mesh 精细度，或指定格子中哪些点的信息需要计算（？这里留个问号，本渣也不是很懂这里）；
- `POTCAR`：赝势库，按 `POSCAR` 中元素列出顺序给出了对应元素的赝势库。

**以上四个文件全为文本文件**。当然我们还可以命令 `VASP` 读取上一次的计算结果，比如 `WAVECAR` ，比如 `CHGCAR` ，用以更快地收敛或者执行某些特殊的计算。

## 我们需要 VASP 的哪些输出文件？（目前为止）

- `OUTCAR`：`VASP` 的计算日志，里面包含了几乎所有我们需要的结果，明文存储，随计算过程实时更新；
- ~~`WAVECAR`：**二进制**文件，经常很大，计算结束时会将内存中的波函数平面波系数写入该文件。（我对这个文件所了解的仅有这些；~~
- `OSZICAR`：`VASP` 计算过程中能量收敛日志，每个电子步结束时写入；
- ~~`EIGENVAL`：`VASP` 计算出的能带信息保存在这个文件中，计算结束时被写入（可以使用 `Python` 等工具以它为输入绘制能带图，但本渣太菜，目前还没写这个小程序；~~
- `CONTCAR`：在[弛豫](https://en.wikipedia.org/wiki/Relaxation_(physics))中，这个文件包含了`VASP`每个离子步结束时系统优化得到的元胞原子的坐标，每个离子步结束时写入；如果希望从还未收敛的弛豫结果继续计算，应直接将这个文件复制并重命名为 `POSCAR` 继续计算即可；
- ~~`CHG` & `CHGCAR`：这个两个文件都包含了元胞中原子的位置信息，以及电荷密度信息，但 `CHG` 比 `CHGCAR` 小一些，因其不包含 `PAW one centre`；~~
- `vasprun.xml`：XML 格式存储的 `VASP` 计算过程日志。
- `PROCAR`：

## 用 PBE 方法跑出能带图的总体步骤

1. 阅读文献，列出文献所给参数（如果有的话），比如 `SIGMA` ，`ISMEAR` ，`ENCUT` ，晶格常数 $a$ 等等信息；
2. 查阅资料，**实现文献所给结构**，关注文献的结构是否经过超胞，是否有真空层，是否考虑 vdW ；
3. 按顺序测出适合该体系的 `SIGMA` 、`ENCUT` 、kpts 、`a` ；（如果文献中给出了这些参数可跳过这一步
4. 对结构进行弛豫，优化出最稳定结构；
5. 对体系进行高精度的电子自洽计算；
6. 跑出能带图、DOS 图以及直接获得所需的能量值等。

# 正文开始

下面以单层 C3B 结构为例，走一遍上面的流程，看看有哪些坑。

## 获取参数

```fortran
a = 5.17         ! 晶格常数
EDIFF = 1.0E-6    ! 弛豫收敛条件
EDIFFG = -0.001    ! 

kpts: Monkhorst    ! KPOINTS 取法
```

文献给出单层 C3B 的结构为 $D_{6h}$ ，长这样（从上往下看：

![](http://owucpthrj.bkt.clouddn.com/FpGVI905Z66B8ksQjiULb3cLahCV)

## 实现结构

建立元胞的格子基矢（D6h 的晶型都可以这样建系，使用 Direct 模式，可以手写所有原子的坐标），并表示出各个原子的坐标

![](http://owucpthrj.bkt.clouddn.com/FiQMZt8-XbO9iXes05rkDhfFjHls)

所以它对应的 `POSCAR` 应为：

```fortran
C3B graphene like structure, a = 5.17               ! 注释
   5.17                                             ! 晶格放大系数
    0.8660254037   -0.5000000000     0.0000000000   ! 下面三行为实空间中定义元胞的三个格矢 a1, a2, a3
    0.8660254037    0.5000000000        0.0000000000
    0.0000000000    0.0000000000     4.0000000000   ! 真空层厚度为 4.0*5.17 = 20.68A
   B    C                                           ! 元素种类，这一行是给人看的，而不是给VASP看的
   2    6                                           ! 对应元素的原子数目
Direct                                              ! 以定义的格矢为基底
    0.0000000000    0.0000000000    0.0000000000    ! B 下面皆为原子坐标，顺序与第六行指定的元素种类相同
    0.6666666666    0.6666666666    0.0000000000    ! 要注意，仅靠POSCAR，并vasp不知道元素的具体种类，而只知道元素种类的个数。
    0.5000000000    0.0000000000    0.0000000000    ! C
    0.1666666666    0.1666666666    0.0000000000
    0.0000000000    0.5000000000    0.0000000000
    0.5000000000    0.5000000000    0.0000000000
    0.6666666666    0.1666666666    0.0000000000
    0.1666666666    0.6666666666    0.0000000000
```

**第一个坑**：文献中的晶格常数应该怎样对应 `POSCAR` 中的晶格参数？

这取决于我们取的基底。上文中我们以相邻两个元胞的 B 原子间模长最短的矢量为基矢，因此这个这个元胞的实际晶格常数应为 $a = |\vec{a}_i| \times a_{POSCAR}$ ， $a$ 即为文中给出的晶格常数，可见，`POSCAR` 中的晶格参数可以理解为缩放系数，所以写完结构时应与文献对照或放入 VESTA 等软件测出原子键长看与文献是否一致。有的文献作者基底的取法特殊，所给的晶格常数也很“魔性”，但不变的是格子的实际大小、原子间距离等，以这些信息为准才能写出与文献中真正等价的结构。此外，二维结构的实现时需要层间有 20 A 左右的真空层，以避免层间不必要的相互作用，但真空层不可取太厚，否则计算量的增加不仅仅是线性的。

**闲话**：如何建系是个见仁见智的问题，有人喜欢直接沿着六边形的边建系，有人喜欢像上文那样建系，但不管怎样，只要实现的结构是等价的那么建系就是没有问题的

## 测出必要的参数

### `INCAR` Template

在测各项参数之前，先给出一个`INCAR` 模板，具体参数的含义见注释或`VASP`手册。

```fortran
C3B Mono Layer  strcut_optim
  SYSTEM = "C3B Mono Layer" # 系统注释

Electronic minimization
  ISMEAR =                  # 使用Gaussian smearing，取 0 用于导体、绝缘体的计算
  SIGMA =                   # Smearring 的宽度，在精度允许范围内应尽量大以减小计算量
  EDIFF =                   # 收敛判据的能量判据
  EDIFFG =                  # 收敛判据，若值为负，则为力判据；若值为正，则为能量判据
  PREC =                    # 精确度
  IBRION =                  # 离子移动方法，跑MD、弛豫抑或静态计算时设置不同
  ISIF =                    # 结构优化类型

Ionic minimization
  NSW =                     # 离子步数上限
  NELMIN =                     # 电子步收敛下限
  ENCUT =                   # 截断能，在保证收敛良好的情况下应尽量小
  LWAVE =                   # 是否输出 WAVECAR；该值为 .FALSE. 时也会有 WAVECAR 输出，但其长度为 0
  LCHARG =                  # 是否输出 CHGCAR

Band Calcs
  ISTART =                  # 是否从当前目录读取 WAVECAR
  ICHARG =                  #  1: 读取 CHGCAR
                            # 10: 不改变当前目录下的 CHGCAR
```

同时 `POTCAR` 这样获得：

```bash
cat path/to/PPs/B/POTCAR >> ./POTCAR
cat path/to/PPs/C/POTCAR >> ./POTCAR
```

这里注意 `POTCAR` 中元素赝势的顺序应与 `POSCAR` 元素出现的顺序一致。

### 测出 `SIGMA`

固定`ENCUT`和kpts为较大的值`ENCUT = 600`、`kpts = 21`，固定晶格常数为“合适”值，取一系列`SIGMA`值做静态计算，当`OSZICAR`中`dE`绝对值小于 1.0 meV/atom 时取最大的 `SIGMA` 值。

使用脚本如下（参考候柱锋老师的入门指南）：

```bash
#!/bin/bash
rm WAVECAR          # 删除目录中的 WAVECAR 以免影响收敛
# in 后面 0.20 0.10 0.05 即为 sigma 所取的值，一般从大到小取，取值越小计算量越大当发现 dE 满足要求后可以及时终止计算
for i in 0.20 0.10 0.05    
do
    cat > INCAR <<! # 在 INCAR 中覆盖以下内容, ! 表示cat内容终止标志，可以自定义，如用 EOF
SYSTEM = Na adsorbed on site 1
ENCUT = 600         # 截断能取较大值
ISMEAR = 0          # Gaussian Smearing
SIGMA = $i          # 每次循环 SIGMA 取 $i 的值
NSW = 0             # 静态计算，离子步数为 1
IBRION = -1            # 静态计算
LWAVE = .FALSE.     # 不输出 WAVECAR，不使用上次计算所得平面波系数作为初始值
!
    echo " SIGMA = $i eV "
    time vasp >> vasp.log   # 执行 vasp 计算，重定向输出到 vasp.log ，并对该过程计时，结果输出到 stdout
    # OUTCAR 中 EENTRO 等于 OSZICAR 中的 dE，这里取出 dE
    TS=`grep "EENTRO" OUTCAR | tail -1 | awk '{printf "%12.6f", $5}'`
    echo "$i   $TS" >> sigma.txt     # 输出 SIGMA 和 dE 到sigma.txt
    echo -e "\n\n"   # 换行 3 次
done
```

所得 sigma.txt 内容如下

```
0.20   -0.00519083
0.10   -0.00009427
0.05   0.00000000
```

显然，这里`SIGMA`取 0.10 即可，当然还可以在 (0.10, 0.20) 之间再取几个点，逼近使 dE < 1meV/atom 的最大 SIGMA。

### 测出 `ENCUT`

将上面测出的 `SIGMA = 0.10` 固定，`ENCUT` 取不同值，过程与上面类似，只是判断 `ENCUT` 取值的依据变成了两次计算结果的`TOTEN`之差绝对值小于 1 meV/atom。

使用脚本如下：

```bash
#!/bin/bash

rm WAVECAR
for i in 300 350 400 450 500 550 600 # 每次循环 ENCUT 的取值
do
    cat > INCAR <<!
SYSTEM = Na adsorbed on site 1
ENCUT = $i
ISMEAR = 0
SIGMA = 0.10        # SIGMA 取上面测得的值
NSW = 0
IBRION = -1
!
    echo " ENCUT = $i eV "
    time vasp >> vasp.log
    E=`grep "TOTEN" OUTCAR | tail -1 | awk '{printf "%12.6f", $5}'`    # 取出 OUTCAR 中的 TOTEN
    echo "$i   $E" >> encut.txt
    echo -e "E = $E\n\n"
done
```

测得 `ENCUT` 取值为 450。这里 `ENCUT` 如果取值过小，后面在弛豫时很可能能量不会收敛。

### 测出 kpts

kpts 指的是 `KPOINTS` 里 meshgrid 的取样点数，取样越多，计算结果收敛越好，同时计算量也变大。取值条件与上面相同，即两次计算的 `TOTEN` 相差小于 1meV/atom 。

**注意，不要忘了把测好的 `SIGMA` 和 `ENCUT` 写入 `INCAR`**

```bash
#!/bin/bash

rm WAVECAR
for i in 9 13 17 21    # kpts 取 9 13 17 21
do
    cat > KPOINTS <<!
Na adsorbed on site1 -- det_kp
0
Monkhorst-Pack        # 只要第一个字母为 M 即可
    $i  $i  1        # 有真空层存在，z方向采样点数为 1
    0   0   0
!
    echo -e "k mesh = $i\t$i\t1"; time vasp >> vasp.log
    E=`grep "TOTEN" OUTCAR | tail -1 | awk '{printf "%18.10f", $5}'`
    KP=`grep "irreducible" OUTCAR | tail -1 | awk '{printf "%5i", $2}'`
    echo $i $KP $E >> det_kp.txt
done
```

测得 kpts 取15。

**第二个坑**：`KPOINTS` 文件名一定要写对，本渣使用的 `VASP` 版本为 5.3.5，计算中如果没检测到 `KPOINTS` 文件存在，则**默认** kpts 取值为 **3** ，导致结果错误。本渣某次计算中误将 `KPOINTS` 写成了 `KPOINS` ，导致后面无论如何修改参数，结果与文献值始终相差很大。（这个东西不仅坑了我，还坑了**比那名居腿娘**(来自WHU的EE大佬)大神熬夜到 3 点多，说来惭愧……

### 测出 `a` 

这里的`a`确切地指 `POSCAR` 中第二行的那个数值，即元胞的放大系数。因为本例中的晶格基矢模长 $|\vec{a_1}| = |\vec{a_2}| = 1$ ，故其放大系等于文献中的晶格常数 $a$。这里 `a` 的取值条件与上面相同。给出测试脚本如下：

```bash
#!/bin/bash

rm WAVECAR
# 晶格参数的取值应在文献值附近取点
for i in 4.97 5.02 5.07 5.12 5.17 5.22 5.27 5.32 5.37
do
    cat > POSCAR <<!
C3B graphene like structure, a = 5.17
   $i
    0.8660254037   -0.5000000000    0.0000000000
    0.8660254037    0.5000000000    0.0000000000
    0.0000000000    0.0000000000    3.0000000000
   B    C
   2    6
Direct
    0.0000000000    0.0000000000    0.0000000000    ! B1
    0.6666666666    0.6666666666    0.0000000000    ! B2
    0.5000000000    0.0000000000    0.0000000000    ! C1
    0.1666666666    0.1666666666    0.0000000000    ! C2
    0.0000000000    0.5000000000    0.0000000000    ! C3
    0.5000000000    0.5000000000    0.0000000000    ! C4
    0.6666666666    0.1666666666    0.0000000000    ! C5
    0.1666666666    0.6666666666    0.0000000000    ! C6
!
    echo -e "a = $i angstrom"; time vasp >> vasp.log
    E=`grep "TOTEN" OUTCAR | tail -1 | awk '{printf "%12.6f", $5 }'`
    V=`grep "volume" OUTCAR | tail -1 | awk '{printf "%12.4f", $5}'`
    printf "%6.3f %10.4f %18.10f\n" $i $V $E >> a.txt
    tail -1 a.txt
done
```

测得 `a` 与文献值相同，5.17。这里的 `a` 还可以取得更精细一些，甚至可以到小数点后三位，这里取值越接近“真实值”，后面弛豫的过程将会越短。

## 弛豫（结构优化，relaxation）

弛豫中需要用到的 `INCAR` 参数有

```fortran
ISMEAR = 0       ! 继承自上面的计算结果
SIGMA = 0.1
ENCUT = 450

PREC = Accurate
EDIFF = 1.0E-6
EDIFFG = -0.01
IBRION = 1       ! 准牛顿法
ISIF = 3         ! 计算 force、stress tensor，对离子、晶格进行弛豫 

NELMIN = 6       ! (optional) 强制每个离子步跑至少 6 电子步，提高收敛效果
NSW = 200        ! 离子步上限为 200 步，目前本渣所算的体系中离子步数没有超过这个值的，也许是我太年轻了吧
```

这个步骤耗时最长，尤其是加上 `PREC` 后，每个电子步计算量上升很多，而为了结构的优化，离子步数的增多也使这个过程耗时不少，本例中 8 个原子的体系优化需要数个小时；本渣另外的一个体系中有 12 个原子，优化就需要 12h+ 。

**第三个坑**：`EDIFFG`有正负之分，**负数**表示收敛的力判据。如果`EDIFFG`误写为正，则`EDIFF`会被覆盖，`VASP`按照`EDIFFG`进行收敛判断，结果就是收敛可能会变得很快（快上几个数量级），但是所得结构不是我们想要的结构。

**第四个坑**：如果 `EDIFF`和`EDIFFG`设置过小，则计算时收敛可能会变得非常漫长，有时甚至会将结构跑散——原子间距离越来越大，这时候有三种选择：1. 提高精度，重新测出晶格常数，晶格常数可以精确到 0.001；2. 放宽收敛判据； 3. `IBRION` 取 2 （如果你对这个结构是否稳定没有信心的话。

## 高精度的电子自洽计算（SCF）

进行高精度的电子自洽计算是为了得到体系的电荷分布，同时也为后面计算能带提供数据。

这一步需要**将 relaxation 得到的 CONTCAR 复制到 SCF 计算目录下，并重命名为 POSCAR** ，`INCAR` 内容相似，仅将弛豫相关参数修改即可，`INCAR` 如下

```fortran
C3B Mono Layer  strcut_optim
  SYSTEM = "C3B Mono Layer" # System Name

Electronic minimization
  ISMEAR = 0                # Toggle Gaussian Method, complete later
  SIGMA = 0.1               # Broadening in eV -4-tet-1-fermi 0-gaus
  EDIFF = 1.E-6             # Energy tolerance eV/atom
#  EDIFFG = -0.01            # Force tolerance
  PREC = Accurate           # Accuracy
  IBRION = -1               # quasi-Newton algorithm
#  ISIF = 3                  # optim totally: volume, stress, force, ion, shape

Ionic minimization
  NSW = 0                   # Converge steps, must set
  ENCUT = 450               # components beyond ENCUT are 'removed' from the projection
                            # operators
  LWAVE = .TRUE.            # Output WAVECAR
  LCHARG = .TRUE.           # Output density of charge to CHG and CHGCAR
  NCORE = 4
```

由于默认输出 `CHGCAR` ， `INCAR` 中的 `LCHARG` 可以不写。

## 计算能带（band）

**复制 SCF 计算所得 CHGCAR 到 band 计算目录下。**

我们需要重写 `KPOINTS` ，之前的 `KPOINTS` 一直是系统帮助我们在布里渊区采样，现在我们想要得到布里渊区某一路径上能带的能量变化，则需要在 `KPOINTS` 指定出路径。

根据 [DOI:10.1016/j.commatsci.2010.05.010](https://arxiv.org/abs/1004.2974) 后面所给资料提示选择对应的布里渊区 K 点路径（注意文中基矢的取法是否与自己一致，否则需要重新画出布里渊区，找出对应的 K 点。这里在寻找 K 点时可以借助 [XCrysDen](http://www.xcrysden.org/) ，选择 K-Point path 即可

![这里取 Gamma -> K -> M -> Gamma](http://owucpthrj.bkt.clouddn.com/FpmnmrDYGQOgi2NLIPUvy31t0c4p)

写出 `KPOINTS` 如下：

```fortran
K-POINTS    C3B Mono Layer Bands    ! 注释行
    40                              ! 每两个K点之间取样点数
Line-mode                           ! Line-mode，区别于自动取样模式，跑能带专用
Rec                                 ! 在倒空间内取点
    0       0       0               ! Gamma
    0.33333 0.66666 0               ! K

    0.33333 0.66666 0               ! K
    0.5     0.5     0               ! M

    0.5     0.5     0               ! M
    0       0       0               ! Gamma
```

同时在 `INCAR` 中加上 `ICHARG = 11` 以读取 SCF 所得 `CHGCAR` 进行能带计算。

最后，取所得 `vasprun.xml` 导入 `p4vasp` 进行绘制能带图。如果读者有兴趣可以以 `EIGENVAL`  文件为输入自己实现绘制band的程序（不会很难）。

> 跑能带的感觉就和抽卡/开箱是一样的，你永远也不知道下一个体系的 band 会是什么鸟样。

跑出的能带是这个样子

![](http://owucpthrj.bkt.clouddn.com/Fodl8Omy6tyxekWIF6pAxHI_lPkN)

文献中的截图是这个样子的

![](http://owucpthrj.bkt.clouddn.com/Fp0bJ1vUyw-dm2uxoVXt61hm6ka_)

这里解释一下，本渣最初在跑 band 时 K 点取的路径和文献不同，所以两幅图基本等于对方水平翻转所得，但本文中所给 `KPOINTS` 里 K 点路径与文献是相符的。细心的大神已经看出两幅图中上图似乎向下平移了一部分，这可能是费米能级的取值差异所致，由于本渣水平太低，姑且只能这样理解了。

## 计算态密度（DOS）

如果阅读过文献的话，我们会发现文献中经常和能带图一起出现的还有 *态密度* 图。何为态密度？[Wikipedia](https://zh.wikipedia.org/zh-hans/%E7%8A%B6%E6%80%81%E5%AF%86%E5%BA%A6) 上的解释为某一能量周围单位区间内的微观状态数量。具体到本文，上面的微观状态便是指占据某能级的电子态。


# 论检查输入文件正确性的重要性

这里的输入文件指 `VASP` 计算时需要用到的文件，如`INCAR`、`POSCAR`、`POTCAR`、`KPOINTS`、`CHGCAR`等。

1. 文件名是否正确。我知道很多人在 `bash` 中喜欢用 Tab 补全，这对提高命令输入效率非常有效，但同时也降低了我们检查文件名正确性的机会，上文中提到的 `KPOINS` 错误就是因为一直使用 Tab 补全而使它一直被继承到跑出 band ，还不止坑了自己，多么可笑；
2. 脚本中重定向输出的文件名是否与需求一致。当使用上一次计算的脚本进行修改时本渣很容易忘了修改输出重定向的目标，从而导致 `POSCAR` 的内容被输出到 `KPOINTS` 里，这时 `VASP` 会报错，遇到这种情况及时修改脚本重新提交任务即可，无非是重新排队的问题；
3. `POTCAR` 是否与体系相对应。有的论文并不明确说出自己使用哪个 `POTCAR` ，而是这样说："We treated 4p, 5s, 4d orbitals of xx, and x orbitals of xxx as valence electrons…"，这就表明我们需要考虑使用带 `_pv` 、 `_sv` 的 `POTCAR` 。**这大概是本人近期踩过的最大的坑了吧**，可以想象，如果从一开始的 `POTCAR` 就出了问题，那么后面所有的计算就都是徒劳的，而在排错时无论如何也很难将目光投向 `POTCAR` 。这里要特别感谢 **比那名居腿娘** 同学的帮助；
4. `POSCAR` 中的晶胞是否需要超胞。有的文献中给出一个 band 图但未给出其对应的结构，本渣按照前文所给结构计算，始终无法得到与文献相符的 band 图，直到本渣使用 2x2 超胞后的结构进行计算，终于重复出和文献很接近的结果；
5. 有时在弛豫后进行 SCF 计算时会提示对称性错误，这时需要在 `INCAR` 中指定 `SYMPREC` 的值，这个值默认为`SYMPREC = 1.0E-5`，指定时可以使用更小的值。

除了上面说到的 `INCAR`、`POSCAR`、`POTCAR`、`KPOINTS`、`CHGCAR`等，有时还会用到 `WAVECAR` ，`WAVECAR` 是二进制文件，在使用 FileZilla 传送时，如果没有勾选 `binary` 选项，则传送时会按照 ASCII 编码进行传送，后果就是 `WAVECAR` 文件损坏。



# 参考资料

1. VASP软件包的使用入门指南——候柱锋
2. VASP the GUIDE(April 20, 2016)——Kresse, Martijn Marsman , Jurgen Furthmuller
3. [High-throughput electronic band structure calculations: challenges and tools](https://arxiv.org/abs/1004.2974) ——[Wahyu Setyawan](https://arxiv.org/find/cond-mat/1/au:+Setyawan_W/0/1/0/all/0/1), [Stefano Curtarolo](https://arxiv.org/find/cond-mat/1/au:+Curtarolo_S/0/1/0/all/0/1)

这次 blog 就写到这里吧，反正也是为自己写的。（当然如果有大佬愿意在评论区指出其中的错误，本渣感激不尽！

最后祝愿各位读到这里的菊苣，提前（
