## 第二周：哈希表、映射、集合；树、二叉树、二叉搜索树；堆和图

### 一，哈希表、映射、集合
哈希表（Hash table），是根据关键码值（Key value）而直接进行访问的数据结构。又叫散列表、散列函数。通过哈希函数将数据映射到某个位置，查找时可以快速查找，若碰撞时需要消除碰撞。java和C\+\+是map，Python是dict.\
C++ map的使用：unordered_map:\
遍历：

```
unordered_map<char,int> mymap;
for ( auto it = mymap.begin(); it != mymap.end(); ++it )
    cout << " " << it->first << ":" << it->second;
插入：map.insert({'A',65});map['a']=97;pair<char,int> c ('d',100); mymap.insert(c);mymap.insert(pair<char,int>('f',102));
删除：erase通过key删除 map.erase('a');map.erase(map.begin());
查找：if(map.find ("coffee")!=map.end())
         print('没找到');
```

### 二，树、二叉树、二叉搜索树
熟知二叉树的前中后遍历。\
二叉搜索树， 也称二叉排序树、有序二叉树（Ordered Binary Tree）、 排序二叉树（Sorted Binary Tree）， 是指一棵空树或者具有下列性质的二叉
树：
1. 左子树上所有结点的值均小于它的根结点的值；
2. 右子树上所有结点的值均大于它的根结点的值；
3. 以此类推：左、右子树也分别为二叉查找树。 
其中序遍历之后是升序排列

### 三，堆和图
堆：heap。heap：可以迅速找到一堆树中最大和最小值的结构。根节点最大的叫大顶堆，根节点最小的叫小顶堆；常见的堆有二叉树堆和斐波那契堆。其操作的时间复杂度如下：
```
find-max:O(1);delete-max:O(logn);insert:O(logn);
```
二叉堆的性质：
1. 二叉堆是一颗完全二叉树；
2. 树中的任意节点总是大于其子节点。

二叉堆一般是可以通过数组来实现的，根节点的索引为0；那么节点i的左孩子节点是(2*i+1),右孩子节点是(2*i+2),父节点是((i-1)/2)。

增删操作：\
1，插入：新元素一律插到堆的尾部，然后再依次向上调整整个堆的结构。\
2，删除：将堆尾元素替换成堆顶元素，然后再依次向下调整整个堆的结构。

PS:
1，链是特殊化的树，树是特殊化的图。\
若树只有一个孩子节点，那么就是树；\
常用的树是二叉树，也会有多叉树。如果树的能够构成闭环的话，那就是图。所以说树是特殊化的图。\
2，树的出现\
玄学点就是它符合人的生活经验。

### code:

[242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
示例 1:
输入: s = "anagram", t = "nagaram"
输出: true
示例 2:
输入: s = "rat", t = "car"
输出: false
说明:
你可以假设字符串只包含小写字母。
```

```cpp
bool isAnagram(string s, string t) {
    if(s.size()!=t.size()) return false;
    unordered_map<char,int> mp;
    for(char c : s)
        mp[c]++;
    for(char c : t){
        mp[c]--;
        if(mp[c]==-1) return false;
    }
    return true;
}
```

[49.字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

```
给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
示例:
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
说明：
所有输入均为小写字母。
不考虑答案输出的顺序。
```

```
def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
    #不使用排序，而是用字符串对应26位字符的频次来当做key；
    word_dict = {}
    result = []
    if len(strs)==0: return []
    for word in strs:
        count = [0]*26
        #sort_word = "".join(sorted(word))
        for ch in word:
            count[ord(ch)-ord('a')] +=1
        key = tuple(count)
        if key in word_dict:
            word_dict[key].append(word)
        else:
            word_dict[key] = [word]
    for k,v in word_dict.items():
        result.append(v);
    return result
```

[94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```
给定一个二叉树，返回它的中序 遍历。
示例:
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```
中序遍历的迭代解法，使用一个栈，来模拟递归的思想。每次都是加入节点的左子树进栈，之后出栈，出栈的时候判断是否有右子树，然后将右子树的左子树数据进栈；

```
//递归的解法
void inorder(TreeNode* root,vector<int>& result_vec){
    if(!root) return;
    inorder(root->left,result_vec);
    result_vec.push_back(root->val);
    inorder(root->right,result_vec);         
}
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> mid_search;
    if(!root) return mid_search;
    inorder(root,mid_search);
    return mid_search;
}
//------------------------------------------
//迭代的解法
vector<int> inorderTraversal(TreeNode* root) {
    stack<TreeNode*> stk;
    vector<int> number;
    if(!root) return number;
    while(root){
        stk.push(root);
        root = root->left;
    }  
    while(!stk.empty()){
        TreeNode* top=stk.top();
        stk.pop();
        number.push_back(top->val);
        top = top->right;
        while(top){
            stk.push(top);
            top = top->left;
        }
    }
    return number;
}
```


[144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```
递归解法
void preorder(TreeNode* root,vector<int>& number){
    if(!root) return;
    number.push_back(root->val);
    preorder(root->left,number);
    preorder(root->right,number);
}

vector<int> preorderTraversal(TreeNode* root) {
    vector<int> pre_number;
    if(!root) return pre_number;
    preorder(root,pre_number);
    return pre_number;
}
//-------------------------------------
//迭代解法
vector<int> preorderTraversal(TreeNode* root) {
    stack<TreeNode*> add_stack;
    vector<int> output;
    while(root || !add_stack.empty()){
        if(root){
            add_stack.push(root);
            output.push_back(root->val);
            root=root->left;
        }
        else{
            root = add_stack.top();
            add_stack.pop();               
            root=root->right;
        }
    }
    return output;
}
```

[N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/description/):

```
给定一个 N 叉树，返回其节点值的前序遍历。

例如，给定一个 3叉树 :
.................
返回其前序遍历: [1,3,5,6,2,4]。
```

```
//递归解法
void preorder_node(Node* root,vector<int>& num){
    if(!root) return ;
    num.push_back(root->val);
    for(int i=0;i<root->children.size();i++){
        preorder_node(root->children[i],num);
    }
}
vector<int> preorder(Node* root) {
    vector<int> num;
    if(!root) return num;
    preorder_node(root,num);
    return num;
}
//------------------------------------------
//迭代的解法：使用栈来辅助。若root不空则进栈，并将其子节点从右到左进栈；
vector<int> preorder(Node* root) {
    stack<Node*> add_stack;
    vector<int> output;
    if(!root) return output;
    add_stack.push(root);
    while(!add_stack.empty()){
        Node* temp = add_stack.top();add_stack.pop();
        output.push_back(temp->val);
        for(int i=temp->children.size()-1;i>=0;i--){
            add_stack.push(temp->children[i]);
        }
    }
    return output;
}

```
