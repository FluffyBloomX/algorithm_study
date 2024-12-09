### 解释文本

为什么要开始进行记录？maybe是人总有遗忘的时候而数据不会

那么今天的主题就是我与2024.12.11所写的算法题

这将是一个长期更新的频道，将记录我的成长 ~~视我心情而定吧~~

~~其实主要是练习markdown语法~~



## 题目一 BSTtoDLL

BST即是Binary Search Tree 即二叉搜索树
DLL即是Doubly Linked List 双向链表

### 描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。如下图所示

![image-20241211210448364](img/1.png)

数据范围：输入二叉树的节点数 0≤n≤10000≤*n*≤1000，二叉树中每个节点的值 0≤val≤10000≤*v**a**l*≤1000
要求：空间复杂度O(1)*O*(1)（即在原树上操作），时间复杂度 O(n)*O*(*n*)

注意：

1. 要求不能创建任何新的结点，只能调整树中结点指针的指向。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继
2. 返回链表中的第一个节点的指针
3. 函数返回的TreeNode，有左右指针，其实可以看成一个双向链表的数据结构
4. 你不用输出双向链表，程序会根据你的返回值自动打印输出

**输入描述:**

二叉树的根节点

**返回值描述:**

双向链表中其中一个头节点。

**示例1**

输入：

```
{10,6,14,4,8,12,16}
```

返回值：

```
From left to right are:4,6,8,10,12,14,16;From right to left are:16,14,12,10,8,6,4;
```

说明：

```'''
输入题面图中二叉树，输出的时候将双向链表的头节点返回即可
```

**示例2**

输入：

```
{5,4,#,3,#,2,#,1}
```

返回值：

```
From left to right are:1,2,3,4,5;From right to left are:5,4,3,2,1;
```

说明：

```
                    5
                  /
                4
              /
            3
          /
        2
      /
    1
树的形状如上图       
```

题目来源 :leetcode 题目难度:中等

**题目思路**

当看到这题时 想到二叉搜索树右节点大于左节点 即通过中序遍历即可以实现有序

代码实现如下：
```
class Solution
{
public:
    // 中序遍历，连接双向链表
    void InOrder(Node *root, Node *&prev, Node *&head)
    {
        if (root == NULL)
            return;

        // 递归左子树
        InOrder(root->left, prev, head);

        // 当前节点处理
        if (prev != NULL)
        {
            prev->right = root; // 上一个节点的右指针指向当前节点
            root->left = prev;  // 当前节点的左指针指向上一个节点
        }
        else
        {
            head = root; // 第一个节点设置为头节点
        }

        prev = root; // 更新前一个节点为当前节点

        // 递归右子树
        InOrder(root->right, prev, head);
    }

    Node *treeToDoublyList(Node *root)
    {
        if (root == NULL)
            return NULL; // 处理空树的情况

        Node *prev = NULL;
        Node *head = NULL;

        // 通过中序遍历将树转换为双向链表
        InOrder(root, prev, head);

        // 由于是双向链表，确保尾节点的右指针指向头节点，头节点的左指针指向尾节点
        if (head != NULL && prev != NULL)
        {
            prev->right = head; // 尾节点指向头节点
            head->left = prev;  // 头节点指向尾节点
        }

        return head; // 返回双向链表的头节点
    }
};
```
