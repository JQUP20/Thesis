# 热场双态（Thermofield Double State, TFD）

## 1. 定义

**热场双态**是吉布斯态最自然的纯化之一。
设哈密顿量 $H$ 的本征态为 $|E_n\rangle$，本征值为 $E_n$，则：

$$|{\rm TFD}(\beta)\rangle = \frac{1}{\sqrt{Z(\beta)}} \sum_n e^{-\beta E_n / 2} |E_n\rangle_\text{phys} \otimes |E_n\rangle_\text{anc}$$

其中 $Z(\beta) = \sum_n e^{-\beta E_n}$ 是配分函数。

### 验证：追踪辅助自由度还原吉布斯态

$$\mathrm{Tr}_\text{anc}\, |\text{TFD}\rangle\langle\text{TFD}| = \frac{1}{Z}\sum_n e^{-\beta E_n} |E_n\rangle\langle E_n| = \frac{e^{-\beta H}}{Z} = \rho_\beta$$

---

## 2. TFD 的虚时演化构造

实际计算中，不需要先对角化 $H$。
**关键观察**：

$$|{\rm TFD}(\beta)\rangle \propto e^{-\beta H/2} \otimes \mathbf{1}_\text{anc} \, |\beta=0\rangle$$

其中无穷温度（$\beta=0$）态是：

$$|\beta=0\rangle = \frac{1}{\sqrt{d^L}} \sum_{\{\sigma\}} |\sigma\rangle_\text{phys} \otimes |\sigma\rangle_\text{anc}$$

（$d$ 是单格点维度，求和遍历所有构型 $\{\sigma\}$）

这个态是**完全纠缠态**，熵最大：$S = L \log d$。

### MPS 构造流程

```
1. 初始化 MPS 纯化：β = 0 时的最大纠缠态 |β=0⟩
   → 每个物理-辅助对形成 Bell 态

2. 虚时演化（Imaginary time evolution）：
   |TFD(β)⟩ = e^{-βH/2} ⊗ 1_anc |β=0⟩ / norm

   使用 TEBD（Time-Evolving Block Decimation）：
   - Trotter 分解：e^{-δτ H} ≈ ∏_{bonds} e^{-δτ h_{i,i+1}}
   - 步长 δτ，总步数 β/(2δτ)
   - 每步后截断 MPS 至键维度 χ

3. 结果：物理子系统的密度矩阵 = 吉布斯态 ρ_β
```

---

## 3. TFD 的纠缠性质

### 两个极端

| 温度 | $\beta$ | 纠缠 | 物理意义 |
|------|---------|------|----------|
| $T \to \infty$ | $\beta \to 0$ | **最大**，$S = L\log d$ | 完全纠缠 |
| $T \to 0$ | $\beta \to \infty$ | **最小**，$S = 0$ | 基态（纯态）不需辅助空间 |

### 中间温度

低温时，TFD 的纠缠仍然较大（但比 $\beta=0$ 小），
因为低能本征态数量少，纠缠主要来自辅助系统与物理系统的关联。

**问题**：TFD 的纠缠是否是"必要的"？还是可以通过 $U_\text{anc}$ 进一步降低？

**本文答案**：通过解纠缠，可以大幅降低纠缠，尤其在低温。

---

## 4. 纠缠熵的直观理解

对于 TFD，将系统在半链处切割，左半部分 $A = \{1,\ldots,L/2\}$ 的纠缠熵：

$$S(A) = S_\text{phys-anc} + S_\text{phys-phys}$$

- **物理-辅助纠缠**：物理格点与辅助格点之间的纠缠（由纯化方案决定）
- **物理-物理纠缠**：物理格点之间的纠缠（由热涨落决定，不可消除）

解纠缠算法的目标是**消除多余的物理-辅助纠缠**，
保留物理-物理纠缠（这是不可约的，反映真实的热关联）。

---

## 5. 与温度淬火（Temperature Quench）的关系

**温度淬火**：从无穷温度态出发，在真实时间演化下研究纠缠增长。

$$|\Psi(t)\rangle = e^{-iHt} \otimes \mathbf{1}_\text{anc} |\beta=0\rangle$$

- 无解纠缠：纠缠线性增长（Lieb-Robinson 界），计算代价指数增长
- 有解纠缠：**可以减慢纠缠增长速率**，延长可模拟时间

这是本文的**第二个主要结果**。
