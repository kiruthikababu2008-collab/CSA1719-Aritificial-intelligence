
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
