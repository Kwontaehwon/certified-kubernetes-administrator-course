# 백업 및 복원 방법
  - [비디오 튜토리얼](https://kodekloud.com/topic/backup-and-restore-methods/)로 이동하기
  
이 섹션에서는 백업 및 복원 방법을 살펴본다.

## Backup Candidates
 ![bc](../../images/bc.PNG)
 
## 리소스 구성
- 명령형 방식
  ![rci](../../images/rci.PNG)

- 선언형 방식 (선호하는 접근법)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: myapp-pod
    labels:
      app: myapp
      type: front-end
  spec:
    containers:
    - name: nginx-container
      image: nginx
  ```
 ![rcd](../../images/rcd.PNG)
 
- 리소스 구성을 GitHub과 같은 소스 코드 저장소에 저장하는 것이 좋은 관행

  ![rcd1](../../images/rcd1.PNG)

## 백업 - 리소스 구성

  ```
  $ kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml (일부 리소스 그룹에 대해서만)
  ```

- 고려해야 할 다른 많은 리소스 그룹이 있다. **`ARK`** 또는 현재 **`Velero`** 로 알려진 Heptio와 같은 도구가 이를 도와줄 수 있다.

  ![brc](../../images/brc.PNG)
  
## 백업 - ETCD
- 리소스를 백업하는 대신 ETCD 클러스터 자체를 백업할 수 있다. 
	- 클러스터의 모든 것이 저장되어 있기 때문
  
  ![be](../../images/be.PNG)
  
- **`etcdctl`** 유틸리티의 스냅샷 저장 명령을 사용하여 etcd 데이터베이스의 스냅샷을 찍을 수 있다.
  ```
  $ ETCDCTL_API=3 etcdctl snapshot save snapshot.db
  ```
  ```
  $ ETCDCTL_API=3 etcdctl snapshot status snapshot.db
  ```
  ![be1](../../images/be1.PNG)
  
## 복원 - ETCD
- 나중에 백업에서 etcd를 복원하려면 먼저 kube-apiserver 서비스를 중지한다.
  ```
  $ service kube-apiserver stop
  ```
- etcdctl 스냅샷 복원 명령을 실행한다.
- etcd 서비스를 업데이트한다.
- 시스템 구성을 다시 로드한다.
  ```
  $ systemctl daemon-reload
  ```
- etcd를 재시작한다.
  ```
  $ service etcd restart
  ```
  
  ![er](../../images/er.PNG)
  
- kube-apiserver를 시작한다.
  ```
  $ service kube-apiserver start
  ```
#### With all etcdctl commands specify the cert,key,cacert and endpoint for authentication.
```
$ ETCDCTL_API=3 etcdctl \
  snapshot save /tmp/snapshot.db \
  --endpoints=https://[127.0.0.1]:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/etcd-server.crt \
  --key=/etc/kubernetes/pki/etcd/etcd-server.key
```

  ![erest](../../images/erest.PNG)

관리형 K8s에서는 ETCD 에 접근하지 못할 수도 있음.
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/


 
