# Authentication
  - [비디오 튜토리얼](https://kodekloud.com/topic/authentication/)로 이동하기
  
이 섹션에서는 Kubernetes 클러스터에서의 인증에 대해 살펴보겠습니다.

## 계정

  ![auth1](../../images/auth1.PNG)
  
#### 클러스터에 접근할 수 있는 다양한 사용자와 클러스터에 배포된 애플리케이션에 접근하는 최종 사용자의 보안은 애플리케이션 자체에서 내부적으로 관리된다.

 ![acc1](../../images/acc1.PNG)
 
- 따라서, 우리는 2가지 유형의 사용자로 나뉜다.
  - 인간, 예를 들어 관리자(Admin)와 개발자(Devlopers)
  - 로봇, 클러스터에 접근이 필요한 다른 프로세스/서비스 또는 애플리케이션


> [!NOTE] 
> k8s는 기본적으로 User 계정을 관리하지 않고 외부 소스에 의존한다.

  ![acc2](../../images/acc2.PNG)
  
- **모든 사용자 접근은 apiserver에 의해 관리되며 모든 요청은 apiserver를 통해 전달된다.**
 
  ![acc3](../../images/acc3.PNG)
  
## 인증 메커니즘
구성할 수 있는 다양한 인증 메커니즘이 있다.
- Static Password File
- Static Token File
- Certificates
- Identity Services
	- LDAP
	- Kerberos
  ![auth2](../../images/auth2.PNG)
  
## 인증 메커니즘 - 기본

> [!Warning] Deprecated
> (Deprecated in 1.19) - 보안 이슈

  - `.csv`로 유저 관리
	  - `password`, `username`, `userId`
  ![auth3](../../images/auth3.PNG)
  
## kube-apiserver 구성
- kubeadm을 통해 설정한 경우 kube-apiserver.yaml 파일을 자동으로 업데이트
  
  ![auth4](../../images/auth4.PNG)
  
## 사용자 인증

- API 서버에 접근할 때 기본 자격 증명을 사용하여 인증하려면 curl 명령어에서 사용자 이름과 비밀번호를 지정한다.
  ```
  $ curl -v -k http://master-node-ip:6443/api/v1/pods -u "user1:password123"
  ```
  ![auth5](../../images/auth5.PNG)


#### 사용자 그룹 할당
user-details.csv 파일에 추가 열로 설정
#### Static Token file
- Static Token file로 password 대신 토큰 정의 가능
- User API 요청시 인자로 password가 아니라 토큰 전달

  ![auth6](../../images/auth6.PNG)
  
## 참고
 - 그냥 텍스트로 저장하는 위의 방식은 Best Practice가 아니고 안전하지도 않음.
 ![note](../../images/note.PNG)
  
  
#### K8s 참조 문서
- https://kubernetes.io/docs/reference/access-authn-authz/authentication/ 
