# Docker Storage 소개

  - [강의](https://kodekloud.com/topic/introduction-to-docker-storage-3/)로 이동하기
  
이 섹션에서는 **Docker storage**에 대해 살펴본다.

- **Kubernetes**와 같은 컨테이너 오케스트레이션 도구에서의 스토리지를 이해하기 위해서는 먼저 컨테이너와 함께 스토리지가 어떻게 작동하는지를 이해하는 것이 중요하다. **Docker**에서 스토리지가 어떻게 작동하는지를 먼저 이해하고 기본을 확실히 하면, 이후 **Kubernetes**에서의 작동 방식을 이해하는 데 훨씬 수월해진다.

- **Docker**에 익숙하지 않은 경우, 무료로 제공되는 [Docker for the absolute beginner course](https://kodekloud.com/courses/docker-for-the-absolute-beginner/)에서 Docker의 기본을 배울 수 있다.

## Docker Storage

- Docker에는 두 가지 개념이 있다: Storage drivers와 Volume drivers plugins.

![class-1](../../images/class1.PNG)

#### 먼저 Storage drivers에 대해 논의하겠다.

#### Docker 참조 문서

- https://docs.docker.com/storage/storagedriver/
- https://docs.docker.com/storage/volumes/