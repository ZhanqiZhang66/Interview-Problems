# 132. Word Search II

Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position. One character only be used once in one word. No same word in dictionary

Example
Example 1:

Input：["doaf","agai","dcan"]，["dog","dad","dgdg","can","again"]
Output：["again","can","dad","dog"]
Explanation：
  d o a f
  a g a i
  d c a n
search in Matrix，so return ["again","can","dad","dog"].
Example 2:

Input：["a"]，["b"]
Output：[]
Explanation：
 a
search in Matrix，return [].
### Challenge
Using trie to implement your algorithm.


```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isWord = False
        self.word = None
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
        node.word = word
    
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
class Solution:
    """
    @param board: A list of lists of character
    @param words: A list of string
    @return: A list of string
    """
    def wordSearchII(self, board, words):
        tree = Trie()
        res = []
        for word in words:
            tree.insert(word)
        for i in range(len(board)):
            for j in range(len(board[i])):
                self.search(board, i, j, tree.root, res)
        return res
    def search(self, board, x, y, root, res):
        DIRECTIONS = [(1,0),(-1,0),(0,1),(0,-1)]
        if board[x][y] not in root.children: #if first letter not in dict's initial children
            return
        child = root.children.get(board[x][y]) #first letter node : board[x][y]
        if child.isWord and child.word not in res:
            print(child.word)
            res.append(child.word)
        
        tmp = board[x][y]
        board[x][y] = 0
        for dx, dy in DIRECTIONS:
            nx = x + dx
            ny = y + dy
            if self.isValid(board, nx, ny):
                self.search(board, nx, ny, child, res)
        board[x][y] = tmp 
        
    def isValid(self, board, x, y):
        return x >= 0 and x < len(board) and y >= 0 and y < len(board[0]) and board[x][y] != 0
        
            

```
