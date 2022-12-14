# 10. 计数排序




<!--more-->

```python
import random


def func(li):
    temp = [0 for _ in range(max(li)+1)]
    for i in li:
        temp[i] += 1

    li.clear()
    for index, value in enumerate(temp):
        for j in range(value):
            li.append(index)
    print(li)


a = [random.randint(0, 100) for _ in range(100)]
func(a)

```


