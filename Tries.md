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
