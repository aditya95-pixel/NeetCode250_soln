## 1 Excel Sheet Column Title

Given an integer columnNumber, return its corresponding column title as it appears in an Excel sheet.

For example:

A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...

```cpp
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string res;
        while(columnNumber>0){
            if(columnNumber<=26){
                res+=(char)('A'+columnNumber-1);
                break;
            }else if(columnNumber%26==0){
                res+='Z';
                columnNumber/=26;
                columnNumber--;
            }else{
                res+=(char)('A'+columnNumber%26-1);
                columnNumber/=26;
            }
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```

## 2 Greatest Common Divisor of Strings

For two strings s and t, we say "t divides s" if and only if s = t + t + t + ... + t + t (i.e., t is concatenated with itself one or more times).

Given two strings str1 and str2, return the largest string x such that x divides both str1 and str2.

```cpp
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
       string res;
       for(int i=0;i<min(str1.size(),str2.size());i++){
            if(str1.size()%(i+1)==0 && str2.size()%(i+1)==0){
                string sub=str1.substr(0,i+1);
                if(sub!=str2.substr(0,i+1))
                break;
                int cnt=str1.size()/(i+1);
                string t;
                for(int j=0;j<cnt;j++)
                t+=sub;
                if(t!=str1)
                continue;
                cnt=str2.size()/(i+1);
                t.clear();
                for(int j=0;j<cnt;j++)
                t+=sub;
                if(t!=str2)
                continue;
                else
                res=sub;
            }
       }
       return res;
    }
};
```

## 3 Insert Greatest Common Divisors in Linked List

Given the head of a linked list head, in which each node contains an integer value.

Between every pair of adjacent nodes, insert a new node with a value equal to the greatest common divisor of them.

Return the linked list after insertion.

The greatest common divisor of two numbers is the largest positive integer that evenly divides both numbers.

```cpp
class Solution {
public:
    ListNode* insertGreatestCommonDivisors(ListNode* head) {
        if(head->next==NULL)
        return head;
        ListNode*ptr=head->next,*preptr=head;
        while(ptr){
            int g=gcd(ptr->val,preptr->val);
            ListNode* node=new ListNode(g);
            preptr->next=node;
            node->next=ptr;
            preptr=ptr;
            ptr=ptr->next;
        }
        return head;
    }
};
```
