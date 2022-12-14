# 2. 查找最小值




<!--more-->

```python
def fun(li):
    min_num = li[0]
    for i in li:
        if i < li[0]:
            min_num = i
    print(min_num)


fun([1, 2, 5, 2, 3])

```


