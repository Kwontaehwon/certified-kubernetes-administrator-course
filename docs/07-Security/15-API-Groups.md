# API Groups
  - [비디오 튜토리얼](https://kodekloud.com/topic/api-groups/)로 이동하기
  
이 섹션에서는 Kubernetes에서 API Groups에 대해 살펴본다.
## API를 통해 버전 반환 및 포드 목록 가져오기

 ![api3](../../images/api3.PNG)
 
- Kubernetes API는 목적에 따라 여러 그룹으로 나뉜다. 예를 들어 **`APIs`**, **`healthz`**, **`metrics`**, **`logs`** 등을 위한 그룹이 있다.
  ![api4](../../images/api4.PNG)
 
## `/API`와 `/APIs`
- 이 API들은 두 가지로 분류된다.
  - Core Group - 모든 기능이 존재하는 곳
    
    ![api5](../../images/api5.PNG)
 
  - Named Group - 더 조직적이며 앞으로 모든 새로운 기능이 이러한 명명된 그룹에 제공될 예정이다.
    ![api6](../../images/api6.PNG)
    
- 모든 API 그룹을 나열하려면

  ![api7](../../images/api7.PNG)
  
## kube-apiserver 접근 인증
- 특정 API (Version) 말고는 `forbidden`. -> 인증서 파일을 전달하여 인증해야 한다.
  ![api8](../../images/api8.PNG)
  
- 대안 : **`kubectlproxy`** 클라이언트로 kube-api-server 접근
  ![api9](../../images/api9.PNG)
  
## kube proxy와 kubectl proxy
### `kubectl proxy` 
 - kube-api-server로의 프록시 서버
 - 개발 / 디버깅 목적으로 사용
### `kube proxy`
[10-Kube-Proxy](../02-Core-Concepts/10-Kube-Proxy.md)
- 클러스터의 각 노드에서 실행되는 네트워크 프록시
- 라우팅 역할 수행

![kp](../../images/kp.PNG)
  
## 주요 요점
- Named API / Core API
	- API Groups
		- Resources
			- Verbs
  ![api10](../../images/api10.PNG)

#### K8s 참조 문서
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/reference/using-api/api-concepts/
- https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/
