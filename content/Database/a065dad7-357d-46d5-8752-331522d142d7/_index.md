---
date: 2021-06-07 08:50:00
resources:
- name: 74072297-96fe-4097-ab35-9a6aa8420a46.png
  src: 74072297-96fe-4097-ab35-9a6aa8420a46.png
  title: ''
- name: 99f2ab49-1425-4328-950f-e678ac602697.png
  src: 99f2ab49-1425-4328-950f-e678ac602697.png
  title: ''
- name: 71b2441f-d59e-4b2e-95b2-be2819768815.png
  src: 71b2441f-d59e-4b2e-95b2-be2819768815.png
  title: ''
- name: cce702cd-f360-4246-b93f-70bceee7f8ab.png
  src: cce702cd-f360-4246-b93f-70bceee7f8ab.png
  title: ''
- name: 2b385f00-6890-409b-b6ed-c8f35f243261.png
  src: 2b385f00-6890-409b-b6ed-c8f35f243261.png
  title: ''
- name: 3a9dd92f-e516-4bd2-8ce2-70adb0fe926d.png
  src: 3a9dd92f-e516-4bd2-8ce2-70adb0fe926d.png
  title: ''
- name: e9aa03ed-6247-4107-94c9-9a5481f71551.png
  src: e9aa03ed-6247-4107-94c9-9a5481f71551.png
  title: ''
- name: 507141fc-af4e-4b19-acd1-dd8359fd99cf.png
  src: 507141fc-af4e-4b19-acd1-dd8359fd99cf.png
  title: ''
title: '[기초] 1. ACID에 대하여'
weight: 0

---
> ***이 블로그는 Notion에서 랜더링 자동화를 통해 제작되었습니다.<br>Notion 페이지에 최적화되어있습니다. → [[기초] 1. ACID에 대하여](https://www.notion.so/hwangseonbi/ACID-a065dad7357d46d58752331522d142d7)***

<br>



<br>



{{< toc >}}

---

<br>



# ACID

ACID란? Atomicity, Consistency, Isolation, Durability를 의미하며 **`Transaction`** 이 안전하게 수행기위해 필요한 4가지 성질들을 의미한다.

<br>



**`Transaction`** 이란? 데이터베이스 내에서 하나의 기능을 수행하기 위해 행해지는 여러 쿼리의 집합이다.

<br>



{{< img name="74072297-96fe-4097-ab35-9a6aa8420a46.png" size="large" width="2611" lazy=false >}}

<br>



```
👉DB 스터디를 시작하게 된 이유는 하나의 의문에서 비롯되었다.

"은행은 DB 연산을 어떻게 안전하게 하는가?"

즉 안전한 트랜잭션이 어떻게 이루어지는가가 궁금했었다.

동시에 여러 요청을 어떻게 충돌없이 안전하게 처리하는가? 분산 DB라면? 완벽한 동기화가 가능한가? 하드웨어 레벨까지도 안전한 트랜잭션이 수립될 수 있나? 등등의 의문들을 알아가는것이 목표다.
```

<br>



<br>



### Atomicity

원자성이라고 한다. 하나의 트랜잭션은 쪼갤 수 없다. 즉 반만 성공하거나 반만 실패할 수 없다. 하나의 쿼리가 실패한다면 그 전까지 실행되던 쿼리들은 모두 롤백해야한다.

<br>



{{< img name="99f2ab49-1425-4328-950f-e678ac602697.png" size="large" width="720" lazy=false >}}

반만 성공할 수 없다. 트랜잭션 하나에 대한 성공 혹은 실패만 있을 뿐이다.

<br>



### Consistency

성공적으로 수행된 트랜잭션은 정당한 데이터들만을 데이터베이스에 반영해야 한다. 트랜잭션의 수행을 데이터베이스 상태 간의 전이(transition)로 봤을 때, 트랜잭션 수행 전후의 데이터베이스 상태는 각각 일관성이 보장되는 서로 다른 상태가 된다. 트랜잭션 수행이 보존해야 할 일관성은 기본 키, 외래 키 제약과 같은 명시적인 무결성 제약 조건들뿐만 아니라, 자금 이체 예에서 두 계좌 잔고의 합은 이체 전후가 같아야 한다는 사항과 같은 비명시적인 일관성 조건들도 있다.

<br>



**Consistency in Data**

데이터 일관성이란 데이터상으로 일관성이 유지되어야 함을 말한다. (정합성)

{{< img name="71b2441f-d59e-4b2e-95b2-be2819768815.png" size="large" width="2795" lazy=false >}}

SBS를 구독하는 유저e가 구독자 테이블에 추가되면 채널테이블에도 이 정보가 반영되어야한다.

<br>



유저는 퍼포먼스와 일관성 사이에서 조절가능하다. 예를들어 Youtube 구독자수를 나타낼 때, 1,020,364라는 정확한 수 대신 1000K로 나타내는 것이 그 예다. 어느정도의 근사값만 구해주고 퍼포먼스 상의 이득을 볼 수 있다.

<br>



<br>



**Consistency in Read**

읽기 일관성이란 데이터를 읽는 트랜잭션의 결과가 일관되어야한다는 것이다.

즉 읽기 일관성이 없다면 업데이트 트랜잭션이 성공적으로 커밋되었음에도 읽기 트랜잭션이 과거값을 읽어가게된다. 이러한 상황은 DB서버를 확장했을 때 발생한다.

Master DB서비의 값은 업데이트되었으나 Slave DB서버에서는 네트워크 지연 등의 이유로 아직 업데이트가 안되었을 때 old value를 읽게 되는 경우가 발생한다.

{{< img name="cce702cd-f360-4246-b93f-70bceee7f8ab.png" size="small" width="432" lazy=false >}}

Eventual consistency란 단기적으로는 일관성을 잃더라도 결국에는 일관성을 유지하는 모델을 뜻한다.

```
💡후에 다루어질 내용일 것 같다.

데이터베이스 Lock과 관련하여, RDBMS에서는 정해진 레벨링이 되어있기때문에 row, column에 Lock을 건다면, 하위레벨을 자유롭게 저장하는 Nosql에서는?
```

<br>



### Isolation

Isolation이란 하나의 트랜잭션이 다른 트랜잭션과 격리되어 서로의 내용을 참조할 수 없는 성질이다. 즉 Isolation성질이 지켜진다면 트랜잭션 중간에는 다른 트랜잭션의 영향을 받지 않아야한다.

그러나 **`Isolation level`** 에 따라 (동시성에 따른)성능과 **`Read Phenomena`** 를 조율할 수 있다. Read phenomena란 데이터베이스 읽기 이상현상정도로 이해하면 된다.

{{< img name="2b385f00-6890-409b-b6ed-c8f35f243261.png" size="large" width="624" lazy=false >}}

<br>



Isolation 수준은 위 표에서와 같이 4가지가 있다. 

Read Uncommitted → Read Committed → Repeatable Read → Serializable 순으로 갈수록 Isolation 정도가 강해진다. Isolation 정도가 강해질수록 동시성은 낮아지며 이에 따른 성능저하가 발생한다. 반대로 데이터 정합성은 더 견고해져 Read Phenomena가 줄어들게된다.

<br>



{{< expand "▼ Isolation Level 모아보기">}}

### Read uncommitted

Isolation이 없는 수준이다. Transaction의 중간내용을 다른 Transaction에서 참조할 수 있다.

```
👉읽기전용으로만 사용해야 한다.
```

- 발생가능한 Read Phenomena : Dirty reads, Lost updates, Non-repetable read, Phantom read

### __Read committed (Default)__

Transaction 내 각각의 쿼리는 다른 Transaction의 Commit완료된 결과만 참조할 수 있다.

- 발생가능한 Read Phenomena : Lost updates, Non-repetable read, Phantom read

### __Repeatable Read__

Transaction 중간중간 Update Undo 상태를 백업해둠으로써 서로 다른 트랜잭션 사이에 업데이트 영향을 받지 않게한다. [→ 자세히](https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation)

- 발생가능한 Read Phenomena : Phantom read

<br>



### __Serializable__

모든 트랜잭션이 직렬화된다. 트랜잭션에서 읽고 쓰는 레코드는 다른 트랜잭션에서 절대 접근 불가능하므로 정합성이 유지된다. 하지만 동시성이 낮아져 성능이 감소한다.

발생가능한 Read Phenomena : -

{{< /expand >}}

<br>



{{< expand "▼ Read phenomena 모아보기">}}

### Dirty reads

커밋되지 않은 트랜잭션의 데이터를 다른 트랜잭션이 읽을 수 있다.

{{< img name="3a9dd92f-e516-4bd2-8ce2-70adb0fe926d.png" size="small" width="432" lazy=false >}}

<br>



<br>



### Lost updates & Non-repeatable read

값을 업데이트하는 트랜잭션이 다른 트랜잭션의 업데이트 작업에 의해 적용되지 않는다(Lost Update).

이 영향으로 하나의 트랜잭션에서 같은 쿼리를 두번이상 수행할때, 똑같은 쿼리임에도 다른 결과가 나오는 현상(Non-repeatable read)

{{< img name="e9aa03ed-6247-4107-94c9-9a5481f71551.png" size="large" width="1724" lazy=false >}}

<br>



### Phantom reads

하나의 트랜잭션에서 일정 범위의 레코드를 두번 이상 읽을 때, 똑같은 쿼리임에도 없던 레코드가 두번째 쿼리에서 나타나는 현상

{{< img name="507141fc-af4e-4b19-acd1-dd8359fd99cf.png" size="small" width="384" lazy=false >}}

<br>



{{< /expand >}}

<br>



<br>



### Durability

성공한 트랜잭션은 영구적으로 결과에 반영되어야하는 성질이다. 비활성 스토리지에 커밋 로그를 저장함으로서 구현할 수 있다.

예를 들어서 항공예약시스템에서 예약이 성공적으로 완료되었다면 시스템이 크래쉬가 나더라도 예약은 유지되야한다.

<br>



```
👉분산트랜잭션에서는 묶여있는 모든 서버가 커밋이 완료되기 전에 조율해야한다. 통상적으로 이는 two-phase commit protocol 에 의해서 행해진다.
```

<br>



---

# 더 읽어보기

- [DBMS는 어떻게 트랜잭션을 관리할까? - Naver D2](https://d2.naver.com/helloworld/407507)

<br>



# 출처

- [Introduction to Database Engineering](https://www.udemy.com/course/database-engines-crash-course/)