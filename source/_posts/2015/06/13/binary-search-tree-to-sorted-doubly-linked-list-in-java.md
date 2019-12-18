title: Java实现二叉查找树转为排序的双向链表
date: 2015-06-13 17:05:49
updated: 2015-06-13 17:05:49
categories:
  - Algorithm
  - Data Structure
tags:
  - java
---

1.把二元查找树转变成排序的双向链表
题目：
输入一棵二元查找树，将该二元查找树转换成一个排序的双向链表。
要求不能创建任何新的结点，只调整指针的指向。
```
     10
   /    \
  6      14
 / \    /  \
4   8  12  16
```
转换成双向链表
4=6=8=10=12=14=16。
首先我们定义的二元查找树节点的数据结构如下：
struct BSTreeNode
{
int m_nValue; // value of node
BSTreeNode *m_pLeft; // left child of node
BSTreeNode *m_pRight; // right child of node
};

<pre class="lang:java decode:true ">
class BSTreeToDoubleLinkNode {
	//二叉树类
	class BSTreeNode {
		Integer m_value;
		BSTreeNode m_left;
		BSTreeNode m_right;

		//二叉树添加新节点
		public void addNewBSTreeNode(int value) {
			if (m_value == null) {
				m_value = value;
			} else if (m_value > value) {
				if (m_left == null) {
					m_left = new BSTreeNode();
					m_left.addNewBSTreeNode(value);
				} else {
					m_left.addNewBSTreeNode(value);
				}
			} else if (m_value < value) {
				if (m_right == null) {
					m_right = new BSTreeNode();
					m_right.addNewBSTreeNode(value);
				} else {
					m_right.addNewBSTreeNode(value);
				}
			} else {
				System.out.println("repeat node.");
			}
		}
	}

	//双向链表索引
	BSTreeNode pIndex = null;
	BSTreeNode pHead = null;

	//中序遍历二叉树
	public void inOrderTraversal(BSTreeNode pNode) {
		if (pNode == null) {
			System.out.println("pNode is null.");
			return;
		} else {
			if (pNode.m_left != null) {
				inOrderTraversal(pNode.m_left);
			}
			//转换当前节点为链表
			convertToDoubleLinkNode(pNode);

			if (pNode.m_right != null) {
				inOrderTraversal(pNode.m_right);
			}
		}
	}

	//转换当前节点为链表
	public void convertToDoubleLinkNode(BSTreeNode pNode) {
		pNode.m_left = pIndex;
		if (null == pIndex) {
			pHead = pNode;
		} else {
			pIndex.m_right = pNode;
		}
		pIndex = pNode;
		System.out.print(pNode.m_value + " ");
	}

	public static void main(String[] args) {
		BSTreeToDoubleLinkNode b = new BSTreeToDoubleLinkNode();

		BSTreeNode pNode = b.new BSTreeNode();
		pNode.addNewBSTreeNode(10);
		pNode.addNewBSTreeNode(6);
		pNode.addNewBSTreeNode(14);
		pNode.addNewBSTreeNode(4);
		pNode.addNewBSTreeNode(8);
		pNode.addNewBSTreeNode(12);
		pNode.addNewBSTreeNode(16);

		b.inOrderTraversal(pNode);
	}
}
</pre>
