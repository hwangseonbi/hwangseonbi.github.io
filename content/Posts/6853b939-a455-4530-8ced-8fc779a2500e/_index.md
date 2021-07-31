---
date: 2021-07-30 09:30:00
resources:
- name: ff72eb95-786c-42da-87a6-95285df0e5dc.png
  src: ff72eb95-786c-42da-87a6-95285df0e5dc.png
  title: ''
- name: 7754f4e3-87f1-43ec-b5b1-bd5988f79212.png
  src: 7754f4e3-87f1-43ec-b5b1-bd5988f79212.png
  title: ''
- name: 4abe62cf-eeea-479e-9988-0dc0d544fe6c.png
  src: 4abe62cf-eeea-479e-9988-0dc0d544fe6c.png
  title: ''
- name: b17238ac-cdaf-4aa8-b0a7-e1222f43246c.png
  src: b17238ac-cdaf-4aa8-b0a7-e1222f43246c.png
  title: ''
- name: 11e8aeda-4b64-4a33-99d6-29e60f8a6576.png
  src: 11e8aeda-4b64-4a33-99d6-29e60f8a6576.png
  title: ''
- name: 1bfa7958-c267-4e28-b389-686dd6f7494c.png
  src: 1bfa7958-c267-4e28-b389-686dd6f7494c.png
  title: ''
- name: 78de027b-e494-4c4f-9bce-5d25975d47d3.png
  src: 78de027b-e494-4c4f-9bce-5d25975d47d3.png
  title: ''
- name: dc0cf4ce-42e2-444c-a484-faf2092f7517.png
  src: dc0cf4ce-42e2-444c-a484-faf2092f7517.png
  title: ''
- name: f2cfb431-5428-4c8b-a695-c370fbfb5377.png
  src: f2cfb431-5428-4c8b-a695-c370fbfb5377.png
  title: ''
- name: 6c095d28-bfdf-4b79-89c5-96908f66495a.png
  src: 6c095d28-bfdf-4b79-89c5-96908f66495a.png
  title: ''
title: Git Action으로 Notion과 Hugo 블로그 동기화하기(Geekdoc Theme)
weight: 10

---
> ***이 블로그는 Notion에서 랜더링 자동화를 통해 제작되었습니다.<br>Notion 페이지에 최적화되어있습니다. → [Notion에서 보기](/6853b939a45545308ced8fc779a2500e)***

<br>



{{< toc >}}

<br>



# 1. 무엇이 필요했는지?

지금까지 몇번이고 블로그 운영과 문서화 시도를 해왔다. 그러나 어느것하나 내가 원하는 기능을 만족시키는 제품이 없었다. 되돌아보니 지금까지 정말 여러 서비스들을 거쳐왔다.

- 네이버 블로그, 티스토리, 워드프레스, OneNotes(MS), Gitbook, Github 정적블로그 등등..

<br>



그런데 무엇하나도 완벽한 것이 없었다. 티스토리와 같은 블로그 서비스들은 테마나 UI가 너무 고정적이다. OneNotes는 에디터의 자유도가 지나치게 높다. Gitbook이 그나마 내가 원하는 바와 가장 맞았다. 그러나 Gitbook도 막상 써보니 생각지못한 곳에서 문제가 많았다. (한글문제라던가.. 등등) Github 정적블로그는 편집이 까다로웠다.

<br>



결국 나는 **`Notion`** 에 정착했다. 충분한 편집 자유도, 웹페이지 노출, 다양한 DB, 문서를 카테고리화하여 정리 등등 내가 원하는 찾던 도구에 거의 완벽했다. 특히 에디터는 완벽 이상이었다.

<br>



단 하나의 흠이라고한다면 **`Notion을 블로그로 사용하기 어렵다는 것`** 이다. Notion으로 글을 작성하고 정적블로그에 손수 옮기는 작업도 참으로 개고생인듯했다. 그래서 이런 방법을 생각했다.

<br>



**`Notion으로 문서를 편집하고 Github 정적블로그로 렌더링을 시키는 것!`** 

<br>



이 문서에서는 구현 방법에 대한 자세한 내용보다는 어떤 문제가 있었고 이를 해결하기위해 어떤 방법을 사용했는지 정도의 흐름으로 정리했다.

<br>



# 2. 정적블로그와 테마 선택하기

정적블로그 제너레이터는 여러가지가 있다. 그 중에서 **[Hugo](https://gohugo.io/)** 를 선택했다. 그리고는 **[Hugo Theme](https://themes.gohugo.io/)** 을 둘러보았다. 내가 찾는 테마의 조건은 이렇다.

- Left Navigation기능이 있고 Category Expanding을 지원해야한다.

- 최대한 깔끔

- Shortcodes(embed, Expand, image size 조절 등)도 풍부했으면 좋겠다.

- 커스터마이징도 가능해야함.

- 다크모드, 화이트모드 모두 지원했으면 좋겠다.

<br>



여러 테마를 직접 받아서 테스트해보고 최종적으로 선택한 테마는 **[hugo-geekdoc](https://themes.gohugo.io/themes/hugo-geekdoc/)** 였다.

{{< img name="ff72eb95-786c-42da-87a6-95285df0e5dc.png" size="large" width="2670" lazy=false >}}

특히 hugo-geekdoc은 다양한 Shortcuts를 지원하고있기 때문에 Notion의 다양한 종류의 Block을 표현할 수 있을 것이라고 생각했다. Notion에서는 페이지를 이루고있는 요소 하나하나(Text Type, Numbered List 등)를 Block이라는 단위로 부른다.

테마 칼라도 각자 입맛에 맞게 수정한다. footer와 header 등 각 컴포넌트도 커스터마이징이 가능하니 이 부분도 입맛에 맞게 수정한다.

<br>



나는 색을 최소한으로 사용하도록 수정했다. 그리고 footer에 댓글기능을 추가했다.

{{< img name="7754f4e3-87f1-43ec-b5b1-bd5988f79212.png" size="large" width="2568" lazy=false >}}

<br>



<br>



*참고로* **_[hugo-clinic-notes](https://themes.gohugo.io/themes/hugo-clinic-notes/)_**  *이 테마도 정말 깔끔하다. (깔끔하기만 하다.)*

<br>



# 3. Github.io에 정적블로그 생성하기

github.io에 정적블로그를 배포하는 방법은 찾아보면 매우 많다.

{{< img name="4abe62cf-eeea-479e-9988-0dc0d544fe6c.png" size="large" width="1966" lazy=false >}}

<br>



여기서는 작은 팁을 하나 공유하고자한다. 우리가 github page에 넣어놓아야 하는 것은 Hugo 빌드를 통해 나온 결과물이다. 어떤 분들은 Hugo 정적파일들과 Hugo 빌드 결과물을 다른 레포지토리에 놓는 경우도 있던데 나는 그것이 더러워보였다. 그래서 브랜치로 나누어 놓았다.

- main 브랜치 : Hugo 정적 파일

- gh-page 브랜치 : Hugo 빌드 결과물

<br>



{{< img name="b17238ac-cdaf-4aa8-b0a7-e1222f43246c.png" size="large" width="1846" lazy=false >}}

{{< img name="11e8aeda-4b64-4a33-99d6-29e60f8a6576.png" size="large" width="1886" lazy=false >}}

<br>



그리고나서 git action을 사용해 main브랜치에 커밋(푸시)이 발생되었을 때 빌드가 된 후 gh-page에 배포되게 했다. 자세한 방법은 **[actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)** 을 참고하면 된다. git action에 대한 내용은 아래에서 다시 기술하겠다.

<br>



# 4. Notion 페이지 정보 가져오기

이제 할 일은 Notion 페이지의 Block에 접근해야 하는 것이다. [Notion](https://developers.notion.com/)에서 공식적으로 API를 제공하긴 하지만 아직 베타버전이며 텍스트 타입 Block만 지원한다. 즉 이미지 같은 블럭은 읽을 수 없다는 것이다. 다행히 Unofficial Notion API이지만 Official보다 괜찮은 파이썬 라이브러리가 있었다.

- **[notion-py](https://github.com/jamalex/notion-py)** 

<br>



이제 이것으로 Notion페이지를 읽고 hugo-geekdoc에 맞게 변환하여 Markdown 파일을 만들어 주어야 한다. 이 부분은 파이썬 패키지로 만들어놓았다.

- **[https://github.com/hwangseonbi/notion2geekdoc](https://github.com/hwangseonbi/notion2geekdoc)** 

<br>



간단히 설명하자면 아래와 같이 Notion 페이지 하나에 테이블을 놓는다. 테이블은 여러개를 놓을 수 있다. 이렇게 여러 개의 테이블을 포함한 페이지를 **`Root Page`** 라고 정의했다. 

{{< img name="1bfa7958-c267-4e28-b389-686dd6f7494c.png" size="large" width="3168" lazy=false >}}

<br>



그후 이 Root Page URL 주소로 notion2geekdoc 모듈을 실행시키면 아래 Left Navigation과 같이 테이블 단위로 카테고리화된다. 그리고 **`Status Property`** 가 **`Published`** 인 문서들이 geekdoc theme에 꼭 맞게 랜더링된다! 자세한 사용법은 **[README.md](https://github.com/hwangseonbi/notion2geekdoc/blob/main/README.md)** 에 적어놓았다.

{{< img name="78de027b-e494-4c4f-9bce-5d25975d47d3.png" size="large" width="2742" lazy=false >}}

<br>



**`그러나 우리는 이 패키지를 직접 사용하지 않을 것이다 😓`** 

<br>



글을 작성하거나 편집할 때마다 파이썬 패키지를 실행시키고 Hugo 빌드 결과물을 Github에 커밋한다는 것... 또한 엄청난 **`노가다`** 이다. 

<br>



**`Notion에 글만 써놓으면 알아서 블로그에 연재되게 할 수는 없을까?`** 

<br>



# 5. 자동화하기

지금까지의 작업들을 자동화해줄 방법을 생각해보았다. 개인 서버에서 스크립트를 짜고 crontab을 돌릴까도 생각해보았지만 찾다보니 멀지않은 곳에서 괜찮은 방법을 찾을 수 있었다.

{{< img name="dc0cf4ce-42e2-444c-a484-faf2092f7517.png" size="large" width="2804" lazy=false >}}

그 동안 힐끗힐끗 신경쓰였던 Github의 Actions 탭이다. 알아보니 빌드, 테스트, 배포 등 다양한 작업을 할 수 있었다. 하나의 큰 작업을 workflow라고 한다. workflow 안에서는 언제 이 workflow가 실행될지 trigger를 정의하고 job과 step 단위로 할 일을 정의할 수 있다. 더 멋진 것은 이러한 workflow를 패키지화하여 다른 사람들과 공유할 수 있다는 것이다.

예를들어 **`Hugo 빌드 환경`** 을 갖추기 위해서는 Hugo를 설치하는 등의 작업을 거쳐야한다. 그런데 이러한 일련의 과정들을 하나의 action으로 만들 수 있다. 우리는 직접 hugo를 설치하는 과정을 정의할 필요가 없다. Hugo를 설치하는 작업들은 **[peaceiris/actions-hugo@v2](https://github.com/peaceiris/actions-hugo)** 에 패키징 되어있다. 아래와 같이 Git Action workflow 정의파일에서 step을 정의할 때 **[peaceiris/actions-hugo@v2](https://github.com/peaceiris/actions-hugo)**  을 거치면 그 이후 step부터는 Hugo를 사용할 수 있는 상태가 된다.

{{< highlight YAML "linenos=table" >}}
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v2
  with:
    hugo-version: '0.81.0'
    # extended: true
{{< /highlight >}}

<br>



<br>



아무튼 나는 아래 순서로 작업들이 실행되게 만들었다.

1. 매일 하루에 한번씩 notion2geekdoc 모듈을 실행

1. notion2geekdoc 모듈을 실행한 결과물(**`contents`** )을 hugo에 포함해 빌드한다.

1. hugo로 빌드한 최종 결과물을 github.io에 커밋&푸쉬한다.

<br>



매일 하루에 한번씩 workflow가 실행되게 하려면 **[scheduled-events](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#scheduled-events)**  트리거를 사용하면 된다. 나는 매일 UTC기준 00:00에 빌드가 되도록 설정했다. 한국 시각은 09:00이다.

{{< highlight YAML "linenos=table" >}}
name: cron_sync_notion_scheduler
on:
  schedule:
    - cron: '0 0 * * *'
{{< /highlight >}}

<br>



그리고 Job을 두 부분으로 나누었다. 하나는 notion2geekdoc을 실행하는 Job(**`render_from_notion`** )이다. 여기서 **`contents`**  디렉토리가 만들어진다. 다른 하나의 Job은 **`render_from_notion`** 에서 만들어진 **`contents`** 를 가지고 Hugo 빌드를 하고 배포하는 Job(**`deploy_content`** )이다.

<br>



이때 Job 간에 **`contents`** 를 공유해야한다. 하지만 Job은 별도의 VM 환경에서 실행되기 때문에 파일시스템이 공유되지 않는다. 따라서 우리는 Job 간에 **`contents`** 를 공유할 수 있도록 해야한다. 이러한 일을 **[download-artifact](https://github.com/actions/download-artifact)**  Action이 해준다.

<br>



자세한 workflow 정의는 **[cron_sync_notion_scheduler.yml](https://github.com/hwangseonbi/hwangseonbi.github.io/blob/main/.github/workflows/cron_sync_notion_scheduler.yml)**  에서 확인할 수 있다. 아래는 매일 한번씩 실행된 git action 기록들을 보여준다.

{{< img name="f2cfb431-5428-4c8b-a695-c370fbfb5377.png" size="large" width="1816" lazy=false >}}

<br>



마지막으로, 테스트할 때 cron을 trigger로 사용하는 것은 비효율적이다. 사용자가 원하는 시점에 원하는 변수로 trigger하는 방법으로 **[manual-events](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#manual-events)**  라는 방법이 있다. 아래와 같이 정의하며 실행할 때마다 input으로 사용자가 원하는 값을 전달할 수도 있다.

{{< highlight YAML "linenos=table" >}}
on:
  workflow_dispatch:
    inputs:
      root_page_url:
        description: 'Root Page URL(public)'
        required: true
{{< /highlight >}}

{{< img name="6c095d28-bfdf-4b79-89c5-96908f66495a.png" size="large" width="2502" lazy=false >}}

<br>



# 6. 결론

이 기록은 아무래도 설명서가 아닌 경험담이기 때문에 자세한 구축방법을 써놓진 않았다. 그리고 이런식으로 툴을 활용한 시스템 구축은 한번 만들어놓고 자세한 문서를 만들어놓지 않으면 나중에 다시 보았을 때 파악하기 힘들다.

<br>



> ***'이게 왜 되고있는 거지?'<br>'이런 일은 누가 해주는거지?'***

<br>



이런 상황을 방지하려면 Zerobase에서도 구축할 수 있는 설명서가 되면 좋다. 그러나 더 좋은 것은 장황한 문서를 안보고도 쉽게 구축할 수 있는 것이다. 아래 작업들로 훨씬 편하게 만들 수 있을 것 같은데 이것들은 Next로 남겨두어야겠다.

<br>



- notion2geekdoc모듈의 setuptools 설정 등 사전 작업 및 pypi 등록

- 하나의 git action으로 위 작업들이 자동화되게끔 git action 제작 또는 Docker conatiner image 제작 (가능할까?)

<br>

