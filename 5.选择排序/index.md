# 5. 选择排序




<!--more-->

```python
def func(li):
    for j in range(len(li)-1):
        max_index = 0  # 每一轮都假设列表中的第一个元素是最大的
        for i in range(len(li)-j):
            if li[i] > li[max_index]:
                max_index = i
        # 把最大的数放在列表最后一位, 减j代表有序区就不用再排序了
        li[len(li)-1-j], li[max_index] = li[max_index], li[len(li)-1-j]


a = [31, 12, 2, 5, 1, 8]
func(a)
print(a)

```


