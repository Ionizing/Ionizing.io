---
title: 如何求解 A * B = C 中的矩阵 B ？
date: 2019-01-25 23:44:50
tags:
 - Linear Algebra
 - VASP
 - OUTCAR
Category:
 - LinearAlgebra
---


# 问题

给定一个矩阵方程
$$
\mathbf{A} \times \mathbf{B} = \mathbf{C}
$$
其中 $\mathbf{B}$ 是方阵， $\mathbf{A}$  和 $\mathbf{C}$ 的形状相同，问如何求解 $\mathbf{B}$ ?

# 解答

$\mathbf{A}$ 和 $\mathbf{C}$ 的形状相同，如果它们都是方阵，则
$$
\begin{align}
\mathbf{A} \times \mathbf{B} ={}& \mathbf{C} \\
\mathbf{A}^{-1} \times \mathbf{A} \times \mathbf{B} ={}& \mathbf{A}^{-1} \times \mathbf{C} \\
\mathbf{B} ={}& \mathbf{A}^{-1} \times \mathbf{C} \label{eq:easy}
\end{align}
$$


但 $\mathbf{A}$ 和 $\mathbf{C}$ 未必是方阵，所以不存在逆矩阵，也就无法通过等式左右同时左乘 $\mathbf{A}^{-1}$ 的方法直接求出 $\mathbf{B}$ 。那么此时如何求解 $\mathbf{B}$ 呢？

既然 $\mathbf{A}$ 和 $\mathbf{C}$ 不是方阵，那么把它们变换成方阵不就好了吗？因此等式左右共同左乘 $\mathbf{A}^{T}$ ，即可将 $\mathbf{A}$ 和 $\mathbf{C}$ 变换成方阵，即
$$
\begin{align}
\mathbf{A}_{m\times n} \times \mathbf{B}_{n\times n} ={}& \mathbf{C}_{m\times n} \\
\mathbf{A}^{T}_{n\times m} \times \mathbf{A}_{m\times n} \times \mathbf{B}_{n\times n} ={}& \mathbf{A}^{T}_{n\times m} \times  \mathbf{C}_{m\times n} \\
\mathbf{A}'_{n\times n} \mathbf{B}_{n\times n} ={}& \mathbf{C}'_{n\times n} \label{eq:difficult}
\end{align}
$$
$\eqref{eq:difficult}$ 即是我们熟悉的形式，直接使用 $\eqref{eq:easy}$ 的解法即可解得 $\mathbf{B}$ 。 
