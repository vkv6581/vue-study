# Kubernetes (쿠버네티스, k8s)

[쿠버네티스 공식문서](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/)

### 쿠버네티스란?
- 컨테이너 기반의 개발 환경에서 컨테이너 배포 / 서버관리를 통합하여 도와주는 툴. (오케스트레이션)

- 쿠버네티스는 추상화된 개념. 랜처, 오픈시프트 등 구현체는 따로 존재함.

### 쿠버네티스가 필요한 이유?
- 컨테이너 기반의 환경에서 많은 컨테이너를 관리하는데 번거롭기 때문에 쿠버네티스를 통해 자동화하는 것.

### 쿠버네티스 구성 요소
![쿠버네티스 구성요소](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

위의 그림처럼 하나의 쿠버네티스(클러스터)엔 하나의 컨트롤 플레인에 여러 노드들이 묶여서 구성된다.

- 쿠버네티스 - 컨테이너 관리 도구.
- 컨테이너 - VM과 유사하지만 격리 속성을 완화하여 OS를 공유함. --> OS를 공유하는 실행 환경.
- 클러스터 - 쿠버네티스를 실행하면 생성되는 노드 머신? 쿠버네티스 최상위 객체
- 노드 - 클러스터 안에 존재하는 객체. 노드 내부에 파드가 있고 파드에서 컨테이너들이 실행됨.
- 파드(Pod) - 쿠버네티스 객체의 최소단위. 하나 이상의 컨테이너로 구성됨.

#### 컨트롤 플레인 컴포넌트 구성요소
- etcd - 모든 클러스터 데이터를 담는 저장소(중요). 파드들의 상태 등 중요 데이터 저장. 직접수정 불가. API를 통해서만 통신가능.
- 스케쥴러 - 파드 생성을 관리하는 스케쥴러.
- 컨트롤러 - 파드들의 상태를 감시하고, 잘못되었을 시 원하는 상태로 변경하기 위해 노력하는 객체.

#### 노드 컴포넌트 구성요소
- kubelet - 노드에서 실행되는 쿠버네티스 에이전트. 노드 안에 실행되는 각 파드에서 컨테이너의 동작 관리.
- kube-proxy - 노드끼리 네트워크 통신을 할 수 있게 해주는 객체.

#### 애드온
- DNS - 필수는 아니지만 대부분 사용. 클러스터 서비스명, 네임스페이스명으로 각 파드들의 주소를 찾아줌.

### 쿠버네티스 객체 종류
[객체 종류 참고자료](https://unit-15.tistory.com/107)
다양한 객체 종류가 있지만, 실습하며 주로 사용했던 것들만 정리.

- *Pod* - 쿠버네티스 최소 배포 단위. 하나의 Pod엔 여러 컨테이너가 들어갈 수도 있음. 보통은 1:1로 사용됨. 
- replicaset - Pod배포 시 복제본. (부하 분산 및 Single failure 방지)
- Deployment - pod + replicaset을 합친 개념. 서비스를 배포하는 하나의 단위.
- Service - Pod(Deployment)를 서비스로 배포하기 위해 사용되는 네트워크 서비스. 
Pod 단일로는 노드 위치나 주소가 일정하지 않기 때문에 서비스를 통해 외부에 노출시킴.
- PV - Persistent Volume. 로컬 등의 저장소를 통칭
- PVC - Percsistent Volume Clain. - 사용자에 의해 어떤 저장소를 사용할지에 대한 명세? 쿠버네티스에 PVC와 옵션을 연결하면, 적절한 PV에 연결해줌.
- Namespace, ns - 네임스페이스. 쿠버네티스에 객체를 생성할 때 네임스페이스로 영역을 나눠서 생성할 수 있음. 네임스페이스 삭제 시 해당 객체 전부 삭제됨.
- Statefullset - DB, kafka 등 pod가 재시작되었을 때 내부 정보를 유지해야 하는 경우 생성하는 객체.
- Configmap - Pod 내부에서 사용되는 환경 변수들을 따로 관리하기 위한 객체.
- Secret - DB 접속 패스워드 등 Pod내부에서 사용하는 민감 정보를 따로 관리하기 위한 객체.
- ingress - nginx와 비슷한 개념. 외부에서 접속했을 때 내부 서비스로 맵핑시켜주는 프록시.

### 쿠버네티스 기본 명령어.
[쿠버네티스 오브젝트](https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/kubernetes-objects/)

쿠버네티스 오브젝트는 .yaml파일로 표시되며 해당 파일을 읽어 쿠버네티스에서 파드를 생성한다.

Deployment 필수 spec 예시 yaml파일.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

기본 명령어 셋

(네임스페이스 생성 후 yaml을 통해 객체 생성)

- 네임스페이스 생성
```
kubectl create ns [네임스페이스명]
```

- 해당 네임스페이스로 이동.
```
kubectl config set-context --current --namespace [네임스페이스명]
```

- 네임스페이스로 이동하지 않고 옵션을 통해 사용가능
```
kubectl ~~~~~~ -n [네임스페이스명]
```

- yaml파일을 통해 객체 생성 / 수정사항 반영
```
kubectl apply -f [yaml파일 경로]
```

- 생성된 Pod 조회
```
kubectl get pods -n [네임스페이스명]
```

- 서비스 생성
```
kubectl scale deploy [서비스명] --replicas=[복제본 수] -n [네임스페이스]
```

- 쿠버네티스 pod 직접 접속(bash)
```
kubectl -n [네임스페이스] exec -it [pod명] -- bash
```

- ingress생성
```
kubectl create ingress [인그레스명] -n [네임스페이스 명] --class=nginx --rule="[외부 도메인]/*=[서비스명]:[서비스포트]"
```


- 쿠버네티스 전체 pod 모니터링
```
watch kubectl get all
```
----------------------
# 추가 기능
## helm
- 쿠버네티스에서 도커 허브와 같은 역할을 함. (docker=dockerhub : kubernetes:helm)
- 원하는 서비스(mysql, kafka등)을 다운받아 바로 쿠버네티스에 배포 가능.
- 배포하는 단위를 차트라고 표현.

## istio
- 서비스 메쉬 = L7게이트웨이와 동일한 기능을 한다고 함. (추가공부필요)
- 각 Pods에 사이드카 pod가 같이 떠서 pod끼리 통신을 관제함. (Envoy?)
- kiali, prometeus, jager 등 애드온들과 함께 사용하여 대상 pod들 모니터링 가능.


- istio를 사용하기 위해 대상 네임스페이스에 라벨 부착해야 함.
```
kubectl config set-context --current --namespace [네임스페이스명]
kubectl label namespace [네임스페이스명] istio-injection=enabled
kubectl get ns --show-labels
```

## Argo
- Argo CD
- github 등 외부에 있는 yaml파일들을 쿠버네티스에 배포하고, 변경 사항이 생길 경우 싱크를 맞춰 자동으로 반영되게 해주는 도구.
- 추가 공부 필요.