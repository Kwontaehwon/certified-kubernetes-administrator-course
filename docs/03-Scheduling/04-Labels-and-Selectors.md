# 레이블과 셀렉터
  - [비디오 튜토리얼]로 이동하기 (https://kodekloud.com/topic/labels-and-selectors/)
  
이 섹션에서는 **`레이블과 셀렉터`**에 대해 살펴보겠습니다.

#### 레이블과 셀렉터는 사물을 그룹화하는 표준 방법입니다.
  
#### 레이블은 각 항목에 부착된 속성입니다.

  ![labels-ckc](../../images/labels-ckc.PNG)
  
#### 셀렉터는 이러한 항목을 필터링하는 데 도움을 줍니다.
 
  ![sl](../../images/sl.PNG)
  
Kubernetes에서 레이블과 셀렉터는 어떻게 사용됩니까?
- 우리는 **`PODs`**, **`ReplicaSets`**, **`Deployments`** 등과 같은 다양한 유형의 객체를 Kubernetes에서 생성했습니다.
  
  ![ls](../../images/ls.PNG)
  
레이블을 어떻게 지정합니까?
   ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: simple-webapp
     labels:
       app: App1
       function: Front-end
    spec:
     containers:
     - name: simple-webapp
       image: simple-webapp
       ports:
       - containerPort: 8080
   ```
 ![lpod](../../images/lpod.PNG)
 
Once the pod is created, to select the pod with labels run the below command
```
$ kubectl get pods --selector app=App1
```

Kubernetes uses labels to connect different objects together
   ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: simple-webapp
      labels:
        app: App1
        function: Front-end
    spec:
     replicas: 3
     selector:
       matchLabels:
        app: **App1**
     template:
       metadata:
         labels:
           app: **App1**
           function: Front-end
       spec:
         containers:
         - name: simple-webapp
           image: simple-webapp   
   ```

  ![lrs](../../images/lrs.PNG)

For services
 
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: my-service
      spec:
       selector:
         app: App1
       ports:
       - protocol: TCP
         port: 80
         targetPort: 9376 
       ```
  ![lrs1](../../images/lrs1.PNG)
  
## Annotations
# Start of Selection
- 레이블과 셀렉터는 객체를 그룹화하는 데 사용되는 반면, Annotation은 정보 제공을 위한 다른 세부 정보를 기록하는 데 사용됩니다.
    ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: simple-webapp
      labels:
        app: App1
        function: Front-end
      annotations:
         buildversion: 1.34
    spec:
     replicas: 3
     selector:
       matchLabels:
        app: App1
    template:
      metadata:
        labels:
          app: App1
          function: Front-end
      spec:
        containers:
        - name: simple-webapp
          image: simple-webapp   
    ```
  ![annotations](../../images/annotations.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
