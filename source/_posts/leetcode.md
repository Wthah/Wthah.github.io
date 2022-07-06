---
title: LeetCode
date: 2022/7/1 11:21:25
tags:
  - 算法
categories:
  - 面试
cover: /img/9.jpg
---

# LeetCode

## 前置学习

### 数据结构

#### 栈

##### 普通栈

```java
import java.util.Arrays;

public class MyStack{
    public int[] elem;
    public int usedSize;
    public MyStack(){
        this.usedSize = 0;
        this.elem = new Int[5];
    }
    
    // 判满
    public boolean full(){
        return usedSize == this.elem.length;
    }
    // 判空
    public boolean empty(){
        return usedSize == 0;
    }
    // 入栈
    public void push(int i){
        if(full()){
            this.elem = Arrays.copyOf(elem,2*this.elem.length);
        }
        elem[usedSize++] = i;
    }
    // 出栈
    public int pop(){
        if(empty()){
            System.out.println("栈空");
            return;
        }
        return elem[--usedSize];
    }
    // 获得栈顶元素 
    public int peak(){
        if(empty()){
            System.out.println("栈空");
            return;
        }
        return elem[usedSize-1];
    }
}
```

#### 队列

##### 普通队列

```java
public Class MyQueue{
    private int rear;
    private int front;
    private T[] elem;
    private int maxSize;
    
    public MyQueue(int size){
        this.maxSize = size;
        this.rear = 0;
        this.front = 0;
        this.elem = new T[maxsize]; 
    }
    
    public boolean full(){
        return rear == maxSize;
    }
    
    public boolean empty(){
        return front == rear;
    }
    
    public void offer(T data){
        if(full()){
            System.out.println("队列已满");
            return;
        }
        elem[rear++] = data;
    }
    
    public T peak(){
        if(empty()){
            System.out.println("队列为空");
            return;
        }
        return elem[front];
    }
    
    public T poll(){
        if(empty()){
            System.out.println("队列为空");
            return;
        }
        return elem[front++];
    }
}
```

##### 循环队列

```java
package com.wshen.serial_bds.entity;

public class SerialQueue {

    private static SerialQueue serialQueue = null;

    private SerialQueue(){}

    private int front;//指向队首
    private int rear;//指向队尾
    private Serial[] elem;
    private int maxSize;//最大容量

    public static SerialQueue getSerialQueue(){
        if(serialQueue==null){
            synchronized (SerialQueue.class){
                if(serialQueue==null){
                    serialQueue = new SerialQueue();
                    serialQueue.front = 0;
                    serialQueue.rear = 0;
                    serialQueue.maxSize = 17;
                    serialQueue.elem = new Serial[serialQueue.maxSize];
                }
            }
        }
        return serialQueue;
    }

    public boolean isEmpty(){
        return rear==front;
    }
    public boolean isFull(){
        return  (rear+1)%maxSize==front;
    }

    public void offer(Serial serial){
        if(isFull())return;
        elem[rear++]=serial;
        rear=rear%maxSize;
    }
    public void poll(){
        if(isEmpty())return;
        front=++front%maxSize;
    }
    public Serial peek(){
        if(isEmpty())return null;
        return elem[front];
    }

    public Serial peekByIndex(int n){
        if(elem[n]==null)return null;
        return elem[n];
    }

    @Override
    public String toString() {
        String str="";
        for (Serial i:elem
        ) {
            str=str+i+'\r';
        }
        return str;
    }
}
```

##### 链表

###### 单链表

```java
package LinkedList;

class Node{
    int data;
    Node next;
    
    public Node(int data){
        this.data = data;
    }
    
    public Node(int data, Node next){
        this.data = data;
        this.next = next;
    }
}

public class SingleLinkedList{
    
    private int size;
    private Node head;
    
    public void addFirst(int data){
        if(size == 0){
            this.head = new Node(data);
            size++;
        }
        else{
            Node node = New Node(data);
            node.next = head;
            head = node;
            size++;
        }
    }
    
    public void addIndex(int index, int data){
        if(index < 0 || index > size){
            System.out.println("add Index illegal");
            return;
        }
        if(index==0){
            Node node = new Node(data,head);
            head = node;
            size++;
            return;
        }
        Node node = new Node(data);
        Node prev = head;
        for(int i=0;i<index-1;i++){
            prev = prev.next;
        }
        node.next=prev.next;
        prev.next=node;
        size++;
    }
    
    public void addLast(int data){
        addIndex(size,data);
    }
    
    public int get(int index){
        if(rangeCheck(index)){
            Node node = head;
            for(int i=0;i<index;i++){
                node = node.next;
            }
            int data = node.data;
            return data;
        }
    }
    
    public int set(int index,int data){
        if(rangeCheck(index)){
            Node node = head;
            for(int i =0; i<index;i++){
                node = node.next;
            }
            int oldData = node.data;
            node.data =data;
            return oldData;
        }
    }
    
    private boolean rangeCheck(int index) {
        if (index < 0 || index >= size) {
            return false;
        }
        return true;
    }
    
    public boolean contains(int data){
        Node node = head;
        while(node != null){
            if(node.data == data){
                return true;
            }
            node = node.next;
        }
        return false;
    }
    
    public void removeFirst(){
        Node node = head;
        head = head.next;
        node.next = null;
        size--;
    }
    
    public void removeIndex(int index){
        Node node_prev = head;
        for(int i=0;i<index-1;i++){
            node_prev = node_prev.next;
        }
        Node node = node_prev.next;
        node_prev.next = node.next;
        node.next = null;
        size--;
    }
    
    public void removeValueOnce(int data){
        Node prev = head;
        while(prev.next != null){
            if(prev.data==data){
                removeFirst();
            }else if(prev.next.data==data){
                Node node = prev.next;
                prev.next = node.next;
                node.next = null;
                size--;
            }else{
                prev = prev.next;
            }
        }
    }
}
```

> hlh最近因为来亲戚了，然后最近不是要考试嘛，你们那个猪头学校安排考试跨度太大了，比较烦，他不是在宿舍和你关系比较好嘛，他很焦虑最近，在宿舍麻烦这段时间多担待一下，晚上能早点熄灯早点熄灯吧，他不知道我来找你了，尽量别和他说吧。我怕到时候又要埃她说，麻烦。

### Hash

### 算法

##### 分治

> **1.分解**
> 将原问题分解为若干规模较小，相互独立，与原问题相同的子问题。
> **2.解决**
> 若干子问题较小而容易被解决则直接解决，否则再继续分解为更小的子问题，直到容易解决。
> **3.合并**
> 将已求解的各个子问题的解，逐步合并为原问题的解。

##### 动态规划

1. 求一个问题的最优解

2. 整体的最优解依赖于各个子问题的最优解
3. 这些小问题之间还有相互重叠的更小的子问题
4. 从上往下分析问题，从下往上求解问题

`重叠子问题`，`最优子结构`，`状态转移方程`

> （3）重叠子问题：我们将大问题分解 为小问题，这些小问题之间还有相互重叠的更小的子问题。不同的问题，可能都要求1个相同问题的解。假如A经理想知道他下面最优秀的人是谁，他必须知道X,Y,Z,O,P组最优秀的人是谁， 甲总监想知道自己下面最优秀的人是谁，也要去知道X,Y,Z组里面最优秀的人是谁？这就有问题重叠了，两个人都需要了解X,Y,Z三个小组最优秀的人。
>
> （4）从上往下分析问题，从下往上求解问题。
>
> （5）无后效性：求出来的子问题并不会因为后面求出来的改变。我们可以理解为，X组长挑选出三个人，即便到了高级经理选出大部门最优秀的三个人，对于X组来说，最优秀的还是这3个人，不会发生改变。

###### https://blog.csdn.net/weixin_43892514/article/details/104438703?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165553241416781435497591%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165553241416781435497591&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-104438703-null-null.142^v17^pc_search_result_control_group,157^v15^new_3&utm_term=%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E5%92%8C%E8%B4%AA%E5%BF%83&spm=1018.2226.3001.4187

## 直击Offer

### 双栈实现队列

##### 菜狗

```java
import java.util.Arrays;

class CQueue {

    private int tail;
    private int maxSize;
    private int[] elem_one;
    private int[] elem_two;

    public CQueue() {
        this.tail = 0;
        this.maxSize = 9;
        this.elem_one = new int[maxSize];
        this.elem_two = new int[maxSize];
    }

    public boolean isFull(){
        return tail == maxSize;
    }

    public boolean isEmpty(){
        return tail == 0;
    }

    public void appendTail(int value) {
        if(isFull()){
            this.maxSize++;
            elem_one = Arrays.copyOf(elem_one,maxSize);
            elem_two = Arrays.copyOf(elem_two,maxSize);
        }
        elem_one[tail++] = value;
        for (int i = 0; i < tail; i++) {
            elem_two[i] = elem_one[tail-i-1];
        }
    }

    public int deleteHead() {
        if(isEmpty()){
            return -1;
        }
        else{
            int elem = elem_two[--tail];
            for (int i = 0; i < tail; i++) {
                elem_one[i] = elem_two[tail-i-1];
            }
            return elem;
        }
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

##### 标准答案

```java
class CQueue {
    Deque<Integer> inStack;
    Deque<Integer> outStack;

    public CQueue() {
        inStack = new ArrayDeque<Integer>();
        outStack = new ArrayDeque<Integer>();
    }

    public void appendTail(int value) {
        inStack.push(value);
    }

    public int deleteHead() {
        if (outStack.isEmpty()) {
            if (inStack.isEmpty()) {
                return -1;
            }
            in2out();
        }
        return outStack.pop();
    }

    private void in2out() {
        while (!inStack.isEmpty()) {
            outStack.push(inStack.pop());
        }
    }
}
```

### min栈

##### 菜狗

```java
import java.util.Arrays;
import java.util.Stack;

public class MinStack {

    int usedSize;
    int[] elem;
    int maxSize;
    int min;
    // 辅助栈
    Stack<Integer> stack;

    /** initialize your data structure here. */
    public MinStack() {
        this.maxSize = 9;
        this.elem = new int[maxSize];
        this.usedSize = 0;
        this.stack = new Stack<Integer>();
    }

    public boolean full(){
        return usedSize==maxSize;
    }

    public void push(int x) {
        if(full()){
            maxSize++;
            elem = Arrays.copyOf(elem,maxSize);
        }
        if(usedSize==0){
            min=x;
        }else if(x<min){
            this.min=x;
        }
        stack.push(min);
        elem[usedSize++]=x;
    }

    public boolean empty(){
        return usedSize==0;
    }
    public void pop() {
        if(empty()){
            return;
        }
        stack.pop();
        if(!stack.empty()){
            min=stack.peek();
        }
        --usedSize;
    }

    public int top() {
        if(empty()){
            return -1000;
        }
        return elem[usedSize-1];
    }

    public int min() {
        if(empty()){
            return -1000;
        }
        min = stack.peek();
        return min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */

```

##### 标准答案

```java
class MinStack {
    Deque<Integer> xStack;
    Deque<Integer> minStack;

    public MinStack() {
        xStack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        xStack.push(x);
        minStack.push(Math.min(minStack.peek(), x));
    }
    
    public void pop() {
        xStack.pop();
        minStack.pop();
    }
    
    public int top() {
        return xStack.peek();
    }
    
    public int min() {
        return minStack.peek();
    }
}
```

### 从尾到头打印链表

##### 菜狗

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    LinkedList<Integer> linkedList = new LinkedList<Integer>();
    public int[] reversePrint(ListNode head) {
        while(head!=null){
            linkedList.addFirst(head.val);
            head = head.next;
        }
        int[] test = new int[linkedList.size()];
        int size = linkedList.size();
        for (int i = 0; i < size; i++) {
            test[i] = linkedList.pop();
        }
        return test;
    }
}
```

### 倒输出链表

##### 菜狗

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        LinkedList<Integer> linkedList = new LinkedList<Integer>();
        while(head!=null){
            linkedList.addFirst(head.val);
            head = head.next;
        }
        ListNode prev;
        if(linkedList.size()!=0){
            prev = new ListNode(linkedList.pop());
        }else{
            return null;
        }
        ListNode temp = new ListNode();
        int size = linkedList.size();
        for(int i=0;i<size;i++){
            if(i==0){
                temp=new ListNode(linkedList.pop());
                prev.next=temp;
                continue;
            }
            temp.next=new ListNode(linkedList.pop());
            temp=temp.next;
        }
        return prev;
    }
}
```

##### 标准答案

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```



## 数据结构

### 存在重复元素

##### 菜狗

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int x : nums) {
            if (!set.add(x)) {
                return true;
            }
        }
        return false;
    }
}
```

##### 标准答案

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}
```

### 最大子数组和

##### 菜狗动态规划

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        int pre = nums[0];
        for(int i=0;i<nums.length;i++){
            if(i==0){
                max=nums[0];
            }
            else if(nums[i]+max>nums[i]){
                max=nums[i]+max;
            }else{
                max=nums[i];
            }
            if(max>pre){
                pre = max;
            }
        }
        return pre;
    }
}
```

### 两数之和

##### hash（空间换时间） pk 暴力求解

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if(map.containsKey(target-nums[i])){
                return new int[]{i,map.get(target-nums[i])};
            }
            else{	
                map.put(nums[i], i);
            }
        }
        return new int[2];
    }
}
```

### 两数组交集

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer,Integer> integerMap = new HashMap<>();
        int[] nums;
        int n = 0;
        if(nums1.length>nums2.length){
            nums = new int[nums2.length];
            for (int j : nums2) {
                if (integerMap.containsKey(j)) {
                    integerMap.put(j, integerMap.get(j) + 1);
                } else {
                    integerMap.put(j, 1);
                }
            }

            for(int k : nums1){
                if(integerMap.containsKey(k)){
                    nums[n] = k;
                    n++;
                    if(integerMap.get(k)-1==0){
                        integerMap.remove(k);
                    }else{
                        integerMap.put(k,integerMap.get(k)-1);
                    }
                }
            }
        }
        else{
            nums = new int[nums1.length];
            for (int j : nums1) {
                if (integerMap.containsKey(j)) {
                    integerMap.put(j, integerMap.get(j) + 1);
                } else {
                    integerMap.put(j, 1);
                }
            }

            for(int k : nums2){
                if(integerMap.containsKey(k)){
                    nums[n] = k;
                    n++;
                    if(integerMap.get(k)-1==0){
                        integerMap.remove(k);
                    }else{
                        integerMap.put(k,integerMap.get(k)-1);
                    }
                }
            }
        }

        nums = Arrays.copyOf(nums,n);
        return nums;
    }
}
```

