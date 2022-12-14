# 15. 完全二叉树




<!--more-->

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.left_child = None
        self.right_child = None


class Tree:
    def __init__(self):
        self.root = None

    def add(self, item):  # 添加节点
        new_node = Node(item)
        if self.root is None:
            self.root = new_node
            return
        queue = [self.root]
        while queue:
            current_node = queue.pop(0)
            if current_node.left_child is None:
                current_node.left_child = new_node
                return
            else:
                queue.append(current_node.left_child)
            if current_node.right_child is None:
                current_node.right_child = new_node
                return
            else:
                queue.append(current_node.right_child)

    def breadth_first_search(self):
        if self.root is None:
            return
        queue = [self.root]
        while queue:
            current_node = queue.pop(0)
            print(current_node.data, end=' ')
            if current_node.left_child:
                queue.append(current_node.left_child)
            if current_node.right_child:
                queue.append(current_node.right_child)

    def depth_first_search_pre_order(self, node):  # 先(根)序遍历
        if node is None:
            return
        print(node.data, end=' ')
        self.depth_first_search_pre_order(node.left_child)
        self.depth_first_search_pre_order(node.right_child)

    def depth_first_search_in_order(self, node):  # 中(根)序遍历
        if node is None:
            return
        self.depth_first_search_in_order(node.left_child)
        print(node.data, end=' ')
        self.depth_first_search_in_order(node.right_child)

    def depth_first_search_post_order(self, node):  # 后(根)序遍历
        if node is None:
            return
        self.depth_first_search_post_order(node.left_child)
        self.depth_first_search_post_order(node.right_child)
        print(node.data, end=' ')


if __name__ == '__main__':
    tree = Tree()
    for i in range(5):
        tree.add(i)
    tree.breadth_first_search()
    print()
    tree.depth_first_search_pre_order(tree.root)
    print()
    tree.depth_first_search_in_order(tree.root)
    print()
    tree.depth_first_search_post_order(tree.root)

```


