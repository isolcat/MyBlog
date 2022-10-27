---
title: JavaScript刷LeetCode
author: isolcat
datetime: 2022-9-11T03:42:51Z
slug: how-do-i-develop-my-terminal-portfolio-website-with-react
featured: false
draft: false
tags:
  - JavaScript
  - 算法
description:
  使用JavaScript刷穿LeetCode
---

# JavaScript刷LeetCode

## 二叉树的前 中 后序

给定一个二叉树的根节点`root`，返回他的中序遍历(使用遍历的方法)：

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var inorderTraversal = function(root) {
    const res=[];//使用数组来模拟栈
    const inorder=(root)=>{
        if(root==null){//当找不到任何节点的时候就return了
            return;
        }
        // 一个记忆的小技巧：二叉树的遍历，前序就是中左右(中在前)，后序就是左右中(中在后面)
        inorder(root.left);
        res.push(root.val);
        inorder(root.right);
    }
    inorder(root);
    return res;
};
```

代码模板基本就是这样子的，当遇见前序遍历的时候，将`res.push(root.val)`给移动到前面即可；后序遍历将`res.push(root.val)`移动到后面即可，其他的保持不变



## 删除字符串中所有相邻且相同的项

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

> 示例：
>
> 输入："abbaca"
> 输出："ca"
> 解释：
> 例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

```js
/**
 * @param {string} s
 * @return {string}
 */
var removeDuplicates = function(s) {
    const skt=[];//数组来模拟栈
    for(const ch of s){
        if(skt.length&&skt[skt.length-1]===ch){//将字符串的内容和栈中的队尾进行比较
            skt.pop();//相同就直接删除
        }
        else{
            skt.push(ch);//不一样的就插入栈中
        }
    }
    return skt.join('')//最后将数组按照字符串的方式进行合并(join)
};
```



## [删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

`快慢指针`：

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let slow=head,fast=head;//快慢指针初始化
    //先让快指针走n步
    while(n--){
        fast=fast.next;
    }
    // 当n==链表中节点总个数时，则为删除的是链表头结点
    if(!fast){
        return head.next;
    }
    //快慢指针一起遍历，知道fast指向最后一个节点
    while(fast.next){
        slow=slow.next;
        fast=fast.next;
    }
    // 再让慢指针指向他的下一个节点的下一个节点(执行删除操作)
    slow.next=slow.next.next
    return head;
};
```



## [有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

> 示例 1:
>
> 输入: s = "anagram", t = "nagaram"
> 输出: true
> 示例 2:
>
> 输入: s = "rat", t = "car"
> 输出: false

可以采用将字符串转为数组再进行排序的方法，也可以采用将`哈希表`的解法，这里重点讲哈希表

哈希表分为三种类型：`数组`,`set`,`map`。具体来讲就是：**哈希值比较小且范围比较小，就用数组，如果数值很大就用set，如果k要对应value的话就用map**

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if(s.length !== t.length) return false;
    const resSet = new Array(26).fill(0);//初始化数组，数据少且范围小的时候用数组来实现哈希表
    const base = "a".charCodeAt();//转为a的Unicode码
    for(const i of s) {
        //第一次循环将其存放在数组中
        resSet[i.charCodeAt() - base]++;
    }
    for(const i of t) {
        //第二次循环将之前循环得到的结果与第二个数组进行大小比较，
        if(!resSet[i.charCodeAt() - base]) return false;
        resSet[i.charCodeAt() - base]--;
    }
    return true;
};
```



## [两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

哈希表最擅长的就是解决`这个元素是否在集合里出现过`，遇见这种题的时候就应该去考虑使用哈希表

> 给定两个数组 nums1 和 nums2 ，返回 它们的交集 。输出结果中的每个元素一定是 唯一 的。我们可以 不考虑输出结果的顺序 。
>
> 示例 1：
>
> 输入：nums1 = [1,2,2,1], nums2 = [2,2]
> 输出：[2]
> 示例 2：
>
> 输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
> 输出：[9,4]
> 解释：[4,9] 也是可通过的

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    //将数组转成set
    let set1=new Set(nums1);
    let set2=new Set(nums2)
    if(set1.size>set2.size){//用size小的数组进行遍历
        [set1,set2]=[set2,set1]
    }
    const intersection=new Set();
    for(const num of set1){//遍历set1
        if(set2.has(num))//元素如果存在于set2中就加入intersection
        intersection.add(num);
    }
    return [...intersection]//转回数组
};
```

这里有个小技巧，在js中，我们将数组转为`set`的时候，根据集合的性质，里面是不会出现重复元素的，故用`let set1=new Set(nums1)`可以直接`去重`，更方便我们进行遍历，遍历的时候便无需再考虑重复的问题了，当遍历结束后再`return [...]`转回数组形式

> 扩展运算符(...)内部是使用for...of循环，也可以用于set结构，扩展运算符和set结合就可以去除数组的重复元素：
>
> ```js
> let arr=[1,2,2,3,4,7];
> let unique=[...new Set(arr)];//1,2,3,4,7
> ```

## [两数之和](https://leetcode.cn/problems/two-sum/)

> 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
>
> 你可以按任意顺序返回答案。
>
> 示例 1：
>
> 输入：nums = [2,7,11,15], target = 9
> 输出：[0,1]
> 解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
> 示例 2：
>
> 输入：nums = [3,2,4], target = 6
> 输出：[1,2]

该题为LeetCode的第一个算法题，这里之所以采用map来解，是因为此时我们不仅需要知道`元素的value`，并且我们要返回元素的`下标`，也就是我们常说的`key`,所以这个时候，我们排除掉数组和set，便只剩下map可以选择了。

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    const map=new Map()
    for(let i=0;i<nums.length;i++){
        //如果 key 存在则返回 true，否则返回 false。
        if(map.has(target-nums[i])){
            // map.get根据键来返回值，如果 map 中不存在对应的 key，则返回 undefined。
            return [map.get(target-nums[i]),i]//map的value为当前遍历的值，key为i
        }
        map.set(nums[i],i);//根据键存储值，便于后续的map遍历
    }
    return [];//什么都没有的话返回空数组
};
```



## [四数相加 II](https://leetcode.cn/problems/4sum-ii/)
