# Deployments
  - [비디오 튜토리얼](https://kodekloud.com/topic/deployments-3/)로 이동하기

이 섹션에서는 쿠버네티스 deployment에 대해 알아보겠습니다

#### Deployment는 쿠버네티스 오브젝트입니다. 
pods, replica-set 보다 상위 개념
#### 기능
- 롤링 업데이트
- 롤백
- 정지
- Resume

등 지원
  
 ![deployment](../../images/deployment.PNG)

#### deployment를 어떻게 생성하나요?
replica-set 에서 `kind` 만 Deployment로 바꾸면 됨.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
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
- 파일이 준비되면 deployment 정의 파일을 사용하여 deployment를 생성합니다
  ```
  $ kubectl create -f deployment-definition.yaml
  ```
- 생성된 deployment를 보려면
  ```
  $ kubectl get deployment
  ```
- deployment는 자동으로 **`ReplicaSet`** 을 생성합니다. ReplicaSet을 보려면
  ```
  $ kubectl get replicaset
  ```
- ReplicaSet은 최종적으로 **`POD`** 들을 생성합니다. POD들을 보려면
  ```
  $ kubectl get pods
  ```
  ![deployment1](../../images/deployment1.PNG)
  
- 모든 오브젝트를 한 번에 보려면
  ```
  $ kubectl get all
  ```
  ![deployment2](../../images/deployment2.PNG)
  
K8s 참조 문서:
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
