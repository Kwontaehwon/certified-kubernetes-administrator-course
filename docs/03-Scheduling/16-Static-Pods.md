# 정적 파드 
  - [비디오 튜토리얼](https://kodekloud.com/topic/static-pods/)로 이동하기
> [!NOTE] Title
> kube-api-server 혹은 다른 k8s 클러스터 요소의 개입 없이 만들어진 Pod

#### kube-apiserver 없이 kubelet에 파드 정의 파일을 제공하는 방법은 무엇인가요?
- kubelet이 파드에 대한 정보를 저장하기 위해 지정된 서버의 디렉토리에서 파드 정의 파일을 읽도록 구성할 수 있습니다.
- Static Pod 로는 Pod 만 생성할 수 있으며, replica-set, deployment 등은 생성할 수 없음.
	- kubelet은 pod 레벨만 이해함.

## 정적 파드 구성 : 옵션으로 경로 전달
- 지정된 디렉토리는 호스트의 어떤 디렉토리든 될 수 있으며, 해당 디렉토리의 위치는 서비스를 실행할 때 kubelet에 옵션으로 전달됩니다.
  - 이 옵션의 이름은 **`--pod-manifest-path`** 입니다.
  ![sp](../../images/sp.PNG)
  
## 정적 파드를 구성하는 또 다른 방법  : 옵션으로 파일 전달
- **`kubelet.service`** 파일에 옵션을 직접 지정하는 대신, 구성 옵션을 사용하여 다른 구성 파일의 경로를 제공하고, 파일 내에서 디렉토리 경로를 staticPodPath로 정의할 수 있습니다.
- `kubeadm` 으로 만들어진 kubelet은 이를 사용함.
  ![sp1](../../images/sp1.PNG)

## 정적 파드 보기
- 정적 파드를 보려면
  ```
  $ docker ps
  ```
  kubectl은 kube-apiserver로 동작하므로 이것이 없을 경우 container runtime 커맨드를 사용해야함.
  ![sp2](../../images/sp2.PNG)

## Static Pod 생성 : POST 요청 이용
kubelet은 정적 파드와 api 서버에서 생성된 파드를 동시에 생성할 수 있음.
- `kubectl get pods` 로 보는 것은 static pods의 미러일 뿐.
	- kubectl 명령으로 삭제만 가능하고 편집 등은 불가능함.
![sp3](../../images/sp3.PNG)

## 정적 파드 - 사용 사례
- controller-manager, kube-api-server, etcd 등의 Control Plane 구성요소를 Static Pod로 생성할 경우 Binary를 다운로드 하지 않아도 kubelet에 의해 자동으로 생성되고 관리됨.
  ![sp4](../../images/sp4.PNG)
  
  ![sp5](../../images/sp5.PNG)
  
## Static PODs vs DaemonSets
두개 모두 스케쥴러에 의해 무시 됨.
- Static PODs
	- kubelet에 의해 생성
	- Control Plane Component 해당
- Daemon Sets
	- Kube-api-server 에 의해 생성
	- 모니터링 Agent 등에 사용
![spvsds](../../images/spvsds.PNG)
  

#### K8s 참조 문서
- https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
