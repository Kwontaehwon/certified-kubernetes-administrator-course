# Persistent Volume Claims
  - [강의](https://kodekloud.com/topic/persistent-volume-claims-4/)로 이동

이 섹션에서는 **Persistent Volume Claim**에 대해 살펴본다.

-노드에 스토리지를 사용할 수 있도록 Persistent Volume Claim을 생성한다.
- Volume과 Persistent Volume Claim은 Kubernetes 네임스페이스에서 두 개의 별도 객체이다.
- Persistent Volume Claim이 생성되면, Kubernetes는 요청 및 볼륨에 설정된 속성을 기반으로 Persistent Volume을 바인딩한다.
![class-17](../../images/class17.PNG)

- If properties not matches or Persistent Volume is not available for the Persistent Volume Claim then it will display the pending state.
```yaml
pvc-definition.yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myclaim
spec:
  accessModes: [ "ReadWriteOnce" ]
  resources:
   requests:
     storage: 1Gi
```

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
    hostPath:
     path: /tmp/data
```

#### Create the Persistent Volume
```
$ kubectl create -f pv-definition.yaml
persistentvolume/pv-vol1 created

$ kubectl get pv
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-vol1   1Gi        RWO            Retain           Available                                   10s
```


#### Create the Persistent Volume Claim
```
$ kubectl create -f pvc-definition.yaml
persistentvolumeclaim/myclaim created

$ kubectl get pvc
NAME      STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
myclaim   Pending                                                     35s

$ kubectl get pvc
NAME      STATUS   VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
myclaim   Bound    pv-vol1   1Gi        RWO                           1min

```

#### Delete the Persistent Volume Claim
```
$ kubectl delete pvc myclaim
```


#### Delete the Persistent Volume
```
$ kubectl delete pv pv-vol1
```
- `persistanceVolumeReclaimPolicy` 옵션을 통해 할당된 POD 삭제 시 영향을 결정할 수 있다.
	- `retain` : 삭제 후 다른 POD에 배정하지 않음
	- delete
	- recycle 

#### Kubernetes Persistent Volume Claims Reference Docs
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims
- https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#persistentvolumeclaim-v1-core
- https://docs.cloud.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengcreatingpersistentvolumeclaim.htm
