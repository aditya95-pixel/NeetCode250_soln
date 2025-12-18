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
