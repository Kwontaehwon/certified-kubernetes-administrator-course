# 수동 스케줄링
  - [비디오 튜토리얼](https://kodekloud.com/topic/manual-scheduling/)로 이동하기
  
이 섹션에서는 **`POD`**를 노드에 **`수동으로 스케줄링`**하는 방법을 살펴보겠습니다.

## 스케줄링 작동 방식
#### Manual Scheduling 은 Pod 를 생성할 때만 적용 가능
- 클러스터에 스케줄러가 없을 때 어떻게 해야 하나요?
  - 모든 POD에는 기본적으로 설정되지 않은 NodeName이라는 필드가 있습니다. 매니페스트 파일을 생성할 때 이 필드를 일반적으로 지정하지 않으며, 쿠버네티스가 자동으로 추가합니다.
  - 식별된 후, 바인딩 객체를 생성하여 nodeName 속성을 노드의 이름으로 설정하여 POD를 노드에 스케줄링합니다.
    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx
     labels:
      name: nginx
    spec:
     containers:
     - name: nginx
       image: nginx
       ports:
       - containerPort: 8080
     nodeName: node02
    ```
    ![sc1](../../images/sc1.png)
    
## 스케줄러 없음
  - POD를 노드에 수동으로 할당할 수 있습니다. 스케줄러 없이 POD를 스케줄링하려면 POD 정의 파일에서 **`nodeName`** 속성을 설정해야 합니다.
    
    ![sc2](../../images/sc2.PNG)
    
  - 또 다른 방법
    ```
    apiVersion: v1
    kind: Binding
    metadata:
      name: nginx
    target:
      apiVersion: v1
      kind: Node
      name: node02
    ```
    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx
     labels:
      name: nginx
    spec:
     containers:
     - name: nginx
       image: nginx
       ports:
       - containerPort: 8080
    ```
    ![sc3](../../images/sc3.PNG)
    #### Pod가 생성된 이후에는 `nodeName` 속성을 변경할 수 없으므로 POST 요청을 통해 스케쥴러의 역할을 따라할 수 있음.
    
K8s 참조 문서:
- https://kubernetes.io/docs/reference/using-api/api-concepts/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename
