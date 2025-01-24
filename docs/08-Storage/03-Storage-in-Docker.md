# Storage in Docker

  - [강의](https://kodekloud.com/topic/storage-in-docker-2/)로 이동하기

이 섹션에서는 Docker의 **Storage driver와 Filesystem**에 대해 살펴본다.

  - Docker가 데이터를 어디에 어떻게 저장하는지, 그리고 컨테이너의 파일 시스템을 어떻게 관리하는지 알아본다.

## File system
- 시스템에 Docker를 처음 설치하면 /var/lib/docker에 이 디렉토리 구조가 생성된다.
```
  $ cd /var/lib/docker/
```
![class-2](../../images/class2.PNG)
- 그 아래에는 aufs, containers, image, volumes 등의 여러 디렉토리가 있고 Docker가 기본적으로 모든 데이터를 저장하는 위치.
	- 컨테이너와 관련된 모든 파일은 containers 디렉토리에 저장
	- 이미지와 관련된 파일은 image 디렉토리에 저장
	- Docker 컨테이너에 의해 생성된 모든 볼륨은 volumes 디렉토리에 생성

- 지금은 Docker가 이미지와 컨테이너의 파일을 어디에 어떤 형식으로 저장하는지 이해하자.
- 이를 이해하기 위해서는 Docker의 Layered Architecture를 이해해야 한다.

## Layered Architecture
Docker가 이미지를 빌드할 때, Layered Architecture로 빌드한다. 
Docker 파일의 각 명령어는 **이전 레이어의 변경 사항만 포함하여** Docker 이미지에 새로운 레이어를 생성한다.
- layer-1은 Ubuntu 기본 레이어
- layer-2는 패키지를 설치하고
- layer-3은 Python 패키지를 설치하며
- layer-4는 소스 코드를 업데이트하고
- layer-5는 이미지의 Entrypoint을 업데이트한다.
 각 레이어는 이전 레이어의 변경 사항만 저장하므로, 크기에도 반영된다.
![class-3](../../images/class3.PNG)

- 이 Layered Architecture의 장점을 더 잘 이해하기 위해, 다른 Dockerfile을 살펴보자. 이는 우리의 첫 번째 애플리케이션과 매우 유사하지만, 소스 코드와 Entrypoint 가 다르다.
- 이미지를 빌드할 때, Docker는 첫 세 개의 레이어를 다시 빌드하지 않고, 첫 번째 애플리케이션을 위해 캐시에서 빌드한 동일한 세 개의 레이어를 재사용한다. 마지막 두 개의 레이어만 새로운 소스와 새로운 Entrypoint으로 생성된다.
- **이렇게 하면 Docker는 이미지를 더 빠르게 빌드하고 디스크 공간을 효율적으로 절약할 수 있다.** 
- 이는 애플리케이션 코드를 업데이트할 때도 적용된다. Docker는 캐시에서 모든 이전 레이어를 재사용하고 최신 소스 코드를 업데이트하여 애플리케이션 이미지를 빠르게 재빌드한다.

![class-4](../../images/class4.PNG)

- 레이어를 아래에서 위로 재배치하여 더 잘 이해해 보자. 이 모든 레이어는 Docker build 명령을 실행할 때 최종 Docker 이미지를 형성하기 위해 생성된다.
- 빌드가 완료되면 이러한 레이어의 내용을 수정할 수 없으며, 읽기 전용(read-only) 상태가 된다. 새로운 빌드를 시작해야만 수정할 수 있다.
![class-5](../../images/class5.PNG)

- 이 이미지를 기반으로 컨테이너를 실행할 때, **Docker run** 명령을 사용하여 Docker는 이러한 레이어를 기반으로 컨테이너를 생성하고 Image Layer 위에 새로운 **Writeable layer**를 생성한다.
	- **Writeable layer**는 애플리케이션에 의해 작성된 로그 파일, 컨테이너에 의해 생성된 임시 파일 등의 데이터를 저장하는 데 사용된다.
	- Writeable Layer 의 생명주기는 컨테이너가 살아있을 동안이다.
		- 컨테이너가 destroy되면 Writeable Layer와 그 안에 저장된 모든 변경 사항도 destroy된다. 
	- 아래의 Image Layer 는 이 이미지를 사용하여 생성된 모든 컨테이너가 공유한다.
![class-6](../../images/class6.PNG)

- 새로 생성된 컨테이너에 temp.txt라는 새 파일을 생성하면, 이는 읽기 및 쓰기가 가능하다.
- Image Layer의 파일은 읽기 전용이므로, 해당 레이어의 내용을 수정할 수 없다.
![class-7](../../images/class7.PNG)
애플리케이션 코드의 예를 들어보자. 코드를 이미지에 포함시키기 때문에, 코드는 이미지의 일부이며 Read Only이다. 컨테이너를 실행한 후 소스 코드를 수정하고 싶다면 어떻게 될까?
- 파일을 수정할 수는 있지만, 수정된 파일을 저장하기 전에 Docker는 자동으로 Read-Write Layer에 파일의 복사본을 생성하고, 나는 Read-Write Layer의 다른 버전을 수정하게 된다. 이후 모든 수정은 Read-Write Layer의 이 복사본에서 이루어진다. 이를 **copy-on-write** 메커니즘이라고 한다.
	- Image Layer가 읽기 전용이라는 것은 이러한 레이어의 파일이 이미지 자체에서 수정되지 않음을 의미한다. 
	- 따라서 **이미지는 Docker build 명령을 사용하여 이미지를 재빌드할 때까지 항상 동일하게 유지된다.** 
	- 컨테이너가 파괴되면 컨테이너 레이어에 저장된 모든 데이터도 삭제된다.

### Storage Driver
- Layered Architecture를 유지하고, Writeable layer를 생성하며, 복사 및 쓰기를 가능하게 하기 위해 레이어 간 파일을 이동하는 등의 작업을 수행하는 것은 **Storage Drivers**이다.
- Docker는 Layered Architecture를 가능하게 하기 위해 스토리지 드라이버를 사용한다.

#### Common Storage Drivers
- AUFS
- ZFS (ubuntu)
- BTRFS
- Device Mapper
- Overlay
- Overlay2

- 스토리지 드라이버의 선택은 기본 OS에 따라 달라진다. Docker는 운영 체제에 따라 자동으로 사용 가능한 최상의 스토리지 드라이버를 선택한다.

## Volumes
### Volume mount
- 컨테이너에서 지속적인 데이터를 얻으려면 `docker volume create` 명령을 사용하여 볼륨을 생성해야 한다. `docker volume create data_volume` 명령을 실행하면 `/var/lib/docker/`의 volumes 디렉토리 아래에 data_volume이라는 디렉토리가 생성된다.
  ```
  $ docker volume create data_volume

  $ ls -l /var/lib/docker/volumes/

  drwxr-xr-x 3 root root  4096 Aug 01 17:53 data_volume

  $ docker volume ls 

  DRIVER              VOLUME NAME
	local               data_volume
  ```
- `docker run` 명령을 사용하여 Docker 컨테이너를 실행할 때, `-v` 옵션으로 이 볼륨을 Docker 컨테이너 내부에 마운트할 수 있다.
- 따라서 `docker run -v`를 실행한 후 새로 생성한 볼륨 이름을 콜론(:)과 함께 지정하고, MySQL이 데이터를 저장하는 기본 위치인 `/var/lib/mysql`를 지정한 후 MySQL의 이미지 이름을 입력하면, 새로운 컨테이너가 생성되고 데이터 볼륨이 마운트된다.

	```
	$ docker run -v data_volume:/var/lib/mysql mysql
  ```
- 컨테이너가 파괴되더라도 데이터는 여전히 사용 가능하다.

- Docker run 명령 전에 Docker 볼륨을 생성하지 않았다면, Docker는 자동으로 볼륨을 생성하고 이를 컨테이너에 마운트한다.
  ```
  $ docker run -v data_volume2:/var/lib/mysql mysql

  $ docker volume ls

	DRIVER              VOLUME NAME
	local               data_volume
	local               data_volume2
  ```
- `/var/lib/docker`의 volumes 디렉토리 내용을 나열하면 이러한 모든 볼륨을 확인할 수 있다. 이를 **Volume mounting**이라고 한다.

### Bind Mounting
데이터가 이미 다른 위치에 있다면 어떻게 될까?
- Docker 호스트의 `/data` 경로에 외부 저장소가 있고, 데이터베이스 데이터를 기본 `/var/lib/docker` volumes 디렉토리가 아닌 해당 볼륨에 저장하고 싶다면, `docker run -v` 명령을 사용하여 컨테이너를 실행해야 한다. 이 경우 마운트할 디렉토리의 전체 경로인 `/data/mysql`을 제공하면, 컨테이너가 생성되고 해당 디렉토리가 컨테이너에 마운트된다. 이를 **Bind mounting**이라고 한다.

따라서 두 가지 유형의 마운트가 있다: **volume mount**, **bind mount**
  * 볼륨 마운트는 volumes 디렉토리에서 볼륨을 마운트하고, 바인드 마운트는 Docker 호스트의 임의의 위치에서 간접적으로 마운트한다.
- `-v` 옵션 대신 **`--mount`** 옵션이 더 많이 쓰임. (보다 명확함.)
  ```
  $ mkdir -p /data/mysql

  $ docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql
  ```
![class-8](../../images/class8.PNG)

#### Docker References

- https://docs.docker.com/storage/
- https://docs.docker.com/engine/reference/commandline/volume_create/
- https://docs.docker.com/engine/reference/commandline/volume_ls/
