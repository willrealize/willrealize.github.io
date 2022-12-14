# 6. 插入排序




<!--more-->

```python
def func(li):
    for i in range(1, len(li)):  # 从第二轮开始, 也是从第二个元素跟第一个开始比
        while i > 0:
            if li[i] < li[i-1]:  # 当前元素比上一个元素小就换位置
                li[i], li[i-1] = li[i-1], li[i]
            else:  # 列表的前面为有序区, 如果当前元素比有序区的最大值还大, 就没必要继续了
                break
            i -= 1  # 当无序区有元素插进来, 会依次根前面的比, 直到在有序区找到合适的位置


a = [55, 11, 2, 3, 1, 15]
func(a)
print(a)

```


