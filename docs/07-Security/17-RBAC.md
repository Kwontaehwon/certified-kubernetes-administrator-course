# RBAC
  - [비디오 튜토리얼](https://kodekloud.com/topic/role-based-access-controls/)로 이동하기

이 섹션에서는 RBAC에 대해 살펴본다.

## 역할을 어떻게 생성하나요?
- 각 역할은 3개의 섹션으로 구성된다.
  - `apiGroups` : 비워둘 경우 Core API group
  - `resources`
  - `verbs`
 kubectl 명령어로 Role 생성
  ```
  $ kubectl create -f developer-role.yaml
  ```

## User Role Binding
- 이를 위해 **`RoleBinding`** 이라는 또 다른 객체를 생성한다. 이 역할 바인딩 객체는 사용자 객체를 역할에 연결한다.
- kubectl 명령어로 역할 바인딩을 생성한다.
  ```
  $ kubectl create -f devuser-developer-binding.yaml
  ```
- 또한 role과 role binding은 네임스페이스의 범위에 속한다는 점에 유의해야 한다.
```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: developer
  rules:
  - apiGroups: [""] # ""는 Core API 그룹을 나타냄
    resources: ["pods"]
    verbs: ["get", "list", "update", "delete", "create"]
  - apiGroups: [""]
    resources: ["ConfigMap"]
    verbs: ["create"]
```

```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: devuser-developer-binding
  subjects: # User 정보
  - kind: User
    name: dev-user # "name"은 대소문자를 구분함
    apiGroup: rbac.authorization.k8s.io
  roleRef: # Role 정보
    kind: Role
    name: developer
    apiGroup: rbac.authorization.k8s.io
```
  ![rbac1](../../images/rbac1.PNG)
  

## RBAC 보기
  
- 역할 목록을 보려면
  ```
  $ kubectl get roles
  ```
- 역할 바인딩 목록을 보려면
  ```
  $ kubectl get rolebindings
  ```
- 역할을 설명하려면 
  ```
  $ kubectl describe role developer
  ```
  
  ![rbac2](../../images/rbac2.PNG)
    
- 역할 바인딩을 설명하려면
  ```
  $ kubectl describe rolebinding devuser-developer-binding
  ```
  
  ![rbac3](../../images/rbac3.PNG)
  
#### 사용자가 클러스터 내 특정 리소스에 접근할 수 있는지 확인하고 싶다면?
## 접근 확인

- `kubectl auth` 명령어를 사용할 수 있다.
	- `can-i` 로 명령 실행이 가능한지 확인
	- `--as` 로 특정 유저의 권한 확인
  ```
  $ kubectl auth can-i create deployments
  $ kubectl auth can-i delete nodes
  ```
  ```
  $ kubectl auth can-i create deployments --as dev-user
  $ kubectl auth can-i create pods --as dev-user
  ```
  ```
  $ kubectl auth can-i create pods --as dev-user --namespace test
  ```
  
  ![rbac5](../../images/rbac5.PNG)
  
## Resource Name
- `resourceNames`으로 리소스안의 특정 인스턴스에 대한 권한 부여
	-> 더 세부적인 규칙
  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: developer
  rules:
  - apiGroups: [""] # ""는 코어 API 그룹을 나타냄
    resources: ["pods"]
    verbs: ["get", "update", "create"]
    resourceNames: ["blue", "orange"]
  ```  
  ![rbac4](../../images/rbac4.PNG)
  
#### K8s 참조 문서
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities
