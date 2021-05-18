---
title: "[프로그래머스 연습문제 풀이] 2019 카카오 개발자 겨울 인턴십 - 크레인 인형뽑기 게임"
date: 2021-05-12T12:15:42+09:00
resources:
  - name: Untitled.png
    src: "Untitled.png"
    title: 
  - name: Untitled1.png
    src: "Untitled1.png"
    title: 
    

---

{{< hint ok >}}
[{{< icon "gdoc_link" >}} 코딩테스트 연습 - 크레인 인형뽑기 게임](https://programmers.co.kr/learn/courses/30/lessons/64061)

{{< /hint >}}


<br>


# 1. 문제의 의도

- 문제 파악 능력
- 기본적인 배열의 사용
- 논리적으로 배열 사용할 수 있는지 확인
- 기본적인 문제로서 문제를 정확히 파악하고 배열을 논리적으로 사고할 수 없으면 시간이 소요되어 다른 문제를 풀 수 없음

<br><br>

# 2. 문제 파악을 잘하자. 주어진 테스트 케이스도 꼼꼼히 확인하자.

일단 이 문제를 풀 때 시간이 오래 걸린 지점은 문제파악이었다. 배열을 그려놓고 예제를 잘 읽어보지 않아서 아래와 같이 착각했다. 테스트케이스의 결과는 맞았지만 채점해보니 전부 틀렸다. 예제를 잘 보니 그냥 배열 그림 보이는대로 판이 만들어지는 거였다.

{{< img name="Untitled.png" size="small" lazy=false >}}


왜 이렇게 생각했을까..
{{< img name="Untitled1.png" size="small" lazy=false >}}


이렇게 보는게 맞다.

이 문제의 핵심은 배열의 rotate라고 생각했다.

```python
board = [[0, 0, 0, 0, 0],
[0, 0, 1, 0, 3],
[0, 2, 5, 0, 1],
[4, 2, 4, 4, 2],
[3, 5, 1, 3, 1]],
```

move의 1이라함은 board의 row를 뜻하므로 위 배열에서는 board[0][1]을 빼야한다. 

그런데 board를 rotate하면 훨씬 쉬워진다.

```python
[   [3, 4, 0, 0, 0],
    [5, 2, 2, 0, 0],
    [1, 4, 5, 1, 0],
    [3, 4, 0, 0, 0],
    [1, 2, 1, 3, 0]]
```

이렇게 만들어 놓으면 board의 move에 해당하는 열의 가장 마지막 index를 뺴주기만하면 된다.
</br>
</br>
</br>
</br>


{{< tabs "정답" >}}
{{< tab "1차 풀이" >}}

```python
def rotated(board):
    list_of_tuples = zip(*board[::-1])
    return [list(elem) for elem in list_of_tuples]

def reduce_space(board):
    for i, line in enumerate(board):
        board[i] = list(filter(lambda x: x != 0, line))

def solution(board, moves):
    answer = 0
    bascket = []
    board = rotated(board)
    reduce_space(board)
    for move in moves:
        if len(board[move-1]) > 0:
            item = board[move-1].pop()
            if item == 0:
                continue
            if len(bascket) > 0 and bascket[-1] == item:
                bascket.pop()
                answer = answer+2
            else:
                bascket.append(item)

    return answer
```
{{< /tab >}}
{{< tab "다른 유저들의 정답" >}}
```python
def solution(board, moves):
    stacklist = []
    answer = 0

    for i in moves:
        for j in range(len(board)):
            if board[j][i-1] != 0:
                stacklist.append(board[j][i-1]) #----(1)
                board[j][i-1] = 0

                if len(stacklist) > 1:
                    if stacklist[-1] == stacklist[-2]:
                        stacklist.pop(-1)
                        stacklist.pop(-1)
                        answer += 2     
                break
    return answer
```
(1) 정사각형이란 조건이 있으므로 이를 이용해도 된다.

가독성도 좋지만 이런 간단한 문제는 저런식으로 배열 원소 접근만 잘하면 간결하게 끝낼 수 있을 것 같다.
{{< /tab >}}
{{< /tabs >}}





