---
title: "프로그래머스 문제풀이를 도와줄 파이썬 템플릿"
date: "2021-05-01"
---

로컬에서 조금 더 편하게 알고리즘을 풀어보기위해 [프로그래머스](https://programmers.co.kr/) 알고리즘 문제의 기본 틀부터 만들었다.
<br><br>

## 템플릿
{{< highlight Python "linenos=table" >}}
"""
분류 : 
문제명 : 
URL : 
"""

import pprint

def solution(board, moves):
    answer = 0
    return answer

pp = pprint.PrettyPrinter(indent=4)
TEST_CASES = [
    {
        "board": None,
        "moves": None,
        "result": None
    }
]

def TEST():
    for i, test_case in enumerate(TEST_CASES):
        test_case_str = "#%d\n%s" % (i, pprint.pformat(test_case))
        print(test_case_str)

        answer = solution(test_case["board"], test_case["moves"])

        if answer is test_case["result"]:
            print("ok")
        else:
            print("not ok(%s/%s)" % (answer, test_case["result"]))
        print("\n\n")

TEST()
{{< /highlight >}}


<br>

## 예시
{{< highlight Python "linenos=table" >}}
"""
분류 : 코딩테스트 연습 / 2019 카카오 개발자 겨울 인턴십
문제명 : 크레인 인형뽑기 게임
URL : https://programmers.co.kr/learn/courses/30/lessons/64061
"""

import pprint # 디버깅용

def solution(board, moves):
    answer = 0
    return answer

pp = pprint.PrettyPrinter(indent=4)
TEST_CASES = [
    {
        "board": [[0, 0, 0, 0, 0], [0, 0, 1, 0, 3], [0, 2, 5, 0, 1], [4, 2, 4, 4, 2], [3, 5, 1, 3, 1]],
        "moves": [1, 5, 3, 5, 1, 2, 1, 4],
        "result": 4
    }
]

def TEST():
    for i, test_case in enumerate(TEST_CASES):
        test_case_str = "#%d\n%s" % (i, pprint.pformat(test_case))
        print(test_case_str)

        answer = solution(test_case["board"], test_case["moves"])

        if answer is test_case["result"]:
            print("ok")
        else:
            print("not ok(%s/%s)" % (answer, test_case["result"]))
        print("\n\n")

TEST()
{{< /highlight >}}
