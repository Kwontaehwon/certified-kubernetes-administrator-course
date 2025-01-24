
> [!Note] 원본 없음
> 왜 없는지 모르겠지만 Service Accounts 에 대한 Docs만 누락됨.

## User / Service
![](images/21-Service%20Accounts.png)
- User : 실제 K8s를 사용하는 사람
- Service : 컴퓨터에서 사용됨
	- Prometheus 가 서비스 계정을 사용하여 k8s API를 가져옴
	- Jenkins

### 예시
![](images/21-Service%20Accounts-1.png)
내가 만든 k8s 대시보드 앱에서 k8s API를 쿼리하려면 인증(Authentication)이 필요
-> 이때 Service Account 사용

### 명령어

- `serviceaccount` 생성
```bash
kubectl create serviceaccount dashboard-sa
```
- `serviceaccount` 보기
```bash
kubectl get serviceaccount
```

![](images/21-Service%20Accounts-2.png)
`serviceaccount` 가 생성되면 자동으로 토큰이 `Secret` 으로 생성된다.
이 토큰을 이용해서 외부 응용프로그램이 인증을 할 수 있게 된다.

## `serviceaccount` 생성과정 (v1.24 전까지)
![](images/21-Service%20Accounts-3.png)
`kubectl create serviceaccount dashboard-sa` 이 실행되었을 때.
1. `serviceaccount` 생성
2. 토큰 자동 생성 (jwt토큰)
3. 토큰을 값으로 가진 `Secret` 객체 생성
4. 이 `Secret` 객체가 `serviceaccount` 에 바인딩

### REST API 에서 사용
![](images/21-Service%20Accounts-4.png)
`--header "Authorization: Bearer <Token value>"`

> [!info]
> k8s 클러스터 안에서 실행되는 3rd-party-app (프로메테우스 등) 은 더 쉽게 Authenciation 을 할 수 있음.


모든 Namespace 는 **`default`** service account 를 가지고 있다.
Pod가 생성될 때 마다 default serviceaccount 와 토큰이 해당 POD의 volume mount로 자동으로 mount 된다.
![](images/21-Service%20Accounts-5.png)
POD를 생성할 때 Volume이나 Secret 을 지정하지 않아도 기본으로 배정되는데 이것이 `default` service account 와 그 토큰이다.

![](images/21-Service%20Accounts-6.png)
실제로 POD describe 에 저장된 결과를 따라서 가보면 deafult service account 에 대한 실제 Secret 토큰이 저장되어 있는 것을 확인할 수 있다.

`default` serviceAccount는 기본 k8s API Query 만을 실행할 수 있다.

### serviceAccount 옵션
- deafult 말고 다른 serviceAccount를 POD에 명시하고 싶을 경우 `spec.serviceAccountName` 에 쓴다.
- default serviceAccount를 할당하고 싶지 않은 경우 `spec.automountServiceAccountToken: False` 로 줘야 한다.


- 실행되고 있는 POD 에 대한 serviceAccount 정보를 변경할 수는 없지만, Deployment는 가능하다.
	- Deployment의 serviceAccount가 변경되면 새로운 depoy 가 트리거 되고 현재 POD들을 삭제하고 변경된 serviceAccount로 새로운 POD를 생성한다.



## v1.22, v1.24 변경사항
![](images/21-Service%20Accounts-7.png)
serviceAccount 에서 생성되는 jwt 토큰은 Expire Date가 없이 생성되어 보안 이슈가 있다.

### v1.22 : `TokenRequestAPI`
![](images/21-Service%20Accounts-8.png)
- Audience Bound
- Time Bound
- Ojbect Bound
![](images/21-Service%20Accounts-9.png)
`projected` Volume 으로 변경

### v1.24
![](images/21-Service%20Accounts-10.png)
v1.24 부터는 더 이상 serviceAccount를 만들면서 token을 함께 만들지 않는다.
또한 토큰이 만들어질 때 expire date를 설정해야하며, 설정하지 않을 경우 1시간임.

Non-expired Token 을 만드는 방법도 있으나 Best Practice는 아님.

#### k8s Reference
https://kubernetes.io/docs/concepts/security/service-accounts/