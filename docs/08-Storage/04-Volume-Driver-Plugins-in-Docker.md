> [!summary]
> Volume 은 Volume Driver 에 의해 관리된다.
# Docker의 Volume Driver Plugins

  - [강의](https://kodekloud.com/topic/volume-driver-plugins-in-docker-4/)로 이동하기

이 섹션에서는 Docker의 **Volume Driver Plugins**에 대해 살펴본다.

- Storage drivers는 이미지와 컨테이너의 저장소를 관리하는 데 도움을 준다.
- 저장소를 지속적으로 유지하려면 볼륨을 생성해야 한다는 것을 이미 보았다. 볼륨은 storage drivers에 의해 처리되지 않으며, volume driver plugins에 의해 처리된다. 기본 volume driver plugin은 local이다.
	- local volume plugin은 Docker 호스트에서 볼륨을 생성하고 `/var/lib/docker/volumes/` 디렉토리 아래에 데이터를 저장하는 데 도움을 준다.
	- Azure 파일 스토리지, DigitalOcean Block Storage, Portworx, Google Compute Persistent Disks 등과 같은 타사 솔루션에서 볼륨을 생성할 수 있는 많은 다른 volume driver plugins가 있다.

![class-9](../../images/class9.PNG)


- When you run a Docker container, you can choose to use a specific volume driver, such as the RexRay EBS to provision a volume from the Amazon EBS. This will create a container and attach a volume from the AWS cloud. When the container exits, your data is safe in the cloud.

```
$ docker run -it --name mysql --volume-driver rexray/ebs --mount src=ebs-vol,target=/var/lib/mysql mysql
```
![class-10](../../images/class10.PNG)
#### Docker Reference Docs
- https://docs.docker.com/engine/extend/legacy_plugins/
- https://github.com/rexray/rexray

