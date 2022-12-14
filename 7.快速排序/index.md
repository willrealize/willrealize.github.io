# 7. 快速排序




<!--more-->

```python
import random


def func(li):
    if len(li) <= 1:  # 把列表一直分到一个元素为止
        return li

    n = li[0]  # 取第一个数作为比较
    left = [i for i in li[1:] if i <= n]  # 比第一个数小的放左边
    right = [j for j in li[1:] if j > n]  # 比第一个数大的放右边
    
    return func(left) + [n] + func(right)  # 递归到列表元素个数为1, 再合并返回


a = [k for k in range(33)]
random.shuffle(a)
print(func(a))

```


