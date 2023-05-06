# 第 3 课 练习

给定整数 $x, m$，如果 $x$ 在模 $m$ 下是二次剩余，即存在整数 $s$，使得 $s^{2} \equiv x(\bmod m)$，则记作 $QR(m, x)=1$ ; 如果 $x$ 在模 $m$ 下不是二次剩余，则记作 $QR(m, x)=0$。

<details>
<summary>英文原文</summary>

Given integers $x, m$, write $QR(m, x)=1$ if $x$ is a quadratic residue $\bmod m$, i.e., there exists an integer $s$ such that $s^{2} \equiv x(\bmod m)$; write $QR(m, x)=0$ if $x$ is not a quadratic residue $\bmod m$.

</details>

## 二次非剩余 Quadratic nonresidue

_假定_：证明者可以为所有 $m, x$ 计算 $QR(m, x)$

_公共输入_：正整数 $m, x$

_目标_：证明者想要说服验证者 $QR(m, x)=0$

::: tip 协议

1. Verifier 在与 $m$ 互质的元素中均匀地随机选取一个 $s \in \mathbb{Z}_{m}$，同时抛硬币 $b \gets_R \{0,1\}$。 设
    $$
    y \leftarrow \begin{cases}
        s^{2} x & \text { if } b=0, \\
        s^{2} & \text { if } b=1.
    \end{cases}
    $$
    Verifier 将 $y$ 发送给 Prover 并挑战 Prover 以确定 $b$。

2. Prover 计算 $QR(m, y)$ 并将其值发送回 Verifier

3. 如果 Verifier 从 Prover 收到的值确实等于 $b$，则 Verifier 接受； 否则它拒绝。

::::

您需要检查：

- (a) **完备性**：如果 $QR(m, x)=0$ 并且双方都按照协议行事，那么验证者总是接受。

- (b) **可靠性**：如果 $QR(m, x)=1$，那么无论 Prover 做什么（Prover 不必遵循协议），Verifer 都会以 $\geq 1 / 2$ 的概率拒绝

<details>
<summary>英文原文</summary>

_ASSUMPTION_: The Prover can compute $QR(m, x)$ for all $m, x$

_COMMON INPUT_: positive integers $m, x$

_GOAL_: the Prover wants to convince the Verifer that $QR(m, x)=0$

::: tip PROTOCOL

1. The Verifier picks a random $s \in \mathbb{Z}_{m}$ uniformly among elements relatively prime to $m$, and also tosses a coin $b \gets_R \{0,1\}$. Set

$$
y \leftarrow \begin{cases}
    s^{2} x & \text { if } b=0, \\
    s^{2} & \text { if } b=1.
\end{cases}
$$

  The Verifier sends $y$ to the Prover and challenges the Prover to determine $b$.

2. The Prover computes $QR(m, y)$ and sends its value back to the Verifier

3. The Verifier accepts if the value it received from the Prover is indeed equal to $b$; otherwise it rejects.

:::

You are asked to check:

- (a) **Completeness**: if $QR(m, x)=0$ and both parties behave according to the protocol, then the Verifier always accepts.

- (b) **Soundness**: if $QR(m, x)=1$, then no matter what the Prover does (the Prover does not have to follow the protocol), the Verifer rejects with probability $\geq 1 / 2$

</details>

答：(1) **完备性**：如果$QR(m, x)=0$，即$x$在模$m$下是二次非剩余。Verifier抛出硬币，有两种可能。

(a)如果$b=0$，$y=s^2x$，Prover是诚实的，计算$QR(m,y)=QR(m,s^2x)$，由于$QR(m,x)=0$，则一定有$QR(m,s^2x)=0$，因此Verifier接收到的就是0，验证$QR(m,y)=0=b$，验证者accept。

(b)如果$b=1$，$y = s^2$，Prover计算$QR(m,y)=QR(m,s^2)$。$s^2$一定是模$m$的二次剩余，因为根据定义，存在一个数$s$，使得$s^2 = s^2 \mod m$。因此$QR(m,s^2)=1$，Verifier就会接收到1，验证$QR(m,y)=1=b$，验证者accept。

综上，验证者总是接受的。

(2) **可靠性**：如果$QR(m,x)=1$，则$x$是模$m$的二次剩余。如果Verifier选择$b=0$，发送$y = s^2x$，$QR(m,y)=QR(m,s^2x)=1 \neq b$；如果Verifier选择$b=1$，发送$y=s^2$，$QR(m,y)=QR(m,s^2)=1=b$。对于Prover来说，如果诚实计算$QR(m,y)$，只会有$1/2$的概率通过验证，如果不诚实计算$QR(m,y)$，不会有超过$1/2$概率通过验证。最终，Verifier都会以超过$1/2$的概率拒绝。

## 二次剩余 Quadratic residue

_公共输入_：正整数 $m, x$ 且满足 $\operatorname{gcd}(m, x)=1$

_私有输入_：证明者知道一个秘密整数 $s$ 使得 $s^{2} \equiv x \bmod m$

_目标_：证明者想要说服验证者 $QR(m, x)=1$ 而不透露有关 $s$ 的任何信息

（请注意，证明者可以通过向验证者发送 $s$ 轻松说服验证者 $QR(m, x)=1$，但这会完全暴露 $s$。）

::: tip 协议

1. Prover 在与 $m$ 互质的剩余中随机选择一个均匀的 $t \in \mathbb{Z}_{m}$。 Prover 将 $y \leftarrow x t^{2} \bmod m$ 发送给 Verifier。

1. Verifier 抛硬币 $b \gets_R \{0,1\}$ 并将 $b$ 发送给 Prover。

1. Prover 收到 $b$ 并将$u$值发送给 Verifier
    $$
    u \leftarrow \begin{cases}
        t & \text { if } b=0, \\
        s t & \text { if } b=1.
    \end{cases}
    $$

1. 验证者接受证明如果
    $$
    y \equiv \begin{cases}
        u^2 x \bmod m & \text { if } b=0, \\
        u^2 \bmod m & \text { if } b=1.
    \end{cases}
    $$
    否则拒绝。

::::

您需要检查：

- (a) **完备性**：如果双方都按照协议行事，那么 Verifier 总是接受。

- (b) **可靠性**：如果 $QR(m, x)=0$（特别是，Prover 不知道任何有效的 $s$ ），那么无论 Prover 做什么，Verifier 都以 $\geq 1 / 2$ 的几率拒绝。

- (b\*) **知识可靠性**：对于任何使得Verifier接受的证明算法，如果允许Verifier同时查询$b\in\{0,1\}$的两个值（对于 相同的 $t$ )，则 Verifier 可以从 Prover 中提取秘密 $s$。 （这里的重点是除非证明者“知道”$s$，否则证明者无法说服验证者。）

- (c) **零知识**：无论验证者做什么（验证者的行为可能与协议不同），验证者都可以在不与证明者交互的情况下自行模拟整个交互，这样如果验证者接受，那么记录下来的信息与实际交互几乎不可区分。

     提示：想象模拟器自己生成一个随机的 $u$ 而不是从 Prover 获取 $u$。

     （这部分有点棘手，因为允许对抗性验证器的行为与协议不同 - 请记住我们试图展示的是，无论验证器做什么，它都不会从证明者那里学到关于 $s$ 的信息 验证者不可能简单地自行生成。如果您想查看解决方案，请参阅 [Barak 笔记中的定理 13.6](https://files.boazbarak.org/crypto/lec_14_zero_knowledge.pdf) 的证明。）

<details>
<summary>英文原文</summary>

_COMMON INPUT_: positive integers $m, x$ with $\operatorname{gcd}(m, x)=1$

_PRIVATE INPUT_: the Prover knows a secret integer $s$ such that $s^{2} \equiv x \bmod m$

_GOAL_: the Prover wants to convince the Verifier that $QR(m, x)=1$ without revealing any information about $s$

(Note that the Prover can easily convince the Verifier that $QR(m, x)=1$ by sending $s$ to the Verifier, but this would reveal $s$ completely.)

::: tip PROTOCOL

1. The Prover picks a random $t \in \mathbb{Z}_{m}$ uniform among residues relatively prime to $m$. The Prover sends $y \leftarrow x t^{2} \bmod m$ to the Verifier.

1. The Verifier flips a coin $b \gets_R \{0,1\}$ and sends $b$ to the Prover.

1. The Prover receives $b$ and sends to the Verifier the value of
    $$
    u \leftarrow \begin{cases}
        t & \text { if } b=0, \\
        s t & \text { if } b=1.
    \end{cases}
    $$

1. The Verifier accepts if
    $$
    y \equiv \begin{cases}
        u^2 x \bmod m & \text { if } b=0, \\
        u^2 \bmod m & \text { if } b=1.
    \end{cases}
    $$
    and rejects otherwise.

:::

You are asked to check:

- (a) **Completeness**: If both parties behaves according to the protocol, then the Verifier always accepts.

- (b) **Soundness**: If $QR(m, x)=0$ (in particular, the Prover does not know any valid $s$ ), then no matter what the Prover does, the Verifer rejects with probability $\geq 1 / 2$.

- (b\*) **Knowledge soundness**: For any Prover algorithm that leads the Verifier to accept, if the Verifier is allowed to simultaneously query both values of $b \in\{0,1\}$ (for the same $t$ ), then the Verifer can extract the secret $s$ from the Prover. (The point here is that the Prover cannot convince the Verifier unless the Prover "knows" $s$.)

- (c) **Zero knowledge**: No matter what the Verifier does (the Verifier could behave differently from the Protocol), the Verifier could have simulated the entire interaction on its own without interacting with the Prover and such that if the Verifier accepts, then the transcript is indistinguishable from the actual interaction.

    Hint: imagine that the simulator generates a random $u$ on its own instead of getting $u$ from the Prover.

    (This part is a little tricky, since the adversarial Verifier is allowed to behave differently from the protocol - remember what we are trying to show is that no matter what the Verifier does, it learns no information about $s$ from the Prover that the Verifier could not have simply generated on its own. See the proof of [Theorem 13.6 in Barak's notes](https://files.boazbarak.org/crypto/lec_14_zero_knowledge.pdf) if you want to see the solution.)

</details>

答：

(a)**完备性**：也就是$QR(m,x)=1$。Verifier选择$b$有两种情况：

(1)$b=0$，此时Prover计算的$u=t$，将$u$发送给Verifier进行验证，等式左边$y = xt^2 \mod m$，等式右边$u^2 x \mod m = t^2x \mod m$，等式两边相等，验证通过。

(2)$b=1$，此时Prover计算$u = st$，将$u$发送给Verifier进行验证，等式左边$y = xt^2 \mod m$，等式右边$u^2 \mod m = s^2t^2 \mod m$，由于$QR(m,x)=1$，因此$s^2 = x \mod m$，从而等式左右两边相等，$y = u^2 \mod m$，验证通过。

(b)**可靠性**：如果$QR(m,x)=0$。

(1)$b=0$，Prover选择$u=t$会通过验证，因为$y=xt^2 = u^2x \mod m$。

(2)$b=1$，Verifier会验证$y = xt^2 = u^2 \mod m$。由于$QR(m,x)=0$，不会存在一个数$s$，使得$s^2 = x \mod m$，将Verifier验证的等式变形为$x = (\frac{u}{t})^2 \mod m$，由于不存在这样的数，因此这里Verifier会验证失败，当然Prover不知道有效的$s$使得$s = \frac{u}{t}$，Verifier会验证失败。

综上，Verifier会以超过$1/2$的概率拒绝。

(b*)**知识可靠性**：Verifier首先选择随机数$b=0$，接收到Prover发送的第一个值$u_1 = t$，接着Verifier时光倒流回到上一步，选择随机数$b=1$，这里$Prover$是无感知的，还是会发送同一个$t$，因此Verifier接收到Prover发送的第二个值$u_2 = st$，此时Verifier将这两个数做除法，$\frac{u_2}{u_1} = \frac{st}{t}=s$，就提取出秘密$s$。

(c)**零知识**：现在我们设计一个模拟器，模拟器的视角和真实交互视角一样，但是模拟器中不知道秘密$s$。

Step1: 随机选择一个随机数$b$。

Step2: 选择一个随机数$u \in \mathbb{Z}_{m}$.

Step3: 根据$b$的值，计算$y$，如果$b=0$，$y=u^2x \mod m$，如果$b=1$，$y = u^2 \mod m$。

Step4: 接着我们时光倒流，回到和Verifier交互的第一步，发送$y$，如果Verifier恰好也是我们在Step1中选择的随机数$b$，那么就继续下面的交互，最终会通过验证。如果Verifier选择的随机数和我们在Step1中选择的随机数不同，我们转到Step1，重新随机选择一个数。这一过程中由于$b$就两个数，因此期望2轮会和Verifier选择的随机数相同，通过验证。

这样的一个模拟器和知道秘密的Prover与Verifier交互，其视角在Verifier看来都是一样的，因此也就证明了零知识。

Note：关于零知识这部分的证明在课程[ZKP MOOC Lecture 1: Introduction and History of ZKP](https://www.youtube.com/watch?v=uchjTIlPzFo&ab_channel=Blockchain-Web3MOOCs)中讲模拟器的部分有详细的说明，其模拟器过程为：

<img src="lecture3/img/QRZK.png" width = "700" height = "350" alt="" align=center />

## 双线性自映射意味着DDH的失效 Self-pairing implies failure of DDH

设 $\mathbb{G}$ 和 $\mathbb{G}_{T}$ 是相同素数阶 $q$ 的阿贝尔群（以加法写出）。 设 $g \in \mathbb{G}$ 为生成元。 假设我们有一个可有效计算的非退化双线性对

$$
e: \mathbb{G} \times \mathbb{G} \rightarrow \mathbb{G}_{T}
$$

给出一个有效的决策算法，给定 $\alpha g, \beta g, y \in \mathbb{G}$（其中 $\alpha, \beta \in \mathbb{Z}_{q}$为未知)，判断是否$\alpha \beta g=y$。

提示：计算与 $g$ 的映射。

此练习表明确定性 Diffie-Hellman (DDH) 假设对于双线性自映射是错误的。

<details>
<summary>英文原文</summary>

Let $\mathbb{G}$ and $\mathbb{G}_{T}$ be abelian groups (written additively) of the same prime order $q$. Let $g \in \mathbb{G}$ be a generator. Suppose that we have an efficiently computable nondegenerate bilinear pairing

$$
e: \mathbb{G} \times \mathbb{G} \rightarrow \mathbb{G}_{T}
$$

Give an efficient algorithm for deciding, given $\alpha g, \beta g, y \in \mathbb{G}$ (with unknown $\alpha, \beta \in \mathbb{Z}_{q}$), whether $\alpha \beta g=y$.

Hint: compute the pairing against $g$.

This exercise shows that the Decisional Diffie-Hellman (DDH) assumption is false for groups with self-pairing.

</details>

答：判断等式$\alpha \beta g = y$是否成立，只需要计算$e(\alpha g, \beta g) = e(y, g)$是否成立。在等式$\alpha \beta g = y$两端同时计算与$g$的映射，得到$e(\alpha \beta g, g) = e(\alpha g, \beta g) = e(y, g)$。

## BLS 签名聚合 BLS signature aggregation

::: tip 设置

1. 映射 Pairing $e: \mathbb{G}_{0} \times \mathbb{G}_{1} \rightarrow \mathbb{G}_{T}$，其中$\mathbb{G}_{0} , \mathbb{G}_{1}, \mathbb{G}_{T}$ 是素数阶 $p$ 的阿贝尔群（以加法写出），生成元分别是 $g_{0}, g_{1}, g_{ T}$

1. 哈希函数 $H$ 在 $\mathbb{G}_{1}$ 中取值
::::

BLS签名方案定义为：

- $\operatorname{KeyGen()}$: 选择随机（私钥）$sk=\alpha \leftarrow{ }_{R} \mathbb{Z}_{q}$ 并设置（公钥）$pk \leftarrow \alpha g_{0} \in \mathbb{G}_{0}$
- $\operatorname{Sign}(sk, m)$: 输出 $\sigma \leftarrow \alpha H(m) \in \mathbb{G}_{1}$ 作为消息 $m$ 上的签名
- $\operatorname{Verify}(pk, m, \sigma)$ ：如果 $e\left(g_{0}, \sigma\right)=e(pk, H(m))$，则接受。

检查验证算法是否始终能接受正确签名。 还争辩说，如果给伪造者的是 $pk$ 而不是 $sk$，那么为任何消息 $m$（伪造者可以自由选择 $m$ ）想出一个伪造的签名 $\sigma$ 是计算上不可行的。 这在选择的消息攻击下被称为存在不可伪造。 你能指定一些计算难度假设吗？

请检查验证算法是否总是接受正确的签名。并论证：在给定$pk$但没有$sk$的情况下，（伪造者可以选择消息）对于任何消息$m$，构造出一个伪造的签名$\sigma$是计算上不可行的 。这被称为 _在选择消息攻击下的存在性不可伪造性_ 。您能找到一些计算难度假设吗？

**签名聚合**。 给定三元组 $\left(pk_{i}, m_{i}, \sigma_{i}\right)$ 其中 $i=1, \ldots, n$（即来自 $n$ 个用户），我们可以聚合这些 $n$ 个签名 $\sigma_{1}, \ldots, \sigma_{n}$ 通过简单地在 $\mathbb{G}_{1}$ 中求和，将其合并为一个签名：

$$
\sigma \leftarrow \sigma_{1}+\cdots \sigma_{n} \in \mathbb{G}_{1}
$$

给定 $\left(pk_{1}, m_{1}\right), \ldots,\left(pk_{n}, m_{n}\right)$ 和聚合签名 $\sigma$，表明我们可以 通过检查来验证确实所有 $n$ 用户都签署了他们的消息

$$
e\left(g_{0}, \sigma\right)=e\left(pk_{1}, H\left(m_{1}\right)\right)+\cdots+e\left(pk_{n}, H\left(m_{n}\right)\right)
$$

注意：由于 _恶意公钥攻击_ ，上述签名聚合方案不安全。 使方案安全的一种方法是确保所有消息 $m_{i}$ 是不同的，例如，要求每个 $m_{i}$ 以用户的公钥 $pk_{i}$ 开头。

有关 BLS 签名聚合的更多信息，请参阅 <https://crypto.stanford.edu/~dabo/pubs/papers/BLSmultisig.html>


<details>
<summary>英文原文</summary>

::: tip SETUP

1. Pairing $e: \mathbb{G}_{0} \times \mathbb{G}_{1} \rightarrow \mathbb{G}_{T}$, where $\mathbb{G}_{0}, \mathbb{G}_{1}, \mathbb{G}_{T}$ are abelian groups (written additively) of prime order $p$, with generators $g_{0}, g_{1}, g_{T}$ respectively

1. Hash function $H$ taking values in $\mathbb{G}_{1}$
:::

The BLS signature scheme is defined as:

- $\operatorname{KeyGen()}$: choose random (secret key) $sk=\alpha \leftarrow{ }_{R} \mathbb{Z}_{q}$ and set (public key) $pk \leftarrow \alpha g_{0} \in \mathbb{G}_{0}$
- $\operatorname{Sign}(sk, m)$: output $\sigma \leftarrow \alpha H(m) \in \mathbb{G}_{1}$ as the signature on the message $m$
- $\operatorname{Verify}(pk, m, \sigma)$ : accept if $e\left(g_{0}, \sigma\right)=e(pk, H(m))$.

Check that the verification algorithm always accepts a correctly signed signature. Also argue that it is computational infeasible to come up with a forged signature $\sigma$ for any message $m$ (the forger is free to chose $m$ ) if the forger is given $pk$ but not $sk$. This is called _existentially unforgeable under a chosen message attack_. Can you specify some computational hardness assumptions?

**Signature aggregation**. Given triples $\left(pk_{i}, m_{i}, \sigma_{i}\right)$ for $i=1, \ldots, n$ (coming from $n$ users), we can aggregate these $n$ signatures $\sigma_{1}, \ldots, \sigma_{n}$ into a single signature by simply taking their sum in $\mathbb{G}_{1}$ :

$$
\sigma \leftarrow \sigma_{1}+\cdots \sigma_{n} \in \mathbb{G}_{1}
$$

Given $\left(pk_{1}, m_{1}\right), \ldots,\left(pk_{n}, m_{n}\right)$ and the aggregate signature $\sigma$, show that we can verify that indeed all $n$ users have signed their messages by checking that

$$
e\left(g_{0}, \sigma\right)=e\left(pk_{1}, H\left(m_{1}\right)\right)+\cdots+e\left(pk_{n}, H\left(m_{n}\right)\right)
$$

Note. The above signature aggregation scheme is not secure due to a _rogue public key attack_. One way to make the scheme secure is to ensure that all messages $m_{i}$ are distinct, for example, by requiring that each $m_{i}$ starts with the user's public key $pk_{i}$.

For more on BLS signature aggregation, see <https://crypto.stanford.edu/~dabo/pubs/papers/BLSmultisig.html>

</details>

答：验证算法始终能接受正确的签名。因为在$Verifiy(pk,m,\sigma)$这一步，等式左端$e(g_0,\sigma) = e(g_0, \alpha H(m))$，等式右端$e(pk,H(m)) = e(\alpha g_0, H(m))= e(g_0,\alpha H(m))$，等式两端相等，会始终通过算法验证。

选择消息攻击下的存在性不可伪造性：攻击者想要伪造签名$\sigma \leftarrow \alpha H(m)$，首先在多项式时间计算出$\alpha$是困难的，因为知道公钥和生成员$g_0$在多项式时间内计算出私钥是离散对数困难问题。其次，攻击者想要任意选择消息$m$使得$H(m)$等于一个特定的值，这在多项式时间内也无法做到，因为哈希函数具有抗碰撞性，无法在多项式时间内找到一个$m$，使得$H(m)$等于一个事先给定的值。