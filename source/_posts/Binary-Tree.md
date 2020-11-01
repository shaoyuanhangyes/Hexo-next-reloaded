---
title: Binary-Tree
date: 2020-06-03 10:33:14
categories:
- Data Structure
tags:
- Data Structure
- C++
mathjax: true
---
# Binary-Tree
本篇文章为[`shaoyuanhangyes.github.io/二叉树`](https://shaoyuanhangyes.github.io/2019/10/30/%E4%BA%8C%E5%8F%89%E6%A0%91/#more)的重置版本 
更新了部分代码 更改了排版 舍弃了冗杂的赘述内容 利用STL容器进行了更新
<!-- more --> 
## 二叉树

<div>
    ![二叉树example](B.png)
</div>

### 二叉树结构体
```C++
struct TreeNode{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x):val(x),left(NULL),right(NULL) {}
};
```
### 二叉树的创建
采用的是更为直观的层序遍历的方式 将二叉树和对应数组序号的元素一一填入
```C++
//以层序的方式创建二叉树 len为数组长度 index为元素位置序号
TreeNode* createTree(const vector<int> &nums,int len,int index){
    TreeNode *root=NULL;
    if(index<len&&nums[index]!=-1){//值为null和位置不合法时直接返回空节点
        root = new TreeNode(nums[index]);
        root->left = createTree(nums,len,2*index+1);
        root->right= createTree(nums,len,2*index+2);
    }
    return root;
}
```
### 二叉树的遍历
二叉树的遍历中 层序遍历属于广度优先(BFS) 前中后序遍历都属于深度优先(DFS)
#### 层序遍历
利用队列进行迭代
```C++
//层序遍历 利用队列进行迭代 返回的是一个二维vector数组 因此需要一个函数来打印这个vector数组
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;
    queue<TreeNode* > Tree;
    Tree.push(root);
    while(!Tree.empty()){
        vector<int> temp;
        int len=Tree.size();
        while(len--){
            TreeNode *pNode=Tree.front();
            Tree.pop();
            temp.push_back(pNode->val);
            if(pNode->left) Tree.push(pNode->left);
            if(pNode->right) Tree.push(pNode->right);
        }
        res.push_back(temp);
    }
    return res;
}
```
打印二维数组的代码为
```C++
//打印二维数组
void PrintMartrix(vector<vector<int>>& res){
    for(int i=0;i<res.size();++i){
        cout<<"[";
        for(int j=0;j<res[i].size();++j){
            cout<<" "<<res[i][j]<<" ";
        }
        cout<<"]"<<endl;
    }
}
```
#### 先序遍历
遍历顺序为 `根 左 右`
```C++
//先序遍历递归算法 遍历到就直接cout输出
void preOrder(TreeNode *root){
    if(!root) return;
    cout<<root->val<<" ";
    preOrder(root->left);
    preOrder(root->right);
}
```
#### 中序遍历
遍历顺序为 `左 根 右`
```C++
//中序遍历递归算法
void inOrder(TreeNode *root){
    if(!root) return;
    inOrder(root->left);
    cout<<root->val<<" ";
    inOrder(root->right);
}
```
#### 后序遍历
遍历顺序为 `左 右 根`
```C++
//后序遍历递归算法
void postOrder(TreeNode *root){
    if(!root) return;
    postOrder(root->left);
    postOrder(root->right);
    cout<<root->val<<" ";
}
```

#### 全部代码
```C++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
struct TreeNode{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x):val(x),left(NULL),right(NULL) {}
};
//以层序的方式创建二叉树 len为数组长度 index为元素位置序号
TreeNode* createTree(const vector<int> &nums,int len,int index){
    TreeNode *root=NULL;
    if(index<len&&nums[index]!=-1){//值为null和位置不合法时直接返回空节点
        root = new TreeNode(nums[index]);
        root->left = createTree(nums,len,2*index+1);
        root->right= createTree(nums,len,2*index+2);
    }
    return root;
}
//打印二维数组
void PrintMartrix(vector<vector<int>>& res){
    for(int i=0;i<res.size();++i){
        cout<<"[";
        for(int j=0;j<res[i].size();++j){
            cout<<" "<<res[i][j]<<" ";
        }
        cout<<"]"<<endl;
    }
}
//层序遍历 利用队列进行迭代
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;
    queue<TreeNode* > Tree;
    Tree.push(root);
    while(!Tree.empty()){
        vector<int> temp;
        int len=Tree.size();
        while(len--){
            TreeNode *pNode=Tree.front();
            Tree.pop();
            temp.push_back(pNode->val);
            if(pNode->left) Tree.push(pNode->left);
            if(pNode->right) Tree.push(pNode->right);
        }
        res.push_back(temp);
    }
    return res;
}
//先序遍历递归算法
void preOrder(TreeNode *root){
    if(!root) return;
    cout<<root->val<<" ";
    preOrder(root->left);
    preOrder(root->right);
}
//中序遍历递归算法
void inOrder(TreeNode *root){
    if(!root) return;
    inOrder(root->left);
    cout<<root->val<<" ";
    inOrder(root->right);
}
//后序遍历递归算法
void postOrder(TreeNode *root){
    if(!root) return;
    postOrder(root->left);
    postOrder(root->right);
    cout<<root->val<<" ";
}

int main(){
    vector<int> nums={0,1,2,3,4,5,6,7,8,-1,10};//示例 -1代表所在位置为空值
    int len=nums.size();
    TreeNode *root=createTree(nums,len,0);
    cout<<"层序遍历的二维数组序列为: "<<endl;
    vector<vector<int>> res=levelOrder(root);
    PrintMartrix(res);
    cout<<endl<<"先序遍历序列为: ";
    preOrder(root);
    cout<<endl<<"中序遍历序列为: ";
    inOrder(root);
    cout<<endl<<"后序遍历序列为: ";
    postOrder(root);
    return 0;
}
```

程序输出结果为
```bash
层序遍历的二维数组序列为:
[ 0 ]
[ 1  2 ]
[ 3  4  5  6 ]
[ 7  8  10 ]

先序遍历序列为: 0 1 3 7 8 4 10 2 5 6
中序遍历序列为: 7 3 8 1 4 10 0 5 2 6
后序遍历序列为: 7 8 3 10 4 1 5 6 2 0
Process finished with exit code 0

```
## 二叉树进阶

更多进阶的算法以及刷题解答见[shaoyuanhangyes/LeetCode/树](https://github.com/shaoyuanhangyes/LeetCode/tree/master/%E6%A0%91)

二叉树刷题C++模版

```C++

/*二叉树结构体*/
struct TreeNode{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x):val(x),left(NULL),right(NULL) {}
};

```
### 创建二叉树

因为二叉树是非线性结构 不易于直接表示 因此采取顺序存储的思想 按照数组的序号对这个数组以层序遍历的方式创建二叉树

```C++

TreeNode* createTree(const vector<int> &nums,int len,int index){
    TreeNode *root=NULL;
    if(index<len&&nums[index]!=-1){//值为null和位置不合法时直接返回空节点
        root = new TreeNode(nums[index]);
        root->left = createTree(nums,len,2*index+1);
        root->right= createTree(nums,len,2*index+2);
    }
    return root;
}
int main(){
    vector<int> nums={0,1,2,3,4,5,6,7,8,-1,10};//示例 -1代表所在位置为空值
    int len=nums.size();
    TreeNode *root=createTree(nums,len,0);
    return 0;
}

```

创建后的二叉树为

```bash

        0
      /   \
    1       2
   / \     / \
  3   4   5   6
 / \   \
7   8   10

```
### 二叉树的遍历们

#### 前序遍历

前序遍历(先序遍历) 遍历的次序为 `根 左 右` 

```bash

        0
      /   \
    1       2
   / \     / \
  3   4   5   6
 / \   \
7   8   10      为例子

前序遍历的序列为 [0 1 3 7 8 4 10 2 5 6]

```

前序遍历序列的第一个结点一定是二叉树的根结点

##### 前序遍历递归代码
```C++

vector<int> res;//必须是全局变量的数组
vector<int> preorderTraversal(TreeNode* root) {
    if(root!=NULL){
        res.push_back(root->val);
        preorderTraversal(root->left);
        preorderTraversal(root->right);
    }
    return res;
}
int main(){
    vector<int> nums={0,1,2,3,4,5,6,7,8,-1,10};//示例 -1代表所在位置为空值
    int len=nums.size();
    TreeNode *root=createTree(nums,len,0);
    cout<<"先序遍历的序列为:"<<endl
    vector<int> preOrder=preOrderTraversal(root);
    for(auto x:preOrder) cout<<x<<" ";
    return 0;
}

```

##### 前序遍历迭代代码
```C++

vector<int> preorderTraversal(TreeNode* root) {
	vector<int> res;
	stack<TreeNode* > st;
	TreeNode* node=root;
	while(!st.empty()||node){
	    while(node){
	        st.push(node);
	        res.push_back(node->val);
	        node=node->left;
	    }
	    node=st.top();
	    st.pop();
	    node=node->right;
	}
	return res;
}
int main(){
    vector<int> nums={0,1,2,3,4,5,6,7,8,-1,10};//示例 -1代表所在位置为空值
    int len=nums.size();
    TreeNode *root=createTree(nums,len,0);
    cout<<"先序遍历的序列为:"<<endl
    vector<int> preOrder=preOrderTraversal(root);
    for(auto x:preOrder) cout<<x<<" ";
    return 0;
}

```

#### 中序遍历 

中序遍历 遍历的次序为 `左 根 右` 

```bash

        0
      /   \
    1       2
   / \     / \
  3   4   5   6
 / \   \
7   8   10      为例子

中序遍历的序列为 [7 3 8 1 4 10 0 5 2 6]

```

通过前序遍历确定了二叉树的根结点后 在中序遍历序列中 根结点的左半部分都是根结点的左子树 根结点的右半部分都是根结点的右子树
BST 二叉搜索树 又名二叉排序树 二叉查找树 的中序序列一定是按从小到大的顺序排列的

##### 中序遍历递归代码

```C++

vector<int> res;//必须是全局变量的数组
vector<int> inorderTraversal(TreeNode* root) {
    if(root!=NULL){
        inorderTraversal(root->left);
        res.push_back(root->val);
        inorderTraversal(root->right);
    }
    return res;
}

/*以下的主函数不赘述了*/

```

##### 中序遍历迭代代码

```C++

vector<int> inorderTraversal(TreeNode* root) {//迭代算法
    vector<int> res;
    stack<TreeNode *> st;
    TreeNode *node=root;
    while(!st.empty()||node){
        while(node){
            st.push(node);
            node=node->left;
        }
        node=st.top();
        st.pop();
        res.push_back(node->val);
        node=node->right;
    }
    return res;
}

```

#### 后序遍历

后序遍历 遍历的次序为 `左 右 根`

```bash

        0
      /   \
    1       2
   / \     / \
  3   4   5   6
 / \   \
7   8   10      为例子

后序遍历的序列为 [7 8 3 10 4 1 5 6 2 0]

```

后序遍历序列的最后一个结点一定是根结点

##### 后序遍历递归代码

```C++

vector<int> res;
vector<int> postorderTraversal(TreeNode* root){
    if(root!=NULL){
        postorderTraversal(root->left);
        postorderTraversal(root->right);
        res.push_back(root->val);
    }
    return res;
}

```

##### 后序遍历迭代代码

第一种解法 标记位

prev记录已经输出到res数组中的上一个结点 防止出现某一结点有右孩子 右孩子输出后 根据栈回到右孩子的父结点 然后又判断是否有右孩子的死循环 当判断到node->right==prev成立 即父结点的右孩子上一次被访问过 就输出这个父结点

```C++

vector<int> postorderTraversal(TreeNode* root){
    vector<int> res;
    stack<TreeNode* > st;
    TreeNode* node=root;
    TreeNode* prev=NULL;
    while(!st.empty()||node){
        while(node){
            st.push(node);
            node=node->left;
        }
        node=st.top();
        if(node->right==NULL||node->right==prev){
            res.push_back(node->val);
            st.pop();
            prev=node;
            node=NULL;
        }
        else node=node->right;
    }
    return res;
}

```

第二种解法 反转

根据前序遍历的思想 将遍历序列规则转换为 `根 右 左` 序列输出到数组后 将整个数组反转过来 就变成了`左 右 根`

```C++

vector<int> preorderTraversal(TreeNode* root) {
	vector<int> res;
	stack<TreeNode* > st;
	TreeNode* node=root;
	while(!st.empty()||node){
	    while(node){
	        st.push(node);
	        res.push_back(node->val);//根
	        node=node->right;//右
	    }
	    node=st.top();
	    st.pop();
	    node=node->left;//左
	}
	reverse(res.begin(),res.end());//#include <algorithm>
	return res;
}

```

#### 层序遍历 

层序遍历 每一层元素 按从左到右的顺序逐层输出 

```bash
        0
      /   \
    1       2
   / \     / \
  3   4   5   6
 / \   \
7   8   10      为例子

层序遍历的序列为 [0 1 2 3 4 5 6 7 8 10]

```

##### 层序遍历迭代算法

利用队列进行迭代 返回二维数组 

```C++

vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;
    queue<TreeNode* > Tree;
    Tree.push(root);
    while(!Tree.empty()){
        vector<int> temp;
        int len=Tree.size();
        while(len--){
            TreeNode *pNode=Tree.front();
            Tree.pop();
            temp.push_back(pNode->val);
            if(pNode->left) Tree.push(pNode->left);
            if(pNode->right) Tree.push(pNode->right);
        }
        res.push_back(temp);
    }
    return res;
}

```

打印二维数组的方法

```C++

void PrintMartrix(vector<vector<int>>& res){
    for(int i=0;i<res.size();++i){
        cout<<"[";
        for(int j=0;j<res[i].size();++j){
            cout<<" "<<res[i][j]<<" ";
        }
        cout<<"]"<<endl;
    }
}

```

## Binary Search Tree

二叉搜索树 又被翻译为二叉排序树 二叉查找树

### BST插入

给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

#### 示例

```bash

例如, 

给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和 插入的值: 5
你可以返回这个二叉搜索树:

         4
       /   \
      2     7
     / \   /
    1   3 5
或者这个树也是有效的:

         5
       /   \
      2     7
     / \   
    1   3
         \
          4

``` 
#### 递归解法
```C++

class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(root==NULL) return new TreeNode(val);
        if(val<root->val) root->left=insertIntoBST(root->left,val);
        if(val>root->val) root->right=insertIntoBST(root->right,val);
        return root;
    }
};

```
##### 复杂度分析
1. 时间复杂度: O(H) H为Tree的高度 平均情况<math xmlns="http://www.w3.org/1998/Math/MathML" display="inline-block"><mi>l</mi><mi>o</mi><msub><mi>g</mi><mn>2</mn></msub><mi>N</mi></math> 最坏情况O(N)
2. 空间复杂度: 平均情况下O(H) 最坏情况O(N) 是在递归过程中堆栈使用的空间

复杂度中`最坏的情况`就是二叉树高度为二叉树结点数量的时候 即
```
        5
       /   
      2     
     /   
    1  
   /
  4 
```
要插入的元素需要遍历到最深才能插入

#### 迭代解法

```C++
TreeNode* insertIntoBST(TreeNode* root,int val){
        TreeNode* node=root;
        while(node!=NULL){
            if(val>node->val){
                if(node->right==NULL){
                    node->right=new TreeNode(val);
                    return root;
                }
                else node=node->right;
            }
            else{
                if(node->left==NULL){
                    node->left=new TreeNode(val);
                    return root;
                }
                else node=node->left;
            }
        }
        return new TreeNode(val);
    }
```

##### 复杂度分析

1. 时间复杂度: O(H) H为Tree的高度 平均情况<math xmlns="http://www.w3.org/1998/Math/MathML" display="inline-block"><mi>l</mi><mi>o</mi><msub><mi>g</mi><mn>2</mn></msub><mi>N</mi></math> 最坏情况O(N)
2. 空间复杂度: O(1)