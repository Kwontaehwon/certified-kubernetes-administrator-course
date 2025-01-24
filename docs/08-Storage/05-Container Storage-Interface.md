# Container Storage Interface
  - [강의](https://kodekloud.com/topic/container-storage-interface/)로 이동하기

이 섹션에서는 **Container Storage Interface**에 대해 살펴본다.

## Container Runtime Interface
- Kubernetes는 Docker를 단독으로 컨테이너 런타임 엔진으로 사용하였으며, Docker와 작업하기 위한 모든 코드는 Kubernetes 소스 코드에 내장되어 있었다. 
	- 현재는 rkt 및 CRI-O와 같은 다른 컨테이너 런타임도 존재한다.
- Container Runtime Interface는 Kubernetes와 같은 오케스트레이션 솔루션이 Docker와 같은 컨테이너 런타임과 통신하는 방법을 정의하는 표준이다.
	- 새로운 컨테이너 런타임 인터페이스가 개발되면 CRI 표준을 따르기만 하면 된다.
![class-11](../../images/class11.PNG)
## Container Networking Interface
- 다양한 네트워킹 솔루션을 지원하기 위해 Container Networking Interface가 도입되었다. 새로운 네트워킹 공급자는 CNI 표준을 기반으로 플러그인을 개발하여 Kubernetes와 함께 작동하도록 할 수 있다.
![class-12](../../images/class12.PNG)

## Container Storage Interface
Container Storage Interface는 여러 스토리지 솔루션을 지원하기 위해 개발되었다. CSI를 사용하면 Kubernetes와 함께 작동할 수 있는 자신의 스토리지 드라이버를 작성할 수 있다.
- Portworx, Amazon EBS, Azure Disk, GlusterFS 등이 있다.
- CSI는 Kubernetes 전용 표준이 아니다. 이는 보편적인 표준을 의미하며, 구현되면 모든 컨테이너 오케스트레이션 도구가 지원되는 플러그인을 가진 모든 스토리지 공급자와 함께 작동할 수 있게 한다. Kubernetes, Cloud Foundry 및 Mesos가 CSI에 참여하고 있다.
- 이는 컨테이너 오케스트레이터에 의해 호출될 RPC(Remote Procedure Call) 집합을 정의한다. 이러한 호출은 스토리지 드라이버에 의해 구현되어야 한다.
![class-13](../../images/class13.PNG)
![](images/05-Container%20Storage-Interface.png)


#### Container Storage Interface 
- https://github.com/container-storage-interface/spec
- https://kubernetes-csi.github.io/docs/
- http://mesos.apache.org/documentation/latest/csi/
- https://www.nomadproject.io/docs/internals/plugins/csi#volume-lifecycle
