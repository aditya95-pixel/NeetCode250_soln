## Reverse Linked List

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

## Merge Two Sorted Lists

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

## Linked List Cycle

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

## Reorder List

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
