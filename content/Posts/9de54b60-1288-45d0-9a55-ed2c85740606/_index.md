---
date: 2021-07-19 06:57:00
resources:
- name: af2a2436-560b-4e7a-9b4f-3839b2e2055b.png
  src: af2a2436-560b-4e7a-9b4f-3839b2e2055b.png
  title: ''
- name: a434a817-becb-4589-afd5-d48354d62051.png
  src: a434a817-becb-4589-afd5-d48354d62051.png
  title: ''
- name: 48a1a328-2146-44b6-a137-f0fcc9c7414d.png
  src: 48a1a328-2146-44b6-a137-f0fcc9c7414d.png
  title: ''
- name: 31539c06-c10b-4a47-b140-0874d2a4c642.png
  src: 31539c06-c10b-4a47-b140-0874d2a4c642.png
  title: ''
title: Introduce To Database 스터디 회고록
weight: 8

---
> ***이 블로그는 Notion에서 랜더링 자동화를 통해 제작되었습니다.<br>Notion 페이지에 최적화되어있습니다. → [Notion에서 보기](https://www.notion.so/hwangseonbi/Introduce-To-Database-9de54b60128845d09a55ed2c85740606)***

---

<br>



{{< toc >}}

<br>



> ***_약 2개월 간의 Database 스터디가 끝났다. 이 스터디를 시작한 이유가 무엇인지, 무엇을 배웠는지 후기를 남겨보려한다._***

<br>



# 1. Database 스터디를 시작한 이유

컴퓨터공학과 학부시절, Database를 공부한다는 것이 정확히 어떤 것을 공부를 하는 것인지 몰랐다. 무엇보다 Database 공부의 필요성을 못느꼈다. 그 필요는 회사에서 개발을 시작하고나서야 느꼈다. 그 전까지는 필요를 느낄만한 경험이 부족했던 것 같다. 

<br>



**`경험을 통해 필요성을 느끼는 것이 무엇보다 큰 학습동기가 된다.`** 

<br>



Database 기초 지식에 대한 필요성을 처음 느낀 때는 회사에서 DynamoDB를 사용하는 프로젝트를 할 때였다. 서문이 길어질 것 같아 자세한 내용은 가장 문서 말미에 써야겠다.

<br>



일단 개발자 커뮤니티에서 Database 기초 스터디가 있는지 찾아봤다. 스터디 모집글의 거의 95%는 Spring, 알고리즘, 프론트앤드 프레임워크 글이었다. 그래서 직접 모집을 시작했다.

<br>



# 2. 모집

직접 모집을 하는 것의 좋은 점은 커리큘럼과 일정 등을 내 마음대로 결정할 수 있다는 점이다. 모집글은 최대한 자세하게 적었다. 아무 계획없이 사람만 모았다가 스터디가 산으로 가거나 그대로 해산하는 경우도 종종 목격했기 때문이다.

<br>



소개, 모임 시간, 인원, 장소 그리고 무엇보다 스터디 방식과 계획을 자세하게 적었다. 아래는 스터디 모집글의 일부다.

{{< img name="af2a2436-560b-4e7a-9b4f-3839b2e2055b.png" size="large" width="1624" lazy=false >}}

{{< img name="a434a817-becb-4589-afd5-d48354d62051.png" size="large" width="1290" lazy=false >}}

<br>



<br>



# 3. 후기

스터디는 성공적으로 완료했다 👍 중간에 이탈하는 인원도 없었고 모두 끝까지 함께 해주셨다.

나름 주최자라서그런지 가장 열심히 해야겠다는 책임감이 들었다. 그래서 모든 챕터를 문서로 남기려고 했다. 이렇게 했을 때 좋은 점은 발표자분께서 갑작스럽게 일이 생기더라도 내가 발표를 하고(공부도 더 된다) 스터디를 계속 진행할 수 있다는 점이다. 참여자분들은 다들 직장에 재직중이셨기 때문에 부담을 최대한 덜어드리려고 노력했다. 물론 내가 쉬고있는 상태라서 가능했다.

그럼에도 불구하고 어쩔 수 없이 뛰어넘은 주차가 1~2회 정도 있었다. 여기서 느낀 것은 스터디가 한 주라도 미뤄지면 텐션이 떨어진다는 것이다. 특히 비대면 스터디라 더욱 크게 느껴졌다. 다음에도 이런 방식의 스터디를 한다면 차라리 발표자를 대체하고 강행하는 것이 나을 것 같다고 생각했다.

<br>



참고로 강의는 그리 만족스럽지 않았다. 강사 본인의 유튜브채널 컨텐츠를 그대로 커리큘럼에 넣어놓았다. 그리고 정적인 영상을 띄워놓고 말만 주구장창해서 집중도와 이해도가 떨어진다.

그래서 **`별한개`** 를 주었다 ^^

<br>



# 4. 배운 것들

ACID

1. AICD란?

1. Isolation 레벨 별로 다른 트랜잭션 동작과 그에 따라 발생할 수 있는 문제들

<br>



Indexing

1. 인덱싱이란?

1. Select 쿼리 시 내부에서 일어나는 일

1. Index Scan과 Index Only Scan의 차이

1. Where 조건에 따른 퍼포먼스 차이

1. Bitmap Index Scan vs Index Scan vs Table Scan

1. Bloom filters

<br>



Database Partitioning

1. Database Partitioning 이란?

1. Partitioning : Vertical vs Horizontal

<br>



Database Sharding

1. Database Sharding 이란?

<br>



Concurrency Control

1. Lock이란?

1. Shared Locks vs Exclusive Lock

1. Two-Phase Locking

<br>



Database Replication

1. Database Replication 이란?

1. 발생할 수 있는 동기화문제

1. 동기화문제 해결 방안

	- Master-Backup vs Multi-Master Replication

	- Synchronous vs Asynchronous Replication

		<br>

		

Row vs Column Oriented Database

1. Row vs Column Oriented Database란 무엇이고 각각 어떤 용도로 사용하는가?

<br>



여러가지 DB 엔진

1. B-Tree Database Engine에는 어떤 것들이 있는지?

1. LSM Database Engine에는 어떤 것들이 있는지?

<br>



Database Cursor

1. Database Cursor란?

1. Server Side vs Client Side Database Cursors

<br>



# 5. 회고

머릿말에서 Database 기초 지식의 필요성을 회사에서 처음 느꼈다고 했다. 회사에서 있었던 일은 아래와 같다.

<br>



## 5.1 Database를 공부해야겠다고 느꼈던 순간

회사에서 **`DynamoDB`** 를 사용하는 프로젝트를 할 때가 DB 기초 지식에 대한 필요성을 처음 느낀 때였다. 당시 Java 웹 어플리케이션 유지보수를 맡았었다. DB로는 AWS의 DynamoDB를 사용하고 있었다. 어느날 검색기능과 선택 항목 삭제 기능을 추가해야하는 상황이 생겼다. 정말 단순한 일이라고 생각했다.

<br>



> ***DynamoDB에 필터를 걸어 find만 해주면 되겠구나. 그리고 Key 값으로 항목을 삭제하면 되겠구나.***

<br>



그런데 왠걸? 검색기능이 문제였다. 검색 결과 반환 시간이 약 **`몇십초`** 가 걸렸다. 게다가 단 한번의 검색에 DynamoDB 컴퓨팅 리소스가 미친듯이 치솟았다. 이는 곧 비용 증가를 의미했다. 그때부터 DynamoDB 도큐먼트를 샅샅이 읽기 시작했다. **`범인은 1차원적이게도 DynamoDB 자체였다.`**  DynamoDB는 NoSQL기반 DB로써 단순 Primary Key로의 접근은 빠르지만 적절한 설계 없이는 검색 성능이 매우 떨어지는 특징을 가지고 있기 때문이었다.

검색이 아예 안되는 것은 아니었다. DynamoDB에서는 **`Partition Key`** 와 **`Sort Key`** 라는 개념이 있다. 이 두가지 Key를 가지고 사용할 수 있는 유즈케이스는 아래와 같다.

<br>



하나의 필드를 Partition Key로 설정

- 이 경우, Partition Key는 Unique한 값으로 이루어져 있어야함.

- partition_key로 쿼리 시, Item 반환 (빠름)

<br>



하나의 필드를 Partition Key로 설정하고 다른 하나의 필드를 Sort Key로 설정

- 이 경우 Partition Key는 중복된 값으로 이루어져 있어도 됨.

- partition_key와 sort_key로 쿼리 시, sort_key 필드로 정렬되어있는 Item 배열 반환 (빠름)

<br>



이제 문제가 되는 상황을 보자. 예를들어, 아래 **`GameScores`**  테이블에서 Partition Key는 **`UserID`** 이고 Sort Key는 **`GameTitle`** 이다.

{{< img name="48a1a328-2146-44b6-a137-f0fcc9c7414d.png" size="large" width="1210" lazy=false >}}

Partition Key와 Sort Key를 사용한 쿼리는 빠르다.

- "**`UserId`**  102번의 **`GameTitle`** 이 Starship X인 row를 줘"

<br>



그러나 그 외의 경우에는 매우 느리므로(전체 스캔 필요) 인덱싱을 사용해야한다.

- "**`GameTitle`** 이 Starship X인 것들 중에서 **`TopScore`** 가 가장 높은 row를 줘"

<br>



**`문제는 필드가 많고 여러 필드를 사용한 검색이 발생할 때이다.`**  

<br>



아래 쿼리 케이스를 전체 스캔없이 반환하려면 인덱스가 필요하다.

- "**`GameTitle`** 이 Starship X인 것들 중에서 **`TopScore`** 가 가장 높은 row를 줘" → Partition Key가 **`GameTitle`** 이고 Sort Key가 **`TopScore`** 인 인덱스 필요


{{< img name="31539c06-c10b-4a47-b140-0874d2a4c642.png" size="small" width="240" lazy=false >}}

- "**`wins`** 가 1인 유저중에 가장 높은 **`TopScore`** 를 가진 row를 줘" → Partition Key가 **`wins`** 이고 Sort Key가 **`TopScore`** 인 인덱스 필요

- ...

- **`필드가 더 많고 여러 조건이 겹친다면?`**  😱

<br>



검색조건이 복잡해질때 전체 스캔을 피하려면 수많은 인덱스를 생성해야하는 상황이 발생하는 것이다. 결국 DynamoDB를 고수한다면 검색조건에 제약이 생기거나 검색조건에 따라 데이터 반환 시간은 다르며 최악의 조건일 경우, 테이블 전체 스캔으로 몇십초 단위의 검색이 이루어진다.

<br>



**`비용 폭탄은 덤이었다.`** 

<br>



## 5.2 해결 방법

이 서비스의 본질은 캐싱이다. 다시말해 클라이언트 측에서 이 서비스를 사용하는 이유는 **`속도`** 다. DynamoDB는 규모에 상관없이 짧은 지연시간을 보장하는 Key-Value DB라고 알려져있다. 즉 DynamoDB를 다른 Database로 교체하려면 어플리케이션의 유즈케이스에서 DynamoDB보다 빠른지 또는 느린지 체크해야하며 느릴경우 느린 정도가 합리적일지, 문제는 없는지 체크해야한다. 그리고 비용도 비교해봐야한다. 고려해야할 사항이 많았다. 검색기능 지원 하나를 위해서 함부로, 쉽게 DynamoDB를 다른 Database로 바꿀 수는 없었다.

<br>



내부 논의 끝에 결국 DB를 갈아치우진 못하였다. 그 대신 DynamoDB **`Stream`** 기능을 사용하기로 했다. DynamoDB로 들어오는 Insert, Delete, Update 등의 Event를 Lambda로 받고 DocumentDB로 미러링 시켰다. 그렇게되면 검색은 DocumentDB에서 담당하게 되는데 속도가 나름 합리적이었다.

<br>



검색기능 하나 때문에 DocumentDB를 사용하는 것은 낭비이긴 하지만 사내 일정 사정 상, 근본적인 해결은 Next Todo List로 남겼다.

**`게다가 DocumentDB는 클러스터 형태라서 인스턴스가 3개 이상으로 돌아간다는건 안비밀`** 

<br>



## 5.3 다시 그때로 돌아간다면?

지금에 와서도 딱히 더 좋은 다른 해결책이 생각나진 않는다. 그러나 스터디에서 배운 개념들에 비추어 더 넓게 생각해볼 수 있을 것 같다. 서비스에서 고려해야할 부분뿐만 아니라 개인적인 호기심에서 비롯된 궁금증이 더 많아지고 더 많이 찾아봤을 것 같다. 

<br>



- 우리 서비스에서 트랜잭션이 필요한 구간은 없는지? DynamoDB는 트랜잭션을 어떻게 구현할 수 있는지?

- DynamoDB에서 Isolation level을 설정할 수 있는지? 그렇게 해서 일관성을 얻을 수 있는지? 그러한 일관성이 우리 서비스에서 필요한지? 아니면 일관성을 일부 포기하고 퍼포먼스를 선택하는게 더 좋을지?

- Postgresql에서는 특정 row에 접근해 attributes를 가져올 때 Heap에서 random access로 가져올텐데 DynamoDB에서는 Partition Key를 통해 특정 row에 접근 후 다른 attributes를 불러올 때 어떻게 가져오는지?

- DynamoDB에서 Partition Key로 쿼리를 하면 Hash함수를 통해 특정 값에 도달할 수 있다는데 Hash Collision은 없는지? 당연히 없게 만들어져있겠지만 어떤식으로 피하는지?

<br>



<br>



<br>

