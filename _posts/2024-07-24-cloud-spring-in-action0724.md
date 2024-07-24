---
title: 클라우드 네이티브 인 액션(6) - Spring boot k8s로 실행,관리하기
author: minseok
date: 2024-07-24 00:02:00 +0800
categories: [Book, Cloud-Native-Spring-In-Action]
tags: [Spring]
math: true
mermaid: true
image: 
    path: /assets/img/cloudNativeSpringInAction/main.png
    alt: Cloud Native Spring In Action
comments: true
---

> 클라우드 네이티브 스프링 인 액션 서적의 데모 프로젝트를 모방하였습니다.
[깃 레포지토리](https://github.com/kkminseok/spring-cloud-native-example)
{: .prompt-info}

저번 장에서는 스프링 애플리케이션을 Docker Container로 올리고 그에 따른 CI/CD를 구축하는 작업을 하였다.

이번장에서는 k8s로 관리하는법을 학습할 것 같다.

> minikube를 사용하였습니다. 참고바랍니다.
{: .prompt-info}

k8s용어에 대해서는 다루지 않을 예정이다 이것까지 다루면 안그래도 긴 글이 더 길어질 것 같아서다.

## □ 배포 객체 생성

루트프로젝트에서 k8s라는 디렉터리를 만들고 `deployment.yml`을 작성하였다.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: catalog-service
  labels:
    app: catalog-service
```

Deployment객체의 이름은 catalog-service, 레이블은 app=catalog-service를 갖는 배포 객체를 생성한다.

그리고 밑에 spec을 적어준다.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: catalog-service
  labels:
    app: catalog-service
spec:
  selector:
    matchLabels:
      app: catalog-service
  template:
    metadata:
      labels:
        app: catalog-service
    spec:
      containers:
        - name: catalog-service
          image: catalog-service
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 9001
          env:
            - name: BPL_JVM_THREAD_COUNT
              value: "50"
            - name: SPRING_CLOUD_CONFIG_URL
              value: http://config-sevice
            - name: SPRING_DATASOURCE_URL
              value: jdbc:postgresql://polar-postgres/polardb_catalog
            - name: SPRING_PROFILES_ACTIVE
              value: testdata
```

selector는 레플리카셋 객체를 통해 확장할 객체를 식별하는 방법을 제공하고, template은 원하는 파드, 컨테이너를 생성하기 위한 규칙을 명세를 정의한다.

컨테이너 포트는 9001, 로컬에 이미지가 존재하지 않을시 저장소에서 이미지를 받아오도록 설정하였다.

밑의 env는 파드에 전달할 환경변수 목록을 key-value 형태로 적는다.

기본적으로 미니큐브는 로컬이미지를 가져올 수 없다. 따라서 따로 명령어를 통해 가져와야한다.

나는 catalog-service라는 이미지를 만들었고 polar라는 클러스터 환경에서 사용하기 위해서

```sh
minikube image load catalog-service --profile polar 
```

같이 명령어를 실행해주었다.

이후 배포 객체를 생성해준다.

```sh
> kubectl apply -f k8s/deployment.yml 
deployment.apps/catalog-service created

# 확인
> kubectl get all -l app=catalog-service    
NAME                                   READY   STATUS    RESTARTS   AGE
pod/catalog-service-54bc4878df-kp47m   1/1     Running   0          6m35s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/catalog-service   1/1     1            1           6m36s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/catalog-service-54bc4878df   1         1         1       6m36s

```

배포, 레프리카셋, 파드를 생성함을 알 수 있다.

배포가 잘 되었는지 확인 하기 위해서 다음 열영어로 확인해준다.

```sh
> kubectl logs deployment/catalog-service                                                                                                                                                  develop [aae578a] modified untracked
Setting Active Processor Count to 12
Calculating JVM memory based on 5868212K available memory
For more information on this calculation, see https://paketo.io/docs/reference/java-reference/#memory-calculator
Calculated JVM Memory Configuration: -XX:MaxDirectMemorySize=10M -Xmx5456885K -XX:MaxMetaspaceSize=104126K -XX:ReservedCodeCacheSize=240M -Xss1M (Total Memory: 5868212K, Thread Count: 50, Loaded Class Count: 15970, Headroom: 0%)
Enabling Java Native Memory Tracking
Adding 137 container CA certificates to JVM truststore
Spring Cloud Bindings Enabled
Picked up JAVA_TOOL_OPTIONS: -Djava.security.properties=/layers/paketo-buildpacks_bellsoft-liberica/java-security-properties/java-security.properties -XX:+ExitOnOutOfMemoryError -XX:ActiveProcessorCount=12 -XX:MaxDirectMemorySize=10M -Xmx5456885K -XX:MaxMetaspaceSize=104126K -XX:ReservedCodeCacheSize=240M -Xss1M -XX:+UnlockDiagnosticVMOptions -XX:NativeMemoryTracking=summary -XX:+PrintNMTStatistics -Dorg.springframework.cloud.bindings.boot.enable=true

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/

 :: Spring Boot ::                (v3.3.1)

...
```

로컬 클러스터로 구축하였기에 아직 외부에서는 사용할 수 없다. 외부 포트와 바인딩을 해주어야한다.

또한 클러스터에서 실행중인 PostgreSQL과도 연결시켜줘야한다. 여기서 등장하는 개념은 **서비스 디스커버리**라는 개념이고 서비스 디스커버리는 클라이언트 패턴과 서버 패턴으로 나뉜다. k8s는 서버 패턴을 사용한다는 점을 알고 있으면 될 것 같다.

간단하게 설명하자면 클라이언트 패턴은 클라이언트 입장에서 다른 서비스를 호출할 때 직접 서비스 저장소(레지스트리)에서 해당 서비스를 찾는 기능을 담당해야하기에 레지스트리에 종속적인 반면에 서버 패턴은 클라이언트가 서비스를 찾아주는 로드밸런스에 요청을 보내 서비스를 찾아주는 패턴이다. 클라이언트 패턴에 비해 의존성은 떨어지지만 프레임워크에서 제공하지 않는 경우 개발자가 직접 구축해야한다는 단점이 있다.

## □ 서비스 매니페스트 정의, 포트포워딩, 테스트

서비스의 포트를 외부에 노출시켜 파드에 접근할 수 있도록 해야한다.

따라서 서비스에 대한 매니페스트를 작성해야한다.

```yml
apiVersion: v1
kind: Service
metadata:
  name: catalog-service
  labels:
    app: catalog-service
spec:
  type: ClusterIP
  selector:
    app: catalog-service
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9001
```

- selector: 서비스를 통해 노출이 될 파드를 찾는데 사용하는 레이블
- protocol: 서비스가 사용하는 네트워크 규약
- port: 서비스 객체의 노출 포트 
- targetPort: 서비스가 파드로 요청을 전달할 떄 사용하는 포트

이후 서비스 객체를 실행해준다.

```sh
> kubectl apply -f k8s/service.yml 
service/catalog-service created

# 확인 작업
> kubectl get svc -l app=catalog-service 
NAME              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
catalog-service   ClusterIP   10.105.189.107   <none>        80/TCP    9m55s
```

ClustIP유형으로, 클러스터 내 다른 파드들은 Ip주소나 이름을 통해 서로 통신할 수 있게 된다.

이를 테스트 하기 위해서 로컬 환경과 포트포워딩을 해줘야하는데 다름 명령어를 통해 포트포워딩을 해주고 테스트를 진행해본다.

```sh
# 로컬의 9001포트와 catalog-service의 서비스 객체 80포트를 연결한다.
> kubectl port-forward service/catalog-service 9001:80 
Forwarding from 127.0.0.1:9001 -> 9001
Forwarding from [::1]:9001 -> 9001

# 다른 터미널에서 실행
> http :9001/                                                          
HTTP/1.1 200 
Connection: keep-alive
Content-Length: 34
Content-Type: text/plain;charset=UTF-8
Date: Wed, 24 Jul 2024 01:55:57 GMT
Keep-Alive: timeout=15

Welcome to the local book catalog!
```

## □ 애플리케이션 확장하기, k8s우아한 종료 설정

이 책에서 설명하는 론에 의하면 애플리케이션은 일회성에 가깝다고 한다.

문제가 생기면 폐기했다가 띄울 수도 있는 것이고, 상태를 갖지 않아서 개발자의 마음대로 서버를 내렸다가 올릴 수 있어야한다.

이러한 일회성의 특징을 갖기 위해서 책은 몇가지 조건을 제시하고 있다.

- 빠른시작을 보장: 클라우드 네이티브를 활용해 몇 초내로 실행될 수 있음을 보장해야한다.
- 우아한 종료: 서버가 종료될때에 클라이언트에게 다운타임이나 오류가 일어나지 않고 종료되어야한다.

우아한 종료를 설정하기 위해 기존 프로젝트의 설정파일을 수정해준다.

```properties
...
server.shutdown=graceful
# 종료기간 15s default: 30s
spring.lifecycle.timeout-per-shutdown-phase=15s
...
```
종료기간은 대기중인 모든 요청에 대해서 애플리케이션이 처리할 수 있는 기간을 지정하는 것이다. 15초가 지나면 대기중인 요청에 대해서 처리를 진행하지 않고 종료한다.

코드가 수정되었기에 다시 minikube에 배포를 진행해야한다.

나중에 책 뒤에서 자동화 작업을 진행하겠지만 기존 방식대로 배포를 진행한다면 다음의 절차를 거쳐야한다.

1. ./gradlew bootBuildImage로 이미지화
2. minikube image load catalog-service --profile polar로 로컬 이미지 로드 


애플리케이션에서 우아한 종료에 대한 설정을 하였으면 k8s의 배포 매니페스트도 수정해줘야한다.

왜냐하면 기본적으로 k8s는 우아한 종료를 위해 30초정도 기다리는데 내가 설정한 애플리케이션 우아한 종료 종료시간(15초)가 더 짧기에 k8s에 해당 정보를 공유해줘야한다.

또한 k8s는 SIGTERM 신호를 파드로 보내면서 다른 구성요소들이 종료되는 파드쪽으로 요청을 전달하는것을 막는다. 그러나 k8s는 분산시스템이고 병렬적으로 작업이 수행되어 종료파드가 이미 종료절차를 시작함에도 불구하고 요청을 받을때가 있다. 이러면 클라이언트에게 오류를 발생시키게 되는데 이를 방지하기 위한 방법들이 있다.

권장되는 해결책은 SIGTERM 신호를 파드로 보내는것을 지연시키는 것이다. 이렇게 하면 클러스터 전체에 요청 중지 명령을 전달할 수 있는 시간을 확보할 수 있게 된다.
이렇게 하면 파드가 우아한 종료 절차를 시작할 때 다른 구성들도 해당 파드가 종료됨을 알 수 있어서 요청을 보내지 않게 된다. **preStop**훅을 통해 제어가 가능하다.

```yml
#k8s/deployment.yml
...
spec:
  selector:
    matchLabels:
      app: catalog-service
  template:
    metadata:
      labels:
        app: catalog-service
    spec:
      containers:
        - name: catalog-service
          image: catalog-service
          imagePullPolicy: IfNotPresent
          lifecycle: # <--
            preStop:
              exec:
                command: ["sh", "-c", "sleep 5"]
...
```

```sh
# 변경사항 적용
> kubectl apply -f k8s/deployment.yml
```

이렇게 해당 애플리케이션에 대한 우아한 종료 설정을 끝냈다.

확장성을 고려해, 파드에 대한 복제본도 설정하기 위해서 yml파일을 또 수정해주자.

```yml
#k8s/deployment.yml
...
spec:
  replicas: 2 # <--
  selector:
    matchLabels:
      app: catalog-service
...
```

복제는 레이블로 관리되고 해당 내용을 적용해본다

```sh
> kubectl apply -f k8s/deployment.yml 

# 확인
> kubectl get pod -l app=catalog-service                                             
NAME                              READY   STATUS    RESTARTS   AGE
catalog-service-54bbb458c-4ghf8   1/1     Running   0          18m
catalog-service-54bbb458c-qwggq   1/1     Running   0          54s
```

`AGE`항목을 통해따끈따근하게 생긴 파드가 하나 더 생긴걸 확인할 수 있다.

파드를 하나 삭제해도 다시 복제됨을 알 수 있다. 이를 테스트해본다. 나는 오래된 파드를 삭제해줄 것이다.

```sh
> kubectl delete pod catalog-service-54bbb458c-4ghf8
pod "catalog-service-54bbb458c-4ghf8" deleted

> kubectl get pod -l app=catalog-service
NAME                              READY   STATUS    RESTARTS   AGE
catalog-service-54bbb458c-9zc28   1/1     Running   0          49s
catalog-service-54bbb458c-qwggq   1/1     Running   0          3m38s

```
이렇듯 내부적으로 레플리카셋 객체는 배포된 복제본을 항상 확인하면서 원하는 상태와 일차하게끔 해준다.

다른 실습을 위해서 레플리카셋을 1로 맞춰주고, 관련 클러스터 정보들을 다 삭제해준다.

```sh
> kubectl delete -f k8s 
deployment.apps "catalog-service" deleted
service "catalog-service" deleted

# postgres 매니페스트 디렉터리로 이동하여
> kubectl delete svc polar-postgres                                                  
service "polar-postgres" deleted
```

## □ 틸트를 사용한 로컬 k8s 개발

현재 배포를 진행하려면 위의 설명한대로 빌드를 진행하고 이미지화 시켜서 kubectl을 통해서 업데이트를 수행해줘야한다.

매 번 이런 작업을 할 수 없어서 자동화하게끔 도와주는 툴이 있는데, 이게 바로 **틸트**이다.
틸트는 인프라에 대한 고민을 덜어주고 개발자로 하여금 비즈니스 로직에만 집중할 수 있게끔 도와준다.

먼저,틸트를 설치해야한다.

나는 mac M2, M1칩을 사용하기에 brew로 설치해줬다.

```sh
> brew install tilt-dev/tap/tilt 
```

다음은 틸트를 활용해서 달성해야할 목표다.

1. 클라우드 네이티브 빌드팩을 사용하여 애플리케이션 컨테이너 이미지화
2. 이미지를 쿠버네티스 클러스터에 업로드
3. YAML파일로 정의된 모든 쿠버네티스 객체에 적용
4. 로컬 호스트에서 엑세스할 수 있도록 포트 활성화
5. 클러스터에서 실행중인 애플리케이션 로그에 쉽게 엑세스

위의 예제 기준, postgresql을 다시 실행해야한다.

```sh
> kubectl apply -f postgresql.yml
```

틸트는 **틸트파일**이라는 스타라크로 된 언어를 사용하여 문서를 작성한다. 루트 프로젝트에 **Tiltfile** 을 작성한다.

```starlark
# Build
custom_build(
    # Name of the container image
    ref = 'catalog-service',
    # Command to build the container image
    command = './gradlew bootBuildImage  --imageName $EXPECTED_REF',
    # Files to watch thar trigger a new build
    deps = ['build.gradle', 'src']
)

# Deploy
k8s_yaml(['k8s/deployment.yml', 'k8s/service.yml'])

# Manage
k8s_resource('catalog-service', port_forwards=['9001'])
```

위 구문은 빌드, k8s로 배포, 엑세스 작업을 수행한다.

이를 실행하기 위해서는 루트 프로젝트로 가서 다음 명령어를 실행해준다.

```sh
> tilt up                                                              
Tilt started on http://localhost:10350/
v0.33.17, built 2024-06-12

(space) to open the browser
(s) to stream logs (--stream=true)
(t) to open legacy terminal mode (--legacy=true)
(ctrl-c) to exit
```

<http://localhost:10350/> 링크로 들어가게 되면 GUI를 통해 틸트가 관리하는 애플리케이션 의 로그 및 배포 수행과정을 확인할 수 있다.

![](/assets/img/cloudNativeSpringInAction/6_tilt.png)

이렇듯 문법에러로 감지해주고 Tiltfile이 수정되면 자동으로 빌드를 진행해주고 업데이트해주게 된다.

문제는 코드랑 동기화되어서 작은 수정에도 tilt가 켜져있는 동안은 자동으로 틸트의 워크로드를 수행하게 되는데 스프링 부트 데브툴스, 패키지 빌드팩에서 이를 조정할 수 있다고 한다.

참고로 `tilt down` 명령어로 배포된 객체들을 종료시킬 수 있다.

## □ 옥탄트를 이용한 워크로드 시각화

쿠버네티스 클러스터에 여러 애플리케이션을 배포하다보면 하나의 애플리케이션에서 문제가 생기는 등 상황속에서 로그를 추적하기 어려울 수 있는데 옥탄트가 이를 도와준다.

문제는 현재 2024년기준 옥탄트 프로젝트가 중단되어 더이상 일반적인 경로로는 사용할 수 없게 되었다.


![](/assets/img/cloudNativeSpringInAction/6_octant.png)

Lens,k9s 같은 다른 시각화 도구를 사용해봐야할듯하다.


## □ Git Action을 통한 k8s 매니페스트 유효성 검사

매니페스트란 k8s를 위한 yaml에 대한 명세를 말하는걸로 이해하면 된다.

책에서는 **kubeval**이라는 툴을 사용해서 매니페스트 유효성 검사를 진행하고 있지만 해당 프로젝트도 종료된 듯 하다.

작가의 깃허브에 들어가보면 **kubeconform**라는 툴을 사용해서 검증을 하는 것 같으므로 해당 툴을 이용해서 검증해보겠다.

```sh
> brew install kubeconform
```
brew로 설치해주면 된다.

테스트를 위해 yml파일 하나 잡고 문법을 일부러 틀려보았다.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: catalog-service
  labels:
    app: catalog-service
spec:
  replicas: 1
  selector:
    matchLabels:

```

이후 kubeconform을 실행해본다.

```sh
> kubeconform k8s                                                             
k8s/deployment.yml - Deployment catalog-service is invalid: problem validating schema. Check JSON formatting: jsonschema: '/spec' does not validate with https://raw.githubusercontent.com/yannh/kubernetes-json-schema/master/master-standalone/deployment-apps-v1.json#/properties/spec/required: missing properties: 'template'

```

잘 되는구만

로컬에서 검증도 되었고, 이를 커밋단계에서 자동화하기 위해서 gitHubAction을 수정해준다.


```yml
- name: Setup tools
uses: alexellis/setup-arkade@v3
- name: Install tools
uses: alexellis/arkade-get@master
with:
    kubeconform: latest
- name: Validate Kubernetes manifests
run: |
    kubeconform --strict catalog-service/k8s
```

이 구문을 추가해준다. 참고로 나는 yaml파일들이 catalog-service/k8s 디렉터리에 있어서 경로를 저러해주었다.

또한 strict옵션은 스키마에 추가적인 속성이 없거나 중복키 검증등을 해주니 넣어주면 좋을 것 같다.

이후 커밋 하고 GitAction이 돌아가는것을 확인해보면 된다.

![](/assets/img/cloudNativeSpringInAction/6_manifest_action.png)


> 추가적인 정보가 필요하면
[깃 레포지토리](https://github.com/kkminseok/spring-cloud-native-example)에서 확인 부탁드립니다.
{: .prompt-info}

































