# Kubelet
  - [비디오 튜토리얼](https://kodekloud.com/topic/kubelet/)로 이동하기
  
이 섹션에서는 kubelet에 대해 알아보겠습니다.

#### Kubelet은 쿠버네티스 클러스터의 유일한 접점입니다
- **`kubelet`** 은 노드에 파드를 생성합니다. 스케줄러는 단지 어떤 파드가 어디에 배치될지만 결정합니다.
  ![kubelet](../../images/kubelet.PNG)
  
## Kubelet 설치
- Kubeadm은 기본적으로 kubelet을 배포하지 않습니다. 수동으로 다운로드하고 설치해야 합니다.
  - 다른 구성요소와의 차이점
- 쿠버네티스 릴리스 페이지에서 kubelet 바이너리를 다운로드하세요 [kubelet](https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet). 예를 들어 kubelet v1.13.0을 다운로드하려면 다음 명령어를 실행하세요.
  ```
  $ wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet
  ```
- 압축을 해제하세요
- 서비스로 실행하세요

  ![kubelet1](../../images/kubelet1.PNG)
  
## Kubelet 옵션 확인
- 워커 노드에서 프로세스를 나열하고 kubelet을 검색하여 실행 중인 프로세스와 적용된 옵션을 확인할 수 있습니다.
  ``` 
  $ ps -aux |grep kubelet
  ```
  
  ![kubelet2](../../images/kubelet2.PNG)

K8s 참조 문서:
- https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/kubelet-integration/
