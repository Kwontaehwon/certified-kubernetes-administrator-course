# Kube Proxy
- [비디오 튜토리얼](https://kodekloud.com/topic/kube-proxy/)로 이동하기

이 섹션에서는 kube-proxy에 대해 알아보겠습니다.

쿠버네티스 클러스터 내에서 모든 파드는 다른 모든 파드에 도달할 수 있습니다. 이는 클러스터에 파드 네트워킹 클러스터를 배포함으로써 달성됩니다. 
- Kube-Proxy는 쿠버네티스 클러스터의 각 노드에서 실행되는 프로세스입니다.
  ![kube-proxy](../../images/kube-proxy.PNG)
- 서비스는 실제 존재하는 것이 아니기 때문에 interface가 없음.
  - 단순히 k8s 메모리에 있는 가상의 구성요소
- 각 노드에서 실행되는 `kube-proxy`가 새로운 서비스가 생성되면 각 노드에 대한 적절한 규칙을 생성하고 이를 전달함.
	- [17-Service-Networking](docs/09-Networking/17-Service-Networking.md#^rkwg83)

## Kube-proxy 설치 - 수동
- 쿠버네티스 릴리스 페이지에서 kube-proxy 바이너리를 다운로드하세요 [kube-proxy](https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-proxy). 예를 들어 kube-proxy v1.13.0을 다운로드하려면 다음 명령어를 실행하세요.
  ```
  $ wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-proxy
  ```
- 압축을 해제하세요
- 서비스로 실행하세요

  ![kube-proxy1](../../images/kube-proxy1.PNG)

## Kube-proxy 옵션 확인 - kubeadm
- kubeadm 도구로 설정한 경우, kubeadm은 kube-proxy를 kube-system 네임스페이스에 파드로 배포합니다. 실제로는 마스터 노드에 데몬셋으로 배포됩니다.
  ```
  $ kubectl get pods -n kube-system
  ```
  ![kube-proxy2](../../images/kube-proxy2.PNG)
  
  
K8s 참조 문서:
- https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/
- https://kubernetes.io/docs/concepts/overview/components/
