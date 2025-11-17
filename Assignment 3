# ENCS205 - Data Structures
# Assignment 03: Campus Navigation and Data Management
# Python Implementation

import heapq
from collections import defaultdict, deque

# -------------------- Building ADT --------------------
class Building:
    def __init__(self, id, name, details):
        self.id = id
        self.name = name
        self.details = details

    def __repr__(self):
        return f"(ID:{self.id}, {self.name})"


# -------------------- Binary Search Tree (BST) --------------------
class BSTNode:
    def __init__(self, building):
        self.b = building
        self.left = None
        self.right = None


class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, b):
        self.root = self._insert(self.root, b)

    def _insert(self, node, b):
        if not node:
            return BSTNode(b)
        if b.id < node.b.id:
            node.left = self._insert(node.left, b)
        elif b.id > node.b.id:
            node.right = self._insert(node.right, b)
        else:
            node.b = b  # update
        return node

    def search(self, id):
        cur = self.root
        while cur:
            if id == cur.b.id:
                return cur.b
            cur = cur.left if id < cur.b.id else cur.right
        return None

    def inorder(self):
        res = []
        self._inorder(self.root, res)
        return res

    def _inorder(self, node, res):
        if node:
            self._inorder(node.left, res)
            res.append(node.b)
            self._inorder(node.right, res)

    def preorder(self):
        res = []
        self._preorder(self.root, res)
        return res

    def _preorder(self, node, res):
        if node:
            res.append(node.b)
            self._preorder(node.left, res)
            self._preorder(node.right, res)

    def postorder(self):
        res = []
        self._postorder(self.root, res)
        return res

    def _postorder(self, node, res):
        if node:
            self._postorder(node.left, res)
            self._postorder(node.right, res)
            res.append(node.b)

    def height(self):
        return self._height(self.root)

    def _height(self, node):
        if not node:
            return 0
        return 1 + max(self._height(node.left), self._height(node.right))


# -------------------- AVL Tree --------------------
class AVLNode:
    def __init__(self, b):
        self.b = b
        self.left = None
        self.right = None
        self.height = 1


class AVLTree:
    def __init__(self):
        self.root = None

    def insert(self, b):
        self.root = self._insert(self.root, b)

    def _insert(self, node, b):
        if not node:
            return AVLNode(b)

        if b.id < node.b.id:
            node.left = self._insert(node.left, b)
        elif b.id > node.b.id:
            node.right = self._insert(node.right, b)
        else:
            node.b = b
            return node

        node.height = 1 + max(self._get_height(node.left),
                              self._get_height(node.right))

        balance = self._get_balance(node)

        # LL
        if balance > 1 and b.id < node.left.b.id:
            return self._right_rotate(node)
        # RR
        if balance < -1 and b.id > node.right.b.id:
            return self._left_rotate(node)
        # LR
        if balance > 1 and b.id > node.left.b.id:
            node.left = self._left_rotate(node.left)
            return self._right_rotate(node)
        # RL
        if balance < -1 and b.id < node.right.b.id:
            node.right = self._right_rotate(node.right)
            return self._left_rotate(node)

        return node

    def _get_height(self, n):
        return n.height if n else 0

    def _get_balance(self, n):
        return self._get_height(n.left) - self._get_height(n.right)

    def _right_rotate(self, y):
        x = y.left
        T2 = x.right
        x.right = y
        y.left = T2

        y.height = 1 + max(self._get_height(y.left),
                           self._get_height(y.right))
        x.height = 1 + max(self._get_height(x.left),
                           self._get_height(x.right))
        return x

    def _left_rotate(self, x):
        y = x.right
        T2 = y.left
        y.left = x
        x.right = T2

        x.height = 1 + max(self._get_height(x.left),
                           self._get_height(x.right))
        y.height = 1 + max(self._get_height(y.left),
                           self._get_height(y.right))
        return y

    def inorder(self):
        res = []
        self._inorder(self.root, res)
        return res

    def _inorder(self, node, res):
        if node:
            self._inorder(node.left, res)
            res.append(node.b)
            self._inorder(node.right, res)

    def height(self):
        return self._get_height(self.root)


# -------------------- Graph + Algorithms --------------------
class Edge:
    def __init__(self, u, v, w):
        self.u = u
        self.v = v
        self.w = w

    def __repr__(self):
        return f"({self.u}-{self.v}: {self.w:.2f})"


class CampusGraph:
    def __init__(self, directed=False):
        self.nodes = {}
        self.adj = defaultdict(list)
        self.directed = directed

    def add_building(self, b):
        self.nodes[b.id] = b

    def add_edge(self, u, v, w):
        self.adj[u].append(Edge(u, v, w))
        if not self.directed:
            self.adj[v].append(Edge(v, u, w))

    # adjacency matrix
    def adjacency_matrix(self):
        ids = sorted(self.nodes.keys())
        idx = {id: i for i, id in enumerate(ids)}
        n = len(ids)
        INF = float('inf')

        mat = [[0 if i == j else INF for j in range(n)] for i in range(n)]

        for u in self.adj:
            for e in self.adj[u]:
                mat[idx[u]][idx[e.v]] = e.w

        return mat

    # BFS
    def bfs(self, start):
        visited = set([start])
        q = deque([start])
        order = []
        while q:
            u = q.popleft()
            order.append(u)
            for e in self.adj[u]:
                if e.v not in visited:
                    visited.add(e.v)
                    q.append(e.v)
        return order

    # DFS
    def dfs(self, start):
        visited = set()
        order = []
        self._dfs(start, visited, order)
        return order

    def _dfs(self, u, vis, order):
        vis.add(u)
        order.append(u)
        for e in self.adj[u]:
            if e.v not in vis:
                self._dfs(e.v, vis, order)

    # Dijkstra
    def dijkstra(self, src):
        dist = {id: float('inf') for id in self.nodes}
        dist[src] = 0
        pq = [(0, src)]

        while pq:
            d, u = heapq.heappop(pq)
            for e in self.adj[u]:
                if dist[e.v] > d + e.w:
                    dist[e.v] = d + e.w
                    heapq.heappush(pq, (dist[e.v], e.v))
        return dist

    # Kruskal MST
    def kruskal_mst(self):
        edges = []
        for u in self.adj:
            for e in self.adj[u]:
                if self.directed:
                    edges.append(e)
                else:
                    if e.u < e.v:
                        edges.append(e)

        edges.sort(key=lambda x: x.w)

        uf = UnionFind()
        for id in self.nodes:
            uf.make_set(id)

        mst = []
        for e in edges:
            if uf.find(e.u) != uf.find(e.v):
                uf.union(e.u, e.v)
                mst.append(e)
        return mst


# -------------------- Union-Find --------------------
class UnionFind:
    def __init__(self):
        self.parent = {}
        self.rank = {}

    def make_set(self, x):
        self.parent[x] = x
        self.rank[x] = 0

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        rx = self.find(x)
        ry = self.find(y)
        if rx == ry:
            return
        if self.rank[rx] < self.rank[ry]:
            self.parent[rx] = ry
        elif self.rank[rx] > self.rank[ry]:
            self.parent[ry] = rx
        else:
            self.parent[ry] = rx
            self.rank[rx] += 1


# -------------------- Expression Tree --------------------
class ExprNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None


class ExpressionTree:
    def is_operator(self, s):
        return s in {"+", "-", "*", "/"}

    def build(self, tokens):
        stack = []
        for t in tokens:
            if self.is_operator(t):
                r = stack.pop()
                l = stack.pop()
                node = ExprNode(t)
                node.left = l
                node.right = r
                stack.append(node)
            else:
                stack.append(ExprNode(t))
        return stack[-1] if stack else None

    def evaluate(self, node):
        if not node:
            return 0
        if not self.is_operator(node.val):
            return float(node.val)

        l = self.evaluate(node.left)
        r = self.evaluate(node.right)

        if node.val == "+": return l + r
        if node.val == "-": return l - r
        if node.val == "*": return l * r
        if node.val == "/": return l / r


# -------------------- Demonstration / Main --------------------
if __name__ == "__main__":

    # Sample Buildings
    b1 = Building(10, "CS Block", "Computer Science Dept")
    b2 = Building(5, "Library", "Central Library")
    b3 = Building(15, "Admin", "Administration")
    b4 = Building(2, "Cafeteria", "Food Court")
    b5 = Building(7, "Hostel A", "Boys Hostel")

    # BST
    bst = BinarySearchTree()
    for b in [b1, b2, b3, b4, b5]:
        bst.insert(b)

    print("BST Inorder:", bst.inorder())
    print("BST Preorder:", bst.preorder())
    print("BST Postorder:", bst.postorder())
    print("BST Height:", bst.height())

    # AVL
    avl = AVLTree()
    for b in [b1, b2, b3, b4, b5]:
        avl.insert(b)

    print("AVL Inorder:", avl.inorder())
    print("AVL Height:", avl.height())

    # Graph
    g = CampusGraph(False)
    for b in [b1, b2, b3, b4, b5]:
        g.add_building(b)

    g.add_edge(10, 5, 4.5)
    g.add_edge(10, 15, 6.0)
    g.add_edge(5, 7, 3.0)
    g.add_edge(2, 5, 2.0)
    g.add_edge(7, 15, 5.5)

    print("Adjacency List:", dict(g.adj))
    print("BFS from 10:", g.bfs(10))
    print("DFS from 10:", g.dfs(10))

    print("Adjacency Matrix:")
    for row in g.adjacency_matrix():
        print(row)

    print("Dijkstra (from 10):", g.dijkstra(10))

    print("Kruskal MST:", g.kruskal_mst())

    # Expression Tree Example
    postfix = ["100", "5", "*", "50", "+"]
    et = ExpressionTree()
    root = et.build(postfix)
    print("Expression Tree Evaluation:", et.evaluate(root))
