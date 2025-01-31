# Kube Controller Manager
  - [비디오 튜토리얼](https://kodekloud.com/topic/kube-controller-manager/)로 이동하기
이 섹션에서는 kube-controller-manager에 대해 알아보겠습니다.

#### `Kube Controller Manager`는 쿠버네티스의 다양한 컨트롤러들을 관리합니다.
- 쿠버네티스에서 컨트롤러란 시스템 내 구성 요소들의 상태를 지속적으로 모니터링하고, 전체 시스템을 원하는 작동 상태로 만들기 위해 노력하는 프로세스입니다.

## Node Controller
   - 노드의 상태를 모니터링하고 애플리케이션이 계속 실행되도록 필요한 조치를 취하는 역할을 합니다. 
   ![node-controller](../../images/node-controller.PNG)
   
## Replication Controller
   - ReplicaSet의 상태를 모니터링하고 설정된 수의 파드가 항상 사용 가능하도록 보장하는 역할을 합니다.
   ![replication-controller](../../images/replication-controller.PNG)
   
## 기타 Controller
   - 쿠버네티스 내에는 이외에도 많은 컨트롤러들이 있습니다 
   ![other-controllers](../../images/other-controllers.PNG)
   
   
## Kube-Controller-Manager 설치하기
  - kube-controller-manager를 설치하면 다른 컨트롤러들도 함께 설치됩니다.
  - 쿠버네티스 릴리스 페이지에서 kube-controller-manager 바이너리를 다운로드하세요. 예: kube-controller-manager v1.13.0을 여기서 다운로드할 수 있습니다 [kube-controller-manager](https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-controller-manager)
    ```
    $ wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-controller-manager
    ```
  - 기본적으로 모든 컨트롤러가 활성화되어 있지만, **`kube-controller-manager.service`** 에서 특정 컨트롤러만 활성화하도록 선택할 수 있습니다
    ```
    $ cat /etc/systemd/system/kube-controller-manager.service
    ```
    ![kube-controller-manager](../../images/kube-controller-manager.PNG)
    
## kube-controller-manager 확인하기 - kubeadm
- kubeadm은 kube-controller-manager를 kube-system 네임스페이스에 파드로 배포합니다
  ```
  $ kubectl get pods -n kube-system
  ```
  ![kube-controller-manager0](../../images/kube-controller-manager0.PNG)
  
## kube-controller-manager 옵션 확인하기 - kubeadm
- **`/etc/kubernetes/manifests/kube-controller-manager.yaml`** 에 있는 파드 내에서 옵션을 확인할 수 있습니다
  ```
  $ cat /etc/kubernetes/manifests/kube-controller-manager.yaml
  ```
  ![kube-controller-manager1](../../images/kube-controller-manager1.PNG)
  
## kube-controller-manager 옵션 확인하기 - 수동 설치
- kubeadm을 사용하지 않는 설정에서는 **`kube-controller-manager.service`** 를 확인하여 옵션을 검사할 수 있습니다
  ```
  $ cat /etc/systemd/system/kube-controller-manager.service
  ```
  ![kube-controller-manager2](../../images/kube-controller-manager2.PNG)
  
- 마스터 노드에서 프로세스를 나열하고 kube-controller-manager를 검색하여 실행 중인 프로세스와 유효한 옵션을 확인할 수도 있습니다
  ```
  $ ps -aux | grep kube-controller-manager
  ```
  ![kube-controller-manager3](../../images/kube-controller-manager3.PNG)
  
K8s 참조 문서:
- https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/
- https://kubernetes.io/docs/concepts/overview/components/
   
     
     
