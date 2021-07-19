---
date: 2021-07-19 06:05:00
resources:
- name: 758f423b-1ee9-45e4-aba3-42af4200431e.png
  src: 758f423b-1ee9-45e4-aba3-42af4200431e.png
  title: ''
- name: a73ec48a-9471-44b0-832b-051180fa6d0d.png
  src: a73ec48a-9471-44b0-832b-051180fa6d0d.png
  title: ''
- name: d3d3d85f-ea4e-4177-9996-354f4add49c5.png
  src: d3d3d85f-ea4e-4177-9996-354f4add49c5.png
  title: ''
- name: 3f126648-6205-458d-89e7-7abe3c0fea9a.png
  src: 3f126648-6205-458d-89e7-7abe3c0fea9a.png
  title: ''
title: 다익스트라 최단거리 알고리즘에 대하여
weight: 0

---
> ***이 블로그는 Notion에서 랜더링 자동화를 통해 제작되었습니다.<br>Notion 페이지에 최적화되어있습니다. → [Notion에서 보기](https://www.notion.so/hwangseonbi/6deb1329a0df4511b16eead4ac2248a5)***

<br>



{{< toc >}}

---

# 1. 다익스트라 알고리즘

그래프들 간에 최단거리를 구할 수 있는 알고리즘이다.

<br>



# 2. 아이디어

사실 다익스트라 알고리즘은 아래 그래프를 보고 최단거리를 구하려할때 나를 포함한 보통의 사람들이 떠올리는 생각의 순서와 비슷하다. 아래 그래프상에서, A에서 B까지의 최단거리를 생각해보자. 

{{< img name="758f423b-1ee9-45e4-aba3-42af4200431e.png" size="large" width="2015" lazy=false >}}

1. 직접 연결된 곳 먼저 가보자        A→B : 100                                 얼라? 거리가 좀 커보이네?

1. 다른 곳 거쳐서 가보자                A→C→B : 400+2 = 402         중간에 400 때문에 더 걸리네..

1. 더 둘러가볼까                            A→C→E→B = 2+4+1=7        거리값이 적은 곳으로 돌아오니 빠르군

<br>



여기서 주목할 포인트는 **3번과정**이다.

<br>



3번 과정을 통해서 우리는 D 또는 E지점으로 갈때 A→B가 아닌 A→C를 거쳐야 최소값이 될 것 같다는 생각이 든다. 즉 어떤 지점까지의 최단거리는 그 이전 지점들까지의 최단거리로 이루어진다는 아이디어다.

```
✅__다익스트라 알고리즘의 핵심__
: 특정 지점에서 목표 지점까지의 최단 거리는 목표 지점과 인접한 지점까지의 최단 거리로 이루어진다.
```

<br>



다시말해, E지점까지의 최단거리는 E와 인접한 노드인 B, C, D 중 한곳을 거쳐와야할 것이고, 그렇다면 A→E 최단거리는 A→B의 최단거리 또는 A→C의 최단거리 또는 A→D의 최단거리를 통해서 얻을 수 있는 것이다.

<br>



<br>



# 3. 알고리즘

위 사고를 조금 더 체계화시켜보자.

<br>



1) A를 시작노드로 설정한다.

2) 현재노드는 A이다. A와 인접한 노드의 간선을 계산한다.

{{< img name="a73ec48a-9471-44b0-832b-051180fa6d0d.png" size="large" width="4000" lazy=false >}}

A는 시작노드이며 자기 자신으로 가는 거리는 0이다. A에서 D나 E로 가는 거리는 아직 모른다.

<br>



3) 다음 방문할 노드를 선택한다. 선택기준은 아래와 같다.

- __아직 방문하지 않은 노드이면서__

- __가장 가까운 노드__

A와 인접한 노드 중에서 가까운 곳은 C이다.

<br>



4) C노드는 D, E, B와 연결되어있다. 지금까지 구한 C까지의 최단거리(2)를 통해 몰랐던 D,E의 최단거리를 알아낼 수 있다. 어쩌면 지금까지 구한 B까지의 최단거리보다 C를 통해 B까지 가는 거리가 더 빠를 가능성도 있다.

{{< img name="d3d3d85f-ea4e-4177-9996-354f4add49c5.png" size="large" width="2478" lazy=false >}}

아쉽게도 C를 거친 B까지의 거리는 402으로 기존 최단거리보다 더 길다. 따라서 갱신하지 않는다.

<br>



5) 현재까지 A, C노드를 방문하였다. 모든 노드를 방문할 때까지 3번 과정부터 다시 반복한다.

<br>



# 4. 일반화

다시 일반화 시켜보면 아래와 같이 정리 가능하다.

1. 출발 노드 설정

1. 출발 노드를 기준으로 각 노드의 최소 비용을 저장

1. 방문하지 않은 노드 중에서 가장 비용이 적은 노드 선택

1. 해당 노드를 거쳐서 특정한 노드로 가는 경우를 고려하여 최소 비용 갱신

1. 모든 노드를 방문완료할 때까지 3번 ~ 4번을 반복

```
⚠️코드 구현 시, 3번 과정에서 비용이 가장 적은 노드를 선택할 때 배열을 돌면서 찾을 수도 있다.
하지만 우선순위 큐로 구현한다면 더 빠르게 찾을 수 있다.
```

<br>



# 5. 구현

{{< highlight Python "linenos=table" >}}
graph = {
    'A': {'B': 100, 'C': 2},
    'B': {'C': 400, 'E': 1},
    'C': {'E': 4, 'D': 2},
    'D': {'E': 3},
    'E': {}
}
{{< /highlight >}}

<br>



{{< highlight Python "linenos=table" >}}
def dijkstra_using_adj(graph, start):
    def get_min_node(current_node, graph, visited):
        min_node = None
        min_distance = float('inf')
        for node, distance in graph[current_node].items():
            if visited[node] != False:
                continue
            if min_distance > distance:
                min_distance = distance
                min_node = node

        if min_node == None:
            for node, is_visited in visited.items():
                if not is_visited:
                    min_node = node
        return min_node

    shortest_distances = {node: float('inf') for node in graph}
    shortest_distances[start] = 0
    shortest_distances.update(graph[start])

    visited = {node: False for node in graph}
    visited[start] = True

    current_node = start
    while False in visited.values():
        current_node = get_min_node(current_node, graph, visited)
        for node, d in graph[current_node].items():
            new_distance = shortest_distances[current_node] + d
            if shortest_distances[node] > new_distance:
                shortest_distances[node] = new_distance

        visited[current_node] = True
    return shortest_distances
{{< /highlight >}}

<br>



{{< highlight Python "linenos=table" >}}
print("--graph--")
print(graph)
print("---------------------")
print(dijkstra_using_adj(graph, "A"))
{{< /highlight >}}

<br>



{{< img name="3f126648-6205-458d-89e7-7abe3c0fea9a.png" size="large" width="1578" lazy=false >}}

<br>



# 6. 문제

`heapq`를 사용하여 인접 노드 중 가장 가까운 노드를 빠르게 구해볼 수 있다. `heapq`방식으로 백준 [1753](https://www.acmicpc.net/problem/1753) 문제를 풀어보았다. `4.일반화`의 3번과정(방문하지 않은 노드 중에서 가장 비용이 적은 노드 선택)을 뺐다. 방문했는지 여부는 사실 `if new_distance &lt; SHORTEST_DISTANCES[adj_node]:` 조건을 통해서 알 수 있기 때문이다.

{{< highlight Python "linenos=table" >}}
import sys
from heapq import heappush, heappop

INF = 100000000
V, E = map(int, sys.stdin.readline().split())
K = int(sys.stdin.readline())
GRAPH = [[] for _ in range(V + 1)]
SHORTEST_DISTANCES = [INF] * (V + 1)

heap = []


def dijkstra(start):
    SHORTEST_DISTANCES[start] = 0
    heappush(heap, (SHORTEST_DISTANCES[start], start))
    while heap:
        current_distance, current_node = heappop(heap)
        for adj_node, d in GRAPH[current_node]:
            new_distance = current_distance + d
            if new_distance < SHORTEST_DISTANCES[adj_node]:
                SHORTEST_DISTANCES[adj_node] = new_distance
                heappush(heap, (new_distance, adj_node))


for i in range(E):
    u, v, w = map(int, sys.stdin.readline().split())
    GRAPH[u].append((v, w))

dijkstra(K)
for d in SHORTEST_DISTANCES[1:]:
    print(d if d != INF else "INF")
{{< /highlight >}}

<br>



<br>



주의할 점은 `heapq.heapush()` 함수 사용 시, 튜플을 인자로 줄 때다. 튜플을 잘못쓰면 오히려 시간초과가 발생할 수 있다.

heappush 인자로 `heappush(heap, (new_distance, adj_node))` 대신 `heappush(heap, (adj_node, new_distance))` 을 전달했다. 이럴경우 `heappop()`을 호출했을 때 `distance`가 최소인 노드가 반환되지 않고 `adj_node` 값이 최소인 노드를 반환하게된다. 즉 그냥 노드번호가 가장 낮은 노드가 반환되는 것이다.

반드시 [Reference-heapq](https://docs.python.org/3/library/heapq.html)를 잘 확인해보고 사용하자.

<br>

