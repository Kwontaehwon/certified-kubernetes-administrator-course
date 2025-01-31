# ReplicaSets
  - [비디오 튜토리얼](https://kodekloud.com/topic/replicasets/)로 이동하기

이 섹션에서는 다음 내용을 살펴보겠습니다
- Replication Controller
- ReplicaSet

#### 컨트롤러는 쿠버네티스의 두뇌입니다

## Replica란 무엇이며 왜 replication controller가 필요한가요?
  ![rc](../../images/rc.PNG)
  
  저장된 수의 Pod가 항상 실행중인지를 확인 함.
  ![rc1](../../images/rc1.PNG)

  Replication Controller는 여러 노드에 걸쳐있을 수 있음.
  
## ReplicaSet과 Replication Controller의 차이점
- **`Replication Controller`** 는 **`ReplicaSet`** 으로 대체되고 있는 이전 기술입니다.
- **`ReplicaSet`** 은 복제를 설정하는 새로운 방법입니다.

## Replication Controller 생성하기

### Replication Controller yaml
  
   ![rc2](../../images/rc2.PNG)
  
```
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: myapp-rc
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
```
`template` 부분은 파드 정의 파일과 동일함.
  - Replication Controller를 생성하려면
    ```
    $ kubectl create -f rc-definition.yaml
    ```
  - 모든 replication controller를 나열하려면
    ```
    $ kubectl get replicationcontroller
    ```
  - Replication controller가 실행한 파드들을 나열하려면
    ```
    $ kubectl get pods
    ```
    ![rc3](../../images/rc3.PNG)
    
## ReplicaSet 생성하기
### ReplicaSet yaml

   ![rs](../../images/rs.PNG)

```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```

#### ReplicaSet은 Replication Controller와 비교했을 때 selector 정의가 필요합니다.
#### 또한 apiVersion이 apps/v1 임.
   
  - ReplicaSet을 생성하려면
    ```
    $ kubectl create -f replicaset-definition.yaml
    ```
  - 모든 replicaset을 나열하려면
    ```
    $ kubectl get replicaset
    ```
  - ReplicaSet이 실행한 파드들을 나열하려면
    ```
    $ kubectl get pods
    ```
   
    ![rs1](../../images/rs1.PNG)
    
## 레이블과 셀렉터
#### 레이블과 셀렉터는 무엇이고, 왜 쿠버네티스에서 파드와 객체에 레이블을 붙이나요?
ReplicaSet은 이미 생성되어 있는 파드에도 `Selector`를 이용하여 ReplicaSet을 생성할 수 있음. <br>
-> 결국 ReplicaSet은 파드를 모니터링하고 관리하는 역할을 함.
  ![labels](../../images/labels.PNG) ^xenz1a
  
## ReplicaSet을 스케일하는 방법
- ReplicaSet을 스케일하는 방법에는 여러 가지가 있습니다
  - 첫 번째 방법은 replicaset-definition.yaml 정의 파일에서 replicas 수를 업데이트하는 것입니다. 예: replicas: 6으로 변경 후 실행
```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 6
     selector:
       matchLabels:
        type: front-end
```

  ```
  $ kubectl apply -f replicaset-definition.yaml
  ```
  - 두 번째 방법은 **`kubectl scale`** 명령어를 사용하는 것입니다.
    - **이 방법은 .yaml 파일을 수정하는 것 이 아님.**
  ```
  $ kubectl scale --replicas=6 -f replicaset-definition.yaml
  ```
  
  - 세 번째 방법은 타입과 이름으로 **`kubectl scale`** 명령어를 사용하는 것입니다.
  ```
  $ kubectl scale --replicas=6 replicaset myapp-replicaset
  ```
  ![rs2](../../images/rs2.PNG)

#### K8s 참조 문서:
- https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
