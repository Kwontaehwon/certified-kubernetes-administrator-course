# Security Context
  - [비디오 튜토리얼](https://kodekloud.com/topic/security-contexts-2/)로 이동
  
이 섹션에서는 security context(보안 컨텍스트)에 대해 살펴본다.


> [!NOTE]
> - POD 수준에서 보안 설정하면 Conatiner Lvevel 로 전달.
> - POD와 Container 둘 다 에서 보안설정 할 경우 **Container Configuration이 우선 순위**

## Container Security(컨테이너 보안)
 ```
 $ docker run --user=1001 ubuntu sleep 3600
 $ docker run --cap-add MAC_ADMIN ubuntu
 ```
 
 ![csec](../../images/csec.PNG)
 
## Kubernetes Security(쿠버네티스 보안)
- 컨테이너 수준 또는 포드 수준에서 보안 설정을 구성할 수 있다.

 ![ksec](../../images/ksec.PNG)

## Security Context(보안 컨텍스트)
- 컨테이너에 보안 컨텍스트를 추가하려면 spec 섹션 아래에 **`securityContext`** 필드를 추가한다.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: web-pod
  spec:
    securityContext:
      runAsUser: 1000
    containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
  ```
  ![sxc1](../../images/sxc1.PNG)
  
- 동일한 컨텍스트를 컨테이너 수준에서 설정하려면 전체 섹션을 컨테이너 섹션 아래로 이동한다.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: web-pod
  spec:
    containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
  ```
  ![sxc2](../../images/sxc2.PNG)
  
- capabilities(권한)을 추가하려면 **`capabilities`** 옵션을 사용한다.
	- 컨테이너 옵션으로만 줄 수 있고 POD 단위에선 안됨.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: web-pod
  spec:
    containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities: 
          add: ["MAC_ADMIN"]
  ```
  ![cap](../../images/cap.PNG)
  
  
### K8s Reference Docs(쿠버네티스 참조 문서)
- https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
