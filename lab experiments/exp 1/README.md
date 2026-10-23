
from heapq import heappush, heappop

goal = (1, 2, 3, 4, 5, 6, 7, 8, 0)

def heuristic(state):
    return sum(state[i] != 0 and state[i] != goal[i]
               for i in range(9))

def solve(start):
    pq = []
    heappush(pq, (heuristic(start), 0, start, []))
    visited = set()

    while pq:
        f, g, state, path = heappop(pq)

        if state == goal:
            for step in path + [state]:
                print(step[:3])
                print(step[3:6])
                print(step[6:])
                print()
            return

        if state in visited:
            continue
        visited.add(state)

        i = state.index(0)
        for j in [i-3, i+3, i-1, i+1]:
            if 0 <= j < 9 and (abs(i-j) == 3 or i//3 == j//3):
                new = list(state)
                new[i], new[j] = new[j], new[i]
                new = tuple(new)

                if new not in visited:
                    heappush(pq, (g+1+heuristic(new),
                                  g+1, new, path+[state]))

start = (1, 2, 3, 4, 0, 6, 7, 5, 8)
solve(start)
