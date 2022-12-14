# 3. 二分法查找




<!--more-->

```python
# 使用二分法查找的前提是, 必须是有序列表
def func(li, n):
    left_index = 0
    right_index = len(li)
    while left_index <= right_index:
        mid_index = (left_index + right_index) // 2
        if n == li[mid_index]:
            return mid_index
        if n < li[mid_index]:
            right_index = mid_index - 1
        else:
            left_index = mid_index + 1
    else:
        return "没有这个数"


print(func([1, 2, 54, 77, 88, 111, 231], 211))

```


