# 4. 冒泡排序




<!--more-->

```python
def func(li):
    flag = False  # 在循环之前作个标记
    for j in range(len(li)-1):
        for i in range(len(li)-1-j):
            if li[i] > li[i+1]:
                flag = True  # 如果进入到这个条件, 说明是无序的, 如果没有进入, 代表是有序的
                li[i], li[i+1] = li[i+1], li[i]
		if flag is False:  # 如果是有序的直接break
            break


a = [1, 2, 3, 4, 5]
func(a)
print(a)

```


