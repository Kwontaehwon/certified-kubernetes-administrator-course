# Volumes
  - [강의](https://kodekloud.com/topic/volumes/)로 이동하기

이 섹션에서는 **Volumes**에 대해 살펴본다.

- Docker 스토리지에 대해 논의했듯이, 컨테이너 런타임에서 볼륨을 연결하지 않으면 컨테이너가 파괴될 때 모든 데이터가 손실된다. 따라서 Docker 컨테이너에 데이터를 지속적으로 저장하기 위해 컨테이너가 생성될 때 볼륨을 연결해야 한다.
- 컨테이너에서 처리된 데이터는 이제 이 볼륨에 저장되어 영구적으로 유지된다. 컨테이너가 삭제되더라도 데이터는 볼륨에 남아 있다.

Kubernetes 세계에서 생성된 POD는 본질적으로 일시적이다. 데이터를 처리하기 위해 POD가 생성되고 삭제되면, 그에 의해 처리된 데이터도 삭제된다.
-> 유지를 위해서는 Volume이 필요하다.
- Volume 생성 시 Host의 경로 `/data`를 지정한다. 파일은 노드의 데이터 디렉토리에 저장된다.
- 컨테이너에서 `volumeMounts` 필드를 사용하여 Volume을 컨테이너 내의 `/opt` 디렉토리에 마운트한다. 
	- 이제 컨테이너 내의 `/opt` 마운트에 기록되며, 이는 실제로 호스트의 `/data` 디렉토리에 있는 volume에 해당한다. 
	- POD가 삭제되면 수정된 파일은 여전히 호스트에 남아 있다.

> [!summary]
> - Host `volumes`
> - Container `volumeMounts`

![class-14](../../images/class14.PNG)
## Volume Storage Options
- 볼륨에서 hostPath 볼륨 유형은 단일 노드에서만 사용해야하고 다중 노드 클러스터에서는 사용을 권장하지 않음.
	- 모든 노드에 `/data` 로 생성되기 때문에 제대로 유지되지 않는다.
- Kubernetes에서는 NFS, GlusterFS, CephFS와 같은 여러 표준 스토리지 솔루션이나 AWS EBS, Azure Disk, Google의 Persistent Disk와 같은 공용 클라우드 솔루션을 지원한다.

![class-15](../../images/class15.PNG)
```
volumes:
- name: data-volume
  awsElasticBlockStore:
    volumeID: <volume-id>
    fsType: ext4
```

#### Kubernetes Volumes Reference Docs
- https://kubernetes.io/docs/concepts/storage/volumes/
- https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/
- https://unofficial-kubernetes.readthedocs.io/en/latest/concepts/storage/volumes/
- https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#volume-v1-core
