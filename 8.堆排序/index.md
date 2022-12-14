# 8. 堆排序




<!--more-->

```python
def sift(li, low, high):  # 用来做筛选
    """
    :param li: 构建堆的列表
    :param low: 堆的根节点(也就是堆顶)的索引
    :param high: 堆的最后一个节点的索引
    :return: 直接对列表操作, 不需要返回值
    """

    # 使用parent和child两个变量分别指向父节点和子节点
    parent = low  # parent最开始指向根节点
    child = 2 * parent + 1  # child开始指向左子节点

    while child <= high:
        if child + 1 <= high and li[child] < li[child + 1]:
            child = child + 1

        if li[child] > li[parent]:
            li[child], li[parent] = li[parent], li[child]
            parent = child
            child = 2 * parent + 1
        else:
            break


def heap_sort(li):
    # 第一步: 构建堆
    n = len(li)
    for low in range((n - 2) // 2, -1, -1):  # low一开始指向最后一个父节点
        sift(li, low, n - 1)
    print('构造的堆: ', li)
	
    # 第二步: 排序
    for high in range(n - 1, -1, -1):
        li[0], li[high] = li[high], li[0]
        sift(li, 0, high - 1)


a = [9, 18, 7, 6, 15, 4, 23, 2, 1]
heap_sort(a)
print(a)

```


