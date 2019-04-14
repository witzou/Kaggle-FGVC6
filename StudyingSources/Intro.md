# 精细化分类

 - __Briefly intro__: from inter-class difference to intra-class difference
 - Reference
 https://blog.csdn.net/qq_25439417/article/details/82764183
 - 机器视觉Attention机制的研究: https://zhuanlan.zhihu.com/cvattention
## [CVPR 2015]  Bilinear CNN Models for Fine-grained Visual Recognition

- 若干注意点
  - __Local + Global__: 细粒度识别基本上就是同时使用全局信息（直接构造整幅图像的特征表示，如：Bag-of-Visual-Words）和局部信息（先对局部定位，之后提取其特征，获得图像特征描述）的分类任务。对于花的细粒度识别来说，全局信息 (global) 就是用户拍摄的整张图像，而局部信息 (local) 则是图像中的花或花的重要部位。这两部分信息都包括在整张图像中，我们希望模型根据整张图像预测出具体的细分类别

  - __Labeling__: 借助知识图谱对世界上的花卉名字进行科学的科、属、种划分，建立了一个非常专业的花卉类别库。而对于其它巨量优质的花卉图像，标注人员通过权威样本库中的文字描述，并在中科院老师的帮助下，根据花卉的叶子、形状、颜色等微观特征进行挑选与标注。此外，百度还进行了标注质量的检查，标注准确率在 95% 以上。最后，这些巨量的标注数据包含了花卉的整体图像和对应的精细品种

  - __Weakly-supervised__: 一种基于弱监督信息的细粒度识别方法，不需要标注局部特征

- 论文简析
  - 定义：双线性CNN模型：包含两个特征提取器，其输出经过外积(外积WiKi)相乘、池化后获得图像描述子。
  - 优点： 
    - 该架构能够以平移不变的方式，对局部的对级（pairwise）特征交互进行建模，适用于细粒度分类。
    - 能够泛化多种顺序无关的特征描述子，如 Fisher 向量，VLAD 及 O2P。实验中使用使用卷积神经网络的作为特征提取器的双线性模型。
    - 双线性形式简化了梯度计算，能够对两个网络在只有图像标签的情况下进行端到端训练。
- 参考链接
  - https://blog.csdn.net/u014593748/article/details/79018108
  - 细粒度分类 | Bilinear model 以及相关的变形： https://blog.csdn.net/u012426298/article/details/81843405
  - 百度搜索：https://www.baidu.com/s?tn=80035161_2_dg&wd=Bilinear+CNN+Models+for+Fine-grained+Visual+Recognition
- 所感
  - local feature 的选取，已经怎么通过马尔可夫决策过程选择那些重要的 features
  - Interested local feature 选取后，怎么加强 feature-to-feature 的相关性？
  
## [CVPR 2017] Fully Convolutional Attention Networks for Fine-Grained Recognition
- 解读：https://zhuanlan.zhihu.com/p/44941451
- Brief Intro
 - ![](https://pic1.zhimg.com/80/v2-96d15d47bda67f52e8bb3be6716cd7e4_hd.jpg)
 - 该框架有一个主体的特征提取架构（Feature Extraction），两个 Part Attention，用于提取感兴趣区域。最后的结果由三个部分的分类结果做一个 fusion 综合得出。
- :question:所感
  - 原文目标函数的意义是：最小化分类损失，最大化奖励函数（是否值得采纳？）
  - Attention regions的形状过于固定，并不能适应物体的多尺度变化。可以采用类似于faster-RCNN的方式，先验框+ROI pooling，可以进一步提高准确率
  - 
## [2018 ECCV] Multi-Attention Multi-Class Constraint for Fine-grained Image Recognition
- Reprinted from: https://zhuanlan.zhihu.com/p/45345038
- Brief Intro:
 - Ming Sun & Baidu
 - 大体思路：用多个 SE 结构获得部位的 Attention ，再用 N-pair Loss 对这些 Attention 进行约束。使得不同 SE 结构生成不同的部位 Attention ，完成弱监督细粒度图像识别。
 
- 文中提出细粒度图像分类的三个准则：
 - 检测的部位应该很好地散布在物体身上，并提取出多种不相关的features
 - 提取出的每个part feature都应该能够区分出一些不同的类别
 - part extractors（部位检测器）应该能轻便使用
 
- 论文就提出了满足这些准则的弱监督信息分类方案：
 - Attention用什么？ 用SENet的方案，设计one-squeeze multi-excitation module (OSME)来定位不同的部分。和现有的方法不同，现有的方法大多是裁剪出部位，再用额外的网络结构来前馈该特征。SEnet的输出可以看做soft-mask的作用。
 - 如何让Attention学习出多个不同的部位？ 受到度量学习损失的启发（如TribleLoss），提出multi-attention multi-class constraint (MAMC) ，鼓励"same-calss和same-attention"的特征 和 "same-calss和different-attention"的特征 距离更接近。
 - 现有的大多方法需要多个前馈过程或者多个交替训练步骤，而论文的方法是端到端的训练。
 
## [ECCV2018] Learning to Navigate for FGVC
https://zhuanlan.zhihu.com/p/48800265

## [ECCV 2018] [MIT] Pairwise Confusion for Fine-Grained Visual Classification

https://zhuanlan.zhihu.com/p/46499846

## [NIPS 2016] Mask-CNN: Localizing Parts and Selecting Descriptors for Fine-Grained Image Recognition

- Brief Intro:

M-CNN是一个四线模型（four-stream），四个输入分别为完整图像、检测到的头部、检测到的躯干和检测到的完整物体，每条线程通过卷积最后都得到了 deep descriptors ，进而得到`1024-d`向量，将四个向量拼接在一起，通过l2l2正则化、全连接层和softmax，最后得到类别

- ![](https://img-blog.csdn.net/20180516183437914?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI5NDYyODQ5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- Mask-CNN 准确率的提升之路
 - 一开始，输入的图像是 224*224，M-CNN 的准确率有 83.1%；
 - 将输入图像变为 448*448 后，准确率提升到了 85.3%；(图像质量提升对于训练有很好的帮助)
 - 提高 4-stream M-CNN 的输入大小到 448*448 后，准确率反而有些下降；
 - 如果从 relu5_2 层来提取 deep descriptors ，并且用Mask过滤一遍，提取出4096-d向量，再和pool5提取出来的拼在一起，变成一个8096-d向量，后续操作相同。该模型称作“4-stream M-CNN+”，它的准确率提升到了85.4%；
 - 用SVD whitening方法将上述的8096-d向量压缩到4096-d，准确率提升到了85.5%；
 - 如果CNN部分采用和part-stacked CNN一样的 Alex-Net模型，准确率只有78.0%，但还是比part-stacked CNN高。关键是替换后的参数只有9.74M了。


## Weakly Supervised Local Attention Network for Fine-Grained Visual Classification
https://zhuanlan.zhihu.com/p/57086099
# RA-CNN
Recurrent Attention Convolutional Neural Network”（RA-CNN，基于递归注意力模型的卷积神经网络）
