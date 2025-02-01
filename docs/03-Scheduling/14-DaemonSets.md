# DaemonSets
  - [비디오 튜토리얼](https://kodekloud.com/topic/daemonsets/)로 이동하기

이 섹션에서는 DaemonSets에 대해 살펴보겠습니다.

#### DaemonSets는 ReplicaSets와 유사하며, 여러 인스턴스의 파드를 배포하는 데 도움을 줍니다. 하지만 클러스터의 각 노드에서 파드의 한 복사본을 실행합니다.
  ![ds](../../images/ds.PNG)
  
## DaemonSets - 사용 사례
모니터링 or 로그 뷰어 같이 모든 노드에 하나씩은 있어야 하는 상황에 적절함.  ![ds-uc](../../images/ds-uc.PNG)
![ds-uc-kp](../../images/ds-uc-kp.PNG)
![ds-ucn](../../images/ds-ucn.PNG)
  
## DaemonSets - 정의
- DaemonSet을 만드는 것은 ReplicaSet 생성 과정과 유사합니다.
- DaemonSets의 경우, apiVersion, kind를 **`DaemonSet`** 으로 시작하고, 메타데이터와 사양을 설정합니다. 
  ```
  apiVersion: apps/v1
  kind: Replicaset
  metadata:
    name: monitoring-daemon
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: monitoring-agent
    template:
      metadata:
       labels:
         app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
  ```
  
  ```
  apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    name: monitoring-daemon
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: monitoring-agent
    template:
      metadata:
       labels:
         app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
  ```
  ![dsd](../../images/dsd.PNG)
  
- 정의 파일에서 DaemonSet을 생성하려면
  ```
  $ kubectl create -f daemon-set-definition.yaml
  ```

## DaemonSets 보기
- DaemonSets 목록을 보려면
  ```
  $ kubectl get daemonsets
  ```
- DaemonSets에 대한 자세한 내용을 보려면
  ```
  $ kubectl describe daemonsets monitoring-daemon
  ```
  ![ds1](../../images/ds1.PNG)
  
## DaemonSets 작동 방식
- ~ v1.12 : [02-Manual-Scheduling](02-Manual-Scheduling.md)(`nodeName`) 
- v.1.13 ~ : Use default schedular with [09-Node-Affinity](09-Node-Affinity.md)
  ![ds2](../../images/ds2.PNG)

#### K8s 참조 문서
- https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/#writing-a-daemonset-spec
