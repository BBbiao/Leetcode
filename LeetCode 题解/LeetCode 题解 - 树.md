# 树  

```cpp
//Definition for a binary tree node.
　struct TreeNode {
　　int val;
　　TreeNode *left;
　　TreeNode *right;
　　TreeNode(int x) : val(x), left(NULL), right(NULL) {}
　};```

<!-- GFM-TOC -->
[104.二叉树的最大深度](#104-二叉树的最大深度)  
[543.二叉树的直径(medium)](#543-二叉树的直径)  
[110.平衡二叉树](#110-平衡二叉树)  
[144.二叉树的前序遍历(medium)](#144-二叉树的前序遍历)  
[94.二叉树的中序遍历(medium)](#94-二叉树的中序遍历)  
[145.二叉树的后序遍历(hard)](#145-二叉树的后序遍历)  


[208.实现 Trie(前缀树)(medium)](#208-实现Trie(前缀树)(medium))

<!-- GFM-TOC -->


**94-二叉树的中序遍历**(medium)    
　　题目:  
　　　给定一个二叉树，返回它的中序遍历。  
　　示例:  
　　　输入: [1,null,2,3]  
　　　输出: [1,3,2]  
　　注意：  
　　　根节点其实早就在找左节点的时候就入栈了，所以可以根节点看做左节点。只不过是出栈顺序不一样。  

[way1](#94-way1): 递归，用一个辅助函数帮助。  
[way2](#94-way2)：迭代，尾递归都可以转化成迭代。  

<div id="94-way1"></div>

```cpp
/******************************* way1 *****************************************/
//执行用时 :4 ms, 在所有 C++ 提交中击败了72.75% 的用户
//内存消耗 :10.5 MB, 在所有 C++ 提交中击败了11.60%的用户

class Solution {
    vector<int> res;
public:
    vector<int> inorderTraversal(TreeNode* root) {
      helper(root);
      return res;
    }
    void helper(TreeNode* root) {
        if(root!=NULL){
            if(root->left !=NULL){
                helper(root->left);
            }
            res.push_back(root->val);
            if(root->right !=NULL){
                helper(root->right);
            }
        }
    }
};```
<div id="94-way2"></div>

```cpp
/******************************* way2 *****************************************/
//执行用时 :4 ms, 在所有 C++ 提交中击败了72.75% 的用户
//内存消耗 :9.9 MB, 在所有 C++ 提交中击败了12.93%的用户
class Solution {
    vector<int> res;
public:
    vector<int> inorderTraversal(TreeNode* root) {
		vector<int> res;
		if(!root) return res;
		stack<TreeNode*> s;
		
		TreeNode *curr = root;
		while(curr!=NULL || !s.empty()){
			while(curr!=NULL){
				s.push(curr);
				curr = curr->left;
			}
			curr = s.top();
            s.pop();
            res.push_back(curr->val);
            curr = curr->right;
		}
		return res;
    }
};```

**104-二叉树的最大深度**(easy)  
题目：  
	给定一个二叉树，找出其最大深度。  
	二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。  
说明:   
	叶子节点是指没有子节点的节点。  
示例：  
	给定二叉树 [3,9,20,null,null,15,7]，  
		3  
	   / \  
	  9  20  
		/  \  
	   15   7  
	返回它的最大深度 3 。  
	
[way1](#104-way1)：递归，其实用了深度优先搜索(DFS)的思想，返回最大的。(就怕爆栈)    
	时间复杂度：我们每个结点只访问一次，因此时间复杂度为 O(N)，其中 N 是结点的数量。    
	空间复杂度：在最糟糕的情况下，树是完全不平衡的，例如每个结点只剩下左子结点，递归将会被调用 N 次（树的高度），因此保持调用栈的存储将是 O(N)。但在最好的情况下（树是完全平衡的），树的高度将是 log(N)。因此，在这种情况下的空间复杂度将是 O(log(N))。  
[way2](#104-way2): 广搜，树的层遍历，建立一个队列queue<TreeNode,depth>，根节点先进队，然后根出队将		他的左右进队,同时这层的depth属性+1。直到最后一个节点遍历完。  
	时间复杂度： O(N)，我们每个结点只访问一次，其中 N 是结点的数量。  
	空间复杂度： O(N)  
<div id="104-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :12 ms, 在所有 C++ 提交中击败了73.20% 的用户
//内存消耗 :21.3 MB, 在所有 C++ 提交中击败了5.05%的用户

class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return NULL;
        int max_Depth=0;
        int left_Depth=0,right_Depth=0;
        left_Depth	=maxDepth(root->left);		//左子树，如果直接放下面判断式中，会造成计算两次
        right_Depth	=maxDepth(root->right);		//右子树
        max_Depth = left_Depth>right_Depth?left_Depth+1:right_Depth+1;
        return max_Depth;
    }
};```
<div id="104-way2"></div>

```cpp
/********************************* way2 *********************************************/
//执行用时 :12 ms, 在所有 C++ 提交中击败了73.20% 的用户
//内存消耗 :21.5 MB, 在所有 C++ 提交中击败了5.05%的用户
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
		queue<pair<TreeNode*,int>> q;			//创建一个队列
		q.push(make_pair(root,1));				//先把根放进去
		int max_Depth=0;
		while(!q.empty()){
			max_Depth = q.front().second;
			if(q.front().first->left) 
				q.push(make_pair(q.front().first->left, max_Depth+1));
			if(q.front().first->right) 
				q.push(make_pair(q.front().first->right, max_Depth+1));
			q.pop();
		}
        return max_Depth;
    }
};```

**110-平衡二叉树**(easy)  
　　题目：  
　　　给定一个二叉树，判断它是否是高度平衡的二叉树。  
　　　本题中，一棵高度平衡二叉树定义为：  
　　　一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。  
　　示例 1:  
　　　给定二叉树 [3,9,20,null,null,15,7]  
　　　　 　　 3   
　　　　　　 / \  
　　　　　　9  20  
　　　　　　　/  \  
　　　　　　15   7  
　　　返回 true 。  
　　注意：  
　　　1.只有每个节点左右子树高度差不大于 1 时，该树才是平衡的。因此可以比较每个节点左右两棵子树的高度差，然后向上递归。  
　　　2.注意子树不平衡，整个树都不平衡，所以这是个子树问题。  
[way1](#110-way1): 自顶向下的递归，从根节点开始，让左右子树差不超过1，且判断左右子树分别是不是平衡的。  
[way2](#110-way2)：自底向上的递归，方法一计算 height 存在大量冗余。每次调用 height时，要同时计算其子树高度。但是自底向上计算，每个子树的高度只会计算一次。可以递归先计算当前节点的子节点高度，然后再通过子节点高度判断当前节点是否平衡，从而消除冗余。  

<div id="110-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :20 ms, 在所有 C++ 提交中击败了48.21% 的用户
//内存消耗 :24.1 MB, 在所有 C++ 提交中击败了6.61%的用户

class Solution {
private:
	int height(TreeNode* root){						//求任意节点高度
		if(root==NULL) return 0;
		return 1+max(height(root->left),height(root->right));
	}
public:
    bool isBalanced(TreeNode* root) {
		if(!root) return true;
		
		return abs(height(root->left)-height(root->right))<2 
			&& isBalanced(root->left) && isBalanced(root->right);

    }
};```
<div id="110-way2"></div>

```cpp
/********************************* way2 *********************************************/
class Solution {
private:
  // Return whether or not the tree at root is balanced while also storing
  // the tree's height in a reference variable. 
  bool isBalancedTreeHelper(TreeNode* root, int& height) {
    // An empty tree is balanced and has height = -1
    if (root == NULL) {
      height = -1;
      return true;
    }

    // Check subtrees to see if they are balanced. If they are, check if the 
    // current node is also balanced using the heights obtained from the 
    // recursive calls.
    int left, right;
    if (isBalancedTreeHelper(root->left, left)  &&
        isBalancedTreeHelper(root->right, right) &&
        abs(left - right) < 2) {
      // Store the current tree's height
      height = max(left, right) + 1;
      return true;
    }
    return false;
  }
public:
  bool isBalanced(TreeNode* root) {
    int height;
    return isBalancedTreeHelper(root, height);
  }
};```

**144-二叉树的前序遍历**(medium)  

　　　给定一个二叉树，返回它的 前序 遍历。  
　　注意：  
　　　前序遍历是，到一个节点，先入vec，并且入栈，然后找他的左儿子，它出栈的时候是为了找这个节点的右儿子，所以只需要一次调用。后序遍历就需要两次了，因为第一次出栈是为了找右儿子，第二次是为了将自己加入vec。  

[way1](144-way1): 递归，用了一个辅助函数，一样，跟中序遍历只换了vector插入元素的时机。  
[way2](114-way2)：迭代，跟中序遍历只换了vector插入元素的时机。  

<div id="-way1"></div>

```cpp
/******************************* way1 ****************************************/
class Solution {
    vector<int> res;
public:
    vector<int> preorderTraversal(TreeNode* root) {
      helper(root);
      return res;
    }
    void helper(TreeNode* root) {
        if(root!=NULL){
            res.push_back(root->val);
            if(root->left !=NULL){
                helper(root->left);
            }
            if(root->right !=NULL){
                helper(root->right);
            }
        }
    }
};```

<div id="-way2"></div>

```cpp
/******************************* way2 ****************************************/
//执行用时 :0 ms, 在所有 C++ 提交中击败了100.00% 的用户
//内存消耗 :9.9 MB, 在所有 C++ 提交中击败了11.77%的用户

class Solution {
    vector<int> res;
public:
    vector<int> preorderTraversal(TreeNode* root) {
		vector<int> res;
		if(!root) return res;
		stack<TreeNode*> s;
		
		TreeNode *curr = root;
		while(curr!=NULL || !s.empty()){
			while(curr!=NULL){
				s.push(curr);
                res.push_back(curr->val);
				curr = curr->left;
			}
			curr = s.top();
            s.pop();            
            curr = curr->right;
		}
		return res;
    }
};```

**145-二叉树的后序遍历**(hard)  
　　给定一个二叉树，返回它的 后序 遍历。  
　　注意：  
　　　后序遍历就需要访问节点两次，因为第一次出栈是为了找右儿子，第二次是为了将自己加入vec。  
[way1](#145-way1): 递归，用了一个辅助函数，与另外两种方式只是vector插入元素的时机不一样。  
[way2](#145-way2)：迭代，  

<div id="145-way1"></div>

```cpp
/******************************* way1 ****************************************/
//执行用时 :0 ms, 在所有 C++ 提交中击败了100.00% 的用户
//内存消耗 :10.3 MB, 在所有 C++ 提交中击败了10.37%的用户

class Solution {
    vector<int> res;
public:
    vector<int> postorderTraversal(TreeNode* root) {
      helper(root);
      return res;
    }
    void helper(TreeNode* root) {
        if(root!=NULL){
            if(root->left !=NULL){
                helper(root->left);
            }
            if(root->right !=NULL){
                helper(root->right);
            }
            res.push_back(root->val);
        }
    }
};```
<div id="145-way2"></div>

```cpp
/******************************* way2 ****************************************/
//执行用时 :8 ms, 在所有 C++ 提交中击败了18.51% 的用户
//内存消耗 :10.3 MB, 在所有 C++ 提交中击败了10.21%的用户
class Solution {
    vector<int> res;
public:
    vector<int> postorderTraversal(TreeNode* root) {
		vector<int> res;
		if(!root) return res;
		stack<pair<TreeNode*,int>> s;
		
		TreeNode *curr = root;
		while(curr!=NULL || !s.empty()){
			while(curr!=NULL){
				s.push({curr,2});
                s.push({curr,1});
				curr = curr->left;
			}
            while(s.top().second ==2){
                res.push_back(s.top().first->val);
                s.pop();
                if(s.empty()) return res;
            }
			curr = s.top().first;
            s.pop();  
            curr = curr->right;
		}
		return res;
    }
};```

**543-二叉树的直径**(easy)  
　　题目：  
　　　给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。
　　示例 :  
　　　给定二叉树  

          1  
         / \  
        2   3  
       / \      
      4   5      

　　　返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。  
　　注意：  
　　　两结点之间的路径长度是以它们之间边的数目表示。  

[way1](#543-way1): BFS遍历所有节点，然后写一个方法，用来求任意节点下的左右子树的直径(注意添加一个mode参数，这样就可以使这个子函数第一次递归返回值不一样。第一次返回左右子树高度和，剩下递归返回左右子树高度最大值)  
[way2](#543-way2)：用递归遍历了所有的节点，算是DFS，注意 最长的直径是存在一个节点，(这个节点的)左子树高度+右子树高度的和为最长的直径.可以用节点数来求对任意节点，以它为中心的路长度为(左子树高度+右子树高度)  

<div id="-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :32 ms, 在所有 C++ 提交中击败了17.85% 的用户
//内存消耗 :22.3 MB, 在所有 C++ 提交中击败了15.80%的用户

class Solution {
private:
    int height(TreeNode* root,int mode){             //求任意节点的左右子树高度和
        if(root==NULL) return 0;            //叶节点返回0
        int left_height=0,right_height=0;
        if(mode==0){                         //任意节点
            if(root->left)
                left_height = 1+height(root->left,1);
            if(root->right)
                right_height= 1+height(root->right,1);
            return left_height+right_height;
        }
        else{                         //要求的任意节点的子节点
            if(root->left)
                left_height = 1+height(root->left,1);
            if(root->right)
                right_height= 1+height(root->right,1);
            return left_height>right_height?left_height:right_height;
        }
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root==NULL) return 0;
        int length=0,max_length=0;
        queue<TreeNode*> q;			            //BFS
		q.push(root);				            //先把根放进去
		while(!q.empty()){
            length = height(q.front(),0);
			max_length = max_length > length? max_length: length;
			if(q.front()->left) 
				q.push(q.front()->left);
			if(q.front()->right) 
				q.push(q.front()->right);
			q.pop();
		}
        return max_length;

    }
};```
<div id="-way2"></div>

```cpp
/********************************* way2 *********************************************/
//执行用时 :12 ms, 在所有 C++ 提交中击败了81.38% 的用户
//内存消耗 :22.5 MB, 在所有 C++ 提交中击败了15.64%的用户
class Solution{
private:
    int res;
    int depth(TreeNode* root){
        if(!root)return 0;			// 访问到空节点了，返回0
        int L = depth(root->left);	// 左儿子为根的子树的深度
        int R = depth(root->right);	// 右儿子为根的子树的深度
        res = max(res,L+R);			// 计算d_node即L+R 并更新ans
        return max(L,R)+1;			// 返回该节点为根的子树的深度
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        res = 0;
        depth(root);
        return res;
    }
};```

**208-实现Trie(前缀树)**(medium)  
Trie(前缀树)介绍：  
　　Trie (发音为 "try") 或前缀树是一种树数据结构，用于检索字符串数据集中的键。这一高效的数据结构有多种应用：1. 自动补全　2. 拼写检查　3. IP 路由 (最长前缀匹配)　4. 单词游戏  
　　尽管哈希表可以在 O(1) 时间内寻找键值，却无法高效的完成以下操作：  
　　　找到具有同一前缀的全部键值。  
　　　按词典序枚举字符串的数据集。  
　　Trie 树优于哈希表的另一个理由是，随着哈希表大小增加，会出现大量的冲突，时间复杂度可能增加到 O(n)，其中 n是插入的键的数量。与哈希表相比，Trie 树在存储多个具有相同前缀的键时可以使用较少的空间。此时 Trie 树只需要 O(m)的时间复杂度，其中 m 为键长。而在平衡树中查找键值需要 O(mlog⁡n) 时间复杂度。  

　　实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。  
　　示例:  
　　　Trie trie = new Trie();  

　　　trie.insert("apple");  
　　　trie.search("apple");   // 返回 true  
　　　trie.search("app");     // 返回 false  
　　　trie.startsWith("app"); // 返回 true  
　　　trie.insert("app");     
　　　trie.search("app");     // 返回 true  

　　说明:  
　　　你可以假设所有的输入都是由小写字母 a-z 构成的。  

```cpp
class Trie {
private:
    bool is_end;
    Trie* next[26];  //因为默认只有26个小写字母，提前建立子节点的指针，但是值都是NULL
public:
    /** Initialize your data structure here. */
    Trie() {
        is_end = false;
        memset(next,0,sizeof(next)); //初始化指针都为0
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        Trie* node =this;
        for(char c: word){
            if(node->next[c-'a']==NULL)//说明没有这个前缀
                node->next[c-'a']=new Trie();
            node=node->next[c-'a'];     //node指向下一个节点
        }
        node->is_end=true;
    }
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trie* node =this;
        for(char c:word){
            if(node->next[c-'a']==NULL)//说明没有这个前缀
                return false;
            node=node->next[c-'a'];
        }
        return node->is_end;
    }
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trie* node =this;
        for(char c:prefix){
            if(node->next[c-'a']==NULL)//说明没有这个前缀
                return false;
            node=node->next[c-'a'];
        }
        return true;
    }
};
```

