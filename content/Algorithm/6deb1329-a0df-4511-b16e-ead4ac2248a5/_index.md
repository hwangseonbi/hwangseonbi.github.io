---
date: 2021-06-11 02:30:00
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
- name: 6b2d307a-d1d3-4823-abcc-531a73bd25af.png
  src: 6b2d307a-d1d3-4823-abcc-531a73bd25af.png
  title: ''
title: 다익스트라 최단거리 알고리즘에 대하여
weight: 0

---
> ***이 블로그는 Notion에서 랜더링 자동화를 통해 제작되었습니다.<br>Notion 페이지에 최적화되어있습니다. → [다익스트라 최단거리 알고리즘에 대하여](https://www.notion.so/hwangseonbi/6deb1329a0df4511b16eead4ac2248a5)***

<br>



{{< toc >}}

---

# 1. 다익스트라 알고리즘

다익스트라 알고리즘이 이해와 납득이 가지않아 며칠동안 앓았다. 블로그 몇개를 훑어보아서는 이해하기가 힘들었다

~~(저 같이 집중안하고 대충 공부해보려다가는 시간만 날립니다. 한번 볼때 집중해서 흐름 따라가세요)~~

나같은 사람을 위해 수학적으로 이 명제가 참이라는 것을 증명하기보다는 경험적으로 이해한 알고리즘의 흐름을 알아본다.

<br>



# 2. 아이디어

몇개의 그래프를 가지고 최단경로를 구하는 생각을해보면 경험적으로 사실이란 것을 알 수 있다.

아래 그래프상에서, A에서 B까지의 최단거리를 생각해보자. 

{{< img name="758f423b-1ee9-45e4-aba3-42af4200431e.png" size="large" width="2015" lazy=false >}}

내 머리속에서는 아래와 같은 사고의 흐름이 펼쳐진다.

1. 직접 연결된 곳 먼저 가보자        A→B : 100                                 얼라? 거리가 좀 커보이네?

1. 다른 곳 거쳐서 가보자                A→C→B : 400+2 = 402         중간에 400 때문에 더 걸리네..

1. 더 둘러가볼까                            A→C→E→B = 2+4+1=7        거리값이 적은 곳으로 돌아오니 빠르군

<br>



<br>



여기서 주목해야할 포인트는 **3번과정**이다.

<br>



3번 과정을 통해서 우리는 C보다 먼 지점(D 또는 E)를 갈때는 A→B가 아닌 A→C를 거쳐야 최소값이 될 것 같다는 생각이 든다. 실제로도 그렇다. 다익스트라 형님의 아이디어가 바로 이 지점인 것이다.

<br>



```
✅__다익스트라 알고리즘의 핵심__
: 특정 지점에서 목표 지점까지의 최단 거리는 목표 지점과 인접한 지점까지의 최단 거리로 이루어진다.
```

<br>



E지점을 최단거리로 가려면 E와 인접한 노드인 B, C, D 중 한곳을 거쳐와야할 것이다.

그렇다면 A→E 최단거리는 A→B의 최단거리 또는 A→C의 최단거리 또는 A→D의 최단거리를 통해서 얻을 수 있는 것이다.

<br>



<br>



# 3. 알고리즘

위 사고를 조금 더 체계화시킨 알고리즘 형태로 보자.

<br>



1) A를 시작노드로 설정한다. 현재노드는 A이다. A와 인접한 노드의 간선을 계산한다.

{{< img name="a73ec48a-9471-44b0-832b-051180fa6d0d.png" size="large" width="4000" lazy=false >}}

A는 시작노드이며 자기 자신으로 가는 거리는 0이다. A에서 D나 E로 가는 거리는 아직 모른다.

<br>



2) 다음 방문할 노드를 선택한다. 선택하는 기준은 현재 노드 A와 연결된 노드 중, **아직 방문하지 않은 노드이면서 가장 가까운 노드**이다. A와 인접한 노드 중에서 가까운 곳은 C이다. C가 현재노드가 된다.

<br>



<br>



3) C노드는 D, E와 연결되어있다. 그리고 우리는 A에서 C까지의 거리를 알고 있다. 그러므로 지금까지 몰랐던 A에서 D,E의 거리를 알아낼 수 있다. 즉 C를 고려했을 때, A→D,E 최단거리를 구할 수 있다.

{{< img name="d3d3d85f-ea4e-4177-9996-354f4add49c5.png" size="large" width="2478" lazy=false >}}

<br>



4) 다시 C에서 연결된 노드 중, **아직 방문하지 않은 노드이면서 가장 가까운 노드**를 선택한다. 이번엔 D가 될 것이다.

이런 식으로 3번과정을 반복한다.

<br>



# 4. 일반화

다시 일반화 시켜보면 아래와 같이 정리 가능하다.

1. 출발 노드 설정

1. 출발 노드를 기준으로 각 노드의 최소 비용을 저장

1. 방문하지 않은 노드 중에서 가장 비용이 적은 노드 선택

1. 해당 노드를 거쳐서 특정한 노드로 가는 경우를 고려하여 최소 비용 갱신

1. 위 과정에서 3번 ~ 4번을 반복

```
⚠️[미리알림]
코드 구현 시, 3번 과정에서 비용이 가장 적은 노드를 선택할 때 그냥 배열을 돌면서 찾을 수도 있다.
하지만 우선순위 큐로 구현한다면 더 빠르게 선택할 수 있다.
```

<br>



# 5. 구현

그래프가 아래와 같이 주어졌을 때

{{< highlight Python "linenos=table" >}}
graph1 = {
    'A': {'B': 8, 'C': 1, 'D': 2},
    'B': {},
    'C': {'B': 5, 'D': 2},
    'D': {'E': 3, 'F': 5},
    'E': {'F': 1},
    'F': {'A': 5}
}
graph2 = {
    'A': {'B': 100, 'C': 2},
    'B': {'C': 400, 'E': 1},
    'C': {'E': 4, 'D': 2},
    'D': {'E': 3},
    'E': {}
}
{{< /highlight >}}

<br>



다음 노드를 선택하는 방식을 heapq로 사용하면 알고리즘은 아래와 같다.

{{< highlight Python "linenos=table" >}}
import heapq
def dijkstra_using_heapq(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = []

    heapq.heappush(queue, (distances[start], start))
    while queue:
        current_distance, current_node = heapq.heappop(queue)

        if distances[current_node] < current_distance:
            continue

        for adj_node, d in graph[current_node].items():
            new_distance = current_distance + d
            if new_distance < distances[adj_node]:
                distances[adj_node] = new_distance
                heapq.heappush(queue, (new_distance, adj_node))
    return distances
{{< /highlight >}}

<br>



다음 노드를 선택하는 방식을 일반 리스트로 사용하면 아래와 같다.

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



<br>



<br>



# 6. 전체 통합 소스 및 실행 결과

{{< highlight Python "linenos=table" >}}
import heapq

graph1 = {
    'A': {'B': 8, 'C': 1, 'D': 2},
    'B': {},
    'C': {'B': 5, 'D': 2},
    'D': {'E': 3, 'F': 5},
    'E': {'F': 1},
    'F': {'A': 5}
}
graph2 = {
    'A': {'B': 100, 'C': 2},
    'B': {'C': 400, 'E': 1},
    'C': {'E': 4, 'D': 2},
    'D': {'E': 3},
    'E': {}
}


def dijkstra_using_heapq(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = []

    heapq.heappush(queue, (distances[start], start))
    while queue:
        current_distance, current_node = heapq.heappop(queue)

        if distances[current_node] < current_distance:
            continue

        for adj_node, d in graph[current_node].items():
            new_distance = current_distance + d
            if new_distance < distances[adj_node]:
                distances[adj_node] = new_distance
                heapq.heappush(queue, (new_distance, adj_node))
    return distances


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


print("--graph1--")
print(graph1)
print("---------------------")
print(dijkstra_using_heapq(graph1, "A"))
print(dijkstra_using_adj(graph1, "A"))

print("\n\n")

print("--graph2--")
print(graph2)
print("---------------------")
print(dijkstra_using_heapq(graph2, "A"))
print(dijkstra_using_adj(graph2, "A"))
{{< /highlight >}}

<br>



실행 결과

{{< img name="6b2d307a-d1d3-4823-abcc-531a73bd25af.png" size="large" width="1662" lazy=false >}}

<br>



<br>



<br>



<br>



<br>

