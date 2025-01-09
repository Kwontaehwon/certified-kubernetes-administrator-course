# Node Selectors
  - [비디오 튜토리얼](https://kodekloud.com/topic/node-selectors/)로 이동하기

이 섹션에서는 Kubernetes의 Node Selectors에 대해 살펴보겠습니다.

#### spec 섹션에 `nodeSelector`라는 새로운 속성을 추가하고 레이블을 지정합니다.
- 스케줄러는 이러한 레이블을 사용하여 파드를 배치할 적절한 노드를 일치시키고 식별합니다.
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
![nsel](../../images/nsel.PNG)
  
- 노드에 레이블을 추가하는 방법

  구문
  ```
  $ kubectl label nodes <node-name> <label-key>=<label-value>
  ```
  예시
  ```
  $ kubectl label nodes node-1 size=Large
  ```
  
![ln](../../images/ln.PNG)
  
- 파드 정의를 생성하는 방법
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
  ```
  $ kubectl create -f pod-definition.yml
  ```
  
![nsel](../../images/nsel.PNG)
  
## Node Selector - 제한 사항
- 우리는 목표를 달성하기 위해 단일 레이블과 선택기를 사용했습니다. 하지만 요구 사항이 훨씬 더 복잡하다면 어떻게 될까요? *OR, NOT 조건
  - 예를 들어, Medium 또는 Large 노드에 배치하고 싶을 때는 nodeSelector로는 불가능함.
    - -> 이를 위해 Node Affinity와 Node Anti Affinity가 있음.
  
![nsl](../../images/nsl.PNG)
 
- 이를 위해 **`Node Affinity`**와 **`Anti Affinity`**가 있습니다.
  
#### K8s 참조 문서
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector
