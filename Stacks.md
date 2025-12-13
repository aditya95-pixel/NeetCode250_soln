### 1 Baseball Game

You are keeping the scores for a baseball game with strange rules. At the beginning of the game, you start with an empty record.

You are given a list of strings operations, where operations[i] is the ith operation you must apply to the record and is one of the following:

An integer x.
Record a new score of x.
'+'.
Record a new score that is the sum of the previous two scores.
'D'.
Record a new score that is the double of the previous score.
'C'.
Invalidate the previous score, removing it from the record.
Return the sum of all the scores on the record after applying all the operations.

The test cases are generated such that the answer and all intermediate calculations fit in a 32-bit integer and that all operations are valid.

```cpp
class Solution {
public:
    int calPoints(vector<string>& operations) {
        stack<int>stk;
        for(auto &c:operations){
            if(c=="+")
            {
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1);
                stk.push(num2);
                stk.push(num1+num2);
            }
            else if(c=="D")
            {
                int num=stk.top();
                stk.push(num*2);
            }
            else if(c=="C")
            stk.pop();
            else
            stk.push(stoi(c));
        }
        int sum=0;
        while(!stk.empty()){
            sum+=stk.top();
            stk.pop();
        }
        return sum;
    }
};
```

### 2 Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char>stk;
        for(auto &c:s){
            if(c=='(' || c=='{' || c=='[')
            stk.push(c);
            else if(stk.empty() && (c==')' || c=='}' || c==']'))
            return false;
            else if((stk.top()=='(' && c==')') || (stk.top()=='{' && c=='}') || (stk.top()=='[' && c==']'))
            stk.pop();
            else return false;
        }
        return stk.empty();
    }
};
```

### 3 Implement Stack using Queues

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the MyStack class:

void push(int x) Pushes element x to the top of the stack.
int pop() Removes the element on the top of the stack and returns it.
int top() Returns the element on the top of the stack.
boolean empty() Returns true if the stack is empty, false otherwise.
Notes:

You must use only standard operations of a queue, which means that only push to back, peek/pop from front, size and is empty operations are valid.
Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

```cpp
class MyStack {
    queue<int>q1,q2;
public:
    MyStack() {
        
    }
    void push(int x) {
        q1.push(x);
    }
    int pop() {
        while(q1.size()>1){
            int ele=q1.front();
            q1.pop();
            q2.push(ele);
        }
        int val=q1.front();
        q1.pop();
        while(!q2.empty()){
            int ele=q2.front();
            q2.pop();
            q1.push(ele);
        }
        return val;
    }
    int top() {
        while(q1.size()>1){
            int ele=q1.front();
            q1.pop();
            q2.push(ele);
        }
        int val=q1.front();
        q1.pop();
        q2.push(val);
        while(!q2.empty()){
            int ele=q2.front();
            q2.pop();
            q1.push(ele);
        }
        return val;
    }
    
    bool empty() {
        return q1.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

### 4 Implement Queue using Stacks

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

void push(int x) Pushes element x to the back of the queue.
int pop() Removes the element from the front of the queue and returns it.
int peek() Returns the element at the front of the queue.
boolean empty() Returns true if the queue is empty, false otherwise.
Notes:

You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

```cpp
class MyQueue {
    stack<int>stk1,stk2;
public:
    MyQueue() {
        
    }
    
    void push(int x) {
        stk1.push(x);
    }
    
    int pop() {
        while(stk1.size()>1)
        {
            int ele=stk1.top();
            stk1.pop();
            stk2.push(ele);
        }
        int val=stk1.top();
        stk1.pop();
        while(!stk2.empty())
        {
            int ele=stk2.top();
            stk2.pop();
            stk1.push(ele);
        }
        return val;
    }
    
    int peek() {
        while(stk1.size()>1)
        {
            int ele=stk1.top();
            stk1.pop();
            stk2.push(ele);
        }
        int val=stk1.top();
        stk1.pop();
        stk2.push(val);
        while(!stk2.empty())
        {
            int ele=stk2.top();
            stk2.pop();
            stk1.push(ele);
        }
        return val;
    }
    
    bool empty() {
        return stk1.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

### 5 Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

MinStack() initializes the stack object.
void push(int val) pushes the element val onto the stack.
void pop() removes the element on the top of the stack.
int top() gets the top element of the stack.
int getMin() retrieves the minimum element in the stack.
You must implement a solution with O(1) time complexity for each function.

```cpp
class MinStack {
    stack<pair<int,int>>stk;
public:
    MinStack() {
        
    }
    
    void push(int val) {
        if(stk.empty() || stk.top().second>=val)
        stk.push({val,val});
        else
        {
            int mino=stk.top().second;
            stk.push({val,mino});
        }
    }
    
    void pop() {
        stk.pop();
    }
    
    int top() {
        return stk.top().first;
    }
    
    int getMin() {
        return stk.top().second;
    }
};
```

### 6 Evaluate Reverse Polish Notation

You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

The valid operators are '+', '-', '*', and '/'.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int>stk;
        for(auto &c:tokens){
            if(c=="+"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1+num2);
            }
            else if(c=="-"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1-num2);
            }
            else if(c=="*"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1*num2);
            }
            else if(c=="/"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1/num2);
            }
            else
            stk.push(stoi(c));
        }
        return stk.top();
    }
};
```

### 7 Asteroid Collision

We are given an array asteroids of integers representing asteroids in a row. The indices of the asteroid in the array represent their relative position in space.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int>res;
        stack<int>stk;
        stk.push(asteroids[0]);
        for(int i=1;i<asteroids.size();i++){
            if(stk.empty())
            stk.push(asteroids[i]);
            else if((stk.top()>0 && asteroids[i]>0) || (stk.top()<0 && asteroids[i]<0))
            stk.push(asteroids[i]);
            else{
                bool chk=true;
                while(!stk.empty()){
                    if((stk.top()>0 && asteroids[i]<0) && abs(asteroids[i])>abs(stk.top()))
                        stk.pop();
                    else if((stk.top()>0 && asteroids[i]<0) && abs(asteroids[i])==abs(stk.top())){
                        stk.pop();
                        chk=false;
                        break;
                    }
                    else if((stk.top()>0 && asteroids[i]<0) && abs(asteroids[i])<abs(stk.top()))
                    {
                        chk=false;
                        break;
                    }
                    else
                    break;
                }
                if(chk)
                stk.push(asteroids[i]);
            }
        }
        while(!stk.empty())
        {
            res.push_back(stk.top());
            stk.pop();
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```

### 8 Daily Temperatures

Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead.

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int>res(temperatures.size(),0);
        stack<int>stk;
        for(int i=temperatures.size()-1;i>=0;i--){
            while(!stk.empty() && temperatures[i]>=temperatures[stk.top()])
            stk.pop();
            if(!stk.empty())
            res[i]=stk.top()-i;
            stk.push(i);
        }
        return res;
    }
};
```

### 9 Online Stock Span

Design an algorithm that collects daily price quotes for some stock and returns the span of that stock's price for the current day.

The span of the stock's price in one day is the maximum number of consecutive days (starting from that day and going backward) for which the stock price was less than or equal to the price of that day.

For example, if the prices of the stock in the last four days is [7,2,1,2] and the price of the stock today is 2, then the span of today is 4 because starting from today, the price of the stock was less than or equal 2 for 4 consecutive days.
Also, if the prices of the stock in the last four days is [7,34,1,2] and the price of the stock today is 8, then the span of today is 3 because starting from today, the price of the stock was less than or equal 8 for 3 consecutive days.
Implement the StockSpanner class:

StockSpanner() Initializes the object of the class.
int next(int price) Returns the span of the stock's price given that today's price is price.

```cpp
class StockSpanner {
    stack<int>stk;
    vector<pair<int,int>>v;
    int cnt=0;
public:
    StockSpanner() {
        
    }
    
    int next(int price) {
       if(stk.empty())
       {
            stk.push(price);
            cnt++;
            return cnt;
       }else if(stk.top()<=price)
       {
            stk.push(price);
            cnt++;
            int end=v.size(),res=cnt;
            for(int i=end-1;i>=0;i--){
                if(v[i].first<=price)
                res+=v[i].second;
                else
                break;
            }
            return res;
       }else{
            v.push_back({stk.top(),cnt});
            stk.push(price);
            cnt=1;
            return cnt;
       }
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```

### 10 Car Fleet

There are n cars at given miles away from the starting mile 0, traveling to reach the mile target.

You are given two integer arrays position and speed, both of length n, where position[i] is the starting mile of the ith car and speed[i] is the speed of the ith car in miles per hour.

A car cannot pass another car, but it can catch up and then travel next to it at the speed of the slower car.

A car fleet is a single car or a group of cars driving next to each other. The speed of the car fleet is the minimum speed of any car in the fleet.

If a car catches up to a car fleet at the mile target, it will still be considered as part of the car fleet.

Return the number of car fleets that will arrive at the destination.

```cpp
class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        vector<pair<int,int>>vp;
        for(int i=0;i<speed.size();i++)
        vp.push_back({position[i],speed[i]});
        sort(vp.rbegin(),vp.rend());
        double time=(double)(target-vp[0].first)/vp[0].second;
        int cnt=1;
        for(int i=1;i<vp.size();i++){
            if((double)(target-vp[i].first)/vp[i].second<=time)
            continue;
            else
            {
                time=(double)(target-vp[i].first)/vp[i].second;
                cnt++;
            }
        }
        return cnt;
    }
};
```

### 11 Simplify Path

You are given an absolute path for a Unix-style file system, which always begins with a slash '/'. Your task is to transform this absolute path into its simplified canonical path.

The rules of a Unix-style file system are as follows:

A single period '.' represents the current directory.
A double period '..' represents the previous/parent directory.
Multiple consecutive slashes such as '//' and '///' are treated as a single slash '/'.
Any sequence of periods that does not match the rules above should be treated as a valid directory or file name. For example, '...' and '....' are valid directory or file names.
The simplified canonical path should follow these rules:

The path must start with a single slash '/'.
Directories within the path must be separated by exactly one slash '/'.
The path must not end with a slash '/', unless it is the root directory.
The path must not have any single or double periods ('.' and '..') used to denote current or parent directories.
Return the simplified canonical path.

```cpp
class Solution {
public:
    string simplifyPath(string path) {
        stack<string>stk;
        int i=0;
        while(i<path.size()){
            i++;
            string temp="/";
            while(i<path.size() && path[i]!='/')
            temp+=path[i++];
            if(temp=="/.." && !stk.empty())
            stk.pop();
            else if(temp=="/..")
            continue;
            else if(temp=="/")
            continue;
            else if(temp=="/.")
            continue;
            else
            stk.push(temp);
        }
        string res;
        while(!stk.empty()){
            res=stk.top()+res;
            stk.pop();
        }
        if(res=="")
        res+='/';
        return res;
    }
};
```

### 12 Decode String

Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there will not be input like 3a or 2[4].

The test cases are generated so that the length of the output will never exceed 10^5.

```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<string>stk;
        int i=0;
        while(i<s.size()){
            if(s[i]>='0' && s[i]<='9'){
                string cnt;
                while(s[i]<='9' && s[i]>='0')
                cnt+=s[i++];
                stk.push(cnt);
            }
            else if(s[i]=='[' || (s[i]<='z' && s[i]>='a'))
            stk.push(string(1,s[i++]));
            else{
                string temp;
                while(stk.top()!="["){
                    temp+=stk.top();
                    stk.pop();
                }
                stk.pop();
                int cnt=stoi(stk.top());
                stk.pop();
                string t;
                while(cnt--){
                    t+=temp;
                }
                stk.push(t);
                i++;
            }
        }
        string res;
        while(!stk.empty())
        {
            res+=stk.top();
            stk.pop();
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```

### 13 Maximum Frequency Stack

Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack.

Implement the FreqStack class:

FreqStack() constructs an empty frequency stack.
void push(int val) pushes an integer val onto the top of the stack.
int pop() removes and returns the most frequent element in the stack.
If there is a tie for the most frequent element, the element closest to the stack's top is removed and returned.

```cpp
class FreqStack {
    map<int,stack<int>>mp1;
    map<int,int>freq;
    int maxfreq=0;
public:
    FreqStack() {
        
    }
    
    void push(int val) {
        int f=++freq[val];
        maxfreq=max(maxfreq,f);
        mp1[f].push(val);
    }
    
    int pop() {
        int val=mp1[maxfreq].top();
        mp1[maxfreq].pop();
        freq[val]--;
        if(mp1[maxfreq].empty())
        maxfreq--;
        return val;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```

### 14 Largest Rectangle in Histogram

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int>stk;
        int maxarea=0;
        for(int i=0;i<heights.size();i++){
            while(!stk.empty() && heights[stk.top()]>=heights[i]){
                int top=stk.top();
                stk.pop();
                int width=stk.empty()?i:i-stk.top()-1;
                maxarea=max(maxarea,heights[top]*width);
            }
            stk.push(i);
        }
        while(!stk.empty()){
            int top=stk.top();
            stk.pop();
            int width=stk.empty()?heights.size():heights.size()-stk.top()-1;
            maxarea=max(maxarea,heights[top]*width);
        }
        return maxarea;
    }
};
```
