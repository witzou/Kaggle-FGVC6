## [iMaterialist Challenge on Product Recognition at FGVC6, CVPR 2019](https://www.kaggle.com/c/imaterialist-product-2019/)

As online shopping and retail AI become ubiquitous in our daily life,  it is imperative for computer vision systems to __(工程目标)automatically and accurately recognize products based on images at the stock keeping unit (SKU) level__. However, this still remains a challenging problem since there is a __(方案难点)large number of SKU-level categories, many of which are fine-grained, with very subtle differences that cannot be easily distinguished__. At the same time, images of the same product or SKU can often look different under different conditions (*e.g.*, user generated content v.s. professional generated content).

This competition is part of the fine-grained visual-categorization workshop ([FGVC6 workshop](https://sites.google.com/view/fgvc6/home)) at [CVPR 2019](http://cvpr2019.thecvf.com/). By providing a large scale dataset and hosting a competition, [Malong Technologies](https://www.malong.com/en/home) and FGVC workshop organizers encourage computer vision researchers to develop novel algorithms to tackle this interesting problem. Individuals/teams with top submissions will present their work at the FGVC6 workshop and win cash prizes:

 - 1st place: $ 1,500
 - 2nd place: $ 1,000
 - 3rd place: $ 500

## Kaggle
We use Kaggle to hold our competiton: https://www.kaggle.com/c/imaterialist-product-2019/

## Important Dates

:exclamation: 注意参赛时间

- **April 1, 2019** - Competition starts.

- **May 19, 2019** - Entry deadline. You must accept the competition rules before this date in order to compete.

- **May 19, 2019** - Team Merger deadline. This is the last day participants may join or merge teams.

- **May 26, 2019** - Final submission deadline.

All deadlines are at 11:59 PM UTC on the corresponding day unless otherwise noted. The competition organizers reserve the right to update the contest timeline if they deem it necessary.


## Dataset Format and Download

> 总共 2019 个商品类别，分别按照四个等级层次的结构来组织 (ProductTree-level1-level2-level3)，详情见 .json 和 .pdf 文件。每个叶节点对应一个类别 id ，其中类别共享同一个父节点，属于同一个超类

This dataset has a total number of 2,019 product categories, which are organized into a hierarchical structure with four levels. This category tree can be found in [product_tree.json](product_tree.json) and visualized with [product_tree.pdf](product_tree.pdf). Each leaf node corresponds to a category id and categories share the same ancestor belong to a same hyper-class. The tree structure is not involved in the evaluation but could be potentially used during model training.

### Files

The competition data can be download on [**Google Drive**](https://drive.google.com/open?id=18xGkrb0pzgPw7l931r87029W0ORaVzP_) or [**Baidu Pan**](https://pan.baidu.com/s/1u0XeLu30_5zkMCJ2imnmCw) (password: qecd).

:+1: `.json` 文件示例：2003 类别属于 (level1_node1 level2_node3 level3_node7)

```json
        {
            "url": "http://pic3.58cdn.com.cn/zhuanzh/n_v2b6852e1e536b441c9a6104186b53b99f.jpg?w=750&h=0", 
            "id": "573ed20b320d15b3636f4ef00da91712.jpg", 
            "class": 2003
        }
```

The filename of each image is its ```id```.

:+1: 统计结果如下:

| dataset           | number                 | Note            |
|:-------------------|:---------------------|:-------------------|
|train|1,011,532|range from 158 to 1050 images for each category|
|val|10,095|around 5 for each category|
|test|90,834|around 45 for each category|

- [**train.json**](https://drive.google.com/open?id=1b6RyQTaAlQYKhvc-OfnfZjtVJ_XFV8a7) contains ```id,class,url``` of each training image, where you can use the ```url``` to download the corresponding image with filename ```id``` and class label ```class```. __The training data consists of 1,011,532 images from 2,019 categories__ (range from 158 to 1050 images for each category). The training data is collected from the Internet and __contrains some noise__. Only image content is allowed for training (e.g., URLs in the provided dataset are not allowed for training).


- [**val.json**](https://drive.google.com/open?id=1o7JlT6E5ffyUUKWsED0XiSXCCfASmlaU) contains ```id,class,url``` of images in the validation set. __The validation data has 10,095 images__ (around 5 for each category). The validation data is collected from the Internet and has been cleaned by human annotators. In this competition, validation data should only be used for validation and is not allowed for training.

- [**test.json**](https://drive.google.com/open?id=1MF50hOqfYVga0f0uXd5w-5cqCqnreLS5) contains ```id,url``` of images in the test set. __The test data has 90,834 images__ (around 45 for each category). The test data is collected from the Internet and has been cleaned by human annotators.



- Train and validation sets have the same format as shown below:

```
{   
    "images": [   
        {   
            "id" : string,   
            "url": string,
            "class": int   
        },
        ...   
    ]
} 
```

Note that for each image, we only provide URL instead of the image content. __Users need to download the images by themselves.__ Note that the image URLs may become unavailable over time. Therefore we suggest that the participants start downloading the images as early as possible. We are considering image hosting service in order to handle unavailable URLs. We'll update here if that could be finalized.

This year, we omit the names of the labels to avoid hand labeling the test images.

- The test data only has image id and url as shown below:

```
{   
    "images": [   
        {   
            "id" : string,   
            "url": string
        },
        ...   
    ]
} 

```



## Evaluation

The challenge is hosted and evaluated on [Kaggle](https://www.kaggle.com/c/imaterialist-product-2019/). Each image has one ground truth label. We use **top-3 classification error** for evaluation: __An algorithm to be evaluated will produce top 3 labels per image. If the predicted labels contain the groundtruth label, then the error for that image is 0, otherwise, it is 1. The final score is the top-3 error across all images.__

:+1: 注意 submission的格式，按照 `sample_submission.csv` 文件的格式来提交自己的 result 

A sample submission file looks like:

```
id,predicted 
20dc3e9e6ca73cfa3ad4d13d1d7526b8.jpg,11 23 58 
c0ff6e315816bb1b27b48687924d41c9.jpg,123 456 789 
221100dc2fd465098a5a7bdd148c7b14.jpg,2 3 5
```

Please include the header as shown above for correct parsing. Each line corresponds to one test image and the predicted class labels. We provide a sample submission file [**sample_submission.csv**](https://drive.google.com/open?id=1v-vF6841uWN5Emn72t0pDSZL3Z4KQRl-) .

## Rules and Terms of Use

Participants should follow the rules and terms of use of this competition as described [here](https://www.kaggle.com/c/imaterialist-product-2019/rules).

- __Pre-trained models are allowed in the competition.__
- Participants are restricted to train their algorithms on iMaterialist 2019 on Product Recognition training images. Validation data should only be used for validation. __Collecting additional data for the target labels is not allowed. Collecting additional unlabeled data for pretraining is ok. Please specify any and all external data used for training when uploading results.__ （意味着引入无监督学习或者其他有效的方法）
- __Only image content is allowed for training__ (e.g., URLs in the provided dataset are not allowed for training).
__Additional annotation on the provided train and validation data is fine (e.g., bounding boxes, keypoints, etc.).__ Teams should specify that they collected additional annotations when submitting results.
- We ask that you respect the spirit of the competition and __do not cheat__. Hand-labeling is forbidden.

## Organizers

Xintong Han, Malong Technologies

Sheng Guo, Malong Technologies

Weilin Huang, Malong Technologies

Lingshu Kong, Malong Technologies

Matt Scott, Malong Technologies

For any questions please contact iproduct2019@malong.com
