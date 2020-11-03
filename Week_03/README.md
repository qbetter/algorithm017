## 第三周 泛型递归，树的递归,分治与回溯
### 泛型与树的递归
1. 不要人肉进行递归（最大误区）
2. 找到最近最简方法，将其拆解成可重复解决的问题（重复子问题）
3. 数学归纳法思维

### 分治与回溯
本质上都是递归；找重复性以及分解问题。\
回溯法采用试错的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。

### code：
[70.爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

```
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。
示例 1：
输入： 2
输出： 2
```

```
int climbStairs(int n) {
    if(n<=3) return n;
    int f1=1,f2=2,f3=3;
    for(int i=3;i<=n;i++){
        f3 = f1 + f2;
        f1 = f2;
        f2 = f3;
    }
    return f3; 
}
```


[226.翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/description/)

```
翻转一棵二叉树。
示例：
输入：
     4
   /   \
  2     7
 / \   / \
1   3 6   9
输出：
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

```cpp
//递归解法
TreeNode* invertTree(TreeNode* root) {
    if(!root) return root;
    TreeNode* tmp_t = root->left;
    root->left = root->right;
    root->right = tmp_t;
    invertTree(root->left);
    invertTree(root->right);
    return root;
}

//迭代解法
//使用queue来辅助，迭代的翻转二叉树
if(!root) return root;
queue<TreeNode*> queue_node;
queue_node.push(root);
while(!queue_node.empty()){
    int n = queue_node.size();
    for(int i=0;i<n;i++){
        TreeNode* top_node = queue_node.front();
        queue_node.pop();
        TreeNode* temp = top_node->left;
        top_node->left = top_node->right;
        top_node->right = temp;
        if(top_node->left)
            queue_node.push(top_node->left);
        if(top_node->right)
            queue_node.push(top_node->right);
    }
}
return root;
```

[括号生成](https://leetcode-cn.com/problems/generate-parentheses/submissions/)

在生成过程中的剪枝：1，左括号随时可以生成，但是需要其生成的数量小于n；2，右括号在左括号后，且个数一定要小于左括号。
```cpp  
//递归的解法
void generate_fun(int left,int right,int n,string s,vector<string>& output){
    if(left==n && right==n){
        output.push_back(s);
        return;
    }
    if(left<=n)
        generate_fun(left+1,right,n,s+'(',output);
    if(left>right)
        generate_fun(left,right+1,n,s+')',output);
}

//--迭代的解法
//括号有效需要满足的条件是左括号不大于n，且右括号数量小于左括号
void gene_fun(int left,int right,int n,vector<string>& output,string s) {
    //终止条件
    if (left==n && right==n) {
        output.push_back(s);
        return ;
    }
    //当前层处理
    string left_s = s+'(';
    string right_s = s+')';
    if (left < n) 
        gene_fun(left+1,right,n,output,left_s);
    if (left>right)
        gene_fun(left,right+1,n,output,right_s);
}
vector<string> generateParenthesis(int n) {
    if (n<=0) return {""};
    vector<string> output;
    gene_fun(0,0,n,output,"");
    return output;
}
```

[98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/solution/98yan-zheng-er-cha-ping-heng-shu-by-free_styles/)\

```
给定一个二叉树，判断其是否是一个有效的二叉搜索树。
假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
示例 1:
输入:
    2
   / \
  1   3
输出: true
```
对于每一层在向下传递的时候都要加上边界来判断当前数据的是不是在合适的范围之内。如果遍历左子树，则其左边界不变，右边界为根节点的值；
如果遍历右子树，则其右边界不变，左边界为根节点的值。这题很容易犯的错是只比较当前根节点对左右子节点是否满足，但是需要其对左右子树都要满足。
```cpp 
bool isValid(TreeNode* root,long long lower,long long upper){
    if(root==NULL) return true;
    if(root->val<=lower || root->val>=upper)
        return false;
    return isValid(root->left,lower,root->val) && isValid(root->right,root->val,upper);
}
bool isValidBST(TreeNode* root) {
    if(!root) return true;
    return isValid(root,LONG_MIN,LONG_MAX);
}
```

[104.二叉树最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/104-er-cha-shu-de-zui-da-shen-du-ti-jie-by-free_st/)：
迭代的解法：广度优先遍历；申请一个队列；每次进队的时候就把该层的所有元素进入队列中，并把深度加1；出队的时候把队列中全部元素都出队列，并将出队时数据的子树加入到队列中
```cpp
//递归解法
int maxDepth(TreeNode* root) {
    if(!root) return 0;
    int left_depth = maxDepth(root->left);
    int right_depth = maxDepth(root->right);
    return 1+max(left_depth,right_depth); 
}

//迭代解法
int maxDepth(TreeNode* root) {
    int mdepth = 0;
    if(!root) return mdepth;
    queue<TreeNode*> q;
    q.push(root);
    mdepth +=1;
    while(!q.empty()){
        int q_size = q.size();
        cout<<q_size;
        int i = 0;
        while(i<q_size){
            TreeNode* tmp = q.front();q.pop();
            if(tmp->left)
                q.push(tmp->left);
            if(tmp->right)
                q.push(tmp->right);
            i += 1;
        }
        mdepth+=1;
        cout<<mdepth;
    }
    return mdepth-1;
}

```

[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```
给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
说明：叶子节点是指没有子节点的节点。
```
递归解法：
```cpp
//递归解法
int minDepth(TreeNode* root) {
    if(!root) return 0;
    //当节点的左右孩子一个是空一个不为空时，那么当前节点就不是叶子节点，要返回有孩子的那个节点
    int left_num = minDepth(root->left);
    int right_num = minDepth(root->right);
    //因为有一个孩子是空，所以left_num，right_num中肯定有一个是0.
    if(!root->left || !root->right) return left_num+right_num+1;
    return 1+min(left_num,right_num);   
}
//迭代的解法
int minDepth(TreeNode* root) {
    //使用队来保存；
    if(!root) return 0;
    queue<TreeNode*> qe;
    qe.push(root);
    int maxnumber=0;
    while(!qe.empty()){
        int layer_size = qe.size();
        bool flag = false;
        for(int i=0;i<layer_size;i++){
            TreeNode* top_nd = qe.front();qe.pop();
            if(!top_nd->left && !top_nd->right){
                flag = true;
                break;
            } 
            if(top_nd->left)
                qe.push(top_nd->left);
            if(top_nd->right)
                qe.push(top_nd->right);
        }
        maxnumber++;
        if(flag)
            break;
    }
    return maxnumber;
}
```
----
[二叉树广度遍历的代码框架,求二叉树的最大、最小深度、最大宽度的题解框架](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-yan-du-bian-li-de-dai-ma-kuang-jia-qiu-/)

```
int minDepth(TreeNode* root) {
    //用队列q，每次将该层的元素都进队；在出队时若出队的元素有子节点，将子节点入队；
    queue<TreeNode*> q;
    if(!root)
    //#此处可以加入当root是空时的一些逻辑
    q.push(root);
    while(!q.empty()){
        int q_size = q.size();
        //#此处若是判断最大宽度就可以在此处处理：
        //##max_kuan = max_kuan>q_size?max_kuan:q_size;
        int i = 0;
        while(i < q_size){
            //将队列中的元素全部出队
            TreeNode* tmp = q.front();q.pop();
            //cout<<tmp->val;
            //如果出队的数据有子节点，则入队
            if(tmp->left)
                q.push(tmp->left);
            if(tmp->right)
                q.push(tmp->right);
            i+=1;
        }
    }
    //##最后根据逻辑返回需要的数据
    return max_kuan;
}
```
----

[50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)
```
实现 pow(x, n) ，即计算 x 的 n 次幂函数。
```
使用分治的方式，使得复杂度降低到O(logn)。
```cpp
double pow(double x,long long level){
    if(level==0)
        return 1;
    double part_sum = pow(x,level/2);
    part_sum = part_sum*part_sum;
    if(level%2)
        part_sum *=x;
    return part_sum;
}
double myPow(double x, int n) {
    long long l_n = n;
    return l_n>=0?pow(x,l_n):1/pow(x,-l_n);
}
```

[78.子集](https://leetcode-cn.com/problems/subsets/solution/78zi-ji-by-free_styles/)
```
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。
示例:
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
使用迭代法：遍历到新的数据value时，将value加入到当前vector的全部数组中生成新的数组，并加入到vector；
递归：使用额外的数据，当前数据加入时递归，以及当前数据不加入时也递归。最后能得到全部的结果。

```cpp
//递归解法
vector<int> cut_temp;
vector<vector<int>> result;
void dfs_search(int level,int n,vector<int> nums){
    if(level>=n){
        result.push_back(cut_temp);
        return;
    }
    cut_temp.push_back(nums[level]);
    dfs_search(level+1,n,nums);
    cut_temp.pop_back();
    dfs_search(level+1,n,nums);
}
vector<vector<int>> subsets(vector<int>& nums) {
    int n = nums.size();
    dfs_search(0,n,nums);
    return result;
}

//迭代法
class Solution {
public:
vector<vector<int>> subsets(vector<int>& nums) {
    //使用迭代法：遍历到新的数据value时，将value加入到当前vector的全部数组中生成新的数组，并加入到vector；
    vector<vector<int>> result;
    result.push_back({});
    int n = nums.size();
    for(int i=0;i<n;i++){
        int value = nums[i];
        int v_n = result.size();
        for(int j=0;j<v_n;j++){
            vector<int> temp = result[j];
            temp.push_back(value);
            result.push_back(temp);
        }
    }
    return result;
}
};
```


[17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/submissions/)

```
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。
```
```
vector<string> letterCombinations(string digits) {
    vector<string> ans;
    if(digits.length()==0) return ans;
    unordered_map<char,string> map{
        {'2',"abc"},
        {'3',"def"},
        {'4',"ghi"},
        {'5',"jkl"},
        {'6',"mno"},
        {'7',"pqrs"},
        {'8',"tuv"},
        {'9',"wxyz"}
    };
    search_dfs(ans,0,digits,"",map);
    return ans;
}
void search_dfs(vector<string>& ans,int level,string digits,string s,unordered_map<char,string> map){
    if (level==digits.length()) {
        ans.push_back(s);
        return;
    }
    else{
        int number = digits[level];
        string letters = map[number];
        for(auto letter:letters){
            s.push_back(letter);
            search_dfs(ans,level+1,digits,s,map);
            s.pop_back();
        }
    }
}
```
