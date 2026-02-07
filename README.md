一、ReLU经典变体
1. Leaky ReLU（带泄漏的ReLU）
公式

$$LeakyReLU(x) = \begin{cases}x, & x\geq0 \\ \alpha x, & x<0\end{cases}$$

$\alpha$ 为小正数（默认0.01，可手动调整，非可学习参数）

核心改进

x<0 时保留微小梯度（$\alpha$），避免神经元永久死亡。

优缺点

-  优点：解决死亡ReLU问题，计算效率与ReLU接近，无需额外参数量

-  缺点：$\alpha$ 为超参数，需人工调优，负区间梯度固定，适应性一般

适用场景

替换ReLU解决神经元死亡的场景，适配CNN、MLP隐藏层。

2. PReLU（参数化ReLU）
公式

$$PReLU(x) = \begin{cases}x, & x\geq0 \\ \alpha_i x, & x<0\end{cases}$$

核心改进

将LeakyReLU的固定$\alpha$改为可学习参数（$\alpha_i$对应第$i$个通道/神经元），由网络训练自动优化。

优缺点

-  优点：自适应调整负区间梯度，拟合能力更强

-  缺点：增加少量参数量，小数据集下易过拟合

适用场景

大数据集的CNN模型（如ResNet），不推荐小样本场景。

3. RReLU（随机化ReLU）
公式

$$RReLU(x) = \begin{cases}x, & x\geq0 \\ \alpha x, & x<0\end{cases}$$

训练时$\alpha$随机取$[l, u]$内均匀分布（如$l=0.1, u=0.3$），测试时固定为均值。

核心改进

通过随机化引入正则化效果，防止过拟合，同时解决死亡ReLU问题。

优缺点

-  优点：兼顾抗死亡ReLU和正则化，无额外参数量

-  缺点：训练过程存在随机性，模型复现性略低

适用场景

小数据集的分类/回归任务，CNN、MLP均适配。

4. ELU（指数线性单元）
公式

$$ELU(x) = \begin{cases}x, & x\geq0 \\ \alpha (e^x - 1), & x<0\end{cases}$$

$\alpha$ 通常取1，可手动调整。

核心改进

- x<0时梯度为$\alpha e^x$，平滑无硬阈值，缓解死亡ReLU

- 输出零中心化（均值接近0），优化后续层梯度传播

优缺点

-  优点：零中心输出，梯度更稳定，抗死亡ReLU效果优于Leaky ReLU

-  缺点：引入指数运算，计算效率低于ReLU、Leaky ReLU

适用场景

对梯度稳定性要求高的模型，不推荐对推理速度敏感的场景（如端侧设备）。

5. SELU（缩放指数线性单元）
公式
