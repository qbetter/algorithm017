## week-04，深度-广度优先、贪心、二分查找学习笔记

### 模板
#### 深度和广度优先模板

深度优先递归版本
```cpp
//C/C++
//递归写法：
map<int, int> visited;

void dfs(Node* root) {
  // terminator
  if (!root) return ;

  if (visited.count(root->val)) {
    // already visited
    return ;
  }

  visited[root->val] = 1;

  // process current node here. 
  // ...
  for (int i = 0; i < root->children.size(); ++i) {
    dfs(root->children[i]);
  }
  return ;
}
```

深度优先非递归版本
```cpp
//C/C++
//非递归写法：
void dfs(Node* root) {
  map<int, int> visited;
  if(!root) return ;

  stack<Node*> stackNode;
  stackNode.push(root);

  while (!stackNode.empty()) {
    Node* node = stackNode.top();
    stackNode.pop();
    if (visited.count(node->val)) continue;
    visited[node->val] = 1;
    //由于栈先进后出的原则,所以从node最大的节点开始进栈
    for (int i = node->children.size() - 1; i >= 0; --i) {
        stackNode.push(node->children[i]);
    }
  }

  return ;
}
```

广度优先非递归版本
```cpp
// C/C++
void bfs(Node* root) {
  map<int, int> visited;
  if(!root) return ;

  queue<Node*> queueNode;
  queueNode.push(root);

  while (!queueNode.empty()) {
    Node* node = queueNode.top();
    queueNode.pop();
    if (visited.count(node->val)) continue;
    visited[node->val] = 1;

    for (int i = 0; i < node->children.size(); ++i) {
        queueNode.push(node->children[i]);
    }
  }

  return ;
}
```

广度优先非递归版本--不借助map，best；
```cpp
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
#### 二分查找模板
```cpp
int binarySearch(const vector<int>& nums, int target) {
  int left = 0, right = (int)nums.size()-1;
  
  while (left <= right) {
    int mid = left + (right - left)/ 2;
    if (nums[mid] == target) return mid;
    else if (nums[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}
```
\
\
#### 贪心算法
贪心算法是一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是全局最好或最优的算法。\
贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。\
适用贪心算法的场景:\
简单地说，问题能够分解成子问题来解决，子问题的最优解能递推到最终问题的最优解。这种子问题最优解称为最优子结构。\
贪心经典题目：
[分发饼干
](https://leetcode-cn.com/problems/assign-cookies/description/)、[跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

#### 二分查找
二分的前提：
1. 目标函数单调性（单调递增或者递减）
2. 存在上下界（bounded）
3. 能够通过索引访问（index accessible)
二分查找的经典题目：[x 的平方根](https://leetcode-cn.com/problems/sqrtx/)，[寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

## code习题:

102. 二叉树的层序遍历
用的是上面广度优先遍历非递归不借助map的模板。
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
     vector<int> temp;
    queue<TreeNode*> node_queue;
    if (!root)
        return {};      
    node_queue.push(root);
    while (!node_queue.empty()) {
        temp.clear();
        int node_size = node_queue.size();
        for (int i=0;i<node_size;i++) {
            TreeNode* curnode = node_queue.front();
            node_queue.pop();
            temp.push_back(curnode->val);
            if (curnode->left) {
                node_queue.push(curnode->left);
            }
            if (curnode->right) {
                node_queue.push(curnode->right);
            }
        }
        result.push_back(temp);
    }
    return result;
}
```
22. 括号生成
```cpp
void generate_fun(int left,int right,int n,vector<string>& output,string s) {
    //终止条件
    if (left==n && right==n) {
        output.push_back(s);
        return ;
    }
    //处理当前层
    string left_s = s + '(';
    string right_s = s + ')';
    //处理下层
    if (left < n) {
        generate_fun(left+1,right,n,output,left_s);
    }
    if (left > right) {
        generate_fun(left,right+1,n,output,right_s);
    }
}
vector<string> generateParenthesis(int n) {
    if (n==0) 
        return {""};
    vector<string> output;
    generate_fun(0,0,n,output,"");
    return output;
}
```

515. 在每个树行中找最大值
```cpp
vector<int> largestValues(TreeNode* root) {
    queue<TreeNode*> q;
    vector<int> result;
    if(!root)
    //#此处可以加入当root是空时的一些逻辑
        return {};
    q.push(root);
    while(!q.empty()){
        int q_size = q.size();
        //#此处若是判断最大宽度就可以在此处处理：
        int i = 0,max_number=INT_MIN;
        while(i < q_size){
            //将队列中的元素全部出队
            TreeNode* tmp = q.front();q.pop();
            //cout<<tmp->val;
            if (max_number<tmp->val) 
                max_number = tmp->val;
            //如果出队的数据有子节点，则入队
            if(tmp->left)
                q.push(tmp->left);
            if(tmp->right)
                q.push(tmp->right);
            i+=1;
        }
        result.push_back(max_number);
    }
    //##最后根据逻辑返回需要的数据
    return result;
}
```

55,跳跃游戏
维护一个最远达到距离变量，在这个距离里面，如果存在最远达到距离在数组长度。表示能够到达.
```cpp
bool canJump(vector<int>& nums) {
    int most_end = 0,n=nums.size();
    for (int i=0;i<n;i++) {
        if (i<=most_end) {
            most_end = max(i+nums[i],most_end);
            if (most_end>=n-1) return true;
        }
    }
    return false;
}
```
455,分发饼干
先对饼干和胃口进行排序，然后按照满足小孩子的胃口就可以了，可以使用贪心算法
```cpp
int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(),g.end());
    sort(s.begin(),s.end());
    int result = 0,i=0,j=0;
    while (i<s.size()&&j<g.size()) {
        if (s[i]>=g[j]) {
            j++;
            result++;
        }
        i++;
    }
    return result;
}
```

69, x 的平方根

```cpp
int mySqrt(int x) {
    //使用二分查找从1到x/2+1中间查找平方大于x的。
    int left=1,right=x/2+1,result = 0;
    while (left<=right) {
        int mid = left + (right-left)/2;
        long long temp = (long long)mid*mid;
        if (temp<=x) {
            result = mid;
            left=mid+1;
        }
        else 
            right = mid-1;
    }
    return result;
}
```