1)Python program to solve the 8-Puzzle problem
from collections import deque

start = (1, 2, 3, 4, 5, 6, 0, 7, 8)
goal = (1, 2, 3, 4, 5, 6, 7, 8, 0)

q = deque([start])
visited = {start}
parent = {start: None}

while q:
    state = q.popleft()
    if state == goal:
        break

    z = state.index(0)
    for d in (-3, 3, -1, 1):
        n = z + d
        if n < 0 or n >= 9:
            continue
        if d == -1 and z % 3 == 0:
            continue
        if d == 1 and z % 3 == 2:
            continue

        a = list(state)
        a[z], a[n] = a[n], a[z]
        nxt = tuple(a)

        if nxt not in visited:
            visited.add(nxt)
            parent[nxt] = state
            q.append(nxt)

path = []
while state is not None:
    path.append(state)
    state = parent[state]

for s in reversed(path):
    print(s[0:3])
    print(s[3:6])
    print(s[6:9])
    print()

2)Python program to solve the 8-Queen problem

N = 8
board = [-1] * N

def solve(row):
    if row == N:
        for r in range(N):
            print(" ".join("Q" if board[r] == c else "." for c in range(N)))
        return True

    for col in range(N):
        safe = True
        for r in range(row):
            if board[r] == col or abs(board[r] - col) == abs(r - row):
                safe = False
                break

        if safe:
            board[row] = col
            if solve(row + 1):
                return True

    return False

solve(0)
3)Python program for the Water Jug problem

from collections import deque

start = (0, 0)
q = deque([start])
visited = {start}
parent = {start: None}

while q:
    a, b = q.popleft()

    if a == 2 or b == 2:
        goal = (a, b)
        break

    states = [
        (4, b), (a, 3), (0, b), (a, 0),
        (a - min(a, 3-b), b + min(a, 3-b)),
        (a + min(b, 4-a), b - min(b, 4-a))
    ]

    for s in states:
        if s not in visited:
            visited.add(s)
            parent[s] = (a, b)
            q.append(s)

path = []
while goal is not None:
    path.append(goal)
    goal = parent[goal]

for s in reversed(path):
    print(s)
4)Python program for the Crypt-Arithmetic problem

from itertools import permutations

letters = "SENDMORY"

for p in permutations(range(10), 8):
    d = dict(zip(letters, p))

    if d['S'] == 0 or d['M'] == 0:
        continue

    SEND = 1000*d['S'] + 100*d['E'] + 10*d['N'] + d['D']
    MORE = 1000*d['M'] + 100*d['O'] + 10*d['R'] + d['E']
    MONEY = 10000*d['M'] + 1000*d['O'] + 100*d['N'] + 10*d['E'] + d['Y']

    if SEND + MORE == MONEY:
        print("SEND =", SEND)
        print("MORE =", MORE)
        print("MONEY =", MONEY)
        break
5)Python program for the Missionaries and Cannibals problem

from collections import deque

start = (3, 3, 1)
goal = (0, 0, 0)
moves = [(1, 0), (2, 0), (0, 1), (0, 2), (1, 1)]

q = deque([start])
parent = {start: None}

while q:
    m, c, boat = q.popleft()

    if (m, c, boat) == goal:
        break

    for x, y in moves:
        if boat == 1:
            nm, nc, nb = m-x, c-y, 0
        else:
            nm, nc, nb = m+x, c+y, 1

        if not (0 <= nm <= 3 and 0 <= nc <= 3):
            continue
        if nm > 0 and nm < nc:
            continue
        if 3-nm > 0 and 3-nm < 3-nc:
            continue

        nxt = (nm, nc, nb)
        if nxt not in parent:
            parent[nxt] = (m, c, boat)
            q.append(nxt)

path = []
state = goal
while state is not None:
    path.append(state)
    state = parent[state]

for s in reversed(path):
    print(s)
6)Vacuum Cleaner Problem

rooms = {'A': 'Dirty', 'B': 'Dirty'}
location = 'A'

while 'Dirty' in rooms.values():
    print("Vacuum at", location)
    rooms[location] = 'Clean'
    print(location, "is Clean")

    if location == 'A':
        location = 'B'
    else:
        location = 'A'

print("All rooms are Clean")
7)Python program to implement BFS

from collections import deque

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

visited = set()
queue = deque(['A'])

while queue:
    node = queue.popleft()
    if node not in visited:
        print(node, end=" ")
        visited.add(node)
        queue.extend(graph[node])

8)Python program to implement DFS

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

visited = set()

def dfs(node):
    if node not in visited:
        print(node, end=" ")
        visited.add(node)
        for neighbor in graph[node]:
            dfs(neighbor)

dfs('A')
9)Python program to implement Travelling Salesman Problem (TSP)

from itertools import permutations

graph = [
    [0, 10, 15, 20],
    [10, 0, 35, 25],
    [15, 35, 0, 30],
    [20, 25, 30, 0]
]

n = len(graph)
best_cost = float('inf')
best_path = None

for p in permutations(range(1, n)):
    path = (0,) + p + (0,)
    cost = sum(graph[path[i]][path[i+1]]
               for i in range(n))

    if cost < best_cost:
        best_cost = cost
        best_path = path

print("Shortest Path:", " -> ".join(str(x+1) for x in best_path))
print("Minimum Cost:", best_cost)
10)Python program to implement A* Algorithm

import heapq

graph = {
    'A': [('B', 1), ('C', 3)],
    'B': [('D', 1), ('E', 5)],
    'C': [('F', 2)],
    'D': [('E', 1)],
    'E': [('G', 2)],
    'F': [('G', 1)],
    'G': []
}

h = {'A': 4, 'B': 3, 'C': 3, 'D': 2,
     'E': 1, 'F': 1, 'G': 0}

pq = [(h['A'], 0, 'A', ['A'])]
visited = set()

while pq:
    f, cost, node, path = heapq.heappop(pq)

    if node == 'G':
        print("Shortest Path:", " -> ".join(path))
        print("Total Cost:", cost)
        break

    if node in visited:
        continue
    visited.add(node)

    for neighbor, weight in graph[node]:
        if neighbor not in visited:
            new_cost = cost + weight
            heapq.heappush(
                pq,
                (new_cost + h[neighbor], new_cost,
                 neighbor, path + [neighbor])
            )













