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
