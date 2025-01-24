# Image Security
  - [비디오 튜토리얼](https://kodekloud.com/topic/image-security/)로 이동
이 섹션에서는 Image Security에 대해 살펴본다.
# Image
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-pod
  spec:
    containers:
    - name: nginx
      image: nginx
  ```
  
  ![img1](../../images/img1.PNG)
  
  ![img2](../../images/img2.PNG)
#### Registry
컨테이너 이미지가 저장되는 장소
- **`docker.io`** : Dockerhub (명시하지 않았을 때 default)
- `gcr.io` : GCP
	- k8s 관련 이미지들이 많이 위치
AWS, Azure 같은 클라우드 서비스는 default 로 private registry 를 제공하므로 거기에 저장하는 거도 좋음.

#### User/Account
사용자 ID (ex. dockerhub ID)
- Library : offical Image 가 저장되는 곳 (명시하지 않았을 때 default)
# Private Registry
- 레지스트리에 로그인
  ```
  $ docker login private-registry.io
  ```
- 개인 레지스트리에서 사용할 수 있는 이미지를 사용하여 애플리케이션을 실행
  ```
  $ docker run private-registry.io/apps/internal-app
  ```
  
  ![prvr](../../images/prvr.PNG)

- Docker credential 을 저장하기 위한 **`docker-registry`** k8s에 정의되어 있음.
	- 노드에서 Docker에 자격 증명을 전달하려면 먼저 자격 증명이 포함된 비밀 객체를 생성
  ```
  $ kubectl create secret docker-registry regcred \
    --docker-server=private-registry.io \ 
    --docker-username=registry-user \
    --docker-password=registry-password \
    --docker-email=registry-user@org.com
  ```
- `imagePullSecrets`섹션 아래에 있는 Pod 정의 파일에 secret 지정한다.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-pod
  spec:
    containers:
    - name: nginx
      image: private-registry.io/apps/internal-app
    imagePullSecrets:
    - name: regcred
  ```
  ![prvr1](../../images/prvr1.PNG)


  #### K8s Reference Docs
  - https://kubernetes.io/docs/concepts/containers/images/
