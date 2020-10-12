学习笔记
第三周
泛型与树的递归
1. 不要人肉进行递归（最大误区）
2. 找到最近最简方法，将其拆解成可重复解决的问题（重复子问题）
3. 数学归纳法思维

分治与回溯
本质上都是递归；找重复性以及分解问题。
回溯法采用试错的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。

code：
爬楼梯：https://leetcode-cn.com/problems/climbing-stairs/

翻转二叉树：https://leetcode-cn.com/problems/invert-binary-tree/description/
翻转二叉树
```cpp
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

括号生成:https://leetcode-cn.com/problems/generate-parentheses/submissions/
在生成过程中的剪枝：1，左括号随时可以生成，但是需要其生成的数量小于n；2，右括号在左括号后，且个数一定要小于左括号。
```cpp  
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
```

98. 验证二叉搜索树:https://leetcode-cn.com/problems/validate-binary-search-tree/solution/98yan-zheng-er-cha-ping-heng-shu-by-free_styles/
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

二叉树最大深度：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/104-er-cha-shu-de-zui-da-shen-du-ti-jie-by-free_st/
```cpp
int maxDepth(TreeNode* root) {
if(!root) return 0;
int left_depth = maxDepth(root->left);
int right_depth = maxDepth(root->right);
return 1+max(left_depth,right_depth); 
}
```

二叉树的最小深度：https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/

```cpp
int minDepth(TreeNode* root) {
    if(!root){
        return 0;
    }
    //到达根节点，返回1
    if(!root->left && !root->right){
        return 1;
    }
    //当节点的左右孩子一个是空一个不为空时，那么当前节点就不是叶子节点，要返回有孩子的那个节点
    int left_num = minDepth(root->left);
    int right_num = minDepth(root->right);
    //因为有一个孩子是空，所以left_num，right_num中肯定有一个是0.
    if(!root->left || !root->right) return left_num+right_num+1;
    return 1+min(left_num,right_num);   
}
```

二叉树广度遍历的代码框架,求二叉树的最大、最小深度、最大宽度的题解:
https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-yan-du-bian-li-de-dai-ma-kuang-jia-qiu-/

50. Pow(x, n):https://leetcode-cn.com/problems/powx-n/
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

子集：https://leetcode-cn.com/problems/subsets/solution/78zi-ji-by-free_styles/
使用额外的数组，当前数据加入时递归，以及当前数据不加入时也递归。最后能得到全部的集合
```cpp
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
```

17. 电话号码的字母组合:https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/submissions/
使用额外的数组，当前数据加入时递归，以及当前数据不加入时也递归。最后能得到全部的集合

