# 精细化分类

 __Briefly intro: from inter-class difference to intra-class difference__
 
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
- :question:所感
  - 原文目标函数的意义是：最小化分类损失，最大化奖励函数（是否值得采纳？）
  - Attention regions的形状过于固定，并不能适应物体的多尺度变化。可以采用类似于faster-RCNN的方式，先验框+ROI pooling，可以进一步提高准确率
  - 
# RA-CNN
Recurrent Attention Convolutional Neural Network”（RA-CNN，基于递归注意力模型的卷积神经网络）
