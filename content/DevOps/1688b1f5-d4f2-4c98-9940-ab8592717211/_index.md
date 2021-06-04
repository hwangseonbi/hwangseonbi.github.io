---
date: 2021-06-04 17:55:00
resources:
- name: 44f6bafc-fe47-4085-9180-8cccaea3d833.png
  src: 44f6bafc-fe47-4085-9180-8cccaea3d833.png
  title: ''
- name: 6ab83552-31ec-4a8c-8ce7-9206e5f0e290.png
  src: 6ab83552-31ec-4a8c-8ce7-9206e5f0e290.png
  title: ''
- name: 5f08aace-f8c6-43de-a1da-33aa8d66e2c6.png
  src: 5f08aace-f8c6-43de-a1da-33aa8d66e2c6.png
  title: ''
title: Kubernetes configuration pattern에 대하여
weight: 4

---
> ***이 블로그는 Notion에서 랜더링 자동화를 통해 제작되었습니다.<br>Notion 페이지에 최적화되어있습니다. → [Kubernetes configuration pattern에 대하여](https://www.notion.so/hwangseonbi/Kubernetes-configuration-pattern-1688b1f5d4f24c989940ab8592717211)***

<br>



---

<br>



{{< toc >}}

<br>



<br>



> ***Kubernetes Pattern<br>Part IV. Configuration Patterns 참고***

<br>



# 1. EnvVar Configuration

가장 간단한 방법이다. Config Data의 개수가 적고 단순할 때 사용하기 적합하다.

<br>



1. default ENV는 이미지에 정의해준다.

	{{< highlight Docker "linenos=table" >}}
FROM ubuntu:latest

ENV BACKEND_URL "/backend"
ENV PROFILE = "DEV"
...
	{{< /highlight >}}

	<br>

	

1. 어플리케이션에서 접근한다.
환경변수로 PROFILE만 주입해주고 어플리케이션 내부에서 PROFILE에 따라 다른 config를 불러오는 것도 하나의 방법이다. (많이 쓰임)

	{{< highlight JavaScript "linenos=table" >}}
contentsTableRefresh:function(){
    const url = `${process.env.BACKEND_URL}/videos/contents`
    fetch(url)
    ...
	{{< /highlight >}}

	<br>

	

1. Kubernetes 정의 파일에서 env를 주입한다. 문자열로 직접 삽입할 수도 있으며 configMap이나 Secret을 참조하여 받아올 수도 있다.

	{{< highlight YAML "linenos=table" >}}
apiVersion: v1
kind: Pod
metadata:
	name: random-generator
spec:
	containers:
	- image: my/jsproject:1.0
		name: jsproject
		env:
		- name: BACKEND_URL
		  value: http://localhost:8080
		- name : PROFILE
			valueFrom: ... 
	{{< /highlight >}}

<br>



장점

- 간편하다.

- 어떤 어플리케이션이든, 어떤 베이스 이미지든 통용되므로 범용성이 좋다.

<br>



단점

- 안전하지 않다.

- 복잡한 Config를 다루기에는 맞지 않다. (그 많은걸 어떻게 다 핸들링할것인가!)

- ENV를 주입할 수 있는 계층이 나눠져 있어서 디버깅하기 어렵다. (Image에서 정의될 수도, K8S정의에서 정의될 수도, APP에서 정의될 수도 있으니..)

- Immutable 하다. 즉 APP이 시작되기 전에 세팅되서 나중에 변경하기 힘들다. → Rolling update할 때 config 바뀔일이 없으니 장점일 수도? 암튼!

<br>



<br>



# 2. Configuration Resouece

위 EnvVar Configuration 패턴에서 조금 변형된 패턴이다.

이 패턴을 사용하면 좋은 점

- config로 사용할 데이터들을 하나의 지점에서 관리할 수 있다.

- configMap을 Pod에서 파일로 마운트 시키면 configMap 변경 사항이 반영되게 할 수도 있다.

<br>



EnvVar Conf 패턴에서는 Pod 정의에 env를 직접 문자열로 때려넣었다면 이 패턴에서는 K8S 네이티브 리소스인 ConfigMap과 Secret를 사용하여 넣는 것이다. ConfigMap과 Secret은 기술적으로는 동일하고 사용법도 동일하다.

<br>



물론 제한 사항도 있으니 체크필요! (ex. Secret은 1MB 제한)

<br>



{{< highlight YAML "linenos=table" >}}
apiVersion: v1
kind: ConfigMap
metadata:
	name: my-config
data:
	BACKEND_URL: /backend
	application.properties: |
		# App properties config
			log.file=/tmp/myapp.log
			server.port=7070
	EXTRA_OPTIONS: "high-secure,native"
...
{{< /highlight >}}

<br>



{{< highlight YAML "linenos=table" >}}
apiVersion: v1
kind: Pod
metadata:
	name: random-generator
spec:
	containers:
	- image: my/application:1.0
		name: myapplication
		volumeMounts:
		- name: config-volume
			mountPath: /config
	- env:
		- name: BACKEND_URL
			valueFrom:
				configMapKeyRef:
					name: my-config
					key: BACKEND_URL
				prefix: CONFIG_
	volumes:
	- name: config-volume
		configMap:
			name: my-config
{{< /highlight >}}

<br>



<br>



```
💡참고 : **Secret은 그다지 secret하지 않다.
그러므로 민감한 정보라면 어플리케이션 단에서도 암호화할 필요가 있다.

-** Secure은 Base64로 인코딩되어 저장되어있다. 그리고 파드에 넣어지기 직전에 디코딩된다.
- Secret의 아래와 같은 특징이 있긴하다.
    - Secret은 Pod가 실행되고있는 노드에만 배포된다.
    - 해당 노드에서 secret은 메모리에 저장된다. pod가 제거되면 같이 제거된다.
    - ETCD에서 secret은 암호화된 형태로 저장된다.
- 그래도 어쨌든 root user가 아닌 user도 접근해서 볼 수 있다. (role-based access control 적용하더라도, pod 생성 권한만 있으면 마운트시켜서 볼 수 있음)
```

<br>



<br>



# 3. Immutable Configuration

이 패턴을 사용하면 좋은점

- Immutability 컨트롤 가능

	- envVar config 패턴에서는 immutability가 패시브로 강제였지만 여기서의 immutability는 원하는 시점에 immutable하게 할 수 있다는 뜻. 예를들면 어플리케이션이 시작되고나면 바꿀수 없게 한다던지.

- config의 버전관리 가능

- ConfigMap이나 Secret의 용량제한을 뛰어넘을 수 있음

<br>



How?

config 관리 컨테이너 이미지를 만들어놓고 어플리케이션 런타임 때 이 컨테이너를 참조한다. 참조하는 방법은 플랫폼에 따라서 다양한 방법으로 가능.

<br>



### 1) Docker

volume-from 옵션으로 다른 컨테이너의 volume 참조

{{< highlight Bash "linenos=table" >}}
docker create --name config-dev myapplication-config-dev:1.0.1
docker run --volume-from config-dev myapplication:1.0

docker create --name config-prdt myapplication-config-prdt:1.0.1
docker run --volume-from config-prdt myapplication:1.0
{{< /highlight >}}

{{< img name="44f6bafc-fe47-4085-9180-8cccaea3d833.png" size="large" width="2075" lazy=false >}}

<br>



### 2) Kubernetes

Kubernetes에서는 Docker의 volume-from 같은 명령을 지원하지 않는다. 다른 방법이 있는데 Init Container을 사용하면 된다.

{{< highlight Docker "linenos=table" >}}
FROM busybox
ADD dev.properties /config-src/demo.properties
ENTRYPOINT ["sh", "-c", "cp /config-src/* $1", "--"]
{{< /highlight >}}

<br>



Deployment의 Pod 템플릿에서는 하나의 volume과 두개의 컨테이너를 가지게된다.

{{< img name="6ab83552-31ec-4a8c-8ce7-9206e5f0e290.png" size="large" width="1269" lazy=false >}}

{{< highlight YAML "linenos=table" >}}
...
initContainers:
- image: k8s/patterns/config-dev:1
	name: init
	args:
	- "/config"
	volumeMounts:
	- mountPath: "/config"
		name: config-directory
containers:
- image: k8spatterns/demo:1
	name: demo
	ports:
	- containerPort: 8080
		name: http
		protocol: TCP
	volumeMounts:
	- mountPath: "/config"
		name: config-directory
volumes:
- name: config-directory
	emptyDir: {}
{{< /highlight >}}

- "config-directory"라는 이름의 volume을 통해서 initContainers를 거쳐 실제 app 컨테이너로 config가 카피된다.

<br>



만약 config를 현재 dev에서 prdt로 바꾸고 싶다면, init container의 image만 바꾸면 된다. (yaml을 통해서든 kubectl을 통해서든) 근데 이방법도 안전하지 않은데?? 그리고 hot reload도 안될듯..

<br>



OpenShift Template에서는 이 부분은 쉽게 파라미터를 넘기는 식으로 사용가능하다.

{{< highlight YAML "linenos=table" >}}
apiVersion: v1
kind: Template
metadata:
	name: demo
parameters:
- name: CONFIG_IMAGE
	description: Name of configuration image
	value: k8spatterns/config-dev:1
objects:
- apiVersion: v1
	kind: DeploymentConfig
	// ...
		spec:
			template:
				metadata:
				//...
					spec:
						initContainers:
						- name: init
							image: ${CONFIG_IMAGE}
							args: ["/config"]
							volumeMounts:
							- mountPath: /config
								name: config-directory
						containers:
						- image: k8spatterns/demo:1
							//...
							volumeMounts:
							- mountPath: /config
								name: config-directory
				volumes:
				- name: config-directory
					emptyDir: {}
{{< /highlight >}}

<br>



{{< highlight Bash "linenos=table" >}}
oc new-app demp -p CONFIG_IMAGE=k8spatterns/config-prod:1
{{< /highlight >}}

<br>



<br>



책에서 말하는 장점과 단점은 아래와 같다.

장점

- container 내부에 config가 들어있으므로 버전관리가 가능하다.

- Configuration created this way can be distributed over a container registry. The configuration can be examined even without accessing the cluster.
→ "config는 컨테이너 레지스트리를 넘어서 배포가 되므로 클러스터에 직접 접근하지 않아도 확인가능하 다." 라는 말인데, 흠... 두가지  때문에 이말을 한것같은데 확실하게 모르겠다.
1) 클러스터 내부로 들어가지 않아도 컨테이너 레지스트리에 접근 가능해서 하는 말이거나(몰랐음ㅋ)
2) 컨테이너 레지스트리 파일시스템에서 확인 가능하므로 하는 말이거나

- config가 컨테이너 이미지 안에 들어있으므로 Immutable하다.

- 복잡한 config도 다루기 쉬움. 사이즈가 큰 파일도 가능.

단점

- 라이프사이클이 복잡함, 이미지 빌드 관리 포인트가 늘어남.

- 민감한 데이터에 대한 concern이 없음.

- 환경 별로 다른 deployment가 필요. (또는 수정 필요)

<br>



```
👉그런데 내 생각에는, 이 방법은 복잡성이 높아서 왠만하면 피해야겠다..

물론 버전관리가 된다는 점에서는 좋은 점 같긴하나 어플리케이션 개발에 따라 config도 변하기마련인데, 별도로 버전관리를 해야한다는게 오히려 짐이 늘어나는 것 같아 그리 장점으로 느껴지지가 않는다.

게다가 이 글을 쓰게된 처음 원칙을 떠올려보면, "config 변경으로 인해 Application의 빌드를 다시하면 안된다" 였는데 물론 Application 이미지는 그대로이긴하다. 그러나 이렇게 하는 이유로는 Application의 환경별 일관성도 있지만 개발자로서 개발하기 편하게 하기위함도있다. 그런데 이방법으로는 개발자는 괴로워질 것 같다..

CI/CD 면에서 관리 포인트도 늘어나고 config 수정이 매우 어려울 듯(hot reload도 불가능).
```

<br>



# 4. Configuration Template

이 패턴은 3. Immutable Configuration에서 조금 진화된 것이라고 보면된다. 매우 복잡한 config이고 환경별로 거의 비슷한 config data를 가진다면 중복되는 부분이 많을 것이다. 따라서 일부 다른 부분만 바꿔주고 나머지 동일한 부분은 중복되어 리소스낭비를 줄일 수 있다.

<br>



이 일을 하는 Configuration Template Tool들을 init container에서 사용하면 된다. Tiller (Ruby) or Gomplate (Go) 같은 것들이 있다.

{{< img name="5f08aace-f8c6-43de-a1da-33aa8d66e2c6.png" size="large" width="1105" lazy=false >}}

이 패턴은 복잡성 때문에 config data가 매우 큰 경우에만 사용하는게 좋을 것이다.

<br>

