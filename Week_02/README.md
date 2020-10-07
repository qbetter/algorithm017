第二周：哈希表、映射、集合；树、二叉树、二叉搜索树；堆和图
学习笔记

一，哈希表、映射、集合
哈希表（Hash table）， 也叫散列表，是根据关键码值（Key value）而直接进行访问的数据结构。又叫散列表、散列函数。通过哈希函数将数据映射到某个位置，查找时可以快速查找，若碰撞时需要消除碰撞。java和C++是map，Python是dict。
C++ map的使用：unordered_map:
遍历：
for ( auto it = mymap.begin(); it != mymap.end(); ++it )
    cout << " " << it->first << ":" << it->second;
插入：map.insert(value);map[1]=value;
删除：map.erase(value);map.erase(map.begin());
查找：if(map.find ("coffee")!=map.end()) print('没找到');

二，树、二叉树、二叉搜索树
熟知二叉树的前中后遍历。
二叉搜索树， 也称二叉排序树、有序二叉树（Ordered Binary Tree）、 排序二叉树（Sorted Binary Tree）， 是指一棵空树或者具有下列性质的二叉
树：
1. 左子树上所有结点的值均小于它的根结点的值；
2. 右子树上所有结点的值均大于它的根结点的值；
3. 以此类推：左、右子树也分别为二叉查找树。 
其中序遍历之后是升序排列


三，堆和图
堆：heap。heap：可以迅速找到一堆树中最大和最小值的结构。根节点最大的叫大顶堆，根节点最小的叫小顶堆；常见的堆有二叉树堆和斐波那契堆。其操作的时间复杂度如下：
find-max:O(1);delete-max:O(logn);insert:O(logn);
二叉堆的性质：
1，二叉堆是一颗完全二叉树；2，树中的任意节点总是大于其子节点。
二叉堆一般是可以通过数组来实现的，根节点的索引为0；那么节点i的左孩子节点是(2*i+1),右孩子节点是(2*i+2),父节点是((i-1)/2)。

增删操作：
1，插入：新元素一律插到堆的尾部，然后再依次向上调整整个堆的结构。
2，删除：将堆尾元素替换成堆顶元素，然后再依次向下调整整个堆的结构。


PS:
1，链是特殊化的树，树是特殊化的图。
若树只有一个孩子节点，那么就是树；
常用的树是二叉树，也会有多叉树。如果树的能够构成闭环的话，那就是图。所以说树是特殊化的图。
2，树的出现
玄学点就是它符合人的生活经验。

code:
有效的字母异位词:https://leetcode-cn.com/problems/valid-anagram/

字母异位词分组:https://leetcode-cn.com/problems/group-anagrams/solution/zi-mu-yi-wei-ci-fen-zu-by-leetcode/

94. 二叉树的中序遍历:https://leetcode-cn.com/problems/binary-tree-inorder-traversal/

144. 二叉树的前序遍历:https://leetcode-cn.com/problems/binary-tree-preorder-traversal/

N 叉树的层序遍历:https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/description/

两数之和:https://leetcode-cn.com/problems/two-sum/description/
