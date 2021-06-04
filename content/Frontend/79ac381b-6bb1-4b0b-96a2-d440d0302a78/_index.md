---
date: 2021-06-04 15:04:00
resources:
- name: 3f158742-e8fc-4f14-abe3-6e6c67376745.png
  src: 3f158742-e8fc-4f14-abe3-6e6c67376745.png
  title: ''
- name: 7833d2e7-375c-4e2b-83a5-538ba734e9fa.png
  src: 7833d2e7-375c-4e2b-83a5-538ba734e9fa.png
  title: ''
- name: 1729ca93-e6c3-4380-b9a5-a49cdefac322.png
  src: 1729ca93-e6c3-4380-b9a5-a49cdefac322.png
  title: ''
- name: fdb20279-ab8c-4b65-b31f-40a235c5bb04.png
  src: fdb20279-ab8c-4b65-b31f-40a235c5bb04.png
  title: ''
- name: fd00bc82-0cf3-4ffe-ac3e-7b0028e67c46.png
  src: fd00bc82-0cf3-4ffe-ac3e-7b0028e67c46.png
  title: ''
- name: 651d79de-66cf-449d-aa74-12b751f2e4ef.png
  src: 651d79de-66cf-449d-aa74-12b751f2e4ef.png
  title: ''
- name: 6518c4d5-1909-46d6-950e-c2a271d1f8b2.png
  src: 6518c4d5-1909-46d6-950e-c2a271d1f8b2.png
  title: ''
- name: 56ef04b3-7923-4189-8941-5c97e5f5bb10.png
  src: 56ef04b3-7923-4189-8941-5c97e5f5bb10.png
  title: ''
- name: f5fd6442-da0b-4839-9f2b-274c472ce6b2.png
  src: f5fd6442-da0b-4839-9f2b-274c472ce6b2.png
  title: ''
- name: 2ed0afb1-6233-4482-9e72-a43142319096.png
  src: 2ed0afb1-6233-4482-9e72-a43142319096.png
  title: ''
title: '[HTML & CSS] Box모델에 대하여'
weight: 0

---
나는 항상 크롬의 개발자도구 탭에서 요소를 검사할 때마다 나타나는 저 박스들이 무섭게 느껴졌다ㅋㅋ

{{< img name="3f158742-e8fc-4f14-abe3-6e6c67376745.png" size="small" width="432" lazy=false >}}

블로그 글을 봐서도 잘 이해가 안갔었다. 도대체 무엇을 의미하는가... 🥵

강의를 듣다가 이것에 대한 내용이 나왔는데 이렇게 쉬운 내용이었다니 무릎을 탁 치며 정리해본다.

---

{{< toc >}}

---

# 1. &lt;div&gt;로 박스먼저 만들기

padding이 뭔지 margin이 뭔지 정의 부터 보기전에 &lt;div&gt;태그로 사각형을 먼저 만들어보자.

height, width 속성으로 사이즈를 조절할 수 있다. 그리고 이에따라 주변의 요소들은 밀려나게된다.

말끔한 날것의(?) blue 사각형이 만들어진다.

{{< img name="7833d2e7-375c-4e2b-83a5-538ba734e9fa.png" size="large" width="781" lazy=false >}}

<br>



<br>



<br>



# 2. boder 속성 살펴보기

두번째로 볼 속성은 border이다. border 속성의 기본값은 none이라서 별도의 속성값을 주지 않으면 위에서 보았던 것처럼 경계선은 보이지 않게된다. 

solid값을 줘보자. 그러면 사각형 테두리에 3px(기본값)짜리 경계선이 둘러쌓인다.

중요한 것은 전체적인 div의 크기는 커졌지만 알맹이 사각형의 크기는 그대로인 상태에서 border의 픽셀수만 추가되었다는 것이다.

<br>



{{< img name="1729ca93-e6c3-4380-b9a5-a49cdefac322.png" size="large" width="1552" lazy=false >}}

<br>



<br>



<br>



<br>



border-width 속성값을 많이주면 더 극명하게 차이가 보인다. 물론 0px로 설정하면 border는 사라지게 된다.

<br>



{{< img name="fdb20279-ab8c-4b65-b31f-40a235c5bb04.png" size="large" width="1554" lazy=false >}}

<br>



<br>



# 3. 사각형 내부에 다른 요소가 들어있다면?

이제 div 사각형 내부에 다른 요소가 들어있다고 해보자.

<br>



{{< img name="fd00bc82-0cf3-4ffe-ac3e-7b0028e67c46.png" size="large" width="1548" lazy=false >}}

<br>



<br>



<br>



이때 글자들이 너무 왼쪽에 붙어있어 조금 여유를 두고싶을 때가 있다.

<br>



이럴 때 사용하는 것이 padding이다.

<br>



{{< img name="651d79de-66cf-449d-aa74-12b751f2e4ef.png" size="small" width="240" lazy=false >}}

<br>



<br>



<br>



# 4. 그러면 외부 요소와 간격을 벌리고 싶을 땐?

반대로 외부요소와 외부요소 사이의 간격을 벌리고 싶을때 사용하는 것이 margin이다.

{{< img name="6518c4d5-1909-46d6-950e-c2a271d1f8b2.png" size="small" width="384" lazy=false >}}

다음 예시를 보자.  &lt;div&gt;태그 안에 &lt;h1&gt;태그와 &lt;p&gt;태그가 포함되어있는 단순한 html이다.

{{< img name="56ef04b3-7923-4189-8941-5c97e5f5bb10.png" size="large" width="1746" lazy=false >}}

<br>



개발자 도구에서 보면 &lt;p&gt;태그에 margin이 들어간 것을 볼 수 있다. 이 값 때문에 &lt;p&gt;태그 위에있는 &lt;h1&gt;태그와 공간이 벌어져있는 것을 볼 수 있다. 주황색으로 표시된 부분

<br>



<br>



# 5. 직접 계산해보기

컨텐츠의 영역을 직접 계산하여 아래와 같이 꼭지점을 잇는 사각형을 만들어보면 박스모델을 제대로 이해할 수 있다. 

<br>



<br>



{{< img name="f5fd6442-da0b-4839-9f2b-274c472ce6b2.png" size="small" width="288" lazy=false >}}

<br>



{{< highlight HTML "linenos=table" >}}
<div class="container1">
</div>
<div class="container2">
</div>
<div class="container3">
</div>
{{< /highlight >}}

<br>



<br>



아래 CSS 코드의 주석 처리된 부분을 풀면 위 결과물이 만들어진다. 왜 저만큼의 픽셀이 밀어져야하는지 생각해보시길.

{{< highlight CSS "linenos=table" >}}
* {
  margin:0px;
}

.container1 {
  width:100px;
  height:100px;
  border:solid;
  border-width:5px;
  background-color:blue;
  
/*   margin-left:10px; */
}
.container2 {
  width:100px;
  height:100px;
  background-color:red;
  border:solid;
  border-width:10px;
  
/*   margin-left:120px; */
}
.container3 {
  width:100px;
  height:100px;
  background-color:yellow;
  border:solid;
  border-width:10px;
  
/*   margin-left:240px; */
}
{{< /highlight >}}

<br>



<br>



{{< expand "▼ ℹ️ 전역 속성 설정 이유">}}

과제를 하다보면 이렇게 margin값을 주지 않았음에도 사각형 주위에 빈공간이 생긴다.

{{< img name="2ed0afb1-6233-4482-9e72-a43142319096.png" size="large" width="1818" lazy=false >}}

그 이유는 기본값 때문이다. 우리가 아무 속성값을 지정하지 않아도 기본적으로 &lt;body&gt;태그에 설정된 속성들이 있다. 그 중에서 margin이 8px으로 기본적으로 적용되어 있다.

그래서 이렇게 &lt;body&gt;를 비롯한 모든 기본 margin값을 0px로 설정해놓은 것이다.

{{< /expand >}}

<br>



<br>



<br>



<br>



<br>



<br>

