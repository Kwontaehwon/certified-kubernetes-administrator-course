# 애플리케이션 로그 관리
  - [비디오 튜토리얼](https://kodekloud.com/topic/managing-application-logs/)로 이동하기

이 섹션에서는 애플리케이션 로그 관리에 대해 살펴보겠습니다.

#### 도커에서 로깅 시작하기
![ld](../../images/ld.PNG)
 - `-d` 옵션은 `daemon` 옵션으로서 백그라운드에서 실행됨.
 - `-f` 옵션은 라이브로 컨테이너의 옵션을 확인할 수 있음.
![ld1](../../images/ld1.PNG)
 
#### Logs - Kubernetes
- Docker와 매우 비슷함.
```
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator-pod
spec:
  containers:
  - name: event-simulator
    image: kodekloud/event-simulator
```
 ![logs-k8s](../../images/logs-k8s.png)
 
- To view the logs
  ```
  $ kubectl logs -f event-simulator-pod
  ```
- 파드 안에 여러 컨테이너가 있을경우 반드시 컨테이너 이름을 명시해야함.
  ```
  $ kubectl logs -f <pod-name> <container-name>
  $ kubectl logs -f even-simulator-pod event-simulator
  ```

  ![logs1](../../images/logs1.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/blog/2015/06/cluster-level-logging-with-kubernetes/
 
