# Game-Theoretic Network IDS

Course project for **Game Theory** (MA14363), Isfahan University of Technology, Spring 2025, Dr. Narimani.

The report studies how an intrusion detection system should spend a limited sampling budget when a smart attacker chooses paths through a network. The setup is a non-cooperative zero-sum game with complete information: the IDS is the defender, the intruder (or a group of them) is the attacker, and the payoff is the chance that the intrusion is detected.

## Model

The network is a directed graph $G = (N, E)$. Each link $e$ has a capacity $f_e$. The IDS cannot inspect every packet; it has a budget $B_s$ of samples and chooses a sampling rate

$$p_e = \frac{s_e}{f_e}$$

on each link, subject to

$$\sum_{e \in E} s_e \le B_s.$$

An attacker who takes a path $P$ is missed with probability $\prod_{e \in P}(1 - p_e)$, so the detection chance on that path is

$$q(P) = 1 - \prod_{e \in P}(1 - p_e).$$

The attacker may mix over paths. The defender may mix over sampling plans. The value of the game is given by a minimax / maximin equality (von Neumann): the IDS wants to maximize the worst-case detection probability, the attacker wants to minimize it.

## What the report does

1. **Single target, several cooperating attackers.** If $n$ attackers try to reach a target and $m$ of them must get through, detection becomes a binomial in the per-path miss probability $\alpha$:

   $$P_{\mathrm{detect}} = \sum_{i=m}^{n} \binom{n}{i} \alpha^{i} (1 - \alpha)^{n-i}.$$

   The report writes the resulting minimax program and simplifies it.

2. **Link to flows.** Under a first-order approximation, the best sampling plan concentrates on a minimum cut. The detection value is essentially $B_s$ divided by the max-flow from the attacker's start node $a$ to the target $t$:

   $$\theta \approx \frac{B_s}{\mathrm{MF}_{t}^{a}(f)}.$$

   Samples are then spread over the min-cut edges in proportion to flow. A small numerical example (edges such as $(C,E)$, $(B,G)$, $(B,D)$) works this out by hand.

3. **Several targets.** When the attacker can aim at any node in a set $\Omega$, the budget is split as $B_s / |\Omega|$ per target. Each target has its own max-flow / min-cut, and the sampling on an edge that sits in several of those cuts is the sum of the per-target shares.

4. **Comparison with naive sampling.** The last chapter contrasts the game-theoretic plan with random sampling and uniform sampling. With a tight budget the naive rules waste samples on links the attacker will never use; the min-cut rule puts the budget on the bottleneck. Plots in the report show detection probability as $B_s$ and the number of attackers change.

The writeup is theoretical: derivations, a couple of worked graphs, and those comparisons. There is no packet-level IDS implementation.

## Files

- `Report.pdf` — project report (Persian, with the English title *Game theoretic models for detecting network intrusions*)
