# Kube API 서버
  - [비디오 튜토리얼](https://kodekloud.com/topic/kube-api-server/)로 이동하기
  
이 섹션에서는 쿠버네티스의 kube-apiserver에 대해 알아보겠습니다.

#### `Kube-apiserver`는 쿠버네티스의 핵심 구성 요소입니다.
- Kube-apiserver는 **`인증`**, **`유효성 검사`**, ETCD 의 데이터 **`검색`** 및 **`업데이트`** 를 담당
- 실제로 kube-apiserver는 **etcd 와 직접 상호 작용하는 유일한 구성 요소**
- kube-scheduler, kube-controller-manager, kubelet과 같은 다른 구성 요소들은 API 서버를 사용하여 클러스터의 각 영역을 업데이트합니다.
  
  ![post](../../images/post.PNG)
  
## kube-apiserver 설치하기
- **`kubeadm`** 도구를 사용하여 kube-apiserver를 부트스트랩하는 경우에는 이것을 알 필요가 없지만, 수동으로 설정하는 경우 쿠버네티스 릴리스 페이지에서 kube-apiserver 바이너리를 다운로드할 수 있습니다.
  - 예: 여기서 kube-apiserver v1.13.0 바이너리를 다운로드할 수 있습니다 [kube-apiserver](https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver)
    ```
    $ wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver
    ```
 
 ![kube-apiserver](../../images/kube-apiserver.PNG)
 
## kube-apiserver 확인하기 - Kubeadm
- kubeadm은 마스터 노드의 kube-system 네임스페이스에 kube-apiserver를 파드로 배포합니다.
  ```
  $ kubectl get pods -n kube-system
  ```
   
  ![kube-apiserver1](../../images/kube-apiserver1.PNG)
   
## kube-apiserver 옵션 확인하기 - Kubeadm
- **`/etc/kubernetes/manifests/kube-apiserver.yaml`**에 있는 파드 정의 파일에서 옵션을 확인할 수 있습니다.
  ```
  $ cat /etc/kubernetes/manifests/kube-apiserver.yaml
  ```
  
  ![kube-apiserver2](../../images/kube-apiserver2.PNG)
   
## kube-apiserver 옵션 확인하기 - 수동 설치
- kubeadm을 사용하지 않는 설정에서는 kube-apiserver.service를 확인하여 옵션을 검사할 수 있습니다.
  ```
  $ cat /etc/systemd/system/kube-apiserver.service
  ```
  
  ![kube-apiserver3](../../images/kube-apiserver3.PNG)
   
- 마스터 노드에서 프로세스를 나열하고 kube-apiserver를 검색하여 실행 중인 프로세스와 유효한 옵션을 확인할 수도 있습니다.
  ```
  $ ps -aux | grep kube-apiserver
  ```
  ![kube-apiserver4](../../images/kube-apiserver4.PNG)

K8s 참조 문서:
- https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/
- https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/
