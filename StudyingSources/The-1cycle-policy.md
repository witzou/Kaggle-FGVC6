# The 1cycle Policy

- minor tweak (fine-tuning)

  - data-augmentation: flip horizontally and random crop (after a [reflection padding](#reflection-padding) of add 4 pixels on each side)
  - reflection padding help author get better result than Leslie

# supplement

## reflection padding

:+1: Torch Blog Recommendation: https://blog.csdn.net/dgyuanshaofeng/article/details/80345103

`ReflectionPad2d`是 paddingLayer ，padding 的方式多种，可以是指定一个值，也可以是不规则方式，即给出一个四元组

输出大小的计算方式如下：

`Ho = Hi + paddingTop + paddingBottom` 

`Wo = Wi + paddingLeft + paddingRight`

```python
input = torch.randn(64, 3, 220, 220) # input size

# 4-tuple
pad = nn.ReflectionPad2d((3, 3, 5, 5)) # a minor padding of 3 pixels on the top and bottom
                                       # a minor padding of 5 pixels on the left and right
output = pad(input) # size(64, 3, 230, 226)

# int
pad = nn.ReflectionPad2d(3)
output = pad(input) # size(64, 3, 226, 226)
```
