# Rolling Updates and Rollback
  - [비디오 튜토리얼](https://kodekloud.com/topic/rolling-updates-and-rollbacks/)로 이동하기
  
이 섹션에서는 배포에서 롤링 업데이트와 롤백을 살펴보겠습니다.

## Rollout 및 버전 관리
  ![rollv](../../images/rollv.PNG)
  
## Rollout 명령어
- 아래 명령어로 롤아웃 상태를 확인할 수 있습니다.
  ```
  $ kubectl rollout status deployment/myapp-deployment
  ```
- 이력 및 수정 사항을 보려면
  ```
  $ kubectl rollout history deployment/myapp-deployment
  ```
 
  ![rollc](../../images/rollc.PNG)
  
## 배포 전략
- 배포 전략에는 2가지 유형이 있습니다.
  1. Recreate
	  1. 새로운 Pod가 배포되기전에 서비스가 제공되지 못할수도 있음.
  2. **RollingUpdate (Default)**
	  1. One-by-One 으로 차근차근 업데이트
  
  ![dst](../../images/dst.PNG)
  
## kubectl apply
- 배포를 업데이트하려면 배포를 편집하고 필요한 변경 사항을 저장한 후 아래 명령어를 실행합니다.
  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
   name: myapp-deployment
   labels:
    app: nginx
  spec:
   template:
     metadata:
       name: myap-pod
       labels:
         app: myapp
         type: front-end
     spec:
      containers:
      - name: nginx-container
        image: nginx:1.7.1
   replicas: 3
   selector:
    matchLabels:
      type: front-end       
  ```
  ```
  $ kubectl apply -f deployment-definition.yaml
  ```
- 이미지를 업데이트하는 예를 들어 배포를 업데이트하는 대체 방법.
	- 파일과 현재 Pod 상태가 달라지게 됨
  ```
  $ kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
  ```
  ![ka](../../images/ka.PNG)
  
## Recreate vs RollingUpdate
  - Recreate
	  - Scale Down to 0 -> Scale Up to 5 (한번에)
  - RollingUpdate
	  - 차근차근 하나씩 Down, Up
  ![rcrl](../../images/rcrl.PNG)
  
## 업그레이드
1. 새로운 Replicaset 을 생성
2. 이전 Replicaset 의 POD 하나 삭제
3. 새로운 POD를 1번에서 만든 Replicaset에 생성
4. 새로운 Replicaset에 최초 Replicaset의 POD 갯수가 똑같아 질때까지 2-3번 반복
  ![up](../../images/up.PNG)
  
## 롤백
  ![rb](../../images/rb.PNG)
  
- 변경 사항을 되돌리려면
  ```
  $ kubectl rollout undo deployment/myapp-deployment
  ```
  
## kubectl create
- Deployment를 생성하려면
  ```
  $ kubectl create deployment nginx --image=nginx
  ```
## Summarize kubectl commands
```
$ kubectl create -f deployment-definition.yaml
$ kubectl get deployments
$ kubectl apply -f deployment-definition.yaml
$ kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
$ kubectl rollout status deployment/myapp-deployment
$ kubectl rollout history deployment/myapp-deployment
$ kubectl rollout undo deployment/myapp-deployment
```

![sum](../../images/sum.PNG)
 
#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment
- https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment
