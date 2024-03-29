# 数组

## 基础习题锻炼

### 二分查找

>给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。
>
>例如：左闭右闭区间
>
>```
>输入: nums = [-1,0,3,5,9,12], target = 9
>输出: 4
>解释: 9 出现在 nums 中并且下标为 4
>```
>
```js
var search = function(nums, target) {
let left = 0;
let right = nums.length - 1;
while(left <= right){   
   let middle = Math.floor((right - left)/2) + left
   if(nums[middle] > target){
       right = middle -1;
   }else if(nums[middle] < target){
       left = middle +1;
   }else{
       return middle
   }
}
return -1

};
```





### 移除元素

>给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
>
>不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
>
>元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
>
>```
>输入：nums = [3,2,2,3], val = 3
>输出：2, nums = [2,2]
>解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
>```
>

移除元素用到的做法是快慢指针，其中快指针表示新数组的元素，慢指针是新数组的下标

- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

```js
//时间复杂度O(n),空间复杂度O(1)，类似js数组delete方法接口的时间复杂度也是O(n)
var removeElement = function(nums, val) {
    let slow = 0; //slow是慢指针
    for(let i = 0;i < nums.length; i++){ //这里的i其实代表快指针front
        if(nums[i] !== val){
            nums[slow] = nums[i]
            slow++; //这句和上面的代码也可以整合写成nums[slow++] = nums[i]
        }
    }
    return slow
};
```

### 有序数组的平方

>使用双指针的方法，一般的while里面的条件和两个指针的位置有关
>
>给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序
>
>```
>输入：nums = [-4,-1,0,3,10]
>输出：[0,1,9,16,100]
>解释：平方后，数组变为 [16,1,0,9,100]
>排序后，数组变为 [0,1,9,16,100]
>```

```js
//双指针方法
var sortedSquares = function(nums) {
    let n = nums.length;
    let res = new Array(n).fill(0); //新数组
    let i = 0, j = n - 1, k = n - 1;  //i相当于左指针，j相当于右指针，k是新数组的指针
    while (i <= j) { //一定要等于，否则会错过等于时的元素
        let left = nums[i] * nums[i],
            right = nums[j] * nums[j];
        if (left < right) {
            res[k] = right;
            k--; //res[k] = right;和k--; 可以结合写成res[k--] = right;
            j--;
        } else {
            res[k] = left;
            k--;
            i++;
        }
    }
    return res;
};



//api方法：注意，这里使用map方法当前所选元素是必填项，例如 nums=[1,2,3] nums.map(parseInt)=[1,NaN,NaN]
//平方可以用Math.pow(x,2)  sort在排序时如果没有具体传入排序规则默认是按照ASCII顺序排序，即使是数组中是数字，也会转化成字符串按照ASCII排序，因此排序出来的结果不是正确结果
// (sort排序方法：https://m.php.cn/article/480049.html)
var sortedSquares = function(nums) {
    let newMums = nums.map((e) => e*e)
    function f(a,b){ //从小到大，从大到小则是-(a-b)
        return (a-b)
    }
    return newMums.sort(f)
};
```

### 长度最小的子数组

>给定一个含有 n 个正整数的数组和一个正整数 target 。
>
>找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。
>
>```
>输入：target = 7, nums = [2,3,1,2,4,3]
>输出：2
>解释：子数组 [4,3] 是该条件下的长度最小的子数组。
>
>输入：target = 11, nums = [1,1,1,1,1,1,1,1]
>输出：0
>```

```js
var minSubArrayLen = function(target, nums) {
    let i=sum=sublength=0; //i为起始位置，j为终止位置
    //result初始值一定是最大值，而最大值是length+1而不是length，这里的result可以等于Infinity
    let result = nums.length+1  
    for(let j=0;j<nums.length;j++){
        sum += nums[j]
        while(sum>=target){
            sublength = j-i+1   //相当于一个中间缓存值
            result=sublength<result? sublength:result
            sum -= nums[i]
            i++ 
        }
    }
    return result==nums.length+1? 0:result 
    //如果result为Infinity则这里也要判断是否等于Infinity，相当于没有进入过第二个while里面
};

```





## 热题

### 34. 在排序数组中查找元素的第一个和最后一个位置

>给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。
>
>如果数组中不存在目标值 target，返回 [-1, -1]。你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。
>
>```
>输入：nums = [5,7,7,8,8,10], target = 8
>输出：[3,4]
>
>输入：nums = [5,7,7,8,8,10], target = 6
>输出：[-1,-1]
>```

```js
api方法 //indexOf的方法，不止可以用于string，也可以用于数组
var searchRange = function(nums, target){
    if(nums.length <= 0){
        return [-1, -1];
    }
   let first = nums.indexOf(target);
   let last = nums.lastIndexOf(target);
   return [first,last];
}
```



### 283. 移动零

>给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
>```
>输入: nums = [0,1,0,3,12]
>输出: [1,3,12,0,0]
>
>输入: nums = [0]
>输出: [0]
>```

```js
//直接移动非零值，剩下的全部置为0
var moveZeroes = function(nums) {
    let slow = 0
    for(let i=0;i<nums.length;i++){
        if(nums[i] !== 0){
            nums[slow] = nums[i]
            slow++
        }
    }
     for(let j=slow;j<nums.length;j++){
         nums[j] = 0
    }
    return nums
};

//也可以一次排序，原理是中间交换
var moveZeroes = function(nums) {
    let slow = 0,tem=0;
    for(let i=0;i<nums.length;i++){
        if(nums[i] !== 0){
            tem = nums[slow]
            nums[slow] = nums[i]
            nums[i] = tem
            slow++
        }
    }
    return nums
};
如果看不懂可以看：https://www.bilibili.com/video/BV1EB4y1Q7qV?spm_id_from=333.337.search-card.all.click
里面包含了两种方法
```



## 练习题

### 904. 水果成篮

> 你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 fruits 表示，其中 fruits[i] 是第 i 棵树上的水果 种类 。
>
> 你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：
>
> 你只有 两个 篮子，并且每个篮子只能装 单一类型 的水果。每个篮子能够装的水果总量没有限制。
> 你可以选择任意一棵树开始采摘，你必须从 每棵 树（包括开始采摘的树）上 恰好摘一个水果 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
> 一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。
> 给你一个整数数组 fruits ，返回你可以收集的水果的 最大 数目
>
> ```
> 输入：fruits = [1,2,1]
> 输出：3
> 解释：可以采摘全部 3 棵树。
> 
> 输入：fruits = [0,1,2,2]
> 输出：3
> 解释：可以采摘 [1,2,2] 这三棵树。
> 如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。
> ```
>
> 教程：
>
> https://www.bilibili.com/video/BV1d7411Q7vx?spm_id_from=333.337.search-card.all.click&vd_source=62f15965c1a32c0c4f9349c8ea68315b

这道题用的是滑动窗口配合在map存储的最后key和value判断是否大于两个种类的水果（key代表2个不重复时出现的树对应的值，value代表不重复时最后出现（因为是在不断更新的）对应的下标）

```js
var totalFruit = function(fruits) {
    const map=new Map()
    let max = 1; //当只有一棵树的时候最大为1
    let j=0; //起始位置
    for(let i=0;i<fruits.length;i++){ //i代表终止位置
        map.set(fruits[i],i); //存入map中并且key为数值，value为实时更新最后出现的下标值
        if(map.size>2){
            let minIndex = Infinity;
            //移除较小的那个数值
            for(let [fruits,index] of map){
                if(index<minIndex){
                    minIndex = index
                }
            }
            map.delete(fruits[minIndex])
            j=minIndex+1
        }
        max = Math.max(max,i-j+1) //每次都比较哪个比较大
    }
    return max
};
```



## 总结

![数组总结](D:\notesfor\6-数据结构与算法\asseet\数组总结.png)



# 链表

## 基础习题锻炼

### 移除链表

>给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点**

```js
var removeElements = function(head, val) {
    //定义一个虚拟头节点
    var dummyHead = new ListNode(0,head)
    var tempNode = dummyHead 
    //定义一个临时指针来遍历，如果直接使用头节点遍历的话头节点不断改变最后无法获得原本的头节点
    //头节点指向dummyHead而不是dummyHead.next的原因在于如果指向的是next则在删除时无法获得上一个链表的值，无法删除
    while(tempNode.next){ //指向实际上的头节点
        if(tempNode.next.val === val){ //是next.val因为只有next.val才知道上一个链表的位置
            tempNode.next = tempNode.next.next
            continue //这里一定要有continue,否则会报错；因为不执行tempNode = tempNode.next这句
        }
        tempNode = tempNode.next
    }
    return dummyHead.next
};
```



### 设计链表

>设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。
>
>在链表类中实现这些功能：
>
>get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
>addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
>addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
>addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
>deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

```js
// //使用class关键字定义一个类，本质上是一个function，定义了同一组对象（又称为实例）共有的属性和方法

// //constructor方法是类的默认方法，通过new生成实例对象的时候就会被调用，一个类必须有constructor方法，
// //如果没有显式定义，一个空的constructor就会被创建
// //一个类只有一个constructor方法，里面专门用于创建和初始化一个由class创建的对象

// //类里面共有的属性和方法必须要添加this使用
// //constructor 里面的 this 指向的是创建的实例对象
class LinkNode {
    //定义节点
    constructor(val, next) {
        this.val = val;
        this.next = next;
    }
}

/**
 * Initialize your data structure here.
 * 单链表 储存头尾节点 和 节点数量
 */
var MyLinkedList = function() {
    this.size = 0;
    this.tail = null;
    this.head = null;
};

/**
 * Get the value of the index-th node in the linked list. If the index is invalid, return -1. 
 * @param {number} index
 * @return {number}
 */
//定义一个getNode可以方便下面获取index下的节点
MyLinkedList.prototype.getNode = function(index) {
    if(index < 0 || index >= this.size) return null;
    // 创建虚拟头节点
    let cur = new LinkNode(0, this.head);
    // 0 -> head
    while(index>=0) { //这里要有等于0的情况
        cur = cur.next;
        index--;
    }
    return cur;
};
MyLinkedList.prototype.get = function(index) {
    if(index < 0 || index >= this.size) return -1;
    // 获取当前节点
    return this.getNode(index).val;
};

/**
 * Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtHead = function(val) { //这里的第一个元素表示头节点
    const node = new LinkNode(val, this.head);
    this.head = node; //新节点作为新的头节点
    this.size++; //节点数加1
    if(!this.tail) {
        this.tail = node;
    }
};

/**
 * Append a node of value val to the last element of the linked list. 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtTail = function(val) {
    const node = new LinkNode(val,null)
    this.size++;
    if(this.tail){
        this.tail.next = node; //交换位置
        this.tail = node;
        return;
    }
    this.tail = node;
    this.head = node;
};


/**
 * Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. 
 * @param {number} index 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtIndex = function(index, val) {
    // 当index > size,索引越界，不能插入节点
    if(index > this.size) return;
     // 当index <= 0,新插入的节点为头节点
    if(index <= 0) {
        this.addAtHead(val);
        return;
    }
    if(index === this.size) {
        this.addAtTail(val);
        return;
    }
    // 获取目标节点的上一个的节点
    const node = this.getNode(index - 1);
    node.next = new LinkNode(val, node.next);
    this.size++;
};

/**
 * Delete the index-th node in the linked list, if the index is valid. 
 * @param {number} index
 * @return {void}
 */
MyLinkedList.prototype.deleteAtIndex = function(index) {
    //判断index是否在存在的节点之内
    if(index < 0 || index >= this.size) return;
    if(index === 0) {
        this.head = this.head.next;
        // 如果删除的这个节点同时是尾节点，要处理尾节点
        if(index === this.size - 1){
            this.tail = this.head
        }
        this.size--;
        return;
    }
    // 获取目标节点的上一个的节点
    const node = this.getNode(index - 1);    
    node.next = node.next.next;
    // 处理尾节点
    if(index === this.size - 1) {
        this.tail = node;
    }
    this.size--;
};
```



### 反转链表

>给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表
>
>```
>输入：head = [1,2,3,4,5]
>输出：[5,4,3,2,1]
>```

```js
var reverseList = function(head) {
    if(!head || !head.next) return head;
    var cur = head; //双指针
    var pre = null; //pre指向虚拟头，即head的前一个，即null
    var temp = null //定义一个临时链表指向cur.next
    while(cur){ //里面的条件相当于当cur指向null的时候结束
        temp = cur.next //得先保存cur的下一个节点位置以便cur移动时可以赋值
        cur.next = pre  //反转
        pre = cur //往前走
        cur = temp
    }
    return pre
};
```



### 两两交换链表中的节点

>给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
>
>```
>输入：head = [1,2,3,4]
>输出：[2,1,4,3]
>```

```js
var swapPairs = function(head) {
    var dummpHead = new ListNode(0,head)
    var cur = dummpHead
    while(cur.next && cur.next.next){
        //这里代表偶数时cur的下一个为null时跳出循环；或者奇数时cur.next.next为null时退出循环
        var temp = cur.next  //这句和下一句一定要放在循环里面，因为temp和temp1是每次都会改变的
        var temp1 = cur.next.next.next
        cur.next = cur.next.next;
        cur.next.next = temp;
        temp.next = temp1;  //这句也可改为cur.next.next.next = temp1
        cur = cur.next.next;
    }
    return dummpHead.next
};
```



### 删除链表的倒数第n个节点

>给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。
>
>```
>输入：head = [1,2,3,4,5], n = 2
>输出：[1,2,3,5]
>
>输入：head = [1], n = 1
>输出：[]
>```

```js
var removeNthFromEnd = function(head, n) {
    var dummyHead = new ListNode(0,head);
    var fast = dummyHead;
    var slow = dummyHead; //快慢指针
    while(n+1){
        fast = fast.next;
        n--;
    }
    while(fast){ 
        //当快指针等于null时，也就是指向最后尾节点null(null相当于0,也就是false，会自动跳出循环)
        //其他不为0的时候为true，会执行while里面的内容
         fast = fast.next;
         slow = slow.next;
    }
    //删除节点
    slow.next= slow.next.next
    return dummyHead.next
};
```



### 环形链表

>给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
>
>如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
>
>```
>输入：head = [3,2,0,-4], pos = 1
>输出：返回索引为 1 的链表节点
>解释：链表中有一个环，其尾部连接到第二个节点。
>
>输入：head = [1,2], pos = 0
>输出：返回索引为 0 的链表节点
>解释：链表中有一个环，其尾部连接到第一个节点。
>```

```js
var detectCycle = function(head) {
    var fast = slow = head
    while(fast && fast.next){ 
        //判断fast节点以及其下一位是否为null
        //如果没有fast.next则当其为null时执行fast.next.next会报错
        fast = fast.next.next;
        slow = slow.next;
        if(fast === slow){
            var index1 = fast;
            var index2 = head;
            while(index1 !== index2){
                index1 = index1.next;
                index2 = index2.next
            }
            return index1;
        }
    }
    return null;
};
```



## 总结

### 链表的基础知识

- 链表的种类主要为：单链表，双链表，循环链表
- 链表的存储方式：链表的节点在内存中是分散存储的，通过指针连在一起

![链表总结](D:\notesfor\6-数据结构与算法\asseet\链表总结.png)



# 哈希表

> **当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**

## 基础习题锻炼

### 有效的字母异位词

>给定两个字符串 `s` 和 `t` ，编写一个函数来判断 `t` 是否是 `s` 的字母异位词
>
>```
>输入: s = "anagram", t = "nagaram"
>输出: true
>
>输入: s = "rat", t = "car"
>输出: false
>```

```js
//方法一
var isAnagram = function(s,t){
    function getLocation(str){
        let arr = Array(26).fill(0);
        let base = 'a'.charCodeAt();
        for(let i=0;i<str.length;i++){
            ascii = str[i].charCodeAt() - base;
            arr[ascii]++;
        }
        return arr.join('')
    }
    return getLocation(s) === getLocation(t)
}
//方法二
var isAnagram = function(s, t) {
    if(s.length !== t.length) return false;
    const resSet = new Array(26).fill(0);
    const base = "a".charCodeAt();
    for(const i of s) {
        resSet[i.charCodeAt() - base]++;
    }
    for(const i of t) {
        if(!resSet[i.charCodeAt() - base]) return false;
        resSet[i.charCodeAt() - base]--;
    }
    return true;
};
```



### 两个数组的交集

>给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序**
>
>```
>输入：nums1 = [1,2,2,1], nums2 = [2,2]
>输出：[2]
>
>输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
>输出：[9,4]
>解释：[4,9] 也是可通过的
>```

```js
//使用set做去重操作
var intersection = function(nums1, nums2) {
    var set = new Set(nums1)
    var res = new Set()
    for(let i=0;i<nums2.length;i++){
        if(set.has(nums2[i])){
            res.add(nums2[i])
        }
    }
    return [...res];  //这里不能直接return res,因为无法return一个set，只能把它转成数组输出
};
```



### 快乐数

>编写一个算法来判断一个数 n 是不是快乐数。
>
>「快乐数」 定义为：
>
>对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
>然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
>如果这个过程 结果为 1，那么这个数就是快乐数。
>如果 n 是 快乐数 就返回 true ；不是，则返回 false 。
>
>```
>输入：n = 19
>输出：true
>解释：
>1^2 + 9^2 = 82
>8^2 + 2^2 = 68
>6^2 + 8^2 = 100
>1^2 + 0^2 + 0^2 = 1
>```

```js
function happyNums(n){
    var sum = 0;
    var arr = n.toString().split('');
    for(let i=0;i<arr.length;i++){
        sum += arr[i] * arr[i]
    }
    return sum
}
var isHappy = function(n) {
    let set = new Set()
    while(n !== 1 && !set.has(n)){
        set.add(n)
        n = happyNums(n)
    }
    return n === 1;
};
```



### 两数之和

>给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。
>
>例如：
>
>```
>输入：nums = [2,7,11,15], target = 9
>输出：[0,1]
>解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1]
>```

自己写的暴力解法：双层循环，复杂度为O(n^2)

```js
var twoSum = function(nums, target) {
    for(let i=0;i<nums.length;i++){
        for(let j=i+1;j<nums.length;j++){
            if(nums[i]+nums[j] === target){
                return [i,j]
            }
        }
    }
};
twoSum([2,7,11,15],9)
```

优化解法：利用HashMap，复杂度为O(n)，在循环的时候进行查找将HashMap的Key定位nums[i]，Value定义为i

```js
var twoSum = function(nums, target) {
	const map = new Map();
    for(let i=0;i<nums.length;i++){
        const diff = target - nums[i]; //计算差值
        if(!map.has(diff)){ //如果差值不在map中则意味着另一个元素还未进入map
            map.set(nums[i],i)  //把目前的key和value值录入，其中key是nums[i],value是i
    }else {
        return [map.get((diff)),i];  //如果差值已经存在则直接输出目前的一个i和存在map里面的diff的key
    }
   }
};
twoSum([2,7,11,15],9)
```

一些函数知识点：

- has(key):has()函数返回一个布尔值,表示某个键(key)是否在当前的Map实例中;
- set(key,value):set()函数设置键名key所对应的键值为value,该函数的返回值是当前的Map实例;
- get(key):get()函数返回key对应的键值,如果找不到对应的key,则返回undefined.



### 四数相加

>给你四个整数数组 nums1、nums2、nums3 、 nums4 ，数组长度都是 n ，请计算多少个元组 (i, j, k, l) 能满足：
>- 0 <= i, j, k, l < n
>- nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
>
>```
>输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
>输出：2
>解释：
>两个元组如下：
>1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
>2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
>```

```js
//自己的写法：
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    var map = new Map();
    for(let i=0;i<nums1.length;i++){
        for(let j=0;j<nums2.length;j++){
            var sum = nums1[i] + nums2[j]
            var count = 1;
            if(!map.has(sum)){ //如果元素不存在的时候
                map.set(sum,count)
            }else{
                var temp = map.get(sum)
                temp = temp + 1;
                map.set(sum,temp)
            }
        }
    }

    var sum = 0;
    for(let i=0;i<nums3.length;i++){
        for(let j=0;j<nums4.length;j++){
            var diff =  0 - (nums3[i] + nums4[j])
            if(map.has(diff)){ //如果里面有存在的话
                sum = sum + map.get(diff)
            }
        }
    }
    return sum
};

//更简洁的写法：
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    const twoSumMap = new Map();
    let count = 0;

    for(const n1 of nums1) {
        for(const n2 of nums2) {
            const sum = n1 + n2;
            twoSumMap.set(sum, (twoSumMap.get(sum) || 0) + 1)
        }
    }

    for(const n3 of nums3) {
        for(const n4 of nums4) {
            const sum = n3 + n4;
            count += (twoSumMap.get(0 - sum) || 0)
        }
    }

    return count;
};
```



### 三数之和

>给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。
>注意：答案中不可以包含重复的三元组。
>
>```
>输入：nums = [-1,0,1,2,-1,-4]
>输出：[[-1,-1,2],[-1,0,1]]
>
>输入：nums = [0,1,1]
>输出：[]
>```

难点在于去重，去重就不适用于哈希表的方法，因此在这里使用双指针的方法

```js
var sortFunction = function(a,b){
    return a-b
}
var threeSum = function(nums) {
    nums.sort(sortFunction) //先排序
    var res = []
    for(let i=0;i<nums.length;i++){
        if(i>0 && nums[i] > 0) return res; //和前一个做对比去重，不能和后面一个(对nums[a]去重)
        if(nums[i] == nums[i-1]) continue; //若第一个大于0则不可能相加等于0，跳出循环
        var left = i + 1;
        var right = nums.length - 1;
        while(left < right){
            var sum = nums[i] + nums[left] + nums[right];
            if(sum > 0){
                right--;
            }else if(sum < 0){
                left++;
            }else{
                res.push([nums[i],nums[left],nums[right]]);
                while(left < right && nums[left] == nums[left+1]){
                    left++; //对nums[b]去重
                }
                while(left < right && nums[right] == nums[right-1]){
                    right--; //对nums[c]去重
                }
                left++;
                right--;
            }
        }
    }
    return res 
};
```



### 四数之和

>

```js
//相比三数之和多了一层嵌套
var fourSum = function(nums, target) {
    const len = nums.length;
    if(len < 4) return []; //没有四个数不符合题意直接返回空数组
    nums.sort((a, b) => a - b);
    const res = [];
    for(let i = 0; i < len; i++) {
        // 一级去重,去重i
        if(i > 0 && nums[i] === nums[i - 1]) continue;
        for(let j = i + 1; j < len; j++) {
            // 二级去重,去重j
            if(j > i + 1 && nums[j] === nums[j - 1]) continue;
            let l = j + 1, r = len - 1;
            while(l < r) {  //这里的逻辑和三数之和的逻辑一样,只是写的更简洁
                const sum = nums[i] + nums[j] + nums[l] + nums[r];
                if(sum < target) { l++; continue}
                if(sum > target) { r--; continue}
                res.push([nums[i], nums[j], nums[l], nums[r]]);
                while(l < r && nums[l] === nums[++l]);
                while(l < r && nums[r] === nums[--r]);
            }
        } 
    }
    return res;
};
```



## 热题

### 49. 字母异位词分组

>给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
>字母异位词 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。
>
>```
>输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
>输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
>
>输入: strs = [""]
>输出: [[""]]
>```

```js
var groupAnagrams = function(strs) {
    var map = new Map();
    for(let i=0;i<strs.length;i++){
        //对每一个数组内的值排序（因为sort方法是数组的方法，因此要先转成数组再排序再转成字符串）
        let key = strs[i].split('').sort().join(''); 
        if(map.get(key)){  //map中是否有值，其中key为排序后的字符串
            var temp = map.get(key);  //临时数组
            temp.push(strs[i]);
            map.set(key,temp);  //相同key下赋值后一个会覆盖前一个
        }else{
            map.set(key,[strs[i]]);
        }
    }
    return [...map.values()];  //map.values获取的值是类数组，需要转化为数组才能输出
    //Array.from(map.values())
};
```



### 438. 找到字符串中所有字母异位符

>给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。异位词 指由相同字母重排列形成的字符串（包括相同的字符串)
>
>```
>输入: s = "cbaebabacd", p = "abc"
>输出: [0,6]
>解释:
>起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
>起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
>
>输入: s = "abab", p = "ab"
>输出: [0,1,2]
>解释:
>起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
>起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
>起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
>```

```js
```



# 字符串

## 基础习题锻炼

### 反转字符串

>编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。
>不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题
>
>```
>输入：s = ["h","e","l","l","o"]
>输出：["o","l","l","e","h"]
>```
>
>api方法可以使用reverse，但是这里要求的是O(1)

```js
var reverseString = function(s) {
    //Do not return anything, modify s in-place instead.
    reverse(s)
};

var reverse = function(s) {
    var start = 0;
    var end = s.length - 1;
    while (start < end) {
        temp = s[start];
        s[start] = s[end];
        s[end] = temp;
        start++;
        end--;
    }
};

//reverse也可以写成下面更简洁的形式
var reverse = function(s) {
    let l = -1, r = s.length;
    while(++l < --r) [s[l], s[r]] = [s[r], s[l]];
};
```



### 反转字符串II

>给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。
>
>如果剩余字符少于 k 个，则将剩余字符全部反转。
>如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。
>
>```
>输入：s = "abcdefg", k = 2
>输出："bacdfeg"
>```
>解决方法：对for里面的条件进行处理，不要用常规的i++

```js
var reverseStr = function(s, k) {
    const len = s.length;
    let resArr = s.split(""); 
    for(let i = 0; i < len; i += 2 * k) {
        let l = i - 1, r = i + k > len ? len : i + k;
        while(++l < --r) [resArr[l], resArr[r]] = [resArr[r], resArr[l]];
    }
    return resArr.join("");
};
```



### 替换空格

>请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"
>
>```
>输入：s = "We are happy."
>输出："We%20are%20happy."
>```

```js
//api方法：
1.replace&replaceAll(/ /g代表全部都替换，如果只是' '就只会替换一次)
var replaceSpace = function(s) {
    return s.replace(/ /g,'%20')
};

2. split('') + join('%20')

3. 双指针方法
 var replaceSpace = function(s) {
   // 字符串转为数组
  const strArr = Array.from(s);
  let count = 0;

  // 计算空格数量
  for(let i = 0; i < strArr.length; i++) {
    if (strArr[i] === ' ') {
      count++;
    }
  }

  let left = strArr.length - 1;
  let right = strArr.length + count * 2 - 1;

  while(left >= 0) {
    if (strArr[left] === ' ') {
      strArr[right--] = '0';
      strArr[right--] = '2';
      strArr[right--] = '%';
      left--;
    } else {
      strArr[right--] = strArr[left--];
    }
  }

  // 数组转字符串
  return strArr.join('');
}; 
```



### 左旋转字符串

>字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"
>
>```
>输入: s = "abcdefg", k = 2
>输出: "cdefgab"
>```

```js
var reverseLeftWords = function (s, n) {
    /** Utils */
    function reverseWords(strArr, start, end) {
        let temp;
        while (start < end) {
            temp = strArr[start];
            strArr[start] = strArr[end];
            strArr[end] = temp;
            start++;
            end--;
        }
    }
    /** Main code */
    let strArr = s.split('');
    let length = strArr.length;
    reverseWords(strArr, 0, n - 1);
    reverseWords(strArr, n, length - 1);
    reverseWords(strArr, 0, length - 1);
    return strArr.join('');
    
    /** Main code也可以是这样 */
    let strArr = s.split('');
    let length = strArr.length;
    reverseWords(strArr, 0, length - 1);
    reverseWords(strArr, 0, length - n - 1);
    reverseWords(strArr, length - n, length - 1);
    return strArr.join('');
};
```



## 热题

### 3. 无重复字符的最长子串

>给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度
>
>例如：
>
>```
>输入: s = "abcabcbb"
>输出: 3 
>解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
>
>输入: s = "bbbbb"
>输出: 1
>解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
>```

解法：使用滑动窗口和双指针，在滑动窗口滑动时相当于把所有子串提取出并且比较是否有重复项，时间复杂度是O(n)

讲解视频：https://www.bilibili.com/video/BV113411v7Ak?spm_id_from=333.337.search-card.all.click&vd_source=62f15965c1a32c0c4f9349c8ea68315b

```js
var lengthOfLongestSubstring = function(s) {
    let set = new Set();
    let left=right=length=maxlength=0;

    while(right<s.length){
        if(!set.has(s[right])){
            set.add(s[right]);
            length++;
            if(length>maxlength){
                maxlength = length;
            }
            right++;
        }else{
            while(set.has(s[right])){
                set.delete(s[left]);
                left++;
                length--;
            }
            set.add(s[right]);
            length++;
            right++;
        }
    }
    return maxlength;
};
```

### 5. 最长回文子串

>给你一个字符串 `s`，找到 `s` 中最长的回文子串。
>
>例如：
>
>```
>输入：s = "babad"
>输出："bab"
>解释："aba" 同样是符合题意的答案。
>
>输入：s = "cbbd"
>输出："bb"
>```

解法：简单快速的解法是中心扩散法，也可以使用动态规划；动态规划适用范围更广

```js
//中心扩散法
var longestPalindrome = function(s) {
    let max = ''

    for(let i=0; i< s.length; i++) {
        // 分奇偶， 一次遍历，每个字符位置都可能存在奇数或偶数回文
        helper(i, i)
        helper(i, i+1)
    }

    function helper(l, r) {
        // 定义左右双指针，满足条件由中心外扩
        while(l>=0 && r< s.length && s[l] === s[r]) {
            l--
            r++
        }
        // 拿到回文字符， 注意 上面while满足条件后多执行了一次，所以需要l+1, r+1-1=r
        const maxStr = s.slice(l + 1, r);
        // 取最大长度的回文字符
        if (maxStr.length > max.length) max = maxStr
    }
    return max //做完所有function之后以及for循环后才输出最后的max
};
```

动态规划方法：注意就按照以下五步方法解决动态规划问题

- dp数组的定义和下标表示的意思
- 递推公式
- dp数组如何初始化，初始化也需要注意（应该初始化为0还是1）
- 遍历顺序，比较考究，例如是先遍历背包还是物品
- （若出现问题）打印dp数组（打印dp数组，检查是否有问题，检验1 2 3 4）

```js
var longestPalindrome = function (s) {
    const n = s.length;
    const dp = new Array(n).fill(0).map(() => new Array(n).fill(0));
    let ans = s[0]; //因为最后输出的回文子串最小也是一个数字（当只有一个/两个字母时）
 
    //先确定对角线为1，1表示true，都是属于回文子串
    for(let i=0;i<n;i++){
        dp[i][i] = 1
    }

    //遍历顺序 一般都是两个for循环，因为j>i，所以二维数组只有上半部分有可能有值
    for(let j=1;j<n;j++){ 
        for(i=j-1;i >= 0; i--){
            //两种情况：只有两个且两个相同 / 大于两个并且外层相同，里层的也是回文子串
            if(s[i] === s[j] && (j-i === 1 || dp[i+1][j-1])) {
                dp[i][j] = 1 //记录其为回文子串
                if( ans.length < j-i+1){
                    ans = s.slice(i,j+1)
                }
            }
        }
    }
    return ans
}
```



# 栈与队列

>栈和队列在js中其实都是用数组代表，在进入的时候都是push，只是在出来的时候队列是先进先出，用的是shift方法（删除数组第一个元素，返回删除值）；栈用的是pop（删除数组最后一个元素，返回删除值）
>

## 基础习题锻炼

### 用栈模拟队列

>#### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

```js
var MyQueue = function() {
    this.stackIn = [];
    this.stackOut = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function(x) {
    this.stackIn.push(x)
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function() {
    var size = this.stackOut.length
    if(size){
        return this.stackOut.pop()
    }
    while(this.stackIn.length){
        this.stackOut.push(this.stackIn.pop())
    }
    return this.stackOut.pop()
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function() {
    const x = this.pop()
    this.stackOut.push(x)
    return x
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function() {
    return !this.stackIn.length && !this.stackOut.length
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.empty()
 */
```

### 用队列模拟栈

>#### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

### 有效括号

>给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
>
>有效字符串需满足：
>
>左括号必须用相同类型的右括号闭合。
>左括号必须以正确的顺序闭合
>
>```
>输入：s = "()"
>输出：true
>
>输入：s = "([)]"
>输出：false
>```

```js
var isValid = function(s) {
    if(s.length%2 !==0) return false;
    const stack = [];
    for(let i=0;i<s.length;i++){
        if(s[i] == '('){
            stack.push(')')
        }else if(s[i] == '{'){
            stack.push('}')
        }else if(s[i] == '['){
            stack.push(']')
        }else{  //处理两种情况：不匹配以及栈为空的时候还有未匹配项
            if(stack.length==0 || s[i] !== stack.pop()) return false
        }
    } //最后判断栈是否为空，即匹配完了但是还有剩余的是多出来的情况
    return stack.length == 0
};
```

### 逆波兰表达式

>根据 逆波兰表示法，求表达式的值。
>有效的算符包括 +、-、*、/ 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。
>注意 两个整数之间的除法只保留整数部分。
>可以保证给定的逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。
>
>```
>输入：tokens = ["2","1","+","3","*"]
>输出：9
>解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
>```

```js
var evalRPN = function(tokens) {
    const s = new Map([
        ["+", (a, b) => a * 1  + b * 1],
        ["-", (a, b) => b - a],
        ["*", (a, b) => b * a],
        ["/", (a, b) => (b / a) | 0] //这里为什么要有0？
    ]);
    const stack = [];
    for (const i of tokens) {
        if(!s.has(i)) {
            stack.push(i);
        }else{
            stack.push(s.get(i)(stack.pop(),stack.pop()))
        }
    }
    return stack.pop();
};
```

### 前k个高频元素

>用了哈希表和堆排序（堆其实就是一个二叉树）

```js
```





# 二叉树

## 理论知识

>满二叉树：其节点数量为2^k - 1，其中k为深度（从1，2，3这样开始计算）
>
>完全二叉树：除了底层以外，其他层都是满的，底层是从左到右连续的（底层不一定满，但是从左到右节点是连续的）--> 满二叉树一定是完全二叉树
>
>二叉搜索树：节点是有顺序的，左边的节点都小于中间节点；右边的都大于中间节点；左子树也是满足上述规律；搜索一个节点的时间复杂度为logn（对节点布局没有要求，只是对顺序有要求）
>
>平衡二叉搜索树：左子树和右子树的高度的绝对值的差值不能超过1；左子树也符合同样的上述规则  

## 基础习题锻炼

### 递归遍历

>- 前序遍历：中左右
>- 中序遍历：左中右
>- 后序遍历：左右中
>
>做遍历的思路有以下三点：
>
>- 确定递归函数的参数和返回值
>- 确定终止条件
>- 确定单层递归的逻辑

```js
/**
这里是js定义二叉树结点，需要知道他的原理
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)  //undefined是没有节点的情况
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */ 
/**
 * @param {TreeNode} root
 * @return {number[]}
 */

//前序遍历：中左右
var preorderTraversal = function(root) {
    var res = [];
    const dfs = function(root){
        if(root === null) return;
        res.push(root.val); //中
        dfs(root.left);  //左
        dfs(root.right);  //右
    }
    dfs(root);
    return res;
};

//中序遍历：左中右
var preorderTraversal = function(root) {
    var res = [];
    const dfs = function(root){
        if(root === null) return ;
        dfs(root.left);  //左
        res.push(root.val); //中
        dfs(root.right);  //右
    }
    dfs(root);
    return res;
};

//后序遍历：左右中
var preorderTraversal = function(root) {
    var res = [];
    const dfs = function(root){
        if(root === null) return;
        dfs(root.left);  //左
        dfs(root.right);  //右
        res.push(root.val); //中
    }
    dfs(root);
    return res;
};
```



### 非递归遍历

>使用栈来模拟递归的逻辑

```js
//前序遍历：中左右 但是用栈来模拟的顺序就是中右左
var preorderTraversal = function(root) {
    var res = []; //存放最后结果的数组
    if(!root) return res;
    var stack = [root]; //模拟栈
    while(stack.length){
        var node = stack.pop();
        res.push(node.val)
        if(node.right){
            stack.push(node.right)
        }
        if(node.left){
            stack.push(node.left)
        }
    }
    return res
}

//中序遍历：入栈的时候是左 右; 出栈的时候是左中右
//因为处理顺序和遍历顺序不一样，所以需要一个指针来帮助遍历二叉树
var inorderTraversal = function(root) {
    var res = [];
    var stack = [];
    let cur = root
    while(stack.length || cur){ //相当于stack.length!==0 && cur!==0
        if(cur){ //相当于if(cur!==0)
            stack.push(cur) //记录指针访问过的元素
            cur = cur.left //一路向左，遇到空再处理右孩子
        }else{ //空节点的情况
            cur = stack.pop()
            res.push(cur.val)
            cur = cur.right  //cur指针来遍历节点，栈来记录遍历过的元素
        }
    }
    return res
};

//后序遍历，就是前序遍历的左右调换位置并且最后返回的翻转过后的结果（中左右 --> 中右左 --> 左右中）
var postorderTraversal = function(root) {
    var res = []; //存放最后结果的数组
    if(!root) return res;
    var stack = [root]; //模拟栈
    while(stack.length){
        var node = stack.pop();
        res.push(node.val)
        if(node.left){
            stack.push(node.left)
        }
        if(node.right){
            stack.push(node.right)
        }
    }
    return res.reverse()
};
```

### 层序遍历

>层序遍历需要配合一个队列来存放每一层元素的length和对应的值1

```js
var levelOrder = function(root) {
    //二叉树的层序遍历
    let res=[],queue=[];
    queue.push(root);
    if(root===null){
        return res;
    }
    while(queue.length!==0){
        // 记录当前层级节点数
        let length=queue.length;
        //存放每一层的节点 
        let curLevel=[];
        for(let i=0;i<length;i++){
            let node=queue.shift();
            curLevel.push(node.val);
            // 存放当前层下一层的节点
            if(node.left){
                queue.push(node.left);
            }
            if(node.right){
                queue.push(node.right);
            }
        }
        //把每一层的结果放到结果数组
        res.push(curLevel);
    }
    return res;
};
```

### 翻转二叉树

>利用递归法翻转二叉树

```js
var invertTree = function(root) {
    if(root){
        [root.left,root.right] = [root.right,root.left] //ES6的语法 前序遍历
        if(root.left) invertTree(root.left)
        if(root.right) invertTree(root.right)
    }
    return root
};
```

### 对称二叉树

>给定一个二叉树，检查它是否是镜像对称的
>做法：只能使用后序遍历，因为要收集孩子的信息（左节点和右节点的信息）向上一级节点返回

```js
var isSymmetric = function(root) {
    //使用递归遍历左右子树 递归三部曲
    // 1. 确定递归的参数 root.left root.right和返回值true false 
    const compareNode=function(left,right){
        //2. 确定终止条件 空的情况
        if(left===null&&right!==null||left!==null&&right===null){
            return false;
        }else if(left===null&&right===null){
            return true;
        }else if(left.val!==right.val){
            return false;
        }
        //3. 确定单层递归逻辑
        let outSide=compareNode(left.left,right.right); //这里其实就在做递归，里层会一直递归到null
        let inSide=compareNode(left.right,right.left);
        return outSide&&inSide;
    }
    if(root===null){
        return true;
    }
    return compareNode(root.left,root.right);
};
```

### 二叉树的最大深度

>深度指的是节点到根节点的节点数
>高度指的是节点到叶子节点的节点数
>例如根节点为3；二层节点为4 6；三层节点为8 null 2 1的二叉树 其深度从根节点到叶子节点分别是3 2 1；而深度从根节点到叶子节点分别是1 2 3
>使用前序遍历可以求得深度，使用后序遍历可以求得高度
>在这里使用的是后序遍历，因为**二叉树根节点的高度等于二叉树的最大深度**

```js
//迭代法
var maxdepth = function(root) {
    //使用递归的方法 递归三部曲
    //1. 确定递归函数的参数和返回值
    const getdepth=function(node){
    //2. 确定终止条件
        if(node===null){
            return 0;
        }
    //3. 确定单层逻辑 
        let leftdepth=getdepth(node.left); //统计左子树的最大深度
        let rightdepth=getdepth(node.right);
        let depth=1+Math.max(leftdepth,rightdepth); //这里使用的是后序遍历 左右中 此处是中处理depth
        return depth;
    }
    return getdepth(root);
};

//迭代版的简洁写法
var maxdepth = function(root) {
    if (root === null) return 0;
    return 1 + Math.max(maxdepth(root.left), maxdepth(root.right))
};

//层序遍历基础上加上参数count
var maxDepth = function(root) {
    if(!root) return 0
    let count = 0
    const queue = [root]
    while(queue.length) {
        let size = queue.length
        /* 层数+1 */
        count++
        while(size--) {
            let node = queue.shift();
            if(node.left) queue.push(node.left);
            if(node.right) queue.push(node.right);
        }
    }
    return count
};
```

### 二叉树的最小深度

>要确定一个根节点的左子树或者右子树没有值的情况

```js
//递归法
var minDepth = function(root) {
     //使用递归的方法 递归三部曲
    //1. 确定递归函数的参数和返回值
    const getdepth=function(node){
    //2. 确定终止条件
        if(node===null){
            return 0;
        }
    //3. 确定单层逻辑
        let leftdepth=getdepth(node.left);
        let rightdepth=getdepth(node.right);
        if(node.left===null&&node.right!==null){
            return 1+rightdepth
        }
        if(node.left!==null&&node.right===null){
            return 1+leftdepth
        }
        let depth=1+Math.min(leftdepth,rightdepth); //这里使用的是后序遍历 左右中 此处是中处理depth
        return depth;
    }
    return getdepth(root);
};

//迭代法
var minDepth = function(root) {
    if(!root) return 0
    let count = 0
    const queue = [root]
    while(queue.length) {
        let size = queue.length
        /* 层数+1 */
        count++;
        while(size--) {
            let node = queue.shift();
            // 到第一个叶子节点 返回 当前深度
            if(!node.left && !node.right) return count;
            if(node.left) queue.push(node.left);
            if(node.right) queue.push(node.right);
        }
    }
    return count
};
```

### 完全二叉树的节点数量

>用普通的前序遍历，后序遍历，中序遍历，层序遍历得到包含所有节点的数组最后再输出数组长度（或者层序遍历最后使用`let final = res.flat(Infinity)展平`）是一种方法，但是这对于普通二叉树和完全二叉树都可以使用，并且它由于需要搜索每一个节点因此它的复杂度是O(n)
>
>如果要利用完全二叉树的性质：如果其子树(例如左子树)为完全二叉树，则左子树的数量为（2^深度）- 1；

```js
//普通二叉树都适用的做法
var countNodes = function(root){
    var res = [];
    const dfs = function(root){
        if(root === null) return;
        res.push(root.val); //中
        dfs(root.left);  //左
        dfs(root.right);  //右
    }
    dfs(root);
    return res.length;
};

//也可以用递归法
var countNodes = function(root) {
    const getNodeSum=function(node){
        if(node===null){
            return 0;
        }
        let leftNum=getNodeSum(node.left);
        let rightNum=getNodeSum(node.right);
        return leftNum+rightNum+1;
    }
    return getNodeSum(root);
};

//利用完全二叉树的性质
var countNodes = function(root) {
    //利用完全二叉树的特点
    //此处的终止条件有两种情况，第一种是运行到最后叶子节点的下一位空节点
    //第二种是满二叉树的条件按照公式得到二叉树的节点数量
    if(root===null){
        return 0; //第一种情况
    }
    //第二种情况：直接递归左侧和右侧的节点，不需要计算中间的节点
    let left=root.left;
    let right=root.right;
    let leftDepth=0,rightDepth=0;
    while(left){
        left=left.left;
        leftDepth++; //得到左边的深度
    }
    while(right){
        right=right.right;
        rightDepth++;//得到右侧的深度
    }
    if(leftDepth==rightDepth){ //如果左右侧深度相等则为完全二叉树
        return Math.pow(2,leftDepth+1)-1;
    }
    let leftNum = countNodes(root.left);
    let rightNum = countNodes(root.right);
    let res = leftNum + rightNum + 1; //加上根节点
    return res; //后序遍历
};
```



### 平衡二叉树

>判断一个树是否为平衡二叉树：仍然使用后序遍历求高度，基本方法是基于二叉树的最大深度

```js
//迭代法
var isBalanced = function(root) {
    //还是用递归三部曲 + 后序遍历 左右中 当前左子树右子树高度相差大于1就返回-1
    // 1. 确定递归函数参数以及返回值
    const getDepth = function(node) {
        // 2. 确定递归函数终止条件
        if(node === null) return 0;
        // 3. 确定单层递归逻辑
        let leftDepth = getDepth(node.left); //左子树高度
        // 当判定左子树不为平衡二叉树时,即可直接返回-1
        if(leftDepth === -1) return -1;
        let rightDepth = getDepth(node.right); //右子树高度
        // 当判定右子树不为平衡二叉树时,即可直接返回-1
        if(rightDepth === -1) return -1;
        if(Math.abs(leftDepth - rightDepth) > 1) {
            return -1;
        } else {
            return 1 + Math.max(leftDepth, rightDepth);
        }
    }
    return !(getDepth(root) === -1);
};
```



### 二叉树的所有路径

>给定一个二叉树，返回所有从根节点到叶子节点的路径

```js
var binaryTreePaths = function(root) {
   //递归遍历+递归三部曲
   let res=[];
   //1. 确定递归函数 函数参数
   const getPath=function(node,curPath){
    //2. 确定终止条件，到叶子节点就终止
       if(node.left===null&&node.right===null){
           curPath+=node.val; //把叶子节点也加入进去
           res.push(curPath);
           return ;
       }
       //3. 确定单层递归逻辑:这里使用前序遍历，因为只有前序遍历才可以从父节点一次向子左右节点遍历
       curPath+=node.val+'->';  //中(逻辑处理)
       if(node.left) getPath(node.left,curPath);  //左
       if(node.right) getPath(node.right,curPath);  //右
   }
   getPath(root,'');
   return res;
};
```



### 左叶子之和

>计算给定二叉树的所有左叶子之和

```js
var sumOfLeftLeaves = function(root) {
    var getSum = function(root){
        //终止条件
        if(root===null) return 0; //根节点为空的情况直接返回0
        //叶子节点返回0因为只知道是叶子节点，不知道是否是左节点，因此在下面重新覆盖
        if(root.left===null && root.right===null) return 0; 
        let leftNum = sumOfLeftLeaves(root.left)
        if(root.left!==null && root.left.left===null && root.left.right===null){
            leftNum=root.left.val;  //重新覆盖左叶子值
        }
        let rightNum = sumOfLeftLeaves(root.right)
        return leftNum + rightNum; //用的是后序遍历
    }
    return getSum(root);
};
```



### 找树左下角的值

>给定一个二叉树，在树的最后一行找到最左边的值

```js
//迭代法只需要把最后一行的第一个值输出即可
var findBottomLeftValue = function(root) {
   //二叉树的层序遍历
    let res=[],queue=[];
    let layout=0;
    queue.push(root);
    if(root===null){
        return res;
    }
    while(queue.length!==0){
        // 记录当前层级节点数
        let length=queue.length;
        //存放每一层的节点 
        let curLevel=[];
        layout++;
        for(let i=0;i<length;i++){
            let node=queue.shift();
            curLevel.push(node.val);
            // 存放当前层下一层的节点
            if(node.left){
                queue.push(node.left);
            }
            if(node.right){
                queue.push(node.right)
            }
        }
        //把每一层的结果放到结果数组
        res.push(curLevel);
    }
    return res[layout-1][0]
};


//递归法 递归法找深度最大的最左侧的叶子节点
var findBottomLeftValue = function(root) {
    //首先考虑递归遍历 前序遍历 找到最大深度的叶子节点即可
    let maxPath = 0,resNode = null;
    // 1. 确定递归函数的函数参数，maxPath用来记录最大深度，resNode记录最大深度最左节点的数值
    const dfsTree = function(node,curPath){ //此处的curPath是记录深度
    // 2. 确定递归函数终止条件，当遇到叶子节点时，需要统计一下最大的深度，因此遇到叶子节点来更新最大深度
        if(node.left===null&&node.right===null){
            if(curPath>maxPath){
            maxPath = curPath;
            resNode = node.val;
            }
        }
        //前序遍历，此处前为空，因为只需要左右即可
        if(node.left) dfsTree(node.left,curPath+1); //先判断是否为空节点,上面操作空节点会异常
        if(node.right) dfsTree(node.right,curPath+1); //先判断是否为空节点,上面操作空节点会异常
    }
    dfsTree(root,1);
    return resNode;
};
```



### 路径总和

>给二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标

```js
//基于二叉树的所有路径之和可以直接修改一小部分得出
var hasPathSum = function(root, targetSum) {
   //递归遍历+递归三部曲
   let res=[];
   //1. 确定递归函数 函数参数
   const getPath=function(node,curPath){ //没有这步的话会空指针异常报错,为空节点
       if(node === null){
           if(targetSum=0){
               return true;
           }else{
               return false;
           }
       }
    //2. 确定终止条件，到叶子节点就终止
       if(node.left===null&&node.right===null){
           curPath+=node.val; //把叶子节点也加入进去
           res.push(curPath);
           return ;
       }
       //3. 确定单层递归逻辑:这里使用前序遍历，因为只有前序遍历才可以从父节点一次向子左右节点遍历
       curPath+=node.val;  //中(逻辑处理)
       if(node.left) getPath(node.left,curPath);  //左
       if(node.right) getPath(node.right,curPath);  //右
   }
   getPath(root,0);
   for(let i=0;i<res.length;i++){
       if(res[i] === targetSum) return true;
   }
   return false
};

//
```



### 从中序与后序遍历构造二叉树

>给定中序和后序的二叉树构造二叉树

```js
//给定中序和后序
var buildTree = function(inorder, postorder) {
    if (!inorder.length) return null;
    let rootVal = postorder.pop(); // 从后序遍历的数组中获取中间节点的值， 即数组最后一个值
    let rootIndex = inorder.indexOf(rootVal); // 获取中间节点在中序遍历中的下标
    let root = new TreeNode(rootVal); // 创建中间节点
    root.left = buildTree(inorder.slice(0, rootIndex), postorder.slice(0, rootIndex)); // 创建左节点
    root.right = buildTree(inorder.slice(rootIndex + 1), postorder.slice(rootIndex)); // 创建右节点
    return root;
};

//给定中序和前序
var buildTree = function(preorder, inorder) {
  if (!preorder.length) return null;
  const rootVal = preorder.shift(); // 从前序遍历的数组中获取中间节点的值， 即数组第一个值
  const index = inorder.indexOf(rootVal); // 获取中间节点在中序遍历中的下标
  const root = new TreeNode(rootVal); // 创建中间节点
  root.left = buildTree(preorder.slice(0, index), inorder.slice(0, index)); // 创建左节点
  root.right = buildTree(preorder.slice(index), inorder.slice(index + 1)); // 创建右节点
  return root;
};
```



### 构造最大二叉树

>凡是构造二叉树的都应该用到前序遍历的方法，因为前序是中左右，先把根节点构造出来然后才能构造左子树和右子树

```js
var constructMaximumBinaryTree = function (nums) {
    const BuildTree = (arr, left, right) => {
        if (left > right) //重点在于这里，他的终止条件（因为leecode里面规定nums.lenght大于1）
            return null;
        let maxValue = -1;
        let maxIndex = -1;
        for (let i = left; i <= right; ++i) {
            if (arr[i] > maxValue) {
                maxValue = arr[i];
                maxIndex = i;
            }
        }
        let root = new TreeNode(maxValue);
        root.left = BuildTree(arr, left, maxIndex - 1);
        root.right = BuildTree(arr, maxIndex + 1, right);
        return root;
    }
    let root = BuildTree(nums, 0, nums.length - 1);
    return root;
};
```



### 合并二叉树

```js
//迭代法 前序b
var mergeTrees = function(root1, root2) {
    if(root1===null) return root2;
    if(root2===null) return root1;
    root1.val += root2.val
    root1.left = mergeTrees(root1.left,root2.left);
    root1.right = mergeTrees(root1.right,root2.right);
    return root1;
};
```



### 验证二叉搜索树

```js
//中序遍历得到的数组是递增的，直接判断是否是递增函数即可
var isValidBST = function(root) {
    let res = []
    var dfs = function(root){
        if(root===null) return;
        dfs(root.left)
        res.push(root.val)
        dfs(root.right)
    }
    dfs(root)
    for(let i=1;i<res.length;i++){
        if(res[i] <= res[i-1]){
            return false
        }
    }
    return true
};
```



### 最近公共祖先

![](C:\Users\Z\Desktop\前端\面试准备问题.assets\Snipaste_2022-10-03_01-26-10.png)

```js
var lowestCommonAncestor = function(root, p, q) {
    // 使用递归的方法
    // 需要从下到上，所以使用后序遍历
    // 1. 确定递归的函数
    var ancestor = function(root,p,q){
        // 2. 确定递归终止条件
        if(root===null || root===p || root===q){
            return root
        }
        let left = ancestor(root.left,p,q)
        let right = ancestor(root.right,p,q)
        // 3. 确定递归单层逻辑
        if(left!==null && right!==null){
            return root
        }else if(left==null){
            return right
        }else{
            return left
        }
    }
    return ancestor(root,p,q)
};
```



### 二叉搜索树转为累加数

```js
//二叉搜索树是递增的有顺序，累加相当于反中序遍历
var convertBST = function(root) { 
    let pre = 0;
    const ReverseInOrder = (cur) => { 
        if(cur) { //不需要递归函数的返回值做什么操作，要遍历整棵树
            ReverseInOrder(cur.right);
            cur.val += pre;
            pre = cur.val;  //pre指针记录当前遍历节点cur的前一个节点
            ReverseInOrder(cur.left);
        }
    }
    ReverseInOrder(root);
    return root;
};
```



# 回溯

## 理论知识

>是一种纯暴力的搜索算法，常用于解决组合问题、切割问题、子集问题、棋盘问题
>
>- 组合问题：N个数里面按一定规则找出k个数的集合
>- 切割问题：一个字符串按一定规则有几种切割方式
>- 子集问题：一个N个数的集合里有多少符合条件的子集
>- 排列问题：N个数按一定规则全排列，有几种排列方式
>- 棋盘问题：N皇后，解数独等等
>
>所有回溯法的问题都可以抽象为树形结构，回溯法解决的都是在集合中递归查找子集，集合的大小就构成了树的宽度；递归的深度则构成的树的深度

```js
//回溯算法模板,for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
回溯三部曲：
1、递归函数的参数和返回值：返回值一般都是空
2、递归函数的终止条件（重要）
3、单层搜索的逻辑（单层递归的逻辑）
```

## 基础习题锻炼

### 组合

>给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合
>
>```
>输入：n = 4, k = 2
>输出：
>[
>  [2,4],
>  [3,4],
>  [2,3],
>  [1,2],
>  [1,3],
>  [1,4],
>]
>```

```js
var combine = function(n, k) {
    let result = []
    let path = []
    var combineBackTracking = function(n,k,startIndex){ //递归和for循环
        if(path.length === k){
            result.push([...path])
            return
        }
        for(let i=startIndex;i<=n;i++){ //i<=n - (k - path.length) + 1则可以剪枝优化
            path.push(i)
            combineBackTracking(n,k,i+1)
            path.pop()  //回溯
    }
}
    combineBackTracking(n,k,1)
    return result
};

//剪枝优化：只需要把i<=n改为n - (k - path.length) + 1
```

### 组合总和III

>找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：
>
>只使用数字1到9
>每个数字 最多使用一次 
>返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。
>
>```
>输入: k = 3, n = 7
>输出: [[1,2,4]]
>解释:
>1 + 2 + 4 = 7
>没有其他符合的组合了。
>```

```js
var combinationSum3 = function(k, n) {
    let result = [],path = [],temp=0
    var combineBackTracking = function(k,n,startIndex){ //递归和for循环
        if(path.length === k){
            //这里可以不用temp,因为用temp的话下面每次计算都要重新置为0
            //可以用const sum = path.reduce((a,b) => a+b) 比 较sum和n是否相等
            for(let i=0;i<path.length;i++){
                temp += path[i]
            } 
            if(temp === n){
                result.push([...path])
            }
            temp = 0
            return
        }
        for(let i=startIndex;i<=9;i++){ //i<= 9 - (k - path.length) + 1则可以剪枝优化
            path.push(i)
            combineBackTracking(k,n,i+1)
            path.pop() //回溯
        }
    }
    combineBackTracking(k,n,1)
    return result
};

//reduce常用于数组累加：
array.reduce(function(accumulator, currentValue, currentIndex, arr), initialValue);
/*
  accumulator:  必需。累计器-上一个值
  currentValue: 必需。当前元素
  
  currentIndex: 可选。当前元素的索引；                    
  arr:          可选。要处理的数组
  initialValue: 可选。传递给函数的初始值，相当于accumulator的初始值
*/
简单来说就是对一个array执行reduce()方法，就是把其中的function()挨个地作用于array中的元素上，而且上一次的输出会作为下一次的一个输入
let array = [1, 2, 3, 4, 5];
array.reduce((sum, curr) => sum + curr, 0); // 得到15
```



### 组合总和

>给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。
>candidates 中的 同一个 数字可以**无限制**重复被选取。如果至少一个数字的被选数量不同，则两种组合不同
>对于给定的输入，保证和为 target 的不同组合数少于 150 个。
>
>```
>输入：candidates = [2,3,6,7], target = 7
>输出：[[2,2,3],[7]]
>解释：
>2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
>7 也是一个候选， 7 = 7 。
>仅有这两种组合。
>```
>

```js
var combinationSum = function(candidates, target) {
    const res = [], path = [];
    function backtracking(candidates,target,startIndex,sum) {
        if (sum === target) {
            res.push([...path]);
            return;
        }
        for(let i = startIndex; i < candidates.length; i++ ) {
            const n = candidates[i];
            if(n > target - sum) break;  //此处是优化剪枝
            path.push(n);
            sum += n;
            backtracking(candidates,target,i,sum);
            path.pop();
            sum -= n;
        }
    }
    candidates.sort((a,b)=>a-b); // 排序，为了优化剪枝
    backtracking(candidates,target,0,0);
    return res;
};
```



### 组合总和II

>给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
>candidates 中的每个数字在每个组合中只能使用 一次 。
>注意：解集不能包含重复的组合
>
>```
>输入: candidates = [10,1,2,7,6,1,5], target = 8,
>输出:
>[
>[1,1,6],
>[1,2,5],
>[1,7],
>[2,6]
>]
>```

```js
//这里和上一道题的不同点在于这里的数组是有重复数据的(集合（数组candidates）有重复元素，但还不能有重复的组合)
//对同一个树层去重，同一个树枝的则不用去重：排序并且i > startindex && candidates[i] === candidates[i-1]
var combinationSum2 = function(candidates, target) {
    const res = []; path = [], len = candidates.length;
    candidates.sort((a,b)=>a-b);
    backtracking(0, 0);
    return res;
    function backtracking(sum, i) {
        if (sum === target) {
            res.push(Array.from(path));
            return;
        }
        for(let j = i; j < len; j++) {
            const n = candidates[j];
            if(j > i && candidates[j] === candidates[j-1]){
              //若当前元素和前一个元素相等
              //则本次循环结束，防止出现重复组合
              continue;
            }
            //如果当前元素值大于目标值-总和的值
            //由于数组已排序，那么该元素之后的元素必定不满足条件
            //直接终止当前层的递归
            if(n > target - sum) break;
            path.push(n);
            sum += n;
            backtracking(sum, j + 1);
            path.pop();
            sum -= n;
        }
    }
};
```



### 电话号码的字母组合

>给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回
>给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
>
>```
>输入：digits = "23"
>输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
>```

```js
//要做一个映射关系  
var letterCombinations = function(digits) {
    let path=[],result=[]
    const map = ["","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"];
    if(!digits.length) return [];
    if(digits.length === 1) return map[digits].split("");
    
    var combinations = function(dights,index){
        if(path.length === dights.length){
            result.push(path.join(""))
            return
        }
        for(let i=0;i<map[digits[index]].length;i++){ //map['23'[0]]的结果是map[2]="abc"
            path.push(map[digits[index]][i]) //取出'abc'里面的每一项即path=['a','b','c']
            combinations(digits,index+1)
            path.pop()
        }
        // for(const v of map[digits[index]]) {  //上面的for也可以改为下面的for of会更简单
        //     path.push(v);
        //     combinations(digits, index + 1);
        //     path.pop();
        // }
    }
    combinations(digits,0)
    return result
};
```



### 分割回文串

>给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。
>
>**回文串** 是正着读和反着读都一样的字符串
>
>```
>输入：s = "aab"
>输出：[["a","a","b"],["aa","b"]]
>```

```js
const isPalindrome = (s, l, r) => {
    // for (let i = l, j = r; i <= j; i++, j--) {
    //     if(s[i] !== s[j]) return false;
    // }
    let i=l,j=r //左右指针分辨是否是回文串，两种方法
    while(i<=j){
        if(s[i] !== s[j]) return false;
        i++;
        j--;
    }
    return true;
}

var partition = function(s) {
    const res = [], path = [], len = s.length;
    var backtracking = function(startindex) {
        if(startindex >= len) {  //判断是否是回文子串会在单层逻辑里面判断
            res.push([...path]);
            return;
        }
        for(let i = startindex; i < len; i++) {
            if(!isPalindrome(s, startindex, i)) continue; //？这里其实不大懂
            path.push(s.slice(startindex, i + 1));
            backtracking(i + 1);
            path.pop();
        }
    }
    backtracking(0);
    return res;
};
```



### 子集问题

>给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。
>
>解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集
>
>```
>输入：nums = [1,2,3]
>输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
>```
>![](C:\Users\Z\Desktop\前端\面试准备问题.assets\2.png)
```js
//组合问题和分割问题都是收集树的叶子节点，而子集问题是找树的所有节点（子集也是一种组合问题，它的集合是无序的）
var subsets = function(nums) {
    let path = [], res = []
    var sets = function(nums,startindex){
        res.push([...path])
        for(let i=startindex;i<nums.length;i++){
            path.push(nums[i])
            sets(nums,i+1)
            path.pop()
        }
    }
    sets(nums,0)
    return res;
};
```



### 子集II

>给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。
>
>解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。
>
>```
>输入：nums = [1,2,2]
>输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
>```

```js
//和有重复项的组合问题类似，排序+同树层去重，树枝不用去重
var subsetsWithDup = function(nums) {
    let path = [], res = []
    let sortNums = nums.sort((a, b) => {
        return a - b
    })
    var sets = function(sortNums,startindex){
        res.push([...path])
        for(let i=startindex;i<sortNums.length;i++){
            if(startindex<i && sortNums[i]===sortNums[i-1]) continue;//区别在于新增了这句和上面的排序
            path.push(sortNums[i])
            sets(sortNums,i+1)
            path.pop()
        }
    }
    sets(sortNums,0)
    return res;
};
```



### 全排列

>给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列*
>
>```
>输入：nums = [1,2,3]
>输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
>```

![](C:\Users\Z\Desktop\前端\面试准备问题.assets\4.png)

```js
//要用used数组标记已经选择的元素
var permute = function(nums) {
    let path=[], res=[]
    var permuteTemp = function(nums,used){
        if(path.length === nums.length){
            res.push([...path])
        }
        for(let i=0;i<nums.length;i++){ //排列是有序的，每次都要重新从第0位
            if(used[i]) continue;
            path.push(nums[i])
            used[i] = true
            permuteTemp(nums,used)
            path.pop()
            used[i] = false
        }
    }
    permuteTemp(nums,[])
    return res
};
```



### 全排列II

>给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。
>
>```
>输入：nums = [1,1,2]
>输出：
>[[1,1,2],
> [1,2,1],
> [2,1,1]]
>```

```js
//排序
//used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
//used[i - 1] == false，说明同一树层candidates[i - 1]使用过
var permuteUnique = function(nums) {
    let path=[], res=[]
    let sortNums = nums.sort((a, b) => {
        return a - b
    })
    var permuteTemp = function(sortNums,used){
        if(path.length === sortNums.length){
            res.push([...path])
        }
        for(let i=0;i<sortNums.length;i++){ //排列是有序的，每次都要重新从第0位
            if(sortNums[i] === sortNums[i-1] && used[i-1] == false) continue //新增了这句和上面排序
            if(used[i]) continue;
            path.push(sortNums[i])
            used[i] = true
            permuteTemp(sortNums,used)
            path.pop()
            used[i] = false
        }
    }
    permuteTemp(nums,[])
    return res
};
```



# 贪心

## 理论知识

>贪心的本质是选择每一阶段的局部最优，从而达到全局最优（就是从局部最优推出全局最优）
>
>贪心算法一般分为如下四步：
>
>- 将问题分解为若干个子问题
>- 找出适合的贪心策略
>- 求解每一个子问题的最优解
>- 将局部最优解堆叠成全局最优解

## 基础习题锻炼

### 分发饼干

>假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。
>
>对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值
>
>```
>输入: g = [1,2,3], s = [1,1]
>输出: 1
>解释: 
>你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
>虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
>所以你应该输出1。
>```

```js
//局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱一个，全局最优就是喂饱尽可能多的小孩
var findContentChildren = function(g, s) {
    let count = 0, index = s.length-1
    s.sort((a,b) => a-b)
    g.sort((a,b) => a-b)
    for(let i=g.length-1;i>=0;i--){  //从后往前去用最大的饼干满足最大胃口的孩子
        if(index >=0 && g[i] <= s[index]){
            count++
            index--
        }
    }
    return count
};
```



### 摆动序列

>- 局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值
>- 整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列
>- 让峰值尽可能的保持峰值，然后删除单一坡度上的节点
>
>#### [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

```js
var wiggleMaxLength = function(nums) {
    let preIndex = 0,curIndex = 0, count = 1
    for(let i=0;i<nums.length-1;i++){
        curIndex = nums[i+1] - nums[i]
        //这里的preIndex等于0的情况是最左边和最右边的情况
        if((curIndex>0 && preIndex<=0) || (curIndex<0 && preIndex>=0)){
            count++
            preIndex = curIndex
        }
    }
    return count
};
```



### 最大子序列

>给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和**子数组** 是数组中的一个连续部分
>
>- 局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小
>- 全局最优：选取最大“连续和”
>- 局部最优的情况下，并记录最大的“连续和”，可以推出全局最优
>
>```
>输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
>输出：6
>解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
>```

```js
var maxSubArray = function(nums) {
    let res = -Infinity,count = 0
    for(let i=0;i<nums.length;i++){
        count += nums[i]
        if(count > res){
            res = count
        }
        if(count < 0){
            count = 0
        }
    }
    return res
};
```



### 买卖股票的最佳时期II

>给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。
>在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。
>返回 你能获得的 最大 利润 
>
>```
>输入：prices = [7,1,5,3,6,4]
>输出：7
>解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
>     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
>     总利润为 4 + 3 = 7 。
>```
>
>**解析：**
>
>最终利润是可以分解的，把利润分解为每天为单位的维度，而不是从0天到第3天整体去考虑
>prices[3] - prices[0] = (prices[3] - prices[2]) + (prices[2] - prices[1]) + (prices[1] - prices[0])
>局部最优：收集每天的正利润；全局最优：求得最大利润

```js
var maxProfit = function(prices) {
    let res = []
    for(let i=0;i<prices.length;i++){
        res.push(prices[i+1] - prices[i])
    }
    let result = res.filter(a => a>0) //收集每天的正利润
    if(!result.length) return 0;
    let final = result.reduce((a,b) => a+b) //求得最大利润
    return final
};
```



### 买卖股票的最佳时期

>给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。
>你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。
>返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 
>
>```
>输入：[7,1,5,3,6,4]
>输出：5
>解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 
>     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
>```
>
>**解析：**像下面那样将最终利润分解，再像最大子序列用中间值一样求最大值

```js
var maxProfit = function(prices) {
    let res = [], count = 0, result = 0
    for(let i=0;i<prices.length;i++){
        res.push(prices[i+1] - prices[i])
    }
    for(let i=0;i<res.length;i++){
        count += res[i]
        if(count<0){
            count = 0
        }else{
            result = count>result? count:result
        }
    }
    return result
};
```



### 跳跃问题

>给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。
>数组中的每个元素代表你在该位置可以跳跃的最大长度。
>判断你是否能够到达最后一个下标
>
>```
>输入：nums = [2,3,1,1,4]
>输出：true
>解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
>```
>**解析：**
>
>- 不用去看一次跳跃几步，而是看可跳的覆盖范围(**跳跃覆盖范围究竟可不可以覆盖到终点**)
>- 局部最优解：每次取最大跳跃步数（取最大覆盖范围）
>- 整体最优解：最后得到整体最大覆盖范围，看是否能到终点
>- i的每次移动只能在cover的范围内移动，每移动一个元素，cover得到该元素数值（新的覆盖范围）的补充，让i继续移动下去
```js
var canJump = function(nums) {
    let cover = 0,temp = 0
    if(nums.length === 1) return true
    for(let i=0;i<=cover;i++){ //这里每次移动只能在cover的范围内移动
        temp = i + nums[i]
        cover = cover>temp? cover:temp
        if(cover>=nums.length-1) return true
    }
    return false
};
```



### 跳跃游戏II

>给你一个非负整数数组 nums ，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。你的目标是使用最少的跳跃次数到达数组的最后一个位置。
>假设你总是可以到达数组的最后一个位置
>
>```
>输入: nums = [2,3,1,1,4]
>输出: 2
>解释: 跳到最后一个位置的最小跳跃数是 2。
>     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
>```
>
>从覆盖范围出发，不管怎么跳，覆盖范围内一定是可以跳到的，**以最小的步数增加覆盖范围**，覆盖范围一旦覆盖了终点，得到的就是最小步数
>
>- 局部最优：求当前这步的最大覆盖，尽可能多走到达覆盖范围的终点
>- 整体最优：达到终点，步数最少

```js
var jump = function(nums) {
    let nextIndex = 0,curIndex = 0,step = 0
    for(let i=0;i<nums.length-1;i++){
        nextIndex = Math.max(i+nums[i],nextIndex)
        if(curIndex === i){ //直接跳到最远覆盖范围的一步
            curIndex = nextIndex
            step++
        }
    }
    return step
};
```



### 根据身高重建队列

>#### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)
>
>确定一个维度，然后在按照另一个维度重新排列（确定一边然后贪心另一边）

```js
var reconstructQueue = function(people) {
    let res = []
    people.sort((a,b) => {//按照身高重新排序
        if(a[0] !== b[0]) {
            return b[0] - a[0]  //若k不相同则直接排序
        }else{  //若k不相同则k越小的排在越前
            return a[1] - b[1]
        }
    })
    people.forEach( item => {
        res.splice(item[1],0,item)  //根据k插入对应的位置
    })
    return res
};
```



# 动态规划

## 理论知识

>动态规划中每一个状态一定是由上一个状态推导出来的；贪心没有状态推导，而是从局部直接选最优的（动规是由前一个状态推导出来的；贪心是局部直接选最优的）
>要注意（1）dp数组以及下标的含义  （2）递推公式  （3）dp数组如何初始化 （4）遍历顺序  （5）打印dp数组
>
>动态规划五部曲：
>
>- 确定dp数组（dp table）以及下标的含义（一般用一维或者二维的数组表示dp数组）
>- 确定递推公式
>- dp数组如何初始化
>- 确定遍历顺序
>- 举例推导dp数组

## 基础习题锻炼

### 斐波那契数列

>- 确定dp[i]含义： dp[i]表示第i个斐波那契数
>- 确定递推公式： dp[i] = dp[i-1] + dp[i-2]
>- dp数组初始化： dp[0] = 1, dp[1] = 1
>- 遍历顺序：从前往后
>- 打印dp数组：一般出bug调试做这步

```js
//动态规划
var fib = function(n) {
    let dp = [0, 1]
    for(let i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2]
    }
    // console.log(dp)
    return dp[n]
};

//递归
var fib = function(n) {
    if (n === 0 ) return 0;
    if (n === 1) return 1;
    return fib(n - 1) + fib(n - 2);
};
```



### 爬楼梯

>- 确定dp数组以及下标的含义：dp[i]是爬到第i层楼梯，有dp[i]种**方法**
>- 确定递推公式：dp[i] = dp[i - 1] + dp[i - 2] 
>- dp数组初始化：dp[1] = 1，dp[2] = 2（dp[0]=1不能解释通）
>- 确定遍历顺序：从前向后遍历
>- 举例推导dp数组

```js
var climbStairs = function(n) {
    // dp[i] 为第 i 阶楼梯有多少种方法爬到楼顶
    // dp[i] = dp[i - 1] + dp[i - 2]
    let dp = [1 , 2]
    for(let i = 2; i < n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2]
    }
    return dp[n - 1]
};
```

