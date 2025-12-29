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

## 4 Transpose Matrix

Given a 2D integer array matrix, return the transpose of matrix.

The transpose of a matrix is the matrix flipped over its main diagonal, switching the matrix's row and column indices.

```cpp
class Solution {
public:
    vector<vector<int>> transpose(vector<vector<int>>& matrix) {
        vector<vector<int>>mat(matrix[0].size(),vector<int>(matrix.size()));
        for(int i=0;i<matrix.size();i++){
            for(int j=0;j<matrix[0].size();j++)
            mat[j][i]=matrix[i][j];
        }
        return mat;
    }
};
```

## 5 Rotate Image

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        for(int i=0;i<matrix.size();i++){
            for(int j=0;j<i;j++)
            swap(matrix[i][j],matrix[j][i]);
        }
        for(int i=0;i<matrix.size();i++)
        reverse(matrix[i].begin(),matrix[i].end());
    }
};
```

## 6 Spiral Matrix

Given an m x n matrix, return all elements of the matrix in spiral order.

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int rowb=0,rowe=matrix.size()-1,colb=0,cole=matrix[0].size()-1,cnt=0;
        vector<int>res;
        while(cnt<matrix.size()*matrix[0].size()){
            for(int i=colb;i<=cole;i++)
            {
                res.push_back(matrix[rowb][i]);
                cnt++;
            }
            if(cnt==matrix.size()*matrix[0].size())
            break;
            rowb++;
            for(int i=rowb;i<=rowe;i++)
            {
                res.push_back(matrix[i][cole]);
                cnt++;
            }
            if(cnt==matrix.size()*matrix[0].size())
            break;
            cole--;
            for(int i=cole;i>=colb;i--)
            {
                res.push_back(matrix[rowe][i]);
                cnt++;
            }
            if(cnt==matrix.size()*matrix[0].size())
            break;
            rowe--;
            for(int i=rowe;i>=rowb;i--)
            {
                res.push_back(matrix[i][colb]);
                cnt++;
            }
            if(cnt==matrix.size()*matrix[0].size())
            break;
            colb++;
        }
        return res;
    }
};
```

## 7 Set Matrix Zeroes

Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's.

You must do it in place.

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        bool c0=0,r0=0;
        for(int i=0;i<matrix.size();i++){
            for(int j=0;j<matrix[0].size();j++){
                if(i==0 && matrix[i][j]==0)
                    r0=1;
                if(j==0 && matrix[i][j]==0)
                    c0=1;
                if(matrix[i][j]==0)
                {
                    matrix[i][0]=0;
                    matrix[0][j]=0;
                }
            }
        }
        for(int i=1;i<matrix.size();i++){
            for(int j=1;j<matrix[0].size();j++){
                if(matrix[i][0]==0 || matrix[0][j]==0)
                    matrix[i][j]=0;
            }
        }
        if(r0==1){
            for(int j=0;j<matrix[0].size();j++)
                matrix[0][j]=0;
        }
        if(c0==1){
            for(int i=0;i<matrix.size();i++)
                matrix[i][0]=0;
        }
    }
};
```

## 8 Happy Number

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.

```cpp
class Solution {
public:
    bool isHappy(int n) {
        set<int>s;
        while(n!=1 && !s.count(n)){
            s.insert(n);
            int sum=0;
            while(n>0)
            {
                int dig=n%10;
                n/=10;
                sum+=dig*dig;
            }
            n=sum;
        }
        return n==1;
    }
};
```
