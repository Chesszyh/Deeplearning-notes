# 生成式对抗网络 (GAN)

## 基本原理与架构

生成式对抗网络 (GANs) 是一种基于可微生成器网络的生成式建模方法。它基于博弈论场景，其中生成器网络与判别器网络竞争。

- **生成器网络**：直接产生样本 $\boldsymbol{x} = G(\boldsymbol{z}; \boldsymbol{\theta}^{(g)})$。$\boldsymbol{z}$ 表示输入到生成器网络的随机噪声，从先验噪声分布$p_z(\boldsymbol{z})$中采样得到。
- **判别器网络**：试图区分从训练数据和生成器抽取的样本，输出概率值 $D(\boldsymbol{x}; \boldsymbol{\theta}^{(d)})$，指示 $\boldsymbol{x}$ 是真实训练样本而不是伪造样本的概率。

## 学习过程

GANs 的学习通过零和游戏的形式化表示，其中判别器的收益函数为 $v(G, D)$，生成器的收益为 $-v(G, D)$。学习的目标是找到使生成器最优的判别器 $G^* = \arg\min_{G} \max_{D} v(G, D)$。$v$ 的默认选择如下：

$$v(G, D)=\mathbb{E}_{\boldsymbol x \sim p_{\text{data}}(x)} \log D(\boldsymbol x) + \mathbb{E}_{\boldsymbol z \sim p_z(\boldsymbol z)} (\log(1 - D(G(\boldsymbol z)))$$ 

或$$v(G, D)=\mathbb{E}_{\boldsymbol x \sim p_{\text{data}}} \log D(\boldsymbol x) + \mathbb{E}_{\boldsymbol x \sim p_{model}} (\log(1 - D(\boldsymbol x)))$$ 

上式定义了一个博弈过程，其中判别器的目标是最大化其正确分类真实和伪造样本的概率，而生成器的目标是欺骗判别器，使其将生成的样本误认为真实样本。在理想情况下，收敛时生成器的样本与实际数据不可区分，判别器输出 $\frac{1}{2}$。

## 训练迭代

每次迭代，执行以下步骤：

- 训练 $k$ 次判别器（其中 $k$ 是一个超参数，通常设置为 1）：
  - 从噪声先验 $p_g(\boldsymbol 𝑧)$ 中采样 $m$ 个噪声样本 ${\boldsymbol 𝑧(1),…,\boldsymbol 𝑧(𝑚)}$。
  - 从数据生成分布 $p_{data}(\boldsymbol x)$ 中采样 $m$ 个真实样本 $\{\boldsymbol 𝑥(1),…,\boldsymbol𝑥(𝑚)\}$。
  - 更新判别器的参数 $𝜃^{(𝑑)}$，通过梯度上升来最大化其损失函数： $∇_{𝜃^{(𝑑)}}\frac{1}{𝑚}∑_{𝑖=1}^𝑚[log⁡𝐷(𝑥^{(𝑖)})+log⁡(1−𝐷(𝐺(𝑧^{(𝑖)})))]$
- 训练1次生成器：
  - 从噪声先验 $p_g(\boldsymbol 𝑧)$ 中采样 $m$ 个噪声样本 ${\boldsymbol 𝑧(1),…,\boldsymbol 𝑧(𝑚)}$。
  - 更新生成器的参数 $𝜃^{(𝑔)}$，通过梯度下降来最小化其损失函数： $∇_{𝜃^{(𝑔)}}\frac{1}{𝑚}∑_{𝑖=1}^𝑚log⁡(1−𝐷(𝐺(\boldsymbol 𝑧^{(𝑖)})))$。

## 代码案例

- 基于MNIST数据集生成手写数字图片
  - keras实现：mnist_gan.py
  - pytorch实现：mnist_gan_pytorch.py
    
    训练过程中生成的图片输出到./mnist_imgs文件夹下，生成的图片如下：
    
    <p>
      <img src="img/mnist_epoch_1.png?raw=true" width="30%">
      <img src="img/mnist_epoch_25.png?raw=true" width="30%">
      <img src="img/mnist_epoch_50.png?raw=true" width="30%">
    </p>
- 生成高斯分布
  - keras实现：simple_gan.ipynb
  - pytorch实现：simple_gan_pytorch.ipynb, gaussian_gan_pytorch.py
    
    gaussian_gan_pytorch.py训练生成的分布图如下：
    <img src="img/gaussian_gan.png?raw=true" width="60%">

## 参考资料
[1] https://blog.slinuxer.com/2016/10/generative-adversarial-networks
[2] https://github.com/SwordYork/simplified-deeplearning/tree/master/GAN
[3] Goodfellow I , Pouget-Abadie J , Mirza M ,et al.Generative Adversarial Nets[J].MIT Press, 2014.DOI:10.3156/JSOFT.29.5_177_2.