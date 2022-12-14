# 13. 单向链表




<!--more-->

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None


class SingleLinkedList:
    def __init__(self, data=None):
        if data is None:
            self.head = data
        else:
            self.head = Node(data)

    def add(self, item):  # 在链表头部插入
        new_node = Node(item)
        new_node.next = self.head
        self.head = new_node

    def append(self, item):  # 在链表尾部插入
        new_node = Node(item)
        if self.head is None:
            self.head = new_node
        else:
            current_node = self.head
            while current_node.next:
                current_node = current_node.next
            current_node.next = new_node

    def insert(self, pos, item):  # 任意位置插入
        pos = self.func(pos)
        if pos == 0:
            self.add(item)
        elif pos >= self.length():
            self.append(item)
        else:
            new_node = Node(item)
            index = 0
            pre_node = self.head
            while index < pos - 1:
                pre_node = pre_node.next
                index += 1
            new_node.next = pre_node.next
            pre_node.next = new_node

    def remove(self, pos):  # 删除指定位置元素
        pos = self.func(pos)
        if pos >= self.length():
            pos = self.length() - 1
        if pos == 0:
            self.head = self.head.next
        else:
            pre_node = None
            current_node = self.head
            index = 0
            while index < pos:
                pre_node = current_node
                current_node = current_node.next
                index += 1
            if pos == self.length() - 1:
                pre_node.next = None
            else:
                pre_node.next = current_node.next

    def update(self, pos, item):  # 更新指定位置元素
        pos = self.func(pos)
        if pos >= self.length():
            pos = self.length() - 1
        current_node = self.head
        index = 0
        while index < pos:
            current_node = current_node.next
            index += 1
        current_node.data = item

    def length(self):  # 查看链表的长度
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

    def search(self, item):  # 搜索元素在链表中的位置
        current_node = self.head
        index = 0
        index_li = []
        while current_node:
            if current_node.data == item:
                index_li.append(index)
            current_node = current_node.next
            index += 1
        if len(index_li) > 0:
            print('元素在链表中的索引为: %s' % index_li)
        else:
            print('没有这个元素')

    def func(self, pos):  # 这个函数是为了支持负索引
        if pos < 0:
            if -pos <= self.length():  # 把负索引转为正索引
                pos = self.length() + pos
            else:  # 如果负索引超过总长, 就把索引置为0
                pos = 0
        return pos


# 创建一个空链表链表
linked_list = SingleLinkedList()

# 在链表头部插入一个元素
linked_list.add(1)

# 在链表尾部插入一个元素
linked_list.append(2)

# 在链表任意一个位置插入一个元素
linked_list.insert(32, 3)

# 删除指定位置元素
linked_list.remove(33)

# 更新指定位置元素
linked_list.update(22, 11)

# 搜索元素在链表中出现的位置
linked_list.search(1)

# 遍历链表
linked_list.travel()

```




