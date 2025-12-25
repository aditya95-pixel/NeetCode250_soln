## 1 Implement Trie (Prefix Tree)

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

Trie() Initializes the trie object.
void insert(String word) Inserts the string word into the trie.
boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

```cpp
class Trie{
    public:
    class Node{
        public:
        Node*next[26];
        bool end;
        Node(){
            end=0;
            for(int i=0;i<26;i++)
            next[i]=NULL;
        }
    };
    Node*trie;
    Trie(){
        trie=new Node();
    }
    void insert(string word){
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
            {
                it->next[word[i]-'a']=new Node();
                it=it->next[word[i]-'a'];
            }else
            it=it->next[word[i]-'a'];
        }
        it->end=1;
    }
    bool search(string word){
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
                return 0;
            else
            it=it->next[word[i]-'a'];
        }
        return it->end;
    }
    bool startsWith(string word) {
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
                return 0;
            else
            it=it->next[word[i]-'a'];
        }
        return 1;
    }
};
class PrefixTree {
    Trie*t;
public:
    PrefixTree() {
        t=new Trie();
    }
    
    void insert(string word) {
        t->insert(word);
    }
    
    bool search(string word) {
        return t->search(word);
    }
    
    bool startsWith(string prefix) {
        return t->startsWith(prefix);
    }
};
```

## 2 Design Add and Search Words Data Structure

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the WordDictionary class:

WordDictionary() Initializes the object.
void addWord(word) Adds word to the data structure, it can be matched later.
bool search(word) Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter.

```cpp
class Trie{
    public:
    class Node{
        public:
        bool end;
        Node*next[26];
        Node(){
            end=0;
            for(int i=0;i<26;i++)
            next[i]=NULL;
        }
    };
    Node*trie;
    Trie(){
        trie=new Node();
    }
    void insert(string word){
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
            {
                it->next[word[i]-'a']=new Node();
                it=it->next[word[i]-'a'];
            }else
            it=it->next[word[i]-'a'];
        }
        it->end=1;
    }
    bool solve(string word,int idx,Node*it){
        if(idx==word.size() && it->end)
        return 1;
        else if(idx==word.size())
        return 0;
        else if(idx<word.size() && word[idx]!='.' && !it->next[word[idx]-'a'])
        return 0;
        else if(idx<word.size() && word[idx]!='.' && it->next[word[idx]-'a'])
        return solve(word,idx+1,it->next[word[idx]-'a']);
        else if(idx<word.size() && word[idx]=='.'){
            for(int j=0;j<26;j++){
                if(it->next[j] && solve(word,idx+1,it->next[j]))
                    return 1;
            }
            return 0;
        }
        return 0;
    }
    bool search(string word){
        Node*it=trie;
        return solve(word,0,it);
    }
};
class WordDictionary {
    Trie*trie;
public:
    WordDictionary() {
        trie=new Trie();
    }
    
    void addWord(string word) {
        trie->insert(word);
    }
    
    bool search(string word) {
        return trie->search(word);
    }
};
```

## 3 Extra Characters in a String

You are given a 0-indexed string s and a dictionary of words dictionary. You have to break s into one or more non-overlapping substrings such that each substring is present in dictionary. There may be some extra characters in s which are not present in any of the substrings.

Return the minimum number of extra characters left over if you break up s optimally.

```cpp
class Solution {
public:
    int minExtraChar(string s, vector<string>& dictionary) {
        set<string>seto(dictionary.begin(),dictionary.end());
        vector<int>dp(s.size()+1,0);
        for(int i=s.size()-1;i>=0;i--){
            dp[i]=1+dp[i+1];
            string str;
            for(int j=i;j<s.size();j++){
                str+=s[j];
                if(seto.count(str))
                dp[i]=min(dp[i],dp[j+1]);
            }
        }
        return dp[0];
    }
};
```

## 4 Word Search II

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

```cpp
class Trie{
    public:
    class Node{
        public:
        int end;
        Node*next[26];
        Node(){
            for(int i=0;i<26;i++)
            next[i]=NULL;
            end=0;
        }
    };
    Node* trie;
    Trie(){
        trie=new Node();
    }
    void insert(string &word){
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
            it->next[word[i]-'a']=new Node();
            it=it->next[word[i]-'a'];
        }
        it->end=1;
    }
    bool search(string &word){
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
            return 0;
            it=it->next[word[i]-'a'];
        }
        if(it->end)
        {
            it->end=0;
            return 1;
        }else
        return 0;
    }
    bool startswith(string &word){
        Node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
            return 0;
            it=it->next[word[i]-'a'];
        }
        return 1;
    }
};
class Solution {
public:
    void dfs(int i,int j,vector<vector<char>>& board,Trie*it,string &temp,vector<string>&res){
        char c=board[i][j];
        if(c=='.')
        return ;        
        temp+=c;
        if(!it->startswith(temp)){
            temp.pop_back();
            return ;
        }
        board[i][j]='.';
        if(it->search(temp))
            res.push_back(temp);
        if(i>0) dfs(i-1,j,board,it,temp,res);
        if(j>0) dfs(i,j-1,board,it,temp,res);
        if(i<board.size()-1) dfs(i+1,j,board,it,temp,res);
        if(j<board[0].size()-1) dfs(i,j+1,board,it,temp,res);
        temp.pop_back();
        board[i][j]=c;
    }
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        Trie*t=new Trie();
        for(auto &word:words)
            t->insert(word);
        vector<string>res;
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                string temp;
                dfs(i,j,board,t,temp,res);
            }
        }
        return res;
    }
};
```
