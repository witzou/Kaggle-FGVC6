# :+1: The 1cycle Policy

- original link: https://sgugger.github.io/the-1cycle-policy.html#the-1cycle-policy
- do a minor tweak (fine-tuning)
  - data-augmentation: flip horizontally and random crop (after a [reflection padding](#reflection-padding) of add 4 pixels on each side)
  - reflection padding help author get better result than Leslie

## Using high learning rates

He recommends to do a cycle with two steps of equal lengths, one going from a lower learning rate to a higher one than go back to the minimum. The maximum should be the value picked with the Learning Rate Finder, and the lower one can be ten times lower. Then, the length of this cycle should be slightly less than the total number of epochs, and, in the last part of training, we should allow the learning rate to decrease more than the minimum, by several orders of magnitude. Like this:

![](https://sgugger.github.io/images/art5_lr_schedule.png)

Leslie doesn't recommend to switch to a higher value directly, however, but to rather slowly go there linearly, and to take as much time going up as going down.

What he observed during his experiments is that the during the middle of the cycle, the high learning rates will act as regularization method, and keep the network from overfitting. They will prevent the model to land in a steep area of the loss function, preferring to find a minimum that is flatter.

For instance:

![](https://sgugger.github.io/images/art5_losses.png)

In this graph, the learning rate was rising from 0.08 to 0.8 between epochs 0 and 41, getting back to 0.08 between epochs 41 and 82 then going to one hundredth of 0.08 in the last few epochs. We can see how the validation loss gets a little bit more volatile during the high learning rate part of the cycle (epochs 20 to 60 mostly) but the important part is that on average, the distance between the training loss and the validation loss doesn't increase. We only really start to overfit at the end of the cycle, when the learning rate gets annihilated.

## Cyclical momentum

To accompany the movement toward larger learning rates, Leslie found in his experiments that __decreasing the momentum led to better results__. This supports the intuition that in that part of the training, we want the SGD to quickly go in new directions __to find a flatter area, so the new gradients need to be given more weight__. 

![](https://sgugger.github.io/images/art5_full_schedule.png)

In practice, he recommends to pick two values likes 0.85 and 0.95, and decrease from the higher one to the lower one when we increase the learning rate, then go back to the higher momentum as the learning rate goes down.

:exclamation: Blog advice is Even if using cyclical momentum always gave slightly better results, I didn't find the same gap as in the paper between using a constant momentum and cyclical ones.

## All the other parameters matter

- How Do You Find A Good Learning Rate: https://sgugger.github.io/how-do-you-find-a-good-learning-rate.html

The way we tune all the other hyper-parameters of the model will impact the best learning rate. That's why when we run the Learning Rate Finder, it's very important to use it with the exact same conditions as during our training. __For instance different batch sizes or weight decays will impact the results__:

![](https://sgugger.github.io/images/art5_wds.png)

This can be useful to set some hyper-parameters. For instance, with weight decay, Leslie's advice is to run the learning rate finder for a few values of weight decay, and __pick the largest one that will still let us train at a high maximum learning rate__. This is how we can come up with the 10−4 used in our experiments.

In his opinion, __the batch size should be set to the highest possible value to fit in the available memory__. Then the other hyper-parameters we may have (__dropout__ for instance) can be tuned the same way as weight decay, or just by trying on a cycle and see the results they give. The only thing is to never forget to re-run the Learning Rate Finder, especially when deciding to pick a strategy with an aggressive learning rate close to the maximum possible value.

Training with the 1cycle policy at high learning rates is a method of regularization in itself, so we shouldn't be surprised if we have to reduce the other forms of regularization we were previously using when we put it in place. It will however be more efficient, since we can train for a long time at large learning rates.

# supplement

## reflection padding

:+1: Torch Blog Recommendation(#how-do-you-find-a-good-learning-rate)

`ReflectionPad2d`是 paddingLayer ，padding 的方式多种，可以是指定一个值，也可以是不规则方式，即给出一个四元组

输出大小的计算方式如下：

`Ho = Hi + paddingTop + paddingBottom` 

`Wo = Wi + paddingLeft + paddingRight`

```python
input = torch.randn(64, 3, 220, 220) # input size

# 4-tuple
pad = nn.ReflectionPad2d((3, 3, 5, 5)) # a mirror padding of 3 pixels on the top and bottom
                                       # a mirror padding of 5 pixels on the left and right
output = pad(input) # size(64, 3, 230, 226)

# int
pad = nn.ReflectionPad2d(3)
output = pad(input) # size(64, 3, 226, 226)
```

## How Do You Find A Good Learning Rate

- Reprinted from: https://sgugger.github.io/how-do-you-find-a-good-learning-rate.html
- zhihu: https://zhuanlan.zhihu.com/p/34281842

```python
# Use lr_find() to find highest learning rate where loss is still clearly improving
learn.lr_find()
# check the plot to find the learning rate where the losss is still improving
learn.sched.plot()
```
