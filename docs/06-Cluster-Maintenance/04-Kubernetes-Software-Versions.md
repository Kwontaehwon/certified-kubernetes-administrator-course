# Kubernetes 소프트웨어 버전
  - [비디오 튜토리얼](https://kodekloud.com/topic/kubernetes-software-versions/)로 이동하기
  
이 섹션에서는 다양한 Kubernetes 릴리스 및 버전에 대해 살펴보겠습니다.

#### We can see the kubernetess version that we installed
```
$ kubectl get nodes
```
![kgn](../../images/kgn.PNG)

#### 버전 번호를 자세히 살펴보자
- 3개의 부분으로 구성되어 있다
  - 첫 번째는 주요 버전(major version)
  - 두 번째는 부 버전(minor version)
  - 마지막은 패치 버전(patch version)
  
  ![mmp](../../images/mmp.PNG)
  
#### Kubernetes는 표준 소프트웨어 릴리스 버전 관리 절차를 따름.
- 모든 Kubernetes 릴리스를 https://github.com/kubernetes/kubernetes/releases 에서 확인 가능.

  ![r1](../../images/r1.PNG)
  
  ![r2](../../images/r2.PNG)
  
#### 다운로드한 패키지에는 모든 Kubernetes 구성 요소가 포함되어 있지만 **`ETCD Cluster`**와 **`CoreDNS`**는 별도의 프로젝트이므로 포함되어 있지 않다.
- 패키지에 있는 구성요소들의 버전은 당연히 동일.
 ![r3](../../images/r3.PNG)
 
#### 참고 문헌

 - https://blog.risingstack.com/the-history-of-kubernetes/
 - https://kubernetes.io/docs/setup/release/version-skew-policy/ 