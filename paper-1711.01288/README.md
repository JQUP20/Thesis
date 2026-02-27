# Finding Purifications with Minimal Entanglement

**arXiv:** [1711.01288](https://arxiv.org/abs/1711.01288)
**Journal:** Physical Review B **98**, 235163 (2018)
**DOI:** 10.1103/PhysRevB.98.235163

## 作者

Johannes Hauschild, Eyal Leviatan, Jens H. Bardarson, Ehud Altman, Michael P. Zaletel, Frank Pollmann

## 目录导航

| 文件 | 内容 |
|------|------|
| [01_背景知识.md](01_背景知识.md) | 混态、纯化、MPS、Rényi熵等基础概念 |
| [02_热场双态.md](02_热场双态.md) | 热场双态 (TFD) 的定义与性质 |
| [03_解纠缠算法.md](03_解纠缠算法.md) | 核心算法：最小纠缠纯化的迭代优化方法 |
| [04_数值结果.md](04_数值结果.md) | Ising 模型和 Heisenberg 模型的数值结果 |
| [05_总结与影响.md](05_总结与影响.md) | 结论、局限性及对后续研究的影响 |

## 一句话总结

> 对于一维量子系统，纯化的纠缠量直接决定 MPS 表示的计算代价；
> 本文提出一种迭代最小化第二 Rényi 熵的算法，找到纠缠最小的纯化，
> 从而大幅降低有限温度模拟的计算成本。

## 问题背景

模拟有限温度量子多体系统是凝聚态物理的核心挑战之一。常用方法：
- **密度矩阵重整化群 (DMRG)** 直接处理密度矩阵（MPO 形式）
- **最小纠缠典型热态 (METTS)** 统计采样纯态
- **纯化方法**：将混态提升为辅助空间上的纯态，用 MPS 表示

纯化不唯一——同一混态对应无穷多纯化，其纠缠量差别巨大。
**本文目标**：在所有等价纯化中找到纠缠最小的那个。
