# 12. 队列




<!--more-->

```python
class Queue:  # 队列
    def __init__(self):
        self.li = []

    # 进队
    def enqueue(self, item):
        self.li.append(item)  # 如果频繁进, 用: append+pop(0)
        # self.li.insert(0, item)  # 如果频繁出, 用: insert(0, item)+pop()

    # 出队
    def dequeue(self):
        self.li.pop(0)
        # self.li.pop()

    def is_empty(self):
        return self.li == []

    def size(self):
        return len(self.li)


if __name__ == '__main__':
    queue = Queue()


class CircularQueue:  # 环形队列
    def __init__(self, size=100):
        self.size = size
        self.queue = [0 for _ in range(size)]
        self.rear = 0  # 队尾指针
        self.front = 0  # 队首指针

    def push(self, element):
        if not self.is_filled():
            self.rear = (self.rear + 1) % self.size
            self.queue[self.rear] = element
        else:
            raise IndexError("Queue is filled")

    def pop(self):
        if not self.is_empty():
            self.front = (self.front + 1) % self.size
            return self.queue[self.front]
        else:
            raise IndexError("Queue is empty")

    def is_empty(self):
        return self.rear == self.front

    def is_filled(self):
        return (self.rear + 1) % self.size == self.front


if __name__ == '__main__':
    queue = CircularQueue(5)
    queue.push(1)

    print(queue.queue)

```

Python内置队列模块deque

```python
from collections import deque


# 可以不传参数, 默认为一个空队列
q1 = deque()
# 下面两组是成对使用
q1.append(1)
q1.popleft()

q1.appendleft(1)
q1.pop()


# 可以传一个列表, 和限定队列长度
q2 = deque([1, 2, 3], 3)
q2.append(1)
print(q2)  # [2, 3, 1] 如果队列中的元素已经满了, 会自动弹出

```

内置模块deque的实际应用

```python
from collections import deque


# 可以使用deque实现tail的功能
def tail(n):
    with open('xxx.txt', 'r') as f:
        q = deque(f, n)
        return q


for line in tail(5):
    print(line, end='')

```


