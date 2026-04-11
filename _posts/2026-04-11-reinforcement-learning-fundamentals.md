---
title: "Reinforcement Learning Fundamentals"
date: 2026-04-11
permalink: /notes/reinforcement-learning-fundamentals/
categories:
  - Notes
tags:
  - reinforcement learning
---

什么是强化学习？智能体通过与环境交互，不断学习，完成特定目标。

强化学习研究**智能体**通过与**环境**反复交互、根据反馈调整行为，从而完成目标。核心循环：在状态 \\(s\\) 下，智能体依据策略 \\(\pi\\) 选择动作 \\(a\\)，环境给出奖励 \\(r\\) 与下一状态 \\(s^{\prime}\\)；奖励信号驱动策略改进，如此迭代。

## 基本元素

| 符号 | 含义 |
|------|------|
| 状态 \\(s\\) | 环境描述 |
| 动作 \\(a\\) | 智能体决策 |
| 策略 \\(\pi(a \mid s) = P(A_t=a \mid S_t=s)\\) | 在状态 \\(s\\) 下选动作 \\(a\\) 的概率（随机策略） |
| 奖励 \\(R_{t+1}\\) | 在 \\(t\\) 时刻于状态 \\(S_t\\) 采取 \\(A_t\\) 后，在 \\(t+1\\) 时刻观测到的标量反馈 |
| 转移 \\(P_{s s^{\prime}}^{a}\\) | 在 \\(s\\) 执行 \\(a\\) 后转移到 \\(s^{\prime}\\) 的概率（模型的一部分） |

**状态价值** \\(v_\pi(s)\\)：从状态 \\(s\\) 出发、之后一直遵循策略 \\(\pi\\)，所能获得的折扣回报期望。

**环境动态（模型）**：若已知 \\(P(s^{\prime}, r \mid s, a)\\)，可用于规划；否则为无模型设定。

## 马尔可夫性质

**马尔可夫性**：给定当前状态，下一状态的条件分布不依赖更早的历史：

\\[
P(S_{t+1} \mid S_t) = P(S_{t+1} \mid S_t, S_{t-1}, \ldots, S_1).
\\]

满足该性质的序列决策过程常建模为 **MDP（马尔可夫决策过程）**。

## 状态价值与动作价值

**折扣回报**（从 \\(t\\) 起）：

\\[
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots
\\]

**状态价值**：

\\[
v_{\pi}(s) = \mathbb{E}_{\pi}(G_t \mid S_t=s).
\\]

**动作价值**：

\\[
q_{\pi}(s,a) = \mathbb{E}_{\pi}(G_t \mid S_t=s, A_t=a).
\\]

**关系**（对随机策略求期望）：

\\[
v_{\pi}(s) = \sum_{a \in \mathcal{A}} \pi(a \mid s)\, q_{\pi}(s,a).
\\]

## 强化学习的分类

### Model-Based 与 Model-Free

- **Model-based**：显式或学习环境的转移/奖励模型 \\(P(s^{\prime}, r \mid s, a)\\)，可做规划（如动态规划）。
- **Model-free**：不显式建模型，直接从样本估计价值或策略（如 MC、TD、Q-learning）。

### On-policy 与 Off-policy

学习都需要 **数据采集** 与 **利用数据更新**。区别在于更新时所优化的**目标策略**与产生数据的**行为策略**是否相同。

- **On-policy**：行为策略与要评估/改进的策略一致（如 SARSA）。
- **Off-policy**：可用非当前策略产生的数据学习目标策略（如 Q-learning：行为可 \\(\epsilon\\)-贪心，更新时向“最优动作”的备份看齐）。

### Online policy 与 Offline policy

- **Online**：训练过程中持续与环境交互采样。
- **Offline（批量 / 离线 RL）**：仅固定数据集上学习，训练期不再交互；数据常由未知或次优策略采集。

与 **模仿学习（IL）** 的对比：离线 RL 数据里通常**有奖励**；IL 常用**无奖励**的专家轨迹，且常隐含“专家接近最优”的假设。

### 无监督强化学习

无监督强化学习中，智能体并没有明确的奖励信号。它依赖于自我探索、环境的固有特性，或者一些内在目标来指导其行为。这意味着没有外部的奖励指引，智能体的目标更多是自我生成目标、获取经验或增强与环境的交互。

## DP（动态规划）(Model-Based)

- **策略迭代**：交替 **策略评估**（用贝尔曼方程算 \\(v_\pi\\)）与 **策略改进**（对 \\(v_\pi\\) 贪心），收敛到 \\(v^{\ast}, \pi^{\ast}\\)。
- **价值迭代**：直接迭代贝尔曼**最优**算子更新 \\(V^{\ast}\\)，再从中导出贪心策略 \\(\pi^{\ast}\\)。

## 蒙特卡洛法 (Model-Free)

用**完整回合**的回报样本估计 \\(v_\pi\\) 或 \\(q_\pi\\)：

\\[
G_t = R_{t+1} + \gamma R_{t+2} + \cdots + \gamma^{T-t-1} R_T.
\\]

可用增量形式：

\\[
V(s_t) \leftarrow V(s_t) + \alpha \left[ G_t - V(s_t) \right].
\\]

\\(\epsilon\\)**-贪心**：以 \\(1-\epsilon\\) 概率选当前估计最优动作，以 \\(\epsilon\\) 概率随机探索。

## 时序差分方法(TD)

不必等回合结束，用**自举**（bootstrap）把下一步估计并入目标。

**TD(0)**（状态价值）：

\\[
V(s_t) \leftarrow V(s_t) + \alpha \left( r_{t+1} + \gamma V(s_{t+1}) - V(s_t) \right).
\\]

括号内为 **TD 误差**。多步 TD 可把 \\(n\\) 步回报与自举（bootstrap）结合（如 \\(n\\)-step TD、TD(\\(\lambda\\))）。

## Q-learning

学习 \\(Q(s,a)\\)：在 \\(s\\) 执行 \\(a\\) 后按**最优**策略走下去的期望回报。最优 \\(Q\\) 满足贝尔曼最优方程：

\\[
Q^{\ast}(s,a) = \mathbb{E}\left[ r + \gamma \max_{a^{\prime}} Q^{\ast}(s^{\prime},a^{\prime}) \right].
\\]

**单步更新**：

\\[
Q(s,a) \leftarrow Q(s,a) + \alpha \left( r + \gamma \max_{a^{\prime}} Q(s^{\prime},a^{\prime}) - Q(s,a) \right).
\\]

直观理解：用 **目标** \\(r + \gamma \max_{a^{\prime}} Q(s^{\prime},a^{\prime})\\) 拉向当前估计 \\(Q(s,a)\\)，步长由 \\(\alpha\\) 控制。

## SARSA

用**实际执行**的下一动作 \\(a_{t+1}\\) 做备份，与行为策略一致：

\\[
Q(s_t,a_t) \leftarrow Q(s_t,a_t) + \alpha \left( r_{t+1} + \gamma Q(s_{t+1},a_{t+1}) - Q(s_t,a_t) \right).
\\]

与 Q-learning 对比：Q-learning 的 max 对应“贪心最优备份”；SARSA 对应“当前策略会继续怎么动”的备份，故受探索策略影响更大。

## 从强化学习到深度强化学习

### 为什么我们需要神经网络

状态/动作空间很大时，表格 \\(Q(s,a)\\) 不可行，用神经网络逼近 \\(Q(s,a;\theta)\\) 或策略 \\(\pi(a \mid s;\phi)\\)。

### DQN

- **函数逼近**：\\(Q(s,a;\theta) \approx Q^{\ast}(s,a)\\)。
- **经验回放**：存储 \\((s,a,r,s^{\prime})\\)，随机小批量训练，减弱样本相关性。
- **目标网络** \\(\theta^{-}\\)：定期从在线网络复制或软更新，TD 目标为

\\[
y = r + \gamma \max_{a^{\prime}} Q(s^{\prime},a^{\prime};\theta^{-}),
\\]

以减轻自举目标漂移带来的不稳定。
- **损失**：如 \\(\big(y - Q(s,a;\theta)\big)^2\\)（Huber 等变体也常见）。

训练循环：\\(\epsilon\\)-贪心交互 \\(\rightarrow\\) 存回放 \\(\rightarrow\\) 采样更新 \\(\theta\\) \\(\rightarrow\\) 每隔 \\(N\\) 步同步 \\(\theta^{-}\\)。

<details markdown="1">
<summary>示例代码（CartPole + PyTorch，示意）</summary>

```python
import gymnasium as gym
import torch
import torch.nn as nn
import torch.optim as optim
from collections import deque
import random
import numpy as np

class QNetwork(nn.Module):
    def __init__(self, input_dim, output_dim):
        super().__init__()
        self.fc1 = nn.Linear(input_dim, 64)
        self.fc2 = nn.Linear(64, 64)
        self.fc3 = nn.Linear(64, output_dim)

    def forward(self, x):
        x = torch.relu(self.fc1(x))
        x = torch.relu(self.fc2(x))
        return self.fc3(x)

env = gym.make("CartPole-v1")
input_dim = env.observation_space.shape[0]
output_dim = env.action_space.n

q_network = QNetwork(input_dim, output_dim)
target_network = QNetwork(input_dim, output_dim)
target_network.load_state_dict(q_network.state_dict())

optimizer = optim.Adam(q_network.parameters(), lr=0.001)
criterion = nn.MSELoss()
replay_buffer = deque(maxlen=10000)

batch_size = 64
gamma = 0.99
epsilon = 1.0
epsilon_decay = 0.995
min_epsilon = 0.01
target_update_interval = 1000
learning_starts = 1000
global_step = 0

def train_step(batch):
    states, actions, rewards, next_states, dones = batch
    states = torch.tensor(np.array(states), dtype=torch.float32)
    actions = torch.tensor(actions, dtype=torch.long)
    rewards = torch.tensor(rewards, dtype=torch.float32)
    next_states = torch.tensor(np.array(next_states), dtype=torch.float32)
    dones = torch.tensor(dones, dtype=torch.float32)

    q_values = q_network(states)
    q_value = q_values.gather(1, actions.unsqueeze(1))

    with torch.no_grad():
        next_q_values = target_network(next_states)
        next_q_value = next_q_values.max(1)[0]
        target = rewards + gamma * next_q_value * (1 - dones)

    loss = criterion(q_value.squeeze(), target)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

def sample_experience(batch_size):
    batch = random.sample(replay_buffer, batch_size)
    states, actions, rewards, next_states, dones = zip(*batch)
    return (
        np.array(states),
        np.array(actions),
        np.array(rewards),
        np.array(next_states),
        np.array(dones),
    )

num_episodes = 1000
for episode in range(num_episodes):
    state, _ = env.reset()
    done = False
    total_reward = 0

    while not done:
        if random.random() < epsilon:
            action = env.action_space.sample()
        else:
            state_tensor = torch.tensor(state, dtype=torch.float32).unsqueeze(0)
            action = int(q_network(state_tensor).argmax().item())

        next_state, reward, terminated, truncated, _ = env.step(action)
        done = terminated or truncated
        total_reward += reward
        replay_buffer.append((state, action, reward, next_state, float(done)))
        state = next_state

        if len(replay_buffer) >= learning_starts:
            train_step(sample_experience(batch_size))

        global_step += 1
        if global_step % target_update_interval == 0:
            target_network.load_state_dict(q_network.state_dict())

    epsilon = max(min_epsilon, epsilon * epsilon_decay)
    print(f"Episode {episode+1}, Total Reward: {total_reward}, Epsilon: {epsilon:.3f}")

env.close()
```

</details>

### 基于策略的算法

**目标**：直接参数化 \\(\pi_{\theta}(a \mid s)\\)，用策略梯度上升期望回报。

- **REINFORCE**：用完整回合的蒙特卡洛回报估计梯度，方差较大。
- **Actor-Critic**：**Actor** 输出策略；**Critic** 估计 \\(V(s)\\) 或 \\(Q(s,a)\\)。用 TD 误差等作为优势信号，同时更新两套参数。

<details markdown="1">
<summary>Actor-Critic示例（一步 TD +优势，CartPole）</summary>

```python
import gymnasium as gym
import torch
import torch.nn as nn
import torch.optim as optim
import torch.nn.functional as F

GAMMA = 0.99
LR = 1e-3
HIDDEN_SIZE = 128
MAX_EPISODES = 5000
DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")

class ActorCritic(nn.Module):
    def __init__(self, state_dim, action_dim):
        super().__init__()
        self.fc1 = nn.Linear(state_dim, HIDDEN_SIZE)
        self.actor = nn.Linear(HIDDEN_SIZE, action_dim)
        self.critic = nn.Linear(HIDDEN_SIZE, 1)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        logits = self.actor(x)
        probs = F.softmax(logits, dim=-1)
        value = self.critic(x)
        return probs, value

def train():
    env = gym.make("CartPole-v1")
    state_dim = env.observation_space.shape[0]
    action_dim = env.action_space.n

    model = ActorCritic(state_dim, action_dim).to(DEVICE)
    optimizer = optim.Adam(model.parameters(), lr=LR)

    for episode in range(MAX_EPISODES):
        state, _ = env.reset()
        done = False
        log_probs, values, rewards = [], [], []

        while not done:
            state_tensor = torch.tensor([state], dtype=torch.float32, device=DEVICE)
            probs, value = model(state_tensor)
            dist = torch.distributions.Categorical(probs)
            action = dist.sample()
            log_probs.append(dist.log_prob(action))
            values.append(value)

            next_state, reward, terminated, truncated, _ = env.step(action.item())
            done = terminated or truncated
            rewards.append(reward)
            state = next_state

        log_probs = torch.stack(log_probs)
        values = torch.cat(values).squeeze()

        with torch.no_grad():
            rewards_t = torch.tensor(rewards, dtype=torch.float32, device=DEVICE)
            next_values = torch.cat([values[1:], torch.zeros(1, device=DEVICE)])
            td_targets = rewards_t + GAMMA * next_values

        advantages = td_targets - values
        actor_loss = -(log_probs * advantages.detach()).mean()
        critic_loss = advantages.pow(2).mean()
        loss = actor_loss + critic_loss

        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        if (episode + 1) % 10 == 0:
            print(f"Episode {episode+1}: return={sum(rewards)}, loss={loss.item():.3f}")

    env.close()

if __name__ == "__main__":
    train()
```

</details>

### SAC（Soft Actor-Critic）(最大熵)

**目标**：最大化累积回报的同时增大策略的熵，鼓励探索：

\\[
\pi^{\ast} = \arg\max_\pi \mathbb{E}\left[ \sum_{t=0}^\infty \gamma^t \left( r_t + \alpha H(\pi(\cdot \mid s_t)) \right) \right],
\\]

其中 \\(H(\pi(\cdot \mid s)) = -\mathbb{E}_{a\sim\pi}[\log \pi(a \mid s)]\\)，\\(\alpha\\) 权衡奖励与探索。

**常见设计**：随机策略（如高斯策略）、**双 Q 网络**取 \\(\min\\) 抑制过估计、**目标 Q 网络**（或 EMA）、**经验回放**、可选的**自动调节** \\(\alpha\\)。

**Critic 目标**（含熵正则项）：

\\[
y = r + \gamma \left( \min_{i=1,2} Q_i^{-}(s^{\prime},a^{\prime}) - \alpha \log \pi(a^{\prime} \mid s^{\prime}) \right).
\\]

### PPO

在**同一条轨迹数据**上做多轮小批量更新，用 **概率比** \\(r_t(\theta)=\frac{\pi_{\theta}(a_t \mid s_t)}{\pi_{\theta_{\text{old}}}(a_t \mid s_t)}\\) 的**裁剪**限制策略变化幅度，稳定训练。常配合 **GAE** 估计优势。

<details markdown="1">
<summary>PPO 片段（结构示意）</summary>

```python
import torch
import torch.nn as nn
import torch.optim as optim
import torch.nn.functional as F
from torch.distributions import Normal

class Actor(nn.Module):
    def __init__(self, state_dim, action_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(state_dim, 64),
            nn.Tanh(),
            nn.Linear(64, 64),
            nn.Tanh(),
        )
        self.mu_head = nn.Linear(64, action_dim)
        self.log_std = nn.Parameter(torch.zeros(action_dim))

    def forward(self, state):
        x = self.net(state)
        mu = self.mu_head(x)
        std = self.log_std.exp()
        return mu, std

    def act(self, state):
        mu, std = self.forward(state)
        dist = Normal(mu, std)
        action = dist.sample()
        log_prob = dist.log_prob(action).sum(dim=-1)
        return action, log_prob

class Critic(nn.Module):
    def __init__(self, state_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(state_dim, 64),
            nn.Tanh(),
            nn.Linear(64, 64),
            nn.Tanh(),
            nn.Linear(64, 1),
        )

    def forward(self, state):
        return self.net(state).squeeze(-1)

class PPO:
    def __init__(self, state_dim, action_dim, clip_eps=0.2, gamma=0.99, lam=0.95, lr=3e-4):
        self.actor = Actor(state_dim, action_dim)
        self.actor_old = Actor(state_dim, action_dim)
        self.actor_old.load_state_dict(self.actor.state_dict())
        self.critic = Critic(state_dim)
        self.opt_actor = optim.Adam(self.actor.parameters(), lr=lr)
        self.opt_critic = optim.Adam(self.critic.parameters(), lr=lr)
        self.gamma, self.lam, self.clip_eps = gamma, lam, clip_eps

    def compute_gae(self, rewards, values, dones):
        adv, gae = [], 0
        next_value = 0
        for r, v, done in zip(reversed(rewards), reversed(values), reversed(dones)):
            delta = r + self.gamma * next_value * (1 - done) - v
            gae = delta + self.gamma * self.lam * (1 - done) * gae
            adv.insert(0, gae)
            next_value = v
        return torch.tensor(adv, dtype=torch.float32)

    def update(self, batch):
        states = torch.stack(batch["states"])
        actions = torch.stack(batch["actions"])
        old_logps = torch.stack(batch["logps"]).detach()

        with torch.no_grad():
            values = self.critic(states)
            adv = self.compute_gae(batch["rewards"], values.tolist(), batch["dones"])
            returns = adv + values
            adv = (adv - adv.mean()) / (adv.std() + 1e-5)

        for _ in range(10):
            mu, std = self.actor(states)
            dist = Normal(mu, std)
            logp = dist.log_prob(actions).sum(-1)
            ratio = torch.exp(logp - old_logps)
            surr1 = ratio * adv
            surr2 = torch.clamp(ratio, 1 - self.clip_eps, 1 + self.clip_eps) * adv
            loss_actor = -torch.mean(torch.min(surr1, surr2))
            value_pred = self.critic(states)
            loss_critic = F.mse_loss(value_pred, returns)
            loss = loss_actor + 0.5 * loss_critic

            self.opt_actor.zero_grad()
            self.opt_critic.zero_grad()
            loss.backward()
            torch.nn.utils.clip_grad_norm_(self.actor.parameters(), 0.5)
            torch.nn.utils.clip_grad_norm_(self.critic.parameters(), 0.5)
            self.opt_actor.step()
            self.opt_critic.step()

        self.actor_old.load_state_dict(self.actor.state_dict())
```

</details>

## 总结

<details markdown="1">
<summary><strong>展开：Q-learning / 基于策略 / SAC 小结</strong></summary>

Q-learning 系列算法：

- DQN 和 SARSA 都属于基于 Q 值的强化学习算法，通过学习状态-动作值函数来选择最优动作。
- DQN 使用 深度神经网络 来近似 Q 函数，而 SARSA 使用的是传统的 表格 Q 值 更新，适用于离散动作空间。
- DQN 是 Off-Policy 算法，而 SARSA 是 On-Policy 算法。

基于策略的算法：

- REINFORCE、PPO 和 A2C 都属于 基于策略的强化学习算法，它们直接优化策略，而不是通过值函数（Q 值）来间接优化。
- PPO 和 A2C 是 Actor-Critic 类算法，它们同时学习一个 策略网络（Actor）和一个 值函数网络（Critic），其中 A2C 是 同步版本，而 PPO 是 异步版本，并通过 目标函数修剪 来稳定训练。
- REINFORCE 是最基础的 策略梯度 方法，它直接优化策略，但 方差较大，训练过程可能不稳定。

Soft Q-learning 系列：

- SAC 是 基于策略的强化学习算法，但是它使用了 最大熵原则 来增强探索性，结合了 Q 值估计 和 策略优化，适用于连续动作空间。

</details>

## 蒙特卡洛树搜索

蒙特卡洛树搜索（MCTS）基于 树结构 进行搜索，每个节点代表一个状态，每条边代表从一个状态到另一个状态的转移。常常被用于 博弈论 和 棋类游戏（如 围棋、象棋）等领域，能够在有限的计算资源下有效地进行决策和策略优化。

<details markdown="1">
<summary><strong>展开：四步流程（选择 / 扩展 / 模拟 / 回传）</strong></summary>

1. **选择（Selection）**：从当前树的根节点出发，选择一条路径逐步进入到树的 叶节点（即未展开的节点），直到达到 树的叶子。在选择过程中，会根据某种 策略 选择子节点。常见的策略是 上置信界（Upper Confidence Bound，UCB），它平衡了 探索（未充分探索的节点）和 利用（已知的好节点）。
2. **扩展（Expansion）**：一旦选择到一个未完全展开的叶节点，算法就会在该节点上扩展（即添加一个或多个新的子节点）。扩展通常是随机的，即选择一个可能的动作或状态进行扩展，生成一个新的节点。
3. **模拟（Simulation）**：从新扩展的节点开始，进行一系列 模拟（或称为回合模拟）。在模拟阶段，算法会通过 随机决策 进行模拟，直到游戏结束（或达到某个终止条件）。模拟的目的是估计该节点的胜率或评估值，这个评估值通过模拟完成后得到的结果来衡量，通常是胜利的概率或奖励的累积。
4. **回传（Backpropagation）**：一旦模拟完成，算法就会回传结果：更新每个节点的统计数据（如胜率、访问次数等）。从叶节点开始，沿着路径回到根节点，将模拟结果更新到路径上所有节点的统计信息中。这个过程是 反向传播，帮助树中各节点调整其评估值。

</details>
