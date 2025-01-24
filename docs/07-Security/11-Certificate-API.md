# Certificate API
  - [비디오 튜토리얼](https://kodekloud.com/topic/certificates-api/)로 이동하기
  
이 섹션에서는 Kubernetes에서 인증서 및 인증서 API를 관리하는 방법을 살펴본다.

## CA (Certificate Authority)
- CA는 우리가 생성한 키와 인증서 파일 쌍에 불과하며, 이 파일 쌍에 접근할 수 있는 사람은 Kubernetes 환경을 위한 모든 인증서에 서명할 수 있다.

#### Kubernetes에는 이를 수행할 수 있는 내장 인증서 API가 있다. 
- 인증서 API를 사용하여 이제 인증서 서명 요청(CSR)을 API 호출을 통해 Kubernetes에 직접 전송한다.
   
  ![csr](../../images/csr.PNG)
   
#### 이 인증서는 추출되어 사용자와 공유될 수 있다.
- 사용자는 먼저 키를 생성한다.
  ```
  $ openssl genrsa -out jane.key 2048
  ```
- CSR을 생성한다.
  ```
  $ openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr 
  ```
- 요청을 관리자에게 전송하고, 관리자는 키를 가져와 CSR 객체를 생성하며, 종류는 "CertificateSigningRequest"이고 인코딩된 "jane.csr"을 포함한다.
  ```
  apiVersion: certificates.k8s.io/v1beta1
  kind: CertificateSigningRequest
  metadata:
    name: jane
  spec:
    groups:
    - system:authenticated
    usages:
    - digital signature
    - key encipherment
    - server auth
    request:
      <certificate-goes-here>
  ```
  - `$ cat jane.csr | base64 ` : request 에는 base64로 인코딩된 sign 요청 유저의 `.csr` 이 들어가야함.
```
$ kubectl create -f jane.yaml
```
![csr1](../../images/csr1.PNG)
  
- CSR 목록을 나열한다.
  ```
  $ kubectl get csr
  ```
- 요청을 승인한다.
  ```
  $ kubectl certificate approve jane
  ```
- 인증서를 보기 위해
  ```
  $ kubectl get csr jane -o yaml
  ```
- 이를 디코드하기 위해
  ```
  $ echo "<certificate>" |base64 --decode
  ```
  ![csr2](../../images/csr2.PNG)
  
#### 모든 인증서 관련 작업은 컨트롤러 매니저에 의해 수행된다. 
- 인증서에 서명해야 하는 경우 CA 서버, 루트 인증서 및 개인 키가 필요하다. 컨트롤러 매니저 구성에는 이를 지정할 수 있는 두 가지 옵션이 있다.

  ![csr3](../../images/csr3.PNG)
  
  ![csr4](../../images/csr4.PNG)
  kube-controller-manager 안에 CA Public key, CA Private Key 를 지정하는 옵션이 있음.
  
#### K8s 참조 문서
- https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/
