预习以及第一周：数组、链表、跳表；栈、队列、优先队列
学习笔记刷题的五毒神掌：
刷题第一遍：
5-10分钟思考
然后直接看解法。注意：看多道解法，比较孰优孰劣；
背诵、默写好的解法
刷题第二遍：
马上自己写，直至没bug
多种解法比较，体会，优化；
刷题第三遍：
过一天后，再重复做题
不同解法的熟练程度练习
刷题第四遍：
过一周后再反复回来练习，同时对于不熟练的题目再进行专项练习
刷题第五遍：
面试前一周恢复性训练


时间复杂度：程序执行时需要花费的次数。通常有O(1)、O(logN)、O(n)、O(nlogN)、O(n^2)、O(K^n) 时间复杂度在n>10的时候会有天壤之别。 
主定理对于递归函数的复杂度计算：
二分查找：O(logN)
二叉树遍历：每个节点 只遍历依次
二维矩阵（有序）：O(n)
归并排序：O(nlogn)

ps: 面试四件套：
首先和面试官确认题目理解无误；
想所有可能的解决办法，比较方法的时间/空间复杂度找到最好的；
写代码
测试结果

空间复杂度
额外空间数组的长度
递归的深度


栈是先进后出；
队列是先进先出：
C++ 中对和栈的常用函数 1，C++中队列queue的常用函数： 显示队顶元素：queue.front() 出队：queue.pop(); 进队：queue.push(val);
2，栈的常用函数 显示栈顶元素：stack.top() 出栈：stack.pop() 进栈：stack.push(val)
3，双端队列：简单理解：两端可以进出的 Queue. Deque - double ended queue. 插入和删除都是 O(1) 操
4,Priority Queue.  插入操作：O(1); 取出操作：O(logN) - 按照元素的优先级取出; 底层具体实现的数据结构较为多样和复杂：
5，Python的优先队列import heapq；双端队列：from collections import deque

keypoint：刷题最忌讳的就是只刷一遍。解决问题的思维是升维和空间还时间

code:
数组链表跳表
283. 移动零:https://leetcode-cn.com/problems/move-zeroes/

15. 三数之和:https://leetcode-cn.com/problems/3sum/submissions/

70. 爬楼梯:https://leetcode-cn.com/problems/climbing-stairs/?utm_source=LCUS&utm_medium=ip_redirect_q_uns&utm_campaign=transfer2china

11. 盛最多水的容器:https://leetcode-cn.com/problems/container-with-most-water/

206. 反转链表:https://leetcode-cn.com/problems/reverse-linked-list/

24. 两两交换链表中的节点:https://leetcode-cn.com/problems/swap-nodes-in-pairs/

栈队列
有效的括号：https://leetcode-cn.com/problems/valid-parentheses/

最小栈：https://leetcode-cn.com/problems/min-stack/

26. 删除排序数组中的重复项：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/submissions/

合并两个有序链表：https://leetcode-cn.com/problems/merge-two-sorted-lists/



