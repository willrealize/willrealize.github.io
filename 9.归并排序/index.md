# 9. 归并排序




<!--more-->

```python
import random


def func(li):
    if len(li) <= 1:
        return li

    mid = len(li) // 2
    left = func(li[0:mid])
    right = func(li[mid:])

    result = []
    while left and right:
        if left[0] <= right[0]:
            result.append(left.pop(0))
        else:
            result.append(right.pop(0))
    result.extend(left), result.extend(right)
    
    return result


a = [i for i in range(111)]
random.shuffle(a)
print(func(a))

```

使用索引的方法

```python
import random


def func(li):
    if len(li) <= 1:
        return li

    mid = len(li) // 2
    left = func(li[0:mid])
    right = func(li[mid:])

    result = []
    left_index, right_index = 0, 0
    while left_index < len(left) and right_index < len(right):
        if left[left_index] <= right[right_index]:
            result.append(left[left_index])
            left_index += 1
        else:
            result.append(right[right_index])
            right_index += 1
    result.extend(left[left_index:]), result.extend(right[right_index:])

    return result


a = [i for i in range(111)]
random.shuffle(a)
print(func(a))

```

然而上面两种方法都不是太好, 因为都要开辟新的空间, 推荐下面写法

```python
import random


def func(li):  # 有返回值的写法
    if len(li) <= 1:
        return li

    mid = len(li) // 2
    left = func(li[:mid])  # 有返回值的话, 要用变量接收
    right = func(li[mid:])

    left_index = 0
    right_index = 0
    li_index = 0
    while left_index < len(left) and right_index < len(right):
        if left[left_index] < right[right_index]:
            li[li_index] = left[left_index]
            left_index += 1
        else:
            li[li_index] = right[right_index]
            right_index += 1
        li_index += 1
    while left_index < len(left):
        li[li_index] = left[left_index]
        left_index += 1
        li_index += 1
    while right_index < len(right):
        li[li_index] = right[right_index]
        right_index += 1
        li_index += 1

    return li


a = [i for i in range(22)]
random.shuffle(a)
print(func(a))


import random


def func(li):  # 无返回值的写法
    if len(li) > 1:
        mid = len(li) // 2
        left, right = li[:mid], li[mid:]
        func(left), func(right)

        left_index, right_index, li_index = 0, 0, 0
        while left_index < len(left) and right_index < len(right):
            if left[left_index] < right[right_index]:
                li[li_index] = left[left_index]
                left_index += 1
            else:
                li[li_index] = right[right_index]
                right_index += 1
            li_index += 1
        while left_index < len(left):
            li[li_index] = left[left_index]
            left_index += 1
            li_index += 1
        while right_index < len(right):
            li[li_index] = right[right_index]
            right_index += 1
            li_index += 1


a = [i for i in range(22)]
random.shuffle(a)
func(a)
print(a)

```


