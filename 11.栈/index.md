# 11. 栈




<!--more-->

```python
class Stack:
    def __init__(self):
        self.li = []

    # 压栈
    def push(self, item):
        self.li.append(item)

    # 出栈
    def pop(self):
        return self.li.pop()

    # 查看栈顶元素
    def peek(self):
        return self.li[-1]

    # 判断是否为空
    def is_empty(self):
        return self.li == []

    # 查看栈的大小
    def size(self):
        return len(self.li)


if __name__ == "__main__":
    stack = Stack()
    stack.push(1)
    stack.pop()

```


