---
title: "Diffusion 笔记"
date: 2025-08-26 10:00:00 +0800
categories: [Others]
layout: single
author_profile: true
---

# Diffusion Model

---

## 一、前向过程（加噪过程）

扩散模型的核心思想：  
从原始样本 $x_0$ 出发，逐步添加噪声，得到高斯噪声$x_T$ 
整个过程构成一个 **马尔可夫链**：

$$
q(x_{1:T} | x_0) = \prod_{t=1}^T q(x_t | x_{t-1})
$$

其中每一步为高斯分布：

$$
q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1 - \beta_t} x_{t-1}, \beta_t \mathbf{I})
$$

- $\beta_t$：预设的噪声强度序列  
- $\alpha_t$ = 1 - $\beta_t$，$\bar{\alpha}_t = \prod_{s=1}^t \alpha_s$

一步采样公式：

$$
x_t = \sqrt{\bar{\alpha}_t} x_0 + \sqrt{1 - \bar{\alpha}_t} \epsilon, \quad \epsilon \sim \mathcal{N}(0, \mathbf{I})
$$

---

## 二、反向过程（去噪过程）

目标：学习从 $x_T$（纯噪声）到 $x_0$ 的逆过程：

$$
p_\theta(x_{0:T}) = p(x_T)\prod_{t=1}^T p_\theta(x_{t-1} | x_t)
$$

其中：

$$
p_\theta(x_{t-1}|x_t) = \mathcal{N}(x_{t-1}; \mu_\theta(x_t, t), \Sigma_\theta(x_t, t))
$$

模型学习均值（或噪声），训练目标为：

$$
\mathcal{L}_{simple} = \mathbb{E}_{x_0, t, \epsilon}\big[\| \epsilon - \epsilon_\theta(x_t, t)\|^2\big]
$$

---

## 三、训练目标推导：VLB（变分下界）

最大化数据似然：

$$
\log p_\theta(x_0) \geq \mathbb{E}_{q}\Big[\log \frac{p_\theta(x_{0:T})}{q(x_{1:T}|x_0)}\Big]
$$

可分解为：

$$
\mathcal{L}_{VLB} = \mathbb{E}_{q}\Big[
D_{KL}\big(q(x_T|x_0) \| p(x_T)\big)
+ \sum_{t>1} D_{KL}\big(q(x_{t-1}|x_t,x_0)\|p_\theta(x_{t-1}|x_t)\big)
- \log p_\theta(x_0|x_1)
\Big]
$$

实际训练常用简化形式（噪声预测 MSE）替代。

---

## 四、网络结构：U-Net

U-Net 是扩散模型的核心结构。

- **Encoder**：下采样，提取多尺度特征  
- **Timestep Embedding**：编码时间步 $t$，加入网络层  
- **Decoder**：上采样 + Skip Connection  
- **Output**：预测噪声 $\epsilon_\theta(x_t, t)$

> 核心思想：多尺度空间特征 + 时间调制

---

## 五、Flow Matching（流匹配）

### 1️⃣ 基本思想

Flow Matching 是 Diffusion 的连续时间化版本：  
不再加噪，而直接学习样本的时间演化速度场：

$$
\frac{d x_t}{d t} = v_\theta(x_t, t)
$$

模型学习数据从噪声流动到样本的「速度方向」。

---

### 2️⃣ 对比 Diffusion

| 对比项 | Diffusion Model | Flow Matching |
|:--|:--|:--|
| 时间形式 | 离散 | 连续 |
| 数学方程 | SDE（随机微分方程） | ODE（常微分方程） |
| 输出 | 噪声 $\epsilon_\theta$ | 速度 $v_\theta$ |
| 路径 | 随机 | 平滑 |
| 优点 | 稳定、泛化强 | 采样快、连续性好 |

---

### 3️⃣ 训练目标

$$
\mathcal{L}_{FM} = \mathbb{E}_{x_0, x_1, t} \|v_\theta(x_t, t) - v^*(x_t, t)\|^2
$$

$v^*(x_t, t)$ 为理论最优流场，可由线性插值或噪声推导得出。

---

### 4️⃣ 理论关系

- Diffusion（SDE）与 Flow Matching（ODE）是**等价的概率流表示**
- Flow Matching 实际是「确定性连续版的扩散模型」

> ✅ 优点：采样更快、轨迹更平滑、可控性更好。

---

## 六、采样加速方法

### 1️⃣ DDIM（Denoising Diffusion Implicit Models）

DDIM 将随机采样改为确定性采样：

$$
x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \hat{x}_0 + \sqrt{1 - \bar{\alpha}_{t-1}} f(x_t, \epsilon_\theta)
$$

- 不再随机噪声  
- 可用较少步数完成采样（几十步）  
- 路径可复现  

> ✅ 优点：生成更快  
> ❌ 缺点：样本多样性略降

---

### 2️⃣ DPM-Solver（Diffusion Probabilistic Model Solver）

DPM-Solver 基于 ODE 形式，将采样过程视为积分问题：

$$
\frac{d x_t}{d t} = f_\theta(x_t, t)
$$

通过二阶或三阶求解器，可在 **10–20 步** 内生成高质量样本。

> ✅ 极快采样、精度高  
> ✅ 现代 Diffusion 加速主流方法（如 Stable Diffusion Turbo）

---

## 七、Diffusion Policy（扩散策略）

### 1️⃣ 定义

将扩散思想应用于**机器人视觉-动作策略（Visuomotor Policy）**：

$$
p_\theta(a_{1:T} | o_{1:T}) \approx \text{Diffusion Process conditioned on } o_{1:T}
$$

> Diffusion 生成图像，Diffusion Policy 生成**动作轨迹**。

---

### 2️⃣ Multi-modal（多模态行为）

同一视觉输入 → 多种合理输出。  
Diffusion Policy 建模**动作分布**，而非单一动作。

- 支持多解（多路径）  
- 表达不确定性  
- 输出更自然多样

---

### 3️⃣ Action Space Scalability

| 方法 | 输入 | 输出 | 特征 |
|------|------|------|------|
| 强化学习 | 状态 | 动作 | 奖励驱动 |
| 行为克隆 | 状态 | 动作 | 模仿专家 |
| Diffusion Policy | 状态/视觉 | 动作分布 | 生成式多模态输出 |

---

### 4️⃣ Flow Matching 在 Diffusion Policy 中的作用

- 将离散去噪 → 连续流场；
- 加速生成，动作轨迹更平滑；
- 结合机器人动力学，实现实时控制。

> Flow Matching 是现代 Diffusion Policy 的关键加速与稳定模块。

---

## 八、整体总结

| 阶段 | 数学形式 | 模型目标 | 优点 |
|------|-----------|-----------|------|
| 前向扩散 | 加噪 SDE | 构造噪声分布 | 稳定 |
| 反向扩散 | 去噪 SDE | 学习恢复过程 | 泛化强 |
| Flow Matching | ODE 连续流 | 学习速度场 | 平滑快速 |
| DDIM / DPM-Solver | 数值积分 | 加速采样 | 高效 |
| Diffusion Policy | 条件扩散 | 动作生成 | 多模态 |

---

