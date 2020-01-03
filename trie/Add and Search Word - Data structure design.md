# 473 Add and Search Word - Data structure design

Design a data structure that supports the following two operations: addWord(word) and search(word)

search(word) can search a literal word or a regular expression string containing only letters a-z or ..

A . means it can represent any one letter.
## need to do again
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isWord = False
  
class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word):
        node = self.root
        root = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.isWord = True
      

    def findHelper(self, node, word, idx):
        if node is None:
            return False
        if idx >= len(word):
            return node.isWord
        curChar = word[idx]
        if curChar != '.':
            return self.findHelper(node.children.get(curChar), word, idx + 1)
        #char = '.'
        #char can be any char between 'a' and 'z'
        for anychar in node.children:
            if self.findHelper(node.children.get(anychar), word, idx + 1):
                return True
        return False
        
        
    def search(self, word):
        if word is None:
            return False
        return  self.findHelper(self.root, word, 0)


```
