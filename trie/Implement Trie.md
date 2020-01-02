# Implement Trie (Prefix Tree)
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isWord = False
class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.isWord = True
    
    def findNode(self, word):
        node = self.root
        for char in word:
            node = node.children.get(char)
            if node is None: return None
        return node
        
    def search(self, word):
        node = self.findNode(word)
        return node is not None and node.isWord


    def startsWith(self, prefix):
        node = self.findNode(prefix)
        return node is not None
```
