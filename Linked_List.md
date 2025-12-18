## 1 Reverse Linked List

Given the head of a singly linked list, reverse the list, and return the reversed list.

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode*p=NULL,*q=NULL,*r=head;
        while(r){
            p=q;
            q=r;
            r=r->next;
            q->next=p;
        }
        return q;
    }
};
```

## 2 Merge Two Sorted Lists

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy=new ListNode();
        ListNode*ptr1=list1,*ptr2=list2,*ptr3=dummy;
        while(ptr1 && ptr2){
            if(ptr1->val<ptr2->val)
            {
                ptr3->next=ptr1;
                ptr3=ptr3->next;
                ptr1=ptr1->next;
            }
            else{
                ptr3->next=ptr2;
                ptr3=ptr3->next;
                ptr2=ptr2->next;
            }
        }
        while(ptr1){
            ptr3->next=ptr1;
            ptr3=ptr3->next;
            ptr1=ptr1->next;
        }
        while(ptr2){
            ptr3->next=ptr2;
            ptr3=ptr3->next;
            ptr2=ptr2->next;
        }
        return dummy->next;
    }
};
```

## 3 Linked List Cycle

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

```cpp
class Solution {
public:
    bool hasCycle(ListNode* head) {
        if(!head || !head->next)
        return 0;
        ListNode*fast=head,*slow=head;
        do{
            slow=slow->next;
            fast=fast->next;
            if(fast)
            fast=fast->next;
        }while(fast && fast!=slow);
        return (fast==slow);
    }
};
```

## 4 Reorder List

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

```cpp
class Solution {
public:
    void reorderList(ListNode* head) {
        if(head->next==NULL)
        return ;
        unordered_map<ListNode*,ListNode*>mp;
        ListNode*ptr=head,*preptr=NULL;
        while(ptr){
            mp[ptr]=preptr;
            preptr=ptr;
            ptr=ptr->next;
        }
        ListNode*rev=preptr;
        ptr=head;
        ListNode*dummy=new ListNode();
        ListNode*ptr2=dummy;
        bool chk=1;
        while(ptr->next!=rev){
            if(chk){
                chk^=1;
                ptr2->next=ptr;
                ptr2=ptr2->next;
                ptr=ptr->next;
            }else{
                chk^=1;
                ptr2->next=rev;
                ptr2=ptr2->next;
                rev=mp[rev];
            }
        }
        if(mp.size()%2!=0){
            ptr2->next=rev;
            ptr2=ptr2->next;
            ptr2->next=ptr;
            ptr2=ptr2->next;
            ptr2->next=NULL;
        }
        else{
            ptr2->next=ptr;
            ptr2=ptr2->next;
            ptr2->next=rev;
            ptr2=ptr2->next;
            ptr2->next=NULL;
        }
        head=dummy->next;
    }
};
```

## 5 Remove Nth Node From End of List

Given the head of a linked list, remove the nth node from the end of the list and return its head.

```cpp
class Solution {
public:
    int length(ListNode*head){
        ListNode*ptr=head;
        int len=0;
        while(ptr){
            ptr=ptr->next;
            len++;
        }
        return len;
    }
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int len=length(head);
        if(n==len)
        return head->next;
        int cnt=0;
        ListNode*ptr=head,*preptr=NULL;
        while(ptr){
            preptr=ptr;
            ptr=ptr->next;
            cnt++;
            if(len-cnt==n)
            break;
        }
        preptr->next=ptr->next;
        delete ptr;
        return head;
    }
};
```

## 6 Copy List with Random Pointer

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null.

Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

For example, if there are two nodes X and Y in the original list, where X.random --> Y, then for the corresponding two nodes x and y in the copied list, x.random --> y.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:

val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) that the random pointer points to, or null if it does not point to any node.
Your code will only be given the head of the original linked list.

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head)
        return head;
        Node*ptr=head;
        while(ptr){
            Node*newNode=new Node(ptr->val);
            newNode->next=ptr->next;
            ptr->next=newNode;
            ptr=ptr->next->next;
        }
        ptr=head;
        while(ptr){
            if(ptr->random)
            ptr->next->random=ptr->random->next;
            ptr=ptr->next->next;
        }
        ptr=head;
        Node*newHead=head->next;
        Node*ptr1=newHead;
        while(ptr1->next){
            ptr->next=ptr->next->next;
            ptr1->next=ptr1->next->next;
            ptr=ptr->next;
            ptr1=ptr1->next;
        }
        ptr->next=NULL;
        ptr1->next=NULL;
        return newHead;
    }
};
```

## 7 Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int carry=0;
        ListNode*ptr1=l1,*ptr2=l2,*dummy=new ListNode();
        ListNode*ptr3=dummy;
        while(ptr1 && ptr2){
            int sum=ptr1->val+ptr2->val+carry;
            ptr3->next=new ListNode(sum%10);
            ptr3=ptr3->next;
            ptr1=ptr1->next;
            ptr2=ptr2->next;
            carry=sum/10;
        }
        while(ptr1)
        {
            int sum=ptr1->val+carry;
            ptr3->next=new ListNode(sum%10);
            ptr3=ptr3->next;
            ptr1=ptr1->next;
            carry=sum/10;
        }
        while(ptr2)
        {
            int sum=ptr2->val+carry;
            ptr3->next=new ListNode(sum%10);
            ptr3=ptr3->next;
            ptr2=ptr2->next;
            carry=sum/10;
        }
        if(carry>0)
        ptr3->next=new ListNode(carry);
        return dummy->next;
    }
};
```

## 8 Find the Duplicate Number

Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and using only constant extra space.

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        vector<int>freq(nums.size()+1,0);
        for(auto &num:nums){
            if(freq[num])
            return num;
            freq[num]++;
        }
        return -1;
    }
};
```

## 9 Reverse Linked List II

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

```cpp
class Solution {
public:
    void reverse(ListNode*&first,ListNode*&last){
        ListNode*r=first,*q=NULL,*p=NULL;
        ListNode*conn=last->next;
        last->next=NULL;
        last=first;
        while(r){
            p=q;
            q=r;
            r=r->next;
            q->next=p;
        }
        first=q;
        last->next=conn;
    }
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        int cnt=1;
        ListNode*first,*last;
        ListNode*ptr=head,*preptr;
        while(ptr){
            if(cnt==left-1)
            preptr=ptr;
            else if(cnt==left)
            first=ptr;
            else if(cnt==right)
            last=ptr;
            ptr=ptr->next;
            cnt++;
        }
        if(left<right)
        reverse(first,last);
        if(left==1)
        head=first;
        else
        preptr->next=first;
        return head;
    }
};
```

## 10 Design Circular Queue

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle, and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implement the MyCircularQueue class:

MyCircularQueue(k) Initializes the object with the size of the queue to be k.
int Front() Gets the front item from the queue. If the queue is empty, return -1.
int Rear() Gets the last item from the queue. If the queue is empty, return -1.
boolean enQueue(int value) Inserts an element into the circular queue. Return true if the operation is successful.
boolean deQueue() Deletes an element from the circular queue. Return true if the operation is successful.
boolean isEmpty() Checks whether the circular queue is empty or not.
boolean isFull() Checks whether the circular queue is full or not.
You must solve the problem without using the built-in queue data structure in your programming language. 

```cpp
class MyCircularQueue {
    vector<int>q;
    int front,rear,size;
public:
    MyCircularQueue(int k) {
        size=k;
        q.resize(k);
        front=rear=-1;
    }
    
    bool enQueue(int value) {
        if(isFull())
        return 0;
        if(isEmpty())
        front=0;
        rear=(rear+1)%size;
        q[rear]=value;
        return 1;
    }
    
    bool deQueue() {
        if(isEmpty())
        return 0;
        else if(front==rear)
            front=rear=-1;
        else
            front=(front+1)%size;
        return 1;
    }
    
    int Front() {
        if(isEmpty())
        return -1;
        return q[front];
    }
    
    int Rear() {
        if(isEmpty())
        return -1;
        return q[rear];
    }
    
    bool isEmpty() {
        if(front==-1)
        return 1;
        else
        return 0;
    }
    
    bool isFull() {
        if((rear+1)%size==front)
        return 1;
        else
        return 0;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```

## 11 LRU Cache

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
The functions get and put must each run in O(1) average time complexity.

```cpp
class Node{
    public:
    int key,value;
    Node*next,*prev;
    Node(int key,int value){
        next=prev=NULL;
        this->key=key;
        this->value=value;
    }
};
class LRUCache {
    int cap,cnt=0;
    Node*head,*tail;
    map<int,Node*>mp;
public:
    LRUCache(int capacity) {
        cap=capacity;
        head=new Node(-1,-1);
        tail=new Node(-1,-1);
        head->next=tail;
        tail->prev=head;
    }
    
    int get(int key) {
        if(!mp.count(key))
        return -1;
        else{
            Node*node=mp[key];
            del(node);
            insert(node);
            return node->value;
        }
    }
    
    void put(int key, int value) {
        if(mp.count(key)){
            Node*node=mp[key];
            del(node);
            insert(node);
            node->value=value;
        }else{
            if(cnt==cap){
                Node*delnode=tail->prev;
                del(delnode);
                mp.erase(delnode->key);
                cnt--;
            }
            Node*node=new Node(key,value);
            mp[key]=node;
            insert(node);
            cnt++;
        }
    }
    void del(Node*node){
        node->prev->next=node->next;
        node->next->prev=node->prev;
    }
    void insert(Node*node){
        node->next=head->next;
        node->next->prev=node;
        head->next=node;
        node->prev=head;
    }
};
```

## 12 LFU Cache

Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the LFUCache class:

LFUCache(int capacity) Initializes the object with the capacity of the data structure.
int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
void put(int key, int value) Update the value of the key if present, or inserts the key if not already present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.
To determine the least frequently used key, a use counter is maintained for each key in the cache. The key with the smallest use counter is the least frequently used key.

When a key is first inserted into the cache, its use counter is set to 1 (due to the put operation). The use counter for a key in the cache is incremented either a get or put operation is called on it.

The functions get and put must each run in O(1) average time complexity.

```cpp
class Node{
    public:
    int key,value,freq;
    Node*next,*prev;
    Node(int key,int value){
        this->key=key;
        this->value=value;
        next=prev=NULL;
        freq=1;
    }
};
class LFUCache {
    int cap,cnt;
    Node*head,*tail;
    map<int,Node*>mp;
public:
    LFUCache(int capacity) {
        cap=capacity;
        head=new Node(-1,-1);
        tail=new Node(-1,-1);
        head->next=tail;
        tail->prev=head;
        cnt=0;
    }
    
    int get(int key) {
        if(mp.count(key)){
            Node*node=mp[key];
            del(node);
            node->freq++;
            insert(node);
            return node->value;
        }else{
            return -1;
        }
    }
    
    void put(int key, int value) {
        if(mp.count(key)){
            Node*node=mp[key];
            del(node);
            node->freq++;
            insert(node);
            node->value=value;
        }else{
            if(cnt==cap){
                Node*delnode=tail->prev;
                del(delnode);
                mp.erase(delnode->key);
                cnt--;
            }
            Node*node=new Node(key,value);
            insert(node);
            mp[key]=node;
            cnt++;
        }
    }
    void del(Node*node){
        node->prev->next=node->next;
        node->next->prev=node->prev;
    }
    void insert(Node*node){
        Node*ptr=head->next;
        while(ptr!=tail && ptr->freq>node->freq){
            ptr=ptr->next;
        }
        ptr->prev->next=node;
        node->prev=ptr->prev;
        node->next=ptr;
        ptr->prev=node;
    }
};
```

## 13 Merge k Sorted Lists

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<int,vector<int>,greater<>>pq;
        for(auto head:lists){
            ListNode*ptr=head;
            while(ptr){
                pq.push(ptr->val);
                ptr=ptr->next;
            }
        }
        ListNode*dummy=new ListNode();
        ListNode*ptr=dummy;
        while(!pq.empty()){
            ListNode*node=new ListNode(pq.top());
            pq.pop();
            ptr->next=node;
            ptr=ptr->next; 
        }
        return dummy->next;
    }
};
```

## 14 Reverse Nodes in k-Group

Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

```cpp
class Solution {
public:
    ListNode*reverse(ListNode*head){
        ListNode*p=NULL,*q=NULL,*r=head;
        while(r){
            p=q;
            q=r;
            r=r->next;
            q->next=p;
        }
        return q;
    }
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode*newHead=NULL,*curr=head,*tail=NULL;
        while(curr){
            ListNode*groupHead=curr;
            ListNode*newNode=NULL;
            ListNode*prev=NULL;
            int cnt=0;
            while(curr && cnt<k){
                newNode=curr->next;
                curr->next=prev;
                prev=curr;
                curr=newNode;
                cnt++;
            }
            if(cnt==k){
                if(newHead==NULL)
                newHead=prev;
                if(tail!=NULL)
                tail->next=prev;
                tail=groupHead;
            }else{
                prev=reverse(prev);
                if(tail!=NULL)
                tail->next=prev;
            }                   
        }
        return newHead;
    }
};
```
