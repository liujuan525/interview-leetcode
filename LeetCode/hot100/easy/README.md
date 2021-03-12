
### 介绍

一句话总结算法思路，LeetCode hot100 easy

总计26题，题目列表请戳 [LeetCode Hot100 easy 列表](https://leetcode-cn.com/problemset/leetcode-hot-100/?difficulty=%E7%AE%80%E5%8D%95)。

全部源码可见我的GitHub [interview-leetcode](https://github.com/minibear2333/interview-leetcode)

注：

有下划线标志的都是超链接。
点击下列题目标题可以跳转到LeetCode中文官网直接阅读题目，提交代码。
点击下列代码链接，可以直接跳转到我的GitHub代码页面。每道题一般精选一种解法，我的GitHub中可能收录多种解法代码，请自行查看。

### 题解

#### 1.两数之和

题目：[数据中是否有两个数之和为目标值](https://leetcode-cn.com/problems/two-sum)

题解：遍历数组，用目标值减去当前值，判断HashMap是否有值存在，如果有则创建新数组返回两者，如果没有循环遍历完返回空数组

时间复杂度：O(1)
代码：[golang](../../all/1.两数之和.go)

#### 20.有效的括号
题目：[存在`[]{})(`的字符串，判断是否合法](https://leetcode-cn.com/problems/valid-parentheses)

题解： 存储左括号和右括号的映射，用栈统计左括号，出现左括号就入栈，出现右括号就和栈顶在map中映射的右括号比较，如果匹配就出栈，不匹配返回false，最后遍历完栈空为false

注意：go语言可以用byte代表单个字符

代码：[golang](../../all/20.有效的括号.go)

#### 21.合并两个有序链表
题目：[两个升序链表，合并成一个](https://leetcode-cn.com/problems/merge-two-sorted-lists)

题解：
* 需要一个空的头节点做辅助，`head.Next`就是结果
* 每次遍历始终维护上一个节点`prev`，初始值`prev=head`
* 循环遍历两个链表，循环条件都不为空，每次把当前节点更小的取出来即可

``` Go
prev.Next = l1
prev = l1
l1 = l1.Next
```

* 最后加入有不为空的节点，则直接赋值

``` Go
if l1!=nil{
	prev.Next = l1
}else{
	prev.Next = l2
}
```

代码：[golang](../../all/21.合并两个有序链表.go)

#### 53.最大子序和
题目：[求和加起来最大的连续子数组](https://leetcode-cn.com/problems/maximum-subarray)

题解：
* 一次循环，数组里有可能出现负数，且只需要统计和即可
* 需要两个计数器，一个存储全局最大的子序列和，只要出现更大的就更新
* 另一个存储当前最大的子序和，判断当前最大子序和的方法是比较子序和与当前值哪个大
* 有可能当前值比子序列和大，就更新子序 `max(nums[i],nums[i]+last)`

核心代码

``` Go
last = max(nums[i],nums[i]+last)
resMax = max(resMax,last)
```

代码：[golang](../../all/53.最大子序和.go)
	
#### 70.爬楼梯
题目：[一次可以上一阶或者二阶，如果是n阶有多少种爬法](https://leetcode-cn.com/problems/climbing-stairs/)

题解：斐波那契数列，返回结果是前两个值的和，只需要保存前两个值和当前结果，递推赋值即可

代码：[golang](../../all/70.爬楼梯.go)
	
#### 101.对称二叉树
题目：[二叉树是不是镜像的](https://leetcode-cn.com/problems/symmetric-tree/description/)

题解： 
* 递归，相当于使用了前序遍历和后续遍历相等的性质
* 函数签名`isMirror(root,root)`，判断前序和后续相等

``` Go
q.Val == p.Val && isMirror(p.Left,q.Right) && isMirror(p.Right,q.Left)
```

* 注意都为空时`true`，其中一个为空时`false`

``` Go
if p == nil && q == nil {
    return true
}
if p==nil || q==nil{
    return false
}
```

代码：[golang](../../all/101.对称二叉树.go)

#### 104.二叉树的最大深度
题目：[根节点到最远叶子节点的最长路径上的节点数](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/description/)

题解：递归，前序遍历，返回值为左右节点最大深度+1，退出条件为null节点返回0，左右子树都为空返回1

代码：[golang](../../all/104.二叉树的最大深度.go)

#### 121.买卖股票的最佳时机
题目：[给定整数数组表示每天股票价格，买一次卖一次求最大收益](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/description/)，要求必须先买再卖

题解：与目前最小值做差，得到当前最大值，更新最大值，一次循环。核心代码如下

``` Go
if v<minNum{
	minNum = v
}else if v - minNum > maxNum{
	maxNum = v - minNum
}
```

代码：[golang](../../all/121.买卖股票的最佳时机.go)
	
#### 136.只出现一次的数字
题目：[数组中某元素只出现一次，其余两次。找那个元素](https://leetcode-cn.com/problems/single-number/description/)

题解：
* 方法一、直接用map统计，超过一次就删掉
* 方法二、每个值都异或，最终得到的就是答案
* 异或的性质：`a^a=0, a^0=a`

代码：[golang](../../all/136.只出现一次的数字.go)
	
#### 141.环形链表
题目：[判断链表中是否有环](https://leetcode-cn.com/problems/linked-list-cycle/description/)

题解：快慢指针，相等时退出，注意循环条件

``` Go
p.Next != nil && q.Next != nil && q.Next.Next != nil
```

代码：[golang](../../all/141.环形链表.go)

#### 155.最小栈
题目：[](https://leetcode-cn.com/problems/min-stack/description/)

题解：


需要辅助栈。【同步栈】，【不同步栈】。
代码：[golang]()

#### 160.相交链表
题目：

题解：


所有寻找同一节点链表题的本质（如相交，环），都是对加法交换律的花式运用。尤其注意代码中如果写成

```
        while(A!=B){
            A=A.next==null? headB:A.next;
            B=B.next==null? headA:B.next;
        }
```

这个循环就永远出不去了。。。
代码：[golang]()

#### 169.多数元素
题目：

题解：


我将这种算法称为 【抱对自杀】，每当一个众数碰到一个非众数就会抱住，然后双双自杀。如果没有遇到足够的非众数，众数们就会等人到齐了再抱对自杀。最后留下来的当然是众数。
代码：[golang]()

#### 206.反转链表
题目：

题解：


其实是234. 回文链表 的一部分。
代码：[golang]()

#### 226.翻转二叉树
题目：

题解：


自身递归，“交换”左右子树时记得备份。
代码：[golang]()
	
#### 234.回文链表
题目：

题解：


快慢指针（整除器），把剩下的一半变成逆序，再进行比较。注意奇偶情况讨论。
代码：[golang]()

#### 283.移动零
题目：

题解：


将非零元素依次交换到前面来。
代码：[golang]()

#### 448.找到所有数组中消失的数字
题目：

题解：


把数字放到对应位置上去，最后依次找出没有正确摆放数字的位置。千万注意，调换的时候，数组的角标含义可能会发生变化。
代码：[golang]()


#### 461.汉明距离
题目：

题解：


十进制向二进制转换技巧：整除/2，或右移>>1；异或^的用法。
1.   把二叉搜索树转换为累加树：树的经典题，dfs，右孩子-根-左孩子。
2.   二叉树的直径：dfs求节点到叶子节点的距离，叶子节点的到叶子节点距离定义为1。dfs遍历所有节点找出最大直径。
3.   最短无序连续子数组：四遍循环。从左至右找无序子数组极小值，从右至左找无序子数组极大值，从左至右找无序子数组左边起始点，从右至左找无序子数组右边起始点。 
4.   合并二叉树：递归，经典划分：树=根+左子树+右子树，跟437 路径总和 III的思想是一样的。

代码：[golang]()

#### 543.二叉树的直径
题目：

题解：


代码：[golang]()
#### 617.合并二叉树
题目：

题解：


代码：[golang]()

### 最后

如果文中有误，欢迎提pr或者issue，**一旦合并或采纳作为贡献奖励可以联系我直接无门槛**加入[技术交流群](https://mp.weixin.qq.com/s/ErQFjJbIsMVGjIRWbQCD1Q)

我是小熊，关注我，知道更多不知道的技术

![](https://coding3min.oss-accelerate.aliyuncs.com/2021/03/11/gQDiQ51116.jpg)