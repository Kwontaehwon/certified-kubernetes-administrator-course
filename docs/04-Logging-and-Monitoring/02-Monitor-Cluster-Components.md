# 클러스터 구성 요소 모니터링
  - [비디오 튜토리얼](https://kodekloud.com/topic/monitor-cluster-components/)로 이동하기
  
이 섹션에서는 kubernetes 클러스터 모니터링에 대해 살펴보겠습니다.

#### kubernetes에서 리소스 소비를 어떻게 모니터링하나요? 또는 더 중요하게는 무엇을 모니터링하고 싶으신가요?
  ![mon](../../images/mon.PNG)
 
## Heapster vs Metrics Server
- Heapster는 이제 더 이상 사용되지 않으며, **`metrics server`** 라는 축소된 버전이 형성되었습니다.
  ![hpms](../../images/hpms.PNG)
  
## Metrics Server
- In-Memory monitoring solution
  - Metric을 스토리지에 저장하지 않고 메모리에 저장.
  ![ms1](../../images/ms1.PNG)

#### 노드에서 POD의 메트릭은 어떻게 생성되나요?
kubelet 안에 `cAdvisor` 가 있어서 pod의 performace metrics를 수집하여 `Metrics Server`로 expose 한다.
  ![ca](../../images/ca.PNG)
  
## Metrics Server - 시작하기
  ![msg](../../images/msg.PNG)
  
- GitHub 리포지토리에서 metric server를 클론합니다.
  ```
  $ git clone https://github.com/kubernetes-incubator/metrics-server.git
  ```
- metric server를 배포합니다.
  ```
  $ kubectl create -f metric-server/deploy/1.8+/
  ```
  
- 클러스터 성능을 확인합니다.
  ```
  $ kubectl top node
  ```
- 파드의 성능 메트릭을 확인합니다.
  ```
  $ kubectl top pod
  ```
  
  ![view](../../images/view.PNG)
