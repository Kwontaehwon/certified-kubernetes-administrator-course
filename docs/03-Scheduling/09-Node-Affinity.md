# Node Affinity
  - [비디오 튜토리얼](https://kodekloud.com/topic/node-affinity-2/)로 이동하기
  
이 섹션에서는 Kubernetes의 "Node Affinity" 기능에 대해 설명합니다.

#### Node Affinity의 주요 기능은 파드가 특정 노드에 호스팅되도록 보장하는 것입니다.
- **`nodeSelector`** 를 사용하면 고급 표현식을 제공할 수 없습니다.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   nodeSelector:
    size: Large
  ```
  #### `In` : 해당 값이 포함된 노드를 선택

  ![ns-old](../../images/ns-old.PNG)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In
              values: 
              - Large
              - Medium
  ```
  ![na](../../images/na.PNG)
  
  #### `NotIn` : 해당 값이 포함되지 않은 노드를 선택
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: NotIn
              values: 
              - Small
  ```
  ![na1](../../images/na1.PNG)
  
  #### `Exists` : 노드에 레이블 값이 존재하는지 만 확인 (값은 상관 X)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: Exists
  ```
  
  ![na2](../../images/na2.PNG)
  

## Node Affinity 유형
- Availiable
  - requiredDuringSchedulingIgnoredDuringExecution
  - preferredDuringSchedulingIgnoredDuringExecution
- Planned
  - requiredDuringSchedulingRequiredDuringExecution
  - preferredDuringSchedulingRequiredDuringExecution
  
  ![nat](../../images/nat.PNG)
  
## Node Affinity 유형 상태
- `required`
  - 매칭되는 게 없으면 스케쥴 X
- `preferred`
  - 매칭되는 게 없어도 스케쥴 O

- `IgnoredDuringExecution`
  - 실행중일 때 노드 레이블이 변경 시 **파드 변경 없음**
- `RequiredDuringExecution`
  - 실행중일 때 노드 레이블이 변경 시 **파드 변경**

  ![nats](../../images/nats.PNG)
    ![nats1](../../images/nats1.PNG)
  
#### K8s 참조 문서
- https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
