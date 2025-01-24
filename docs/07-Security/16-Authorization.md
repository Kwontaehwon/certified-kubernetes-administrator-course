# 권한 부여
  - [비디오 튜토리얼](https://kodekloud.com/topic/authorization/)로 이동하기
  
이 섹션에서는 Kubernetes에서의 권한 부여를 살펴본다.
## 클러스터에서 권한 부여가 필요한 이유
- 관리자로서 모든 작업을 수행할 수 있다.
  ```
  $ kubectl get nodes
  $ kubectl get pods
  $ kubectl delete node worker-2
  ```

User 마다 권한을 다르게 해줘야 할 필요 O
  ![at1](../../images/at1.PNG)
  
## 권한 부여 메커니즘
- Kubernetes에서 지원하는 다양한 권한 부여 메커니즘이 있다.
  - 노드 권한 부여 (Node Authorization)
  - 속성 기반 권한 부여 (Attribute-based Authorization, ABAC)
  - 역할 기반 권한 부여 (Role-Based Authorization, RBAC)
  - 웹훅 (Webhook)
  
## 노드 권한 부여
  ![node-auth](../../images/node-auth.png)
  system:node:nodename 의 그룹에 속해있는 node가 사용할 수 잇음.
  [06-TLS-in-Kubernetes](06-TLS-in-Kubernetes.md)

## ABAC (Attribute Based)
**사용자 or 그룹에 직접 권한을 부여**
정책이 바뀔 때 마다 파일을 직접 수정해야함.
-> 관리 어려움
  ![abac](../../images/abac.PNG)
  
## RBAC (Role-Based)
Role 을 만들고 그 Role에 User, Group을 매핑.
-> ABAC 보다 일반적이고 효율적
  ![rbac](../../images/rbac.PNG)

## 웹훅
  외부에서 3rd party tool 로 권한 조정하고 싶을 때
  ![webhook](../../images/webhook.PNG)
  
## 권한 부여 모드
- 위 4가지 외에 `AlwaysAllow`, `AlwaysDeny` 2개 옵션이 더 있음.
- **모드 옵션은 kube-apiserver에서 정의할 수 있다.**  
	![mode](../../images/mode.PNG)
  
- 여러 모드를 지정할 경우, 지정된 순서대로 권한 부여가 이루어진다.
	- NODE 권한 얻기 실패 -> RBAC로 이동 -> RBAC 권한 얻기 실패 -> WEBHOOK 이동...
		- 권한을 얻을 때 까지 뒤로 이동한다.
  ![mode1](../../images/mode1.PNG)

  #### K8s 참조 문서
  - https://kubernetes.io/docs/reference/access-authn-authz/authorization/
