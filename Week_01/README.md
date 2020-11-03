## 预习以及第一周：
数组、链表、跳表；栈、队列、优先队列\
学习笔记刷题的五毒神掌：\
刷题第一遍：\
5-10分钟思考,然后直接看解法。注意：看多道解法，比较孰优孰劣；背诵、默写好的解法\
刷题第二遍：
马上自己写，直至没bug多种解法比较，体会，优化；\
刷题第三遍：过一天后，再重复做题,不同解法的熟练程度练习\
刷题第四遍：
过一周后再反复回来练习，同时对于不熟练的题目再进行专项练习\
刷题第五遍：
面试前一周恢复性训练


时间复杂度：程序执行时需要花费的次数。通常有O(1)、O(logN)、O(n)、O(nlogN)、O(n^2)、O(K^n) 时间复杂度在n>10的时候会有天壤之别。 \
主定理对于递归函数的复杂度计算：\
二分查找：O(logN)\
二叉树遍历：每个节点 只遍历依次\
二维矩阵（有序）：O(n)\
归并排序：O(nlogn)

ps: 面试四件套：\
首先和面试官确认题目理解无误；\
想所有可能的解决办法，比较方法的时间/空间复杂度找到最好的；\
写代码\
测试结果


空间复杂度:\
额外空间数组的长度\
递归的深度

栈是先进后出；\
队列是先进先出

C++ 中对和栈的常用函数\
1，C++中队列queue的常用函数\
显示队顶元素：queue.front() \
出队：queue.pop(); \
进队：queue.push(val);\
2，栈的常用函数 \
显示栈顶元素：stack.top() \
出栈：stack.pop() \
进栈：stack.push(val)\
3，双端队列：\
简单理解：两端可以进出的 Queue. Deque - double ended queue. 插入和删除都是 O(1)\
双端队列的操作：deque.front()、\
deque.begin():显示头部值；\
deque.back():显示尾部值；\
deque.end():显示头部值后面的；\
deque.push_front(x):从前面插入x值；\
deque.pop_front(x):将前面的值去除掉；\
deque.push_back(x):从后面插入x值；\
deque.pop_back(x):将后面的值去除掉；\
4,Priority Queue.（==堆==） \
插入操作：O(1);\
取出操作：O(logN) - 按照元素的优先级取出;\
底层具体实现的数据结构较为多样和复杂：\
5,Python的优先队列:import heapq；\
双端队列：from collections import deque

**keypoint：刷题最忌讳的就是只刷一遍。解决问题的思维是升维和空间还时间**

### code:
#### 数组链表跳表
[283.移动零](https://leetcode-cn.com/problems/move-zeroes/)\

```
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```
使用变量记录非0数据下一个位置的下标，遍历nums，遇到非0数据，就交换非零与变量的位置，变量前进。

```cpp
void moveZeroes(vector<int>& nums) {
    //使用变量记录非0数据下一个位置的下标，遍历nums，遇到非0数据，就交换非零与变量的位置，变量前进。
    int nozero_idx = 0,n = nums.size();
    for(int i=0;i<n;i++){
        if(nums[i]!=0){
            int temp = nums[nozero_idx];
            nums[nozero_idx++]=nums[i];
            nums[i] = temp;
        }
    }
}
```

[1.两数之和](https://leetcode-cn.com/problems/two-sum/)\

```
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
给定 nums = [2, 7, 11, 15], target = 9
```
使用一个map每次记录当前数据的目标值的差值，并且在遍历的过程中遇到存在map中的数据就结束。
```python
diff_dict = {}
output = []
for i in range(len(nums)):
    if nums[i] in diff_dict:
        output.append(diff_dict[nums[i]])
        output.append(i)
        break
    diff_v = target - nums[i]
    diff_dict[diff_v] = i
return output
```

[15. 三数之和](https://leetcode-cn.com/problems/3sum/submissions/)

```
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0？
请你找出所有满足条件且不重复的三元组。
注意：答案中不可以包含重复的三元组。
给定数组 nums = [-1, 0, 1, 2, -1, -4]，
满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    int first,second,third;
    vector<vector<int>> result_num;
    vector<int> sort_nums;
    //num = quick_sort(nums,0,nums.size()-1);
    sort(nums.begin(), nums.end());
    int n_size = nums.size();
    for(first=0;first<n_size;first++){
        if(first!=0 && nums[first]==nums[first-1])
            continue;
        third = n_size-1;
        int target = -nums[first];
        for(second=first+1;second<n_size;second++){
            if(second!=first+1 && nums[second]==nums[second-1])
                continue;
            while(second<third && nums[second]+nums[third]>target)
                --third;
            if(second==third)
                break;
            if(nums[second]+nums[third]==target)
                result_num.push_back({nums[first], nums[second], nums[third]});               
        }
    }
    return result_num;
} 
```
[70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

```
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。
输入： 2
输出： 2
```

```cpp
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

[11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)\

```
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，
垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
说明：你不能倾斜容器，且 n 的值至少为 2。
```
双指针法，首尾指针head/tail执行首尾；移动的时候首指针往后移动，尾指针往前移动；每次移动较小的那个数值，计算移动之后的容量，更新最大容量值；最后输出容量值.
```cpp
int maxArea(vector<int>& height) {
   int n=height.size();
   if(n<1) return 0;
   int head=0,tail=nA-1;
   int max_capacity = (tail-head)*min(height[head],height[tail]);
   while(head<tail){
       if(height[head]<height[tail])
           head++;
       else
        tail--;
    int capacity = (tail-head)*min(height[head],height[tail]);
    if(max_capacity<capacity)
        max_capacity=capacity;
   }
   return max_capacity;
}
```
[206. 反转链表:](https://leetcode-cn.com/problems/reverse-linked-list/)\

```
反转一个单链表。
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？
```
反转时指针变换的顺序。
```cpp  
//递归
ListNode* reverseList(ListNode* head) {
    if(!head || !head->next) return head;
    ListNode* cur = reverseList(head->next);
    head->next->next = head;
    head->next = NULL;
    return cur;
}
//迭代
ListNode* reverseList(ListNode* head) {
    ListNode* cur=head;
    ListNode* pre=NULL;
    while(cur){
        ListNode* temp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = temp;
    }
    return pre;
}
```

[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
```

```cpp
ListNode* swapPairs(ListNode* head) {
    if(!head || !head->next) return head;
    ListNode* odd=head;
    //ListNode* even=head->next;
    ListNode* result = new ListNode(-1);
    //ListNode* result = even;
    ListNode* pre = result;
    //result = even;
    while(odd && odd->next){
        ListNode* even=odd->next;
        odd->next = even->next;
        even->next = odd;
        pre->next=even;
        pre=odd;
        odd = odd->next;
    }
    return result->next;
}
```

[141.环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)\

```
给定一个链表，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。为了表示给定链表中的环，我们使用整数 pos
来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
如果链表中存在环，则返回 true 。 否则，返回 false 。
```
核心是快慢指针，快的每次走两步，慢的每次走一步，代码如下:
```cpp
bool hasCycle(ListNode *head) {
    if(!head || !head->next) return false;
    ListNode* fast = head,*slow = head;
    bool cycle_flag = false;
    while(fast!=NULL && fast->next!=NULL){
        fast = fast->next->next;
        slow = slow->next;
        if(fast == slow){
            cycle_flag = true;
            break;
        }
    }
    if(cycle_flag) return true;
    return false;
    
}
```
有一个进阶的题目是找到循环的入口。思路也是快慢指针，前面与上面的思路一样，再结束之后，将快指针定位到头部，然后快慢指针每次前进一步，相遇时的位置就是入口位置。
#### 栈队列
[20.有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)
```
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。
输入: "()[]{}"
输出: true
```

```cpp
bool isValid(string s) {
    map<char,char> bracket_dt;
    stack<char> bracket_stack;
    bracket_dt['('] = ')';
    bracket_dt['['] = ']';
    bracket_dt['{'] = '}';
    bracket_dt['?'] = '?';
    bracket_stack.push('?');
    for(int i=0;i<s.size();i++){
        char cur = s[i];
        if(bracket_dt.find(cur)!=bracket_dt.end()){
            bracket_stack.push(cur);
        }
        else{
            char top_bk = bracket_stack.top();bracket_stack.pop();
            char top_bk_right = bracket_dt[top_bk];
            if(cur!=top_bk_right)
                return false;
        }
    }
    if(bracket_stack.size()>1)
        return false;
    return true;
}
```

[155.最小栈](https://leetcode-cn.com/problems/min-stack/)：


[26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/submissions//)：
```
给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
```
s使用数组在处理数据;遍历j指向当前非重复的位置，遍历数组，遇到与前面不同的数据就赋值到j的位置，j++；最后返回j. 
```
int removeDuplicates(vector<int>& nums) {
    int j=1;
    int n = nums.size();
    if(n<1) return 0;
    for(int i=1;i<n;i++){
        if(nums[i-1]==nums[i])
            continue;
        else{
            nums[j]=nums[i];
            j++;
        }
    }
    return j;
}
```

[21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)：

```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode* l3=new ListNode();
    ListNode* l3_head=l3;
    if(!l1) return l2;
    if(!l2) return l1;
    while(l1 && l2){
        if(l1->val<l2->val){
            l3->next=l1;
            l1=l1->next;
        }
        else{
            l3->next=l2;
            l2=l2->next;
        }
        l3=l3->next;               
    }
    if(l1) l3->next = l1;
    if(l2) l3->next = l2;
    return l3_head->next;
}
```

[239.滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/submissions/):

```
给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动
窗口内的 k 个数字。滑动窗口每次只向右移动一位。
进阶：
你能在线性时间复杂度内解决此题吗？
返回滑动窗口中的最大值。
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 
  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
使用双端队列处理滑动窗口的最大值；时间复杂度O(n),空间复杂度O(n)

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> window;
    vector<int> result;
    int n = nums.size();
    //初始化nums中前k个数据
    for(int i=0;i<k;i++){
        while(!window.empty() && nums[i] > nums[window.back()])
            window.pop_back();
        window.push_back(i);
    }
    result.push_back(nums[window.front()]);
    for(int i=k;i<n;i++){
        //将双端队列中不在k范围内的数据都去除掉
        if(!window.empty()&&window.front()<=i-k)
            window.pop_front();
        while(!window.empty()&&nums[i]>nums[window.back()])
            window.pop_back();
        window.push_back(i);
        result.push_back(nums[window.front()]);
    }
    return result;
}
```

