# 클러스터 업그레이드 소개
  - [비디오 튜토리얼](https://kodekloud.com/topic/cluster-upgrade-introduction/)로 이동하기
  
#### 모든 Kubernetes 구성 요소가 동일한 버전을 가져야 하나?
- 아니요, 구성 요소는 서로 다른 릴리스 버전을 가질 수 있다.
  
#### Kubernetes는 항상 최근 3개의 부 버전(minor version)만 지원한다
- 권장하는 방법은 한 번에 하나의 부 버전(minor version)씩 업그레이드하는 것이다.
  
  ![up2](../../images/up2.PNG)
  
#### k8s 클러스터 업그레이드 옵션 
![opt](../../images/opt.PNG)
  
## 클러스터 업그레이드
- 클러스터 업그레이드는 2개의 주요 단계를 포함한다.
  
#### 워커 노드를 업그레이드하는 다양한 전략이 있다
- 첫 번째는 모든 노드를 한 번에 업그레이드하는 것이다. 그러나 이 경우 포드(pod)가 중단되고 사용자가 애플리케이션에 접근할 수 없게 된다.
  ![stg1](../../images/stg1.PNG)
- 두 번째는 한 번에 하나의 노드를 업그레이드하는 것이다. 
  ![stg2](../../images/stg2.PNG)
- 세 번째는 클러스터에 새로운 노드를 추가하는 것이다.
  ![stg3](../../images/stg3.PNG)
  
## kubeadm - 마스터 노드 업그레이드
- kubeadm에는 클러스터 업그레이드를 돕는 업그레이드 명령이 있다.
  ```
  $ kubeadm upgrade plan
  ```
  ![kube1](../../images/kube1.png)
  
- kubeadm을 v1.11에서 v1.12로 업그레이드
  ```
  $ apt-get upgrade -y kubeadm=1.12.0-00
  ```
- 클러스터 업그레이드
  ```
  $ kubeadm upgrade apply v1.12.0
  ```
- `kubectl get nodes` 명령을 실행하면 이전 버전을 볼 수 있다. 이는 명령의 출력에서 API 서버에 등록된 각 노드의 kubelet 버전을 보여주고 API 서버 자체의 버전은 보여주지 않기 때문이다.  
  ```
  $ kubectl get nodes
  ```
  
  ![kubeu](../../images/kubeu.PNG)
  
- 마스터 노드에서 'kubelet' 업그레이드
  ```
  $ apt-get upgrade kubelet=1.12.0-00
  ```
- kubelet 재시작
  ```
  $ systemctl restart kubelet
  ```
- 'kubectl get nodes'를 실행하여 확인
  ```
  $ kubectl get nodes
  ```
  
  ![kubeu1](../../images/kubeu1.PNG)
 
## kubeadm - 워커 노드 업그레이드
  
- 마스터 노드에서 'kubectl drain' 명령을 실행하여 작업 부하를 다른 노드로 이동
  ```
  $ kubectl drain node-1
  ```
- kubeadm 및 kubelet 패키지 업그레이드
  ```
  $ apt-get upgrade -y kubeadm=1.12.0-00
  $ apt-get upgrade -y kubelet=1.12.0-00
  ```
- 새로운 kubelet 버전에 대한 노드 구성 업데이트
  ```
  $ kubeadm upgrade node config --kubelet-version v1.12.0
  ```
- kubelet 서비스 재시작
  ```
  $ systemctl restart kubelet
  ```
- 노드를 다시 스케줄 가능 상태로 표시
  ```
  $ kubectl uncordon node-1
  ```
  
  ![kubeu2](../../images/kubeu2.PNG)
  
- 모든 워커 노드를 같은 방식으로 업그레이드
  ![kubeu3](../../images/kubeu3.PNG)
  

#### 클러스터 업그레이드에 대한 [데모 비디오](https://kodekloud.com/topic/demo-cluster-upgrade/) 

#### K8s 참조 문서
- https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
- https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/  