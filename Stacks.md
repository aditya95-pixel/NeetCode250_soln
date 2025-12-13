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
