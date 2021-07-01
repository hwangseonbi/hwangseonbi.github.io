---
date: 2021-06-30 15:11:00
resources:
- name: ddf73190-2e53-405c-bd39-3d5dec372df1.png
  src: ddf73190-2e53-405c-bd39-3d5dec372df1.png
  title: ''
- name: 9217bc4e-550f-4988-9771-f573cbf7df95.png
  src: 9217bc4e-550f-4988-9771-f573cbf7df95.png
  title: ''
- name: 4e8598f5-e4e3-4012-a5fa-014b34a99a5d.png
  src: 4e8598f5-e4e3-4012-a5fa-014b34a99a5d.png
  title: ''
title: Python에서 GIL이란?
weight: 6

---
# GIL이란?

<br>



GIL이 무엇인지 설명하기 전에 Python으로 멀티스레딩과 일반적인 경우의 시간을 비교해본다. 시스템환경은 아래와 같다. 

{{< img name="ddf73190-2e53-405c-bd39-3d5dec372df1.png" size="large" width="614" lazy=false >}}

<br>



아래 코드는 5억개의 배열에 랜덤값을 집어넣고 최대값을 찾는 작업을 하나의 스레드와 두개의 스레드일 때로 나누어 실행하고 시간을 측정한다.

{{< highlight Python "linenos=table" >}}
import random
import threading
import time


def working():
    max([random.random() for i in range(100000000)])


# One Thread
s_time = time.time()
working()
working()
e_time = time.time()
print(f'[One Thread] {e_time - s_time:.5f}')


# Two Threads
s_time = time.time()
threads = []
for i in range(2):
    threads.append(threading.Thread(target=working))
    threads[-1].start()

for t in threads:
    t.join()

e_time = time.time()
print(f'[Two Thread] {e_time - s_time:.5f}')
{{< /highlight >}}

<br>



당연히 멀티스레드일때 시간이 더 적게 걸렸을 것 같지만 실제로는 정반대의 결과다. 바로 GIL 때문이다.

{{< img name="9217bc4e-550f-4988-9771-f573cbf7df95.png" size="small" width="366" lazy=false >}}

<br>



GIL(Global Interpreter Lock)은 간단히 말해 뮤텍스로, 하나의 쓰레드만이 파이썬 인터프리터를 컨트롤할 수 있게 해주는 락이다. 쉽게말해 파이썬(Cython)에서는 스레드가 여러개 만들어두어도 바이트코드를 실행시킬 수 있는 스레드는 단 하나여야하는 정책이 걸려있고 이러한 정책을 구현해주는 것이 GIL이다.

<br>



아래 그림에서 스레드는 세개지만 Lock이 걸리면서 동시간대에 바이트코드는 하나의 스레드만 실행할 수 있다. 물론 thread context switching까지 생각하면 싱글스레드보다 시간이 오래 걸리는 문제가 발생한다.

{{< img name="4e8598f5-e4e3-4012-a5fa-014b34a99a5d.png" size="large" width="1418" lazy=false >}}

<br>



### 그렇다면 파이썬은 왜 굳이 이러한 정책을 가지는 것일까?

Python은 Garbage Collection과 Reference Counting을 통해 할당된 메모리를 관리한다. 따라서 파이썬의 모든 객체는 reference count, 즉 해당 변수에 참조된 수를 저장하고 있다. 여기서 문제가 발생한다. 멀티스레드인 경우 여러 스레드가 하나의 객체를 사용한다면 reference count를 관리하기 위해서 모든 객체에 대한 lock이 필요하다. 이러한 비효율을 막기 위해서 Python에서는 GIL을 사용하게 되었다. 하나의 Lock을 통해서 모든 객체들에 대한 Reference Count의 동기화 문제를 해결했다.

<br>



### 그렇다고 Python의 멀티스레딩이 무조건 느린 것은 아니다.

아래 코드에서는 멀티스레드 동작이 더 빠르다.

{{< highlight Python "linenos=table" >}}
import random
import threading
import time


def working():
    time.sleep(0.1)
    max([random.random() for i in range(10000000)])
    time.sleep(0.1)
    max([random.random() for i in range(10000000)])
    time.sleep(0.1)
    max([random.random() for i in range(10000000)])
    time.sleep(0.1)
    max([random.random() for i in range(10000000)])
    time.sleep(0.1)
    max([random.random() for i in range(10000000)])
    time.sleep(0.1)


# One Thread
s_time = time.time()
working()
working()
e_time = time.time()
print(f'{e_time - s_time:.5f}')


# Two Threads
s_time = time.time()
threads = []
for i in range(2):
    threads.append(threading.Thread(target=working))
    threads[-1].start()

for t in threads:
    t.join()

e_time = time.time()
print(f'{e_time - s_time:.5f}')
{{< /highlight >}}

그 이유는 `sleep` 때문이다. 싱글스레드 작업에서는 sleep으로 인해 그대로 0.1초씩 쉬게된다. 그러나 멀티스레드에서는 sleep으로 멈춘 경우 다른 스레드로 context switching을 하기때문에 효율이 개선된다. 즉 하나의 스레드에서 sleep으로 멈추었을 때 다른 스레드가 동작하는 것이다.

여기서는 sleep으로 대기상황을 재연했지만 I/O작업이 많은 경우도 이와 동일한 상황으로 볼 수 있다. I/O작업이 길고 빈번하다면 멀티스레드로 수행하는 것이 더 빠르다는 일반적인 결론을 내릴 수 있다.

<br>



# Reference

- [https://ssungkang.tistory.com/entry/python-GIL-Global-interpreter-Lock은-무엇일까](https://ssungkang.tistory.com/entry/python-GIL-Global-interpreter-Lock%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)