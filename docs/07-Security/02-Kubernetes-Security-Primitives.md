# Kubernetes Security Primitives
  - [비디오 튜토리얼](https://kodekloud.com/topic/kubernetes-security-primitives/)로 이동하기
  
이 섹션에서는 Kubernetes 보안 원칙에 대해 살펴보겠습니다.


> [!Title] 요점
> - 누가 접근할 수 있는가?
> - 접근할 수 있다면 어떤 권한을 가지고 있는가?

## Secure Hosts

 ![sech](../../images/sech.PNG)
  
## Secure Kubernetes
- 두 가지 유형의 결정을 내려야 한다.
  - 누가 접근할 수 있는가?
  - 그들이 무엇을 할 수 있는가?
 
  ![seck](../../images/seck.PNG)
  
## Authentication
- API Server에 접근할 수 있는 사람은 인증 메커니즘에 의해 정의된다.
  
## Authorization
- 클러스터에 접근한 후, 그들이 무엇을 할 수 있는지는 권한 부여 메커니즘에 의해 정의된다.

## TLS Certificates
- 클러스터와의 모든 통신은 ETCD 클러스터, kube-controller-manager, 스케줄러, API 서버와 같은 다양한 구성 요소 간의 통신 및 kubelet과 kubeproxy와 같은 작업 노드에서 실행되는 구성 요소 간의 통신을 TLS 암호화를 사용하여 보호한다.

 ![tls](../../images/tls.PNG)
 
## Network Policies
클러스터 내 애플리케이션 간의 통신은 어떻게 되는가?

  ![np](../../images/np.PNG)
