# 背景知识

## 1. 混态与密度矩阵

量子多体系统在有限温度 $T = 1/\beta$ 下处于**吉布斯态（Gibbs state）**：

$$\rho = \frac{e^{-\beta H}}{Z}, \qquad Z = \mathrm{Tr}\, e^{-\beta H}$$

这是一个**混态（mixed state）**，不能写成单个量子态的投影子。
物理量期望值为：

$$\langle O \rangle = \mathrm{Tr}(\rho O)$$

---

## 2. 纯化（Purification）

**核心思想**：将混态嵌入到更大 Hilbert 空间中的纯态。

给定物理系统 $\mathcal{H}_\text{phys}$，引入辅助空间 $\mathcal{H}_\text{anc}$，
则存在纯态 $|\Psi\rangle \in \mathcal{H}_\text{phys} \otimes \mathcal{H}_\text{anc}$，使得：

$$\rho = \mathrm{Tr}_\text{anc}\, |\Psi\rangle\langle\Psi|$$

### 纯化不唯一

若 $|\Psi\rangle$ 是 $\rho$ 的一个纯化，则对辅助空间任意酉变换 $U_\text{anc}$：

$$|\Psi'\rangle = (\mathbf{1}_\text{phys} \otimes U_\text{anc})|\Psi\rangle$$

也是 $\rho$ 的一个纯化，且物理观测量不变（因为 $\mathrm{Tr}_\text{anc}$ 对 $U_\text{anc}$ 不变）。

这个自由度是本文**算法的核心利用点**。

---

## 3. 矩阵乘积态（MPS）

对于一维 $L$ 格点系统，矩阵乘积态写为：

$$|\Psi\rangle = \sum_{\sigma_1,\ldots,\sigma_L} A^{\sigma_1} A^{\sigma_2} \cdots A^{\sigma_L} |\sigma_1 \sigma_2 \cdots \sigma_L\rangle$$

其中每个 $A^{\sigma_i}$ 是 $\chi \times \chi$ 的矩阵（**键维度** bond dimension $\chi$）。

### MPS 纯化（MPP）

将物理格点和辅助格点交替排列：

```
物理: p_1  p_2  p_3  ...  p_L
辅助: a_1  a_2  a_3  ...  a_L
```

排列方式：$p_1 - a_1 - p_2 - a_2 - \cdots - p_L - a_L$（或其他排列）

对应 MPS 可以高效表示有限温度态。

### 为何要最小化纠缠？

- MPS 精确表示纯态所需键维度 $\chi$ 由**纠缠熵**决定
- 对于满足面积律的态：$S \sim \mathcal{O}(1) \Rightarrow \chi = \mathcal{O}(1)$（多项式增长）
- 纠缠越大，需要的 $\chi$ 越大，计算代价 $\mathcal{O}(\chi^3)$ 急剧增加

**结论**：找到纠缠最小的纯化 → 最小的 $\chi$ → 最高效的有限温度模拟。

---

## 4. 第二 Rényi 熵

**第二 Rényi 熵**是本文优化的目标函数：

$$S_2(A) = -\log \mathrm{Tr}\, \rho_A^2$$

其中 $\rho_A = \mathrm{Tr}_{\bar{A}} |\Psi\rangle\langle\Psi|$ 是子系统 $A$ 的约化密度矩阵。

### 为何选择 $S_2$ 而非 von Neumann 熵？

Von Neumann 熵 $S_1 = -\mathrm{Tr}(\rho_A \log \rho_A)$（$n \to 1$ 极限）更自然，
但 $S_2$ 的**梯度更易计算**：

$$\frac{\partial S_2}{\partial U} = -\frac{1}{\mathrm{Tr}\,\rho_A^2} \cdot \frac{\partial \mathrm{Tr}\,\rho_A^2}{\partial U}$$

而 $\mathrm{Tr}\,\rho_A^2 = \langle\Psi|\langle\Psi| \text{SWAP}_A |\Psi\rangle|\Psi\rangle$（SWAP 算符技巧）

---

## 5. 与相关方法的关系

| 方法 | 核心思路 | 优缺点 |
|------|----------|--------|
| **MPO 热态** | 直接表示密度矩阵 | 无统计误差，但代价高 |
| **METTS** | 随机采样极小纠缠典型热态 | 低温有效，但有统计噪声 |
| **MPP（本文）** | 纯化 + 解纠缠 | 无统计误差，键维度更小 |
| **DMRG** | 基态变分优化 | 仅适用于零温 |

本文的方法可以看作对**标准 MPS 纯化方法**的增强：
在虚时演化（imaginary-time evolution）得到 TFD 态后，
额外施加一个解纠缠幺正变换来进一步压缩纠缠。
