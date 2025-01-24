# Persistent Volumes
  - [강의](https://kodekloud.com/topic/persistent-volumes-4/)로 이동하기

이 섹션에서는 **Persistent Volumes**에 대해 살펴본다.

- 대규모 환경에서 많은 사용자가 많은 Pods를 배포할 경우, 사용자는 각 Pod마다 매번 스토리지를 구성해야 한다.
- 어떤 스토리지 솔루션을 사용하든, Pods를 배포하는 사용자는 자신의 환경의 모든 Pod 정의 파일에서 이를 구성해야 한다. 변경이 필요할 때마다 사용자는 자신의 모든 Pods에서 변경을 해야 한다.

![class-16](../../images/class16.PNG)
- Persistent Volume은 클러스터 전체에서 사용되는 스토리지 볼륨의 풀로, 관리자가 애플리케이션을 클러스터에 배포하는 사용자를 위해 구성한다. 사용자는 이제 Persistent Volume Claims를 사용하여 이 풀에서 스토리지를 선택할 수 있다.

```yaml
  pv-definition.yaml
  
  kind: PersistentVolume
  apiVersion: v1
  metadata:
    name: pv-vol1
  spec:
    accessModes: [ "ReadWriteOnce" ]
    capacity:
     storage: 1Gi
    hostPath: // Production 환경에서는 Cloud Storage 사용
     path: /tmp/data
```
- `accessModes`
	- `ReadOnlyMany`
	- `ReadWriteOnce`
	- `ReadWriteMany`

  ```shell
  $ kubectl create -f pv-definition.yaml
  persistentvolume/pv-vol1 created

  $ kubectl get pv
  NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
  pv-vol1   1Gi        RWO            Retain           Available                                   3min
  
  $ kubectl delete pv pv-vol1
  persistentvolume "pv-vol1" deleted
  ```

#### Kubernetes Persistent Volumes

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://portworx.com/tutorial-kubernetes-persistent-volumes/
