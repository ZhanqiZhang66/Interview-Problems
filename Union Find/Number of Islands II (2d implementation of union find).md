# 434. Number of Islands II

Given a n,m which means the row and column of the 2D matrix and an array of pair A( size k). Originally, the 2D matrix is all 0 which means there is only sea in the matrix. The list pair has k operator and each operator has two integer A[i].x, A[i].y means that you can change the grid matrix[A[i].x][A[i].y] from sea to island. Return how many island are there in the matrix after each operator.

Example
Example 1:

Input: n = 4, m = 5, A = [[1,1],[0,1],[3,3],[3,4]]
Output: [1,1,2,2]
Explanation:
0.  00000
    00000
    00000
    00000
1.  00000
    01000
    00000
    00000
2.  01000
    01000
    00000
    00000
3.  01000
    01000
    00000
    00010
4.  01000
    01000
    00000
    00011
Example 2:

Input: n = 3, m = 3, A = [[0,0],[0,1],[2,2],[2,1]]
Output: [1,1,2,2]
## Notice
0 is represented as the sea, 1 is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.
## Solution
```python
class Solution:
    """
    @param n: An integer
    @param m: An integer
    @param operators: an array of point
    @return: an integer array
    """
   
    def numIslands2(self, n, m, operators):
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]  # DIRECTIONS
        island = set() #Hash set
        unionFind = ConnectingGraph(n*m)
        numIsland = []
        for operators in operators:
            x,y = operators.x, operators.y
            if (x,y) in island:  #Deal with repeatance
                numIsland.append(unionFind.size)
                continue
            island.add((x,y))
            unionFind.father[(x,y)] = (x,y) 
            unionFind.size += 1
            for dx, dy in directions:
                nx = x + dx
                ny = y + dy
                if (nx, ny) in island:
                    unionFind.connect((nx, ny) , (x,y))
            numIsland.append(unionFind.size)
        return numIsland
    
class ConnectingGraph:

    def __init__(self, size):
        self.father = {i: i for i in range(size)}
        self.size = 0
        
    def connect(self, a, b):
        root_a = self.find(a)
        root_b = self.find(b)
        if root_b != root_a:
            self.father[root_b] = root_a
            self.size -= 1
            
    def find(self, x):
        path = []
        while x != self.father[x]:
            path.append(x)
            x = self.father[x] #x now becomes the father
        for p in path:
            self.father[p] = x
        return x   
            

```
## Thoughts
定义一个矩阵记录地图状态, 还需要定义初始含 n \times mn×m 个集合的并查集, 以及一个计数器, 记录当前岛屿的个数.

对于每一次操作(x, y), 如果(x, y)的上下左右都是0, 那么计数器加一; 如果不全为0, 则:

并查集查询其四周的1所属的集合, 假设它们属于 k 个不同的集合
- 计数器减去 k-1
- 将这 k 个集合, 连同 (x, y), 合并
并查集的实现可以利用Point结构体, 也可以将二维坐标转换成一维.
