---
title: Morris遍历二叉树
date: 2020-09-24 15:23:58
categories:
- Data Structure
tags:
- Data Structure
- C++
mathjax: true
---
# Morris

本篇文章为[Binary-Tree](https://shaoyuanhangyes.github.io/2020/06/03/Binary-Tree/#more)中遍历二叉树的进阶版本

二叉树的普通遍历有递归和迭代的方式 递归会有递归空间开销 迭代会有通过栈实现的空间开销 而Morris遍历可以将非递归遍历中的<b>空间复杂度降为O(1)</b> 利用的则是二叉树中大量的叶结点的左右孩子为空(空闲指针) 来实现的极限缩减空间 思想与线索二叉树很像 但不像线索二叉树一样需要开辟两个指针域

<!-- more --> 
## Morris遍历规则

当前结点为cur 

若cur无左孩子 `cur=cur->right` 

若cur有左孩子 则找到cur的左子树上最右侧的结点 记为mostright mostright初值为cur->left 循环找到最右侧结点再停止
    若mostright->right=NULL 就使mostright->right=cur 并同时cur=cur->left
    若mostright->right=cur 就使mostright->right=NULL 并同时cur=cur->right

从结果上来观察 这个mostright结点是cur结点的前驱结点时 cur就借由这个前驱结点的桥梁回到上一个遍历点

## Morris遍历实例

针对层序遍历结构为{1,2,3,4,5,-1,6}的二叉树实例 来进行Morris遍历

```bash
        1
       / \
      2   3
     / \   \
    4   5   6
```

Ⅰ cur=1 存在左孩子 cur左子树上最右侧的结点为5 则mostright=5 并且5->right=NULL 所以使5->right=cur=1 同时 cur=cur->left=2

Ⅱ cur=2 存在左孩子 cur左子树上最右侧的结点为4 则mostright=4 并且4->right=NULL 所以使得4->right=cur=2 同时 cur=cur->left=4

Ⅲ cur=4 不存在左孩子 所以cur=cur->right <b>此时4->right=2</b> 即cur=2

Ⅳ cur=2 存在左孩子 cur左子树上最右侧的结点为4 则mostright=4 此时4->right=2 所以使得4->right=NULL 同时cur=cur->right=5

Ⅴ cur=5 不存在左孩子 所以cur=cur-right <b>此时5->right=1</b> 即cur=1

Ⅵ cur=1 存在左孩子 cur左子树上最右侧的结点为5 则mostright=5 并且5->right=1 所以使5->right=NULL 同时cur=cur->right=3

Ⅶ cur=3 不存在左孩子 cur=cur->right=6

Ⅷ cur=6 不存在左孩子 结点遍历完毕

## Morris遍历代码

### 先序遍历

```C++
vector<int> Morris_preOrder(TreeNode* root){
    vector<int> res;
    if(!root) return res;
    TreeNode* cur=root;
    TreeNode* mostright=NULL;
    while(cur!=NULL){
        mostright=cur->left;
        if(mostright!=NULL){
            while(mostright->right!=NULL&&mostright->right!=cur) mostright=mostright->right;
            if(mostright->right==NULL){
                mostright->right=cur;
                res.push_back(cur->val);//先序遍历是在向左孩子移动前输出结点值
                cur=cur->left;
                continue;
            }
            else mostright->right=NULL;
        }
        else res.push_back(cur->val);
        cur=cur->right;
    }
    return res;
}
```

### 中序遍历

```C++
vector<int> Morris_inOrder(TreeNode* root){
    vector<int> res;
    if(!root) return res;
    TreeNode* cur=root;
    TreeNode* mostright=NULL;
    while(cur!=NULL){
        mostright=cur->left;
        if(mostright!=NULL){
            while(mostright->right!=NULL&&mostright->right!=cur) mostright=mostright->right;
            if(mostright->right==NULL){
                mostright->right=cur;
                cur=cur->left;
                continue;
            }
            else{
                mostright->right=NULL;
                //res.push_back(cur->val);//中序遍历是在要回溯到上一个结点之前将结点值输出
            }
        }
        //else res.push_back(cur->val);
        res.push_back(cur->val);
        cur=cur->right;
    }
    return res;
}
```

PS: 中序遍历代码中的注释 是第一次设计Morris中序遍历时写的代码 对于结点的输出情况 虽然结果相同 但是代码结构上多余了一个else的判断后输出 过于冗余 因此进行修改  但是第一次设计的代码虽然冗余但更为易读 所以以注释的形式保留下来

### 后序遍历

```C++
class PostOrder{
public:
    TreeNode* reverseEdge(TreeNode* root){//使一串右子树的结点逆序
        TreeNode* pre=NULL;
        TreeNode* next=NULL;
        while(root!=NULL){
            next=root->right;
            root->right=pre;
            pre=root;
            root=next;
        }//一顿操作下来树结构 pre->right=root
        return pre;
    }
    void printEdge(TreeNode* root){//打印结点
        TreeNode* tail=reverseEdge(root);
        TreeNode* cur=tail;
        while(cur!=NULL){
            res.push_back(cur->val);
            cur=cur->right;
        }
        reverseEdge(tail);
    }
    vector<int> Morris_postOrder(TreeNode* root){
        if(!root) return res;
        TreeNode* cur=root;
        TreeNode* mostright=NULL;
        while(cur!=NULL){
            mostright=cur->left;
            if(mostright!=NULL){
                while(mostright->right!=NULL&&mostright->right!=cur) mostright=mostright->right;
                if(mostright->right==NULL){
                    mostright->right=cur;
                    cur=cur->left;
                    continue;
                }
                else{
                    mostright->right=NULL;
                    printEdge(cur->left);
                }
            }
            cur=cur->right;
        }
        printEdge(root);
        return res;
    }

private:
    vector<int> res;
};

int main(){
    vector<int> nums={1,2,3,4,5,-1,6};
    TreeNode* root=createTree(nums,nums.size(),0);
    PostOrder ans;
    vector<int> res=ans.Morris_postOrder(root);
    for(auto x:res) cout<<x<<" ";
}
```