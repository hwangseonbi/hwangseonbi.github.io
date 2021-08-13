---
date: 2021-08-12 12:11:00
resources:
- name: f159b50a-fb5a-4da9-9b73-e27cd58177c8.png
  src: f159b50a-fb5a-4da9-9b73-e27cd58177c8.png
  title: ''
- name: 77a9512c-2ff3-498c-92fd-cf49e09bd662.png
  src: 77a9512c-2ff3-498c-92fd-cf49e09bd662.png
  title: ''
- name: f7c7605d-c0f0-4e1f-a89e-b39842ce8150.png
  src: f7c7605d-c0f0-4e1f-a89e-b39842ce8150.png
  title: ''
- name: 0dec6ab3-fd51-4422-b907-df355a0924dc.png
  src: 0dec6ab3-fd51-4422-b907-df355a0924dc.png
  title: ''
title: DFS와 BFS
weight: 4

---
기본적으로 DFS와 BFS의 탐색과정은 아래와 같다.

{{< img name="f159b50a-fb5a-4da9-9b73-e27cd58177c8.png" size="large" width="640" lazy=false >}}

<br>



코딩테스트에서는 이를 응용하여 대략 두가지 유형으로 나뉜다.

- 그래프

- 2차원 배열

<br>



# 그래프

[백준 DFS와 BFS 문제](https://www.acmicpc.net/problem/1260)(1260)가 그래프형태의 기본적이 예시다. 문제를 요약하면 대략 이렇다.

<br>



아래와 같이 그래프가 주어졌을 때, DFS와 BFS로 탐색하게 될 경로를 출력해야한다. 하나의 정점에서 다음 정점으로 가려할 때 갈 수 있는 정점이 두개 이상이라면 숫자가 작은 쪽으로 이동해야한다.

{{< img name="77a9512c-2ff3-498c-92fd-cf49e09bd662.png" size="small" width="288" lazy=false >}}

<br>



가장 먼저 해야할 것은 그래프를 만드는 것이다. Dictionary로 그래프를 나타내면 문제 풀기가 (주관적)한결 쉬워진다. 양방향이므로 아래와 같이 만들어진다.

{{< img name="f7c7605d-c0f0-4e1f-a89e-b39842ce8150.png" size="large" width="780" lazy=false >}}

BFS는 큐로, DFS는 스택을 사용한다. 주의할 점은 다음 선택할 지점이 여러개일때 지점의 숫자가 적은 쪽으로 넘어가야하므로 정렬이 필요하다.

{{< highlight Python "linenos=table" >}}
import sys

#표준 입력
N, M, V = map(int, sys.stdin.readline().split())
GRAPH = []
for _ in range(M):
    GRAPH.append(tuple(map(int, sys.stdin.readline().split())))


def dictionarify(graph):
    result = {i: [] for i in range(1, N + 1)}
    for s, e in graph:
        result[s].append(e)
        result[e].append(s)

    return result


def bfs(graph, start_point):
    queue = [start_point]
    visited = []
    while queue:
        current_point = queue.pop(0)
        if current_point in visited:
            continue
        visited.append(current_point)

        # 다음 지점들이 여러개일 때 숫자가 작은 지점부터 큐에 넣어야한다.
        # 왜냐하면 While 루프에서 큐의 가장 앞쪽에 존재하는 숫자가 꺼내지기 때문이다.
        # 앞쪽의 숫자가 가장 적은 지점이어야한다.
        for next_point in sorted(graph[current_point]):
            if next_point in visited:
                continue
            queue.append(next_point)

    return visited


def dfs(graph, start_point):
    stack = [start_point]
    visited = []
    while stack:
        current_point = stack.pop()
        if current_point in visited:
            continue
        visited.append(current_point)

        # 다음 지점들이 여러개일 때 숫자가 큰 지점부터 스택에 넣어야한다.
        # 왜냐하면 While 루프에서 스택의 가장 마지막 쪽에 존재하는 숫자가 꺼내지기 때문이다.
        # 마지먹 쪽의 숫자가 가장 적은 지점이어야한다.
        for next_point in sorted(graph[current_point], reverse=True):
            if next_point in visited:
                continue
            stack.append(next_point)

    return visited


def solution():
    #Dictionary 형태의 그래프를 만들어준다.
    graph = dictionarify(GRAPH)


    dfs_route = dfs(graph, start_point=V)
    bfs_route = bfs(graph, start_point=V)
    return dfs_route, bfs_route


result_dfs, result_bfs = solution()
result = " ".join(map(str, result_dfs)) + "\n" + " ".join(map(str, result_bfs))
print(result)
{{< /highlight >}}

<br>



<br>



<br>



# 2차원배열

이 문제를 풀어보자. 2차월 배열이 주어진다. 시작점은 (0,0)이며 도착점은 (C,R)이다. 시작점에서 도착점까지 도달하는 여러 경로가 있고 해당 경로에 존재하는 값의 합 중 최소값을 구해야한다.

{{< img name="0dec6ab3-fd51-4422-b907-df355a0924dc.png" size="large" width="576" lazy=false >}}

위 예시같은 경우, 아래와 같이 다섯가지의 최단 경로가 있으며 그 중 최소값은 13이다.

2→2→3→1→5 : 13

2→2→3→8→5 : 20

2→2→1→8→5 : 13

2→6→1→8→5 : 17

<br>



배열의 범위는 1 ≤ R, C ≤ 1,000 이며 R, C는 정수이다.

각 칸에 적혀있는 숫자의 범위는 1 ≤ N ≤ 10,000 이며 N은 정수이다.

<br>



입출력 예시

#1

- grid : [ [1, 2], [3, 4] ]

- result : 7

#2

- grid : [ [1, 8, 3, 2], [7, 4, 6, 5] ]

- result : 19

<br>



<br>



이 문제는 BFS이든 DFS이든 상관없이 풀 수 있다. 우리가 응용해야할 지점은 두가지다.

- BFS, DFS는 기본적으로 탐색 알고리즘이다. 따라서 최단경로를 구하기 위해서는 DFS,BFS로 탐색한 결과를 응용하여 찾아야한다. 다행히도 2차원 배열에서 시작점이 (0,0)이고 도착점이 (C,R)으로 정해져있다면 항상 오른쪽이나 아래쪽으로만 가면 최단거리가 된다.

- 탐색을 통해 최종점에 도달만하면 되는 것이 아니라 그곳까지 도달하기까지의 경로를 알고있어야한다.

<br>



이제 풀어보자. 아래 DFS로 짠 아래 코드를 읽어보자.

{{< highlight Python "linenos=table" >}}
# 2차원 배열에서 시작점과 도착점은 (0,0)과 (C,R)으로 정해져있다. 이때 최단거리는 항상 오른쪽과 아래쪽으로 내려가면 된다.
dx = [1, 0]
dy = [0, 1]


def dfs(r, c, tracks):
    # 시작지점은 0,0이다. 0,0에서부터 시작하므로 경로에 시작지점을 추가해놓는다.
    start_point = (0, 0, [(0, 0)])

    # DFS는 스택에 다음 이동할 지점들을 쌓아놓는다.
    stack = [start_point]

    while stack:
        x, y, track = stack.pop()

        # 이 반복문을 통해 nx, ny는 한칸 오른쪽과 한칸 아래쪽 방향을 향하게 된다.
        for _dx, _dy in zip(dx, dy):
            nx = x + _dx
            ny = y + _dy

            # 범위 밖이라면 진행하지 않는다.
            if (nx < 0 or nx >= c or ny < 0 or ny >= r): continue

            # 스택에 다음 지점 좌표와 다음 지점 좌표를 경로에 추가한다.
            stack.append((nx, ny, track + [(nx, ny)]))

        # 끝지점에 도착하면 그때까지의 track을 tracks에 추가한다.
        if x == c - 1 and y == r - 1:
            tracks.append(track)


def solution(grid):
    answer = 0

    # 2차원 배열의 크기를 구한다.
    r = len(grid)
    c = len(grid[0])
    tracks = []

    # dfs함수를 실행하면 최단거리 경로들이 tracks에 저장된다.
    # 예시1의 경우 tracks는 아래와 같이 2개 경로가 최단거리다.
    # [[(0, 0), (0, 1), (1, 1)],
    #   [(0, 0), (1, 0), (1, 1)]]

    # 예시2의 경우 tracks는 아래와 같이 4개 경로가 최단거리다.
    # [[(0, 0), (0, 1), (1, 1), (2, 1), (3, 1)],
    # [(0, 0), (1, 0), (1, 1), (2, 1), (3, 1)],
    # [(0, 0), (1, 0), (2, 0), (2, 1), (3, 1)],
    # [(0, 0), (1, 0), (2, 0), (3, 0), (3, 1)]]
    dfs(r, c, tracks)
    
    # tracks에서 각 칸의 합 중 최소값을 구한다.
    min_cost = float('inf')
    for track in tracks:
        cost = 0
        for x, y in track:
            cost += grid[y][x]

        if min_cost > cost:
            min_cost = cost

    answer = min_cost
    return answer


assert solution(grid=[[1, 2], [3, 4]]) == 7
assert solution(grid=[[1, 8, 3, 2], [7, 4, 6, 5]]) == 19
{{< /highlight >}}

<br>



dfs함수를 실행할 경우 tracks는 아래와 같다.

{{< highlight Python "linenos=table" >}}
#입출력예시1
[[(0, 0), (0, 1), (1, 1)],
 [(0, 0), (1, 0), (1, 1)]]
{{< /highlight >}}

{{< highlight Python "linenos=table" >}}
#입출력예시2
[[(0, 0), (0, 1), (1, 1), (2, 1), (3, 1)],
 [(0, 0), (1, 0), (1, 1), (2, 1), (3, 1)],
 [(0, 0), (1, 0), (2, 0), (2, 1), (3, 1)],
 [(0, 0), (1, 0), (2, 0), (3, 0), (3, 1)]]

{{< /highlight >}}

<br>



BFS함수를 실행할 경우 tracks는 아래와 같다.

{{< highlight Python "linenos=table" >}}
#입출력예시1
[[(0, 0), (1, 0), (1, 1)],
 [(0, 0), (0, 1), (1, 1)]]
{{< /highlight >}}

{{< highlight Python "linenos=table" >}}
#입출력예시2
[[(0, 0), (1, 0), (2, 0), (3, 0), (3, 1)],
 [(0, 0), (1, 0), (2, 0), (2, 1), (3, 1)],
 [(0, 0), (1, 0), (1, 1), (2, 1), (3, 1)],
 [(0, 0), (0, 1), (1, 1), (2, 1), (3, 1)]]
{{< /highlight >}}

<br>



경로를 각각 비교해보면 탐색이 어떤 차이가 있는지 알 수 있을 것이다.

<br>

