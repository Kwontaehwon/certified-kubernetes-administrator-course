# OS 업그레이드
  - [비디오 튜토리얼](https://kodekloud.com/topic/os-upgrades/)로 이동하기
  
이 섹션에서는 OS 업그레이드에 대해 살펴보겠습니다.

#### 노드가 5분 이상 다운되면, 해당 노드에서 pods가 종료됩니다.
- 5분 값은 `kube-controller-manager`의 `--pod-eviction-timeout`에 저장
- 해당 Pod가 `replicaSet`에 속해있으면 다른 노드에 Pod 생성
- 해당 Pod가 `replicaSet`에 속해있지 않으면 그냥 삭제
  ![os](../../images/os.PNG)
  
- 노드의 모든 작업 부하를 **`drain`** 하여 다른 노드로 이동할 수 있습니다.
  - drain 한 노드의 Pod 를 **삭제후 다른 노드에 재성성** 
  - drain 한 노드는 새로운 Pod 가 스케쥴 될 수 없음.
  ```
  $ kubectl drain node-1
  ```
- 노드는 또한 cordoned 또는 스케줄 불가능으로 표시됩니다.
- 유지 관리 후 노드가 다시 온라인 상태가 되면 여전히 스케줄 불가능합니다. 그러므로 uncordon 해야 합니다.
  ```
  $ kubectl uncordon node-1
  ```
- cordon이라는 또 다른 명령어도 있습니다. Cordon은 단순히 노드를 스케줄 불가능으로 표시합니다. drain과는 달리 기존 노드에서 pods를 종료하거나 이동하지 않습니다.
  ```
  $ kubectl cordon node-1
  ```
  ![drain](../../images/drain.PNG)
  
  
#### K8s 참조 문서
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
