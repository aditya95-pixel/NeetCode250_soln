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

## 9 Plus One

You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        reverse(digits.begin(),digits.end());
        vector<int>res;
        int sum=digits[0]+1;
        res.push_back(sum%10);
        int carry=sum/10;
        for(int i=1;i<digits.size();i++){
            sum=digits[i]+carry;
            res.push_back(sum%10);
            carry=sum/10;
        }
        if(carry)
        res.push_back(carry);
        reverse(res.begin(),res.end());
        return res;
    }
};
```

## 10 Roman to Integer

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

```cpp
class Solution {
public:
    int romanToInt(string s) {
        set<char>seto;
        int res=0;
        for(auto &c:s){
            if(c=='I')
            {
                res++;
                seto.insert(c);
            }else if(c=='V' && seto.count('I'))
                res+=3;
            else if(c=='V')
                res+=5;
            else if(c=='X' && seto.count('I'))
            {
                res+=8;
                seto.insert(c);
            }
            else if(c=='X')
            {
                res+=10;
                seto.insert(c);
            }
            else if(c=='L' && seto.count('X'))
                res+=30;
            else if(c=='L')
                res+=50;
            else if(c=='C' && seto.count('X'))
            {
                res+=80;
                seto.insert(c);
            }
            else if(c=='C')
            {
                res+=100;
                seto.insert(c);
            }
            else if(c=='D' && seto.count('C'))
                res+=300;
            else if(c=='D')
                res+=500;
            else if(c=='M' && seto.count('C'))
            {
                res+=800;
                seto.insert(c);
            }
            else if(c=='M')
                res+=1000;
        }
        return res;
    }
};
```

## 11 Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (i.e., xn).

```cpp
class Solution {
public:
    double Pow(double x,long long n){
        if(n<0)
        return 1.0/Pow(x,-n);
        if(n==0)
        return 1;
        else if(n==1)
        return x;
        else if(n%2==0)
        return Pow(x*x,n/2);
        else
        return Pow(x*x,n/2)*x;
    }
    double myPow(double x, int n) {
        return Pow(x,n);
    }
};
```

## 12 Multiply Strings

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.

```cpp
class Solution {
public:
    string add(string num1,string num2){
        reverse(num1.begin(),num1.end());
        reverse(num2.begin(),num2.end());
        string res;
        int i=0,j=0,carry=0;
        while(i<num1.size() && j<num2.size()){
            int sum=(num1[i]-'0')+(num2[j]-'0')+carry;
            res+=to_string(sum%10);
            carry=sum/10;
            i++;
            j++;
        }
        while(i<num1.size()){
            int sum=(num1[i]-'0')+carry;
            res+=to_string(sum%10);
            carry=sum/10;
            i++;
        }
        while(j<num2.size()){
            int sum=(num2[j]-'0')+carry;
            res+=to_string(sum%10);
            carry=sum/10;
            j++;
        }
        if(carry)
        res+=to_string(carry);
        reverse(res.begin(),res.end());
        return res;
    }
    string multiply(string num1, string num2) {
        if(num1=="0" || num2=="0")
        return "0";
        string res;
        int sum=0,c=0;
        for(int i=num1.size()-1;i>=0;i--){
            int dig1=num1[i]-'0',carry=0;
            string tempsum;
            for(int j=num2.size()-1;j>=0;j--){
                int dig2=num2[j]-'0';
                int s=dig1*dig2+carry;
                tempsum+=to_string(s%10);
                carry=s/10;
            }
            if(carry)
            tempsum+=to_string(carry);
            reverse(tempsum.begin(),tempsum.end());
            tempsum+=string(c++,'0');
            if(res.empty())
            res=tempsum;
            else
            res=add(res,tempsum);
        }
        return res;
    }
};
```

## 13 Detect Squares

You are given a stream of points on the X-Y plane. Design an algorithm that:

Adds new points from the stream into a data structure. Duplicate points are allowed and should be treated as different points.
Given a query point, counts the number of ways to choose three points from the data structure such that the three points and the query point form an axis-aligned square with positive area.
An axis-aligned square is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the DetectSquares class:

DetectSquares() Initializes the object with an empty data structure.
void add(int[] point) Adds a new point point = [x, y] to the data structure.
int count(int[] point) Counts the number of ways to form axis-aligned squares with point point = [x, y] as described above.

```cpp
class DetectSquares {
    map<pair<int,int>,int>mp;
    unordered_map<int,unordered_map<int,int>>ypts;
public:
    DetectSquares() {
        
    }
    
    void add(vector<int> point) {
        mp[{point[0],point[1]}]++;
        ypts[point[1]][point[0]]++;
    }
    
    int count(vector<int> point) {
        int ways=0;
        if(!ypts.count(point[1]))
        return 0;
        for(auto &[x,cnt]:ypts[point[1]]){
            if(x==point[0])
            continue;
            int d=abs(x-point[0]);
            ways+=cnt*mp[{point[0],point[1]+d}]*mp[{x,point[1]+d}];
            ways+=cnt*mp[{point[0],point[1]-d}]*mp[{x,point[1]-d}];
        }
        return ways;
    }
};
```
