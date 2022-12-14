# 14. 双向链表




<!--more-->

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.pre = None
        self.next = None


class DoubleLinkedList:
    def __init__(self, node=None):
        if node is None:
            self.head = None
        else:
            self.head = Node(node)

    def add(self, item):  # 在链表头部添加元素
        new_node = Node(item)
        if self.head is None:
            self.head = new_node
        else:
            new_node.next = self.head
            self.head.pre = new_node
            self.head = new_node

    def append(self, item):  # 在链表尾部添加元素
        new_node = Node(item)
        if self.head is None:
            self.head = new_node
        else:
            current_node = self.head
            while current_node.next:
                current_node = current_node.next
            current_node.next = new_node
            new_node.pre = current_node

    def insert(self, pos, item):
        pos = self.func(pos)
        if pos == 0:
            self.add(item)
        elif pos >= self.length():
            self.append(item)
        else:
            new_node = Node(item)
            current_node = self.current_node(pos)
            new_node.pre = current_node.pre  # 从一个节点前面插入新节点, 要先连新节点和当前节点前面的节点
            current_node.pre.next = new_node
            new_node.next = current_node
            current_node.pre = new_node

    def remove(self, pos):
        pos = self.func(pos)
        if pos >= self.length():
            pos = self.length() - 1
        current_node = self.current_node(pos)
        if pos == 0:
            self.head = self.head.next
        elif pos == self.length() - 1:
            current_node.pre.next = None
        else:
            current_node.next.pre = current_node.pre
            current_node.pre.next = current_node.next

    def update(self, pos, item):
        pos = self.func(pos)
        if pos >= self.length():
            pos = self.length() - 1
        current_node = self.current_node(pos)
        current_node.data = item

    def length(self):  # 查看链表长度
        length = 0
        current_node = self.head
        while current_node:
            current_node = current_node.next
            length += 1
        return length

    def travel(self):  # 遍历链表
        current_node = self.head
        while current_node:
            print(current_node.data, end=' ')
            current_node = current_node.next

    def current_node(self, pos):
        current_node = self.head
        index = 0
        while index < pos:
            current_node = current_node.next
            index += 1
        return current_node

    def func(self, pos):  # 这个函数是为了支持负索引
        if pos < 0:
            if -pos <= self.length():  # 把负索引转为正索引
                pos = self.length() + pos
            else:  # 如果负索引超过总长, 就把索引置为0
                pos = 0
        return pos


db_linked_list = DoubleLinkedList()
db_linked_list.add(1)
db_linked_list.append(2)
db_linked_list.insert(1, 11)
db_linked_list.remove(1)
db_linked_list.update(1, 33)
db_linked_list.travel()

```


