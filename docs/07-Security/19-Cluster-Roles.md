# Cluster Roles
  - [비디오 튜토리얼](https://kodekloud.com/topic/cluster-roles/)로 이동하기
  
이 섹션에서는 cluster roles에 대해 살펴본다.

## Roles
- `Roles`와 `Rolebindings`는 네임스페이스가 있는 자원으로, 네임스페이스 내에서 생성된다.
	- 따로 명시하지 않으면 `default` 네임스페이스로 배정
  
  ![roles](../../images/roles.PNG)
  
## Namespaces
- 네임스페이스 내에서 노드를 그룹화하거나 격리할 수 있는가?
  - **불가능** : 노드는 클러스터 전체 또는 클러스터 범위 자원이다. 
    -> 특정 네임스페이스에 연관될 수 없다.
  
  ![namespace](../../images/namespace.PNG)
  
- 따라서 Resource 는 **Namespaced Resources** 또는 **Cluster Scoped Resources** 으로 분류된다.
- **Namespaced Resources**
  ```
  $ kubectl api-resources --namespaced=true
  ```
- **Cluster Scoped Resources**
  ```
  $ kubectl api-resources --namespaced=false
  ```
  ![namespace1](../../images/namespace1.PNG)
  
## Cluster Roles and Cluster Role Bindings
- Cluster Roles는 역할이지만 Cluster Scope Resources 에 대한 것이다. 종류는 **`ClusterRole`** 
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    name: cluster-administrator
  rules:
  - apiGroups: [""] # ""는 코어 API 그룹을 나타냄
    resources: ["nodes"]
    verbs: ["get", "list", "delete", "create"]
  ```
- `ClutserRoleBinding`
```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    name: cluster-admin-role-binding
  subjects:
  - kind: User
    name: cluster-admin
    apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: ClusterRole
    name: cluster-administrator
    apiGroup: rbac.authorization.k8s.io
  ```
  ```
  $ kubectl create -f cluster-admin-role.yaml
  $ kubectl create -f cluster-admin-role-binding.yaml
  ```
 ![cr1](../../images/cr1.PNG)

`ClusterRole` 에 Namespace scoped Resources (ex. Pods, deployments...) 도 할당 할 수 있다.
-> 이 Role을 가지면 모든 네임스페이스에서 이러한 resource 에 접근할 수 있다.

#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities
