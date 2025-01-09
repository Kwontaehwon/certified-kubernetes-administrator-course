# 여러 스케줄러 
  - [비디오 튜토리얼](https://kodekloud.com/topic/multiple-schedulers/)로 이동하기

이 섹션에서는 Multiple Schedulers 에 대해 살펴보겠습니다.

## 사용자 정의 스케줄러
- 귀하의 kubernetes 클러스터는 동시에 여러 스케줄러를 스케줄링할 수 있습니다.

  ![ms](../../images/ms.PNG)
  
## 추가 스케줄러 배포
- 바이너리 다운로드
  ```
  $ wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler
  ```
  ![das](../../images/das.PNG)
  
## 추가 스케줄러 배포 - kubeadm
- `leaderElect`
  ![dask](../../images/dask.PNG)
  
  - 스케줄러 파드를 생성하려면
    ```
    $ kubectl create -f my-custom-scheduler.yaml
    ```
  
## 스케줄러 보기
- 스케줄러 파드를 나열하려면
  ```
  $ kubectl get pods -n kube-system
  ```

## 사용자 정의 스케줄러 사용
- 파드 정의 파일을 생성하고 **`schedulerName`**이라는 새 섹션을 추가한 다음 새 스케줄러의 이름을 지정합니다.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
  spec:
    containers:
    - image: nginx
      name: nginx
    schedulerName: my-custom-scheduler
  ```
  ![cs](../../images/cs.png)
  
- 파드 정의를 생성하려면
  ```
  $ kubectl create -f pod-definition.yaml
  ```
- 파드 목록을 보려면
  ```
  $ kubectl get pods
  ```

## 이벤트 보기
- 이벤트를 보려면
  ```
  $ kubectl get events
  ```
  ![cs1](../../images/cs1.PNG)
  
## 스케줄러 로그 보기
- 스케줄러 로그를 보려면
  ```
  $ kubectl logs my-custom-scheduler -n kube-system
  ```
  ![cs2](../../images/cs2.PNG)
  
#### K8s 참조 문서
- https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/
