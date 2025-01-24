(직접 추가)
### Docker Namespace
![](images/23-(Pre-requisite)%20Security%20in%20Docker.png)
Docker는 Host 위에서 실행된다. (커널을 공유한다.)
리눅스 시스템의 `namespace` 를 이용해서 격리.
![](images/23-(Pre-requisite)%20Security%20in%20Docker-1.png)
`ps aux` 로 프로세스를 확인하면 namespace로 격리된 컨테이너도 하나의 프로세스로 표시된다.
-> Host의 컨테이너 외부에서 프로세스는 **`root`** 사용자로 실행됨. (물론 일반 root User 와는 좀 다름)

 
보안 이슈를 막기 위해서 `-user` 옵션으로 root 사용자가 아닌 user로 컨테이너 실행 가능.
```shell
docker run -user=1000 ubuntu sleep 3600
```

혹은 Dockerfile에 User를 정의하는 방식도 가능.
```Dockerfile
FROM Ubuntu

USER 1000
```
```shell
docker built -t my-ubuntu-image
docker run my-ubuntu-image sleep 3600
```


## Docker 내 root user 권한 제한 정책
![](images/23-(Pre-requisite)%20Security%20in%20Docker-3.png)
User 권한 제한도 Linux 기능을 활용하여 제한.

![](images/23-(Pre-requisite)%20Security%20in%20Docker-4.png)

#### `--cap-add`
```shell
docker run --cap-add MAC_ADMIN ubuntu
```
특정 권한 추가
#### `--cap-drop`
```shell
docker run --cap-add MAC_ADMIN ubuntu
```
특정 권한 제거
#### `--previleged`
```shell
docker run --cap-add MAC_ADMIN ubuntu
```
모든 권한 활성화